# Contents

- [Elasticsearch](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [metricbeat](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [kibana](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [filebeat](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [logstach](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [kafka](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)
- [Best Environment](https://www.notion.so/Elasticsearch-Analysis-34d5dca9a5914bf5b8fa979c6b1eb235)

# Elasticsearch

[https://app.strigo.io/training/ondemand/stLthQ4oZyvy28gsu](https://app.strigo.io/training/ondemand/stLthQ4oZyvy28gsu)

[https://learn.elastic.co/learn/course/391/play](https://learn.elastic.co/learn/course/391/play)

구독 상품별 사용 가능 기능 리스트 > [https://www.elastic.co/subscriptions](https://www.elastic.co/subscriptions)

### Installation:

[https://elk-docker.readthedocs.io/#installation](https://elk-docker.readthedocs.io/#installation)

### how monitoring works:

Each Elasticsearch node, Logstash node, Kibana instance, and Beat instance is considered
unique in the cluster based on its persistent UUID, which is written to the
`[path.data](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/important-settings.html#path-settings)` directory when the node or instance starts.

Monitoring documents are just ordinary JSON documents built by monitoring each
Elastic Stack component at a specified collection interval. If you want to alter the
templates for these indices, see *[Configuring indices for monitoring](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/config-monitoring-indices.html)*.

Metricbeat is used to collect monitoring data and to ship it directly to the
monitoring cluster.

To learn how to collect monitoring data, see:

- *[Legacy collection methods](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/collecting-monitoring-data.html)*
- *[Collecting monitoring data with Metricbeat](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/configuring-metricbeat.html)*
- [Monitoring Kibana](https://www.elastic.co/guide/en/kibana/7.12/xpack-monitoring.html)
- [Monitoring Logstash](https://www.elastic.co/guide/en/logstash/7.12/configuring-logstash.html)
- Monitoring Beats:
    - [Auditbeat](https://www.elastic.co/guide/en/beats/auditbeat/7.12/monitoring.html)
    - [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/7.12/monitoring.html)
    - [Functionbeat](https://www.elastic.co/guide/en/beats/functionbeat/7.12/monitoring.html)
    - [Heartbeat](https://www.elastic.co/guide/en/beats/heartbeat/7.12/monitoring.html)
    - [Metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/7.12/monitoring.html)
    - [Packetbeat](https://www.elastic.co/guide/en/beats/packetbeat/7.12/monitoring.html)
    - [Winlogbeat](https://www.elastic.co/guide/en/beats/winlogbeat/7.12/monitoring.html)

### elasticsearch - elastic stack

elastic stack은 filebeat, logstash, elasticsearch, kibana 이렇게 여러 개의 컴포넌트들을 조합하여 로그를 수집, 분석, 시각화하는 시스템을 말한다.

Filebeat는 로그가 발생하는 애플리케이션 서버에 설치하며, 애플리케이션에서 발생하는 로그를 그대로 Logstash로 전달하는 역할을 한다.

Logstash는 Filebeat로부터 전달 받은 로그들을 파싱하여 JSON 형태의 문서로 만든 다음 Elasticsearch 클러스터에 저장하는 역할을 한다.

Elasticsearch는 Logstash가 파싱한 JSON 형태의 문서를 저장하는 저장소의 역할을 한다.

Kibana는 Elasticsearch에 저장된 로그들을 조회하거나 시각화하는 역할을 한다.

Elastic Stack의 각 요소들을 각각의 특성에 맞게 이중화할 수 있다. Logstash는 LB를 사용하거나 filebeat.yml 파일에 Logstash 서버들을 리스팅 형태로 열거하여 이중화할 수 있고, Kibana의 경우는 Active / Standby 방식으로 Active 서버에 문제가 발생했을 경우 Standby 서버를 실행시켜서 장애가 나지 않은 것처럼 사용할 수 있다.

elasticsearch 는 logstash가 파싱한 JSON 형태의 문서를 인덱스에 저장한다. 이때의 elasticsearch는 데이터 저장소 역할을 한다. 대부분의 경우 날짜가 뒤에 붙는 형태로 인덱스가 생성되며 해당 날짜의데이터를 해당 날짜의 인덱스에 저장한다.

# Metricbeat

[MetricBeat Install Guide](https://www.notion.so/MetricBeat-Install-Guide-8188293fd5884aeebe3874ec298cc2d8)

### monitoring:

1. Install Metricbeat on the same server as Elasticsearch

[Follow these instructions.](https://www.elastic.co/guide/en/beats/metricbeat/7.12/metricbeat-installation-configuration.html)

1. Enable and configure the Elasticsearch x-pack module in Metricbeat

From the installation directory, run:

```
metricbeat modules enable elasticsearch-xpack
```

By default the module collects Elasticsearch metrics from http://localhost:9200. If the local server has a different address, add it to the hosts setting in modules.d/elasticsearch-xpack.yml.

If security is enabled,  [additional setup](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/configuring-metricbeat.html) might be required.

1. Configure Metricbeat to send data to the monitoring cluster

Modify metricbeat.yml to set the connection information.

```
output.elasticsearch:
  hosts: ["http://localhost:9200"] ## Monitoring cluster

  # Optional protocol and basic auth credentials.
  #protocol: "https"
  #username: "elastic"
  #password: "changeme"

```

If security is enabled,  [additional setup](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/configuring-metricbeat.html) might be required.

1. Start Metricbeat

[Follow these instructions.](https://www.elastic.co/guide/en/beats/metricbeat/7.12/metricbeat-starting.html)

1.  is incomplete

Monitoring status

No monitoring data detected, but we’ll continue checking.

metricbeat에서 monitoring을 할 수 있다.

기본적으로 x-pack basic license 를 제공하기 때문이다.

하지만 filebeat도 없는 상태에선 log를 보는 건 불가능했다.


# Kibana

### monitoring guide

![kibana metric flow](/assets/images/kibana-1.png)

![elasticstack map](/assets/images/kibana-2.png)

kibana는 elasticsearch에 저장된 데이터를 조회하거나 시각화할 때 사용한다. 데이터를 기반으로 그래프를 그리거나 데이터를 조회할 수 있다. elastic stack 에서 사용자의 인입점을 맡게 된다.

### Active - Standby

kibana 또한 이중화 구성을 하는 것을 고려해 볼만 하다.

여기서는 Active - Standby 구조를 사용하는 것을 권장한다.

서버를 이중화하여 구성하지만 동시에 부하분산을 통해 모든 기기에서 서비스하는 것이 아니라 장애시에 서비스를 이전하여 운영하는 형태로 구성된 것을 Active - Standby 라고 한다. 흔히 운영 시스템이라고 부르는 운영 시스템 서버(메인 서버)가 장애시 서비스 장애를 즉시 인지하여 서브 서버로 서비스를 이전한다.

이러한 과정은 클러스터 하트비트(Heart) 등으로 시스템의 정상 상태를 주기적으로 체킹하고 특이사항이 발생하는 경우 시스템 엔지니어의 의사결정을 통해 수동으로 서브 서버로 서비스를 전환하거나, 크리티컬한 장애시 자동으로 서비스를 전환한다.

이 밖에 Active - Active 구조는 L4 스위치 등의 부하분산(SLB) 로드 밸런싱을 통해 기능 또는 성격 등에 따라 1번 또는 2번 서버로 나누어서 처리하도록 구성한다. 웹 서버 이후에 데이터베이스 서버에 접근이 필요한 경우에도 2개 이상의 서버를 둔다.

대부분의 웹 서버는 L4 스위치 SLB(Server Load Balancing)으로 구성하고 DB서버는 RAC(Real Application Cluster)를 활용한다. 디스크 공유도 마찬가지인데 Veritas CFS(Cluster File System) 등으로 구성할 수 있다. 이러한 구성은 특정 기기 장애시 1번 또는 2번 서버 등으로 서비스를 지속 운영할 수 있다는 장점이 있고 Down time이 존재하지 않는다.

결국 두 구조 공통의 목적은 불가피한 장애에 대비하는 것이며 서비스의 다운 타임을 최소화시키고자 하는 것이다.

# Filebeat

저장된 위치에 있는 로그 파일을 읽어서 Logstash 서버로 보내주는 역할을 한다. 

Filebeat에서 로그 파일을 읽어서 JSON 형태의 문서로 바로 가공할 수도 있지만 이렇게 하지 않는 이유는 위와 같이 하면 로그 파일의 포맷이 달라져서 애플리케이션 서버들에 설치해놓은 Filebeat를 모두 변경해야 하는 어려움이 있다. 반면에 filebeat에서 elasticsearch로 바로 전송할 수 있기 때문에 더 빠르게 로그를 수집할 수 있는 장점이 생긴다.

filebeat는 로그 수집의 대상이 되는 서버에서 직접 동작하기 때문에 별도의 이중화 구성이 필요없다.

filebeat가 설치 되어 있는 서버에 장애가 발생해서 서비스에서 제외되거나 하면 로그 자체가 쌓이지 않기 때문에 굳이 이중와 구성을 해줄 필요가 없다.

# Logstash

filebeat로부터 받은 로그 파일을 룰에 맞게 파싱하여 JSON 형태의 문서로 만드는 역할을 한다.

하나의 로그에 포함된 정보를 모두 파싱할 수도 있고, 일부 필드만 파싱해서 JSON 문서로 만들 수 도 있다. 파싱할 때는 다양한 패턴을 사용할 수 있으며 대부분 `grok` 패턴을 이용해서 파싱 룰을 정의한다.

`grok은 비정형 데이터를 정형 데이터로 변경해 주는 라이브러리로, 다양한 정규표현식 형태의 패턴을 제공한다.`

logstash는 filebeat로부터 로그를 전달받아서 파싱한 후에 JSON 문서로 변환해서 Elasticsearch에 문서를 저장하는 역할을 한다. 때문에 Logstash 서버군에 장애가 발생하여 동작하지 않는다면 Elasticsearch에 로그를 넣지 못하게 되고, 로그 수집 불가 상태가 된다. 

그래서 Logstash는 이중화 작업을 고려해볼만 하다. 크게 두 가지 방법을 고려해볼 수 있다.

1. 로드밸런서(LoadBalancer, 이하 LB)를 활용
2. Filebeat 서버에서 환경을 설정할 때 다수의 Logstash 서버를 기록하는 것.

# Kafka

### what is kafka

kafka는 Distributed Apps, 분산 애플리케이션이다.

분산 애플리케이션은 그 기능을 여러 개의 독립적이고 작은 부분들로 나누어 동작하는 애플리케이션이다. 분산 애플리케이션들은 폭넓게 바라본 애플리케이션의 범주 내에서 서로 다른 문제를 처리하는 개별 [마이크로서비스](https://glossary.cncf.io/ko/microservices/) 컴포넌트들로 구성된다. 

클라우드 네이티브 환경에서는 개별 컴포넌트들이 일반적으로 [클러스터](https://glossary.cncf.io/cluster) 에서 [컨테이너](https://glossary.cncf.io/ko/container/) 로 실행된다.

단일 컴퓨터에서 실행 중인 애플리케이션은 곧 단일 장애 지점(Single Point Of Failure)을 나타낸다. 이는 해당 컴퓨터에 장애가 발생할 경우 애플리케이션 역시 사용할 수 없게 된다는 의미이다.

분산 애플리케이션은 종종 [모놀리식 애플리케이션](https://glossary.cncf.io/ko/monolithic-apps/) 과 대조되는데,
모놀리식 애플리케이션은 다양한 컴포넌트들의 규모를 독립적으로 조절할 수 없기 때문에, 유연하게 규모를 조절(scale)하는 것이 매우 어렵다.

또한 애플리케이션의 규모가 커질수록 개발자들의 생산성이 저해되는데,
더 많은 개발자들이 확실한 경계가 정해지지 않은 상태의 공유 코드베이스에서 작업을 해야 하기 때문이다.

애플리케이션을 분산화하여 여러 컴퓨터에서 실행하면, 몇몇 컴퓨터에서 장애가 발생하더라도 여전히 애플리케이션을 사용할 수 있다.

또한 규모를 유연하게 조절할 수 있는 특성인 [수평적 확장](https://glossary.cncf.io/horizontal-scaling/)이 가능하다는 장점을 갖는다.
이는 단일 애플리케이션으로 실행시킬 때는 불가능하다.

하지만 사용자는 더 이상 하나의 애플리케이션이 아닌 수많은 애플리케이션 컴포넌트들을 실행해야 하기 때문에, 운영하는 측면에서의 부담이 증가한다.

### kafka data model

**Topic:**

kafka cluster이 data를 저장하는 장소. main address라고 생각하면 이해하기 쉽다.

**Partition:**

topic를 분할한 것.메시지를 병렬 처리 방식으로 받음으로서 전송 속도를 효율적으로 높였고, 메시지의 순서 또한 보장해줌.

**Offset:**

각 파티션마다 메시지가 저장되는 위치. 파티션 내에서 유일하고 순차적으로 증가하는 숫자(64비트 정수) 형태로 되어 있다.

[https://www.confluent.io/blog/event-streaming-platform-2/](https://www.confluent.io/blog/event-streaming-platform-2/)

# Best Environment

- linkedIn data pipeline
    
    ![linkedIn data pipeline](/assets/images/linkedin-data-pipeline.png)
    
- 사람인 Heimdall container 기반 application
    
    ![Heimdall structure](/assets/images/Heimdall-structure.png)
    
- kafka + zookeeper
    
    ![kafka zookeeper](/assets/images/kafka-zookeeper.drawio.png)
    