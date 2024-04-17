![api-gateway-1](/assets/images/api-gateway-1.png)

# API Gateway Service

microservice management flatform pattern

## API gateway service의 역할

![api-gateway-2](/assets/images/api-gateway-2.png)
- 인증 및 권한 부여
- 서비스 검색 통합
- 응답 캐싱
- 정책, 회로 차단기 및 Qos 다시 시도
- 속도 제한
- 부하 분산
- 로깅, 추적, 상관 관계
- 헤더, 쿼리 문자열 및 청구 변환
- IP 허용 목록에 추가

- 사용자가 설정한 라우팅 설정에 따라서 각각 엔드포인트로 클라이언트를 대신해서 요청하고 응답을 받으면 다시 클라이언트한테 전달해주는 proxy 역할
- 시스템의 내부 구조는 숨기고 외부의 요청에 대해서 적절한 형태로 가공해서 응답할 수 있다는 장점
- 서비스의 주소가 변경되거나 파라미터의 인자값이 변경되는 경우 마이크로 서비스 자체가 독립적으로 빌드가 되고 배포가 되는 장점
- 이런 이유로 단일 진입점을 가진 형태로 개발하는 패러다임이 펼쳐짐

![api-gateway-3](/assets/images/api-gateway-3.png)

단일 진입점을 가진 패러다임을 통해 서버단(백엔드)과 클라이언트단 중간에 게이트웨이 역할을 해줄 수 있는 진입로, API gateway를 두어 각각의 마이크로 서비스로 요청되는 모든 정보에 대해서 일괄적으로 처리할 수 있도록 한다.

즉, 클라이언트는 API gateway로만 호출하면 마이크로 서비스에게 직접적으로 호출할 필요 없음

이를 통해 **서비스의 검색 통합. 응답할 수 있는 캐싱 정보 저장, 일괄적인 정책 반영, 문제 발생 클라이언트로부터의 회로 차단, 속도 제한, 로드밸런싱, 부하 분산 처리, 로깅-추적-상관 관계 파악, 헤더-쿼리 문자열 및 청구 변환, IP 화이트/블랙 리스트**를 실현 가능하다.

단점은 개발, 배포 및 관리해야 하는 지점 증가, Endpoint 노출을 위해 gateway 업데이트해야 하는 개발 병목 현상

- single point of failure (SPoF) 단일 장애점

  gateway 서버가 죽은 경우에 대해서 모든 서비스가 중단되는 문제가 발생할 수 있다.
  → Backend For Frontend (BFF) 가 극복 방안
  → set API gateway each API.

![api-gateway-4](/assets/images/api-gateway-4.png)

filter → prefilter, postfilter, gateway, exception, network, load balance, default-filter etc.

predicate → condition of response

route → request for API

---

@Configuration (정식 필터 클래스)으로 선언된 클래스에서 headerValue로 선언을 통해 filter에서 걸러준다.
그리고 @RequestHeader 를 통해 API 중에 해당하는 API 찾아서 요청을 보내준다.

```java
@Configuration
public class FilterConfig {
    @Bean
    public RouteLocator gatewayRoutes(RouteLocatorBuilder builder) {
        return builder.routes()
                .route(r -> r.path("/first-service/**")
                            .filters(f -> f.addRequestHeader("first-request", "first-request-header")
                                           .addResponseHeader("first-response", "first-response-header"))
                            .uri("http://localhost:8081"))
                .route(r -> r.path("/second-service/**")
                        .filters(f -> f.addRequestHeader("second-request", "second-request-header")
                                .addResponseHeader("second-response", "second-response-header"))
                        .uri("http://localhost:8082"))
                .build();
    }
}
```

사용자 정의 필터, 커스텀 필터는 @Component 로 선언한 필터 클래스에서 진행 extends AbstractGatewayFilterFactory<CustomFilter.config> 필요

```java
@Component
@Slf4j
public class CustomFilter extends AbstractGatewayFilterFactory<CustomFilter.Config> {
    public CustomFilter() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        // Custom Pre Filter (before request)
        return (exchange, chain) -> {
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("Custom PRE filter: request id -> {}", request.getId());

            // Custom Post Filter (after request)
            return chain.filter(exchange).then(Mono.fromRunnable(() -> {
                log.info("Custom POST filter: response code -> {}", response.getStatusCode());
            }));
        };
    }

    public static class Config {
        // Put the configuration properties
    }
}
```

위의 커스텀 필터는 개별적으로 원하는 라우팅 정보를 등록해야 한다는 불편함이 있다.

이런 단점을 극복한, 공통적으로 시행되는 필터, 공통 필터를 만드는 방법이 있다.

글로벌 필터 (Global Filter)는 아래와 같이 하면 된다.

원하는 정보는 Config 로부터 얻어오는 것이다.

그리고 이 필터를 default-filters로 설정하면 된다.

이 글로벌 필터가 먼저 실행되고, 그 다음에 해당 API에 묶인 커스텀필터가 실행된다.

```java
@Component
@Slf4j
public class GlobalFilter extends AbstractGatewayFilterFactory<GlobalFilter.Config> {
    public GlobalFilter() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        return ((exchange, chain) -> {
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("Global Filter baseMessage: {}, {}", config.getBaseMessage(), request.getRemoteAddress());
            if (config.isPreLogger()) {
                log.info("Global Filter Start: request id -> {}", request.getId());
            }
            return chain.filter(exchange).then(Mono.fromRunnable(()->{
                if (config.isPostLogger()) {
                    log.info("Global Filter End: response code -> {}", response.getStatusCode());
                }
            }));
        });
    }

    @Data
    public static class Config {
        private String baseMessage;
        private boolean preLogger;
        private boolean postLogger;
    }
}

```

```java
spring:
  application:
    name: apigateway-service
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
  cloud:
    gateway:
      default-filters:
        - name: GlobalFilter
          args:
            baseMessage: Spring Cloud Gateway Global Filter
            preLogger: true
            postLogger: true
```

이제 로깅을 더욱 그럴듯한 로깅으로 만들기 위한 로깅 필터 (Logging Filter)를 알아보자

```java
@Component
@Slf4j
public class LoggingFilter extends AbstractGatewayFilterFactory<LoggingFilter.Config> {
    public LoggingFilter() {
        super(Config.class);
    }

    @Override
    public GatewayFilter apply(Config config) {
        GatewayFilter filter = new OrderedGatewayFilter((exchange, chain) -> {
            ServerHttpRequest request = exchange.getRequest();
            ServerHttpResponse response = exchange.getResponse();

            log.info("Logging Filter baseMessage: {}", config.getBaseMessage());
            if (config.isPreLogger()) {
                log.info("Logging PRE Filter: request id -> {}", request.getId());
            }
            return chain.filter(exchange).then(Mono.fromRunnable(()->{
                if (config.isPostLogger()) {
                    log.info("Logging POST Filter: response code -> {}", response.getStatusCode());
                }
            }));
        }, Ordered.LOWEST_PRECEDENCE);

        return filter;
    }

    @Data
    public static class Config {
        private String baseMessage;
        private boolean preLogger;
        private boolean postLogger;
    }
}
```

```java
  gateway:
      default-filters:
        - name: GlobalFilter (여기 array라서 여러 개 넣을 수 있다.)
          args:
            baseMessage: Spring Cloud Gateway Global Filter
            preLogger: true
            postLogger: true
      routes:
          filters:
            - AddRequestHeader=second-request, second-request-header2
            - AddResponseHeader=second-response, second-response-header2
            - name: LoggingFilter
              args:
                baseMessage: Hi, there.
                preLogger: true
                postLogger: true
```

apply() 메서드를 구현시켜주어야 한다. 이는 반환값으로 gateway filter 역할을 한다.

이 필터의 역할이 정해지면 이 내용이 모든 API, Spring Cloud, gateway로 전송되는 모든 client들에서 공통적인 작업을 처리할 수 있게 된다.

왜 lambda 표현식을 사용하는가?
→ global filter is instance, so can’t create directly.
→ gateway filter가 가지고 있어야 할 parameter가 있다. 그 parameter를 filter라는 method까지 한꺼번에 구현하기 때문에 lambda 표현식을 통해 간결하게 표현하는 것.

GatewayFilter filter = new **OrderedGatewayFilter**((exchange, chain) -> {
→ this filter is implements GatewayFilterChain class.

}, Ordered.LOWEST_PRECEDENCE);
→ OrderGatewayFilter는 두 번째 매개변수가 필요하다. 우선순위 값이다.
이를 HIGHEST_PRECEDENCE 혹은 LOWEST_PRECEDENCE 로 선언
이 값을 통해 어떤 필터가 먼저 실행될 지 설정 가능

```java
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        return this.delegate.filter(exchange, chain);
    }
```

이 상속받은 메서드가 제일 중요하다. 이 메서드를 재정의 함으로써 이 필터가 해야할 역할을 재정의할 수 있다.

ServerWebExchange 는 server request, server response 두 instance를 지원한다. == HTTP Servlet 과 Request를 지원한다.

- zuul

  gateway 역할을 하는 어플리케이션 == service application 이라고도 불림.

  zuul1이 동기와 블로킹 방식(서블릿 프레임워크 기반), zuul2가 비동기와 논블로킹 방식을 지원한다.


## API gateway usage

- request에 대한 rewriting

  한 번 받은 request의 json은 한 번 열어보면 두 번 보낼 순 없다. 이런 경우, 한 번 열어본 request를 API gateway에서 나누어 보내질 API 를 정해 선별적으로 request를 처리할 수 있다.

- 암호화 protocol을 사용하는 경우

  request를 받을 때 복호화 필터를 거치도록 하고, response를 보낼 때 암호화 필터를 거치도록 해서 모든 메서드가 공동의 작업을 할 수 있도록 함

- log divide

  API gateway를 통해 log 구분 및 에러 로그만 구분하여 로깅하도록 할 수 있음


# Eureka

![api-gateway-5](/assets/images/api-gateway-5.png)

application.yml 파일에서 port를 0으로 설정하면 랜덤 포트 번호가 부여된다.