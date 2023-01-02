# Contents:

- Definition of Synchronous, Asynchronous
- Blocking, Non-Blocking
- Reference

# Definition of Synchronous, Asynchronous

동기, Synchronous (~~중국어로는 同步라고도 한다.~~) 

단어 자체의 의미는 한 상황 내에서 발생하는 이벤트들이 같은 시간 상에서 동일한 동작과 일관성을 보이며 동화가 되어있는 현상을 말한다.

컴퓨터 공학에서 이야기 하는 동기란 프로세스 수행 순서가 A 다음엔 B, C , … 이렇게 작업이 끝나고 그 다음 작업이 진행되도록 하는 처리 방식을 말한다.

**쉽게 말하면 프로세스 내의 이벤트가 동시에 진행되는 것이다.**

즉, 한 프로세스의 내에서 동기화 된 이벤트들은 서로의 상태를 공유하고 있고, 한 이벤트가 끝나는 **즉시!** 다음 이벤트 작업을 시작하는 것이다.

---

예를 들어보자.

OSI 7계층과 같은 네트워크 계층의 인접 계층 간의 프로토콜 상호 작용과 같은 상황에서, 발신자와 수신자의 상태의 상태를 생각해 보자.

- 발신자가 메시지를 보낼 때, 이 순간 수신자는 어떤 상태에 들어가는가?
    
    메시지를 받기 위한 준비를 하고 있을 것이다.
    
    그러는 동안 다른 작업은 할 수 없을 것이고, 발신자의 메시지을 기다리고 있을 것이다.
    
    그리고 **발신자로부터 메시지가 오는 순간** 수신자는 본인의 작업을 시작할 수 있을 것이다.
    

![Sync.png](/assets/images/Sync.png)

조금 더 쉽게 풀어보자면, 동기는 요청을 하고 난 뒤에 그 결과가 올 때까지 기다리며 **********************************요청 작업과 결과 반환 작업이 동시에** 일어나도록 하는 방식이다.

Task1을 하고 난 뒤에만 Task2 작업을 진행할 수 있기 때문에, 다른 작업을 하려면 앞의 작업이 끝날 때 까지 기다려야만 한다.

![Synchronous](/assets/images/Sync1.png)

Synchronous

![Asynchronous](/assets/images/Async.png)

Asynchronous

---

비동기, Asynchronous (非同步)는 동기의 반대라고 설명할 수 있다.

프로세스 내의 이벤트가 동시에 진행되지 않는 것이다.

즉, 작업을 요청하고 결과를 기다리지 않는 것이다.

Task1을 하고 Task2를 하는 것이 아닌, Task1, Task2, Task3을 동시에 작업할 수 있다.

그저 작업을 지시하고 결과를 기다리지 않고 그 다음 작업을 시작하기 때문이다.

물론 한 번에 여러 작업을 진행하니 동기 작업처럼 빠른 결과를 얻지 못하는 단점이 있다.

---

# Blocking, Non-Blocking

동기, 비동기와 블로킹(blocking), 논블로킹(non-blocking)간에 무슨 차이점이 있을까?

동기와 비동기는 프로세스의 흐름에 중점을 두고 있고 그 흐름의 주체인 프로세스; 

즉, 작업을 지시하는 주체의 관점에서 바라보아야 한다.

블로킹과 논블로킹은 작업의 상태에 중점을 두고 있고 그 상태의 주체인 작업의 관점에서 바라보아야 한다.

블로킹은 작업이 시작되면 결과가 반환되기 전까지 프로세스가 다른 작업을 못하게 꽉 잡고 놓아주지 않는 방식이다. 

논블로킹은 작업 요청이 들어오면 해당 작업을 시작하게 되고 작업이 끝나면 결과를 반환해준다. 이 과정 동안 프로세스는 방해를 받지 않고 다른 작업을 할 수 있다.

![Blocking 방식](/assets/images/Async1.png)

Blocking 방식

그림과 같이 블로킹 방식은 순서대로 작업을 처리하게 된다.

앞의 작업이 끝나 결과가 반환 되어야 주체(프로세스)가 다음 작업을 지시할 수 있기 때문이다.

---

![Non-Blocking 방식](/assets/images/Async2.png)

Non-Blocking 방식

그림과 같이 논블로킹 방식은 앞의 작업의 상태와는 관계 없이 언제든 요청이 들어오면 작업을 시작한다.

앞의 작업의 완료 여부에 구애받지 않으니 주체(프로세스)는 효율적으로 작업을 할 수 있게 된다.

---

# Reference

[https://evan-moon.github.io/2019/09/19/sync-async-blocking-non-blocking/#컴퓨터-공학에서의-비동기](https://evan-moon.github.io/2019/09/19/sync-async-blocking-non-blocking/#%EC%BB%B4%ED%93%A8%ED%84%B0-%EA%B3%B5%ED%95%99%EC%97%90%EC%84%9C%EC%9D%98-%EB%B9%84%EB%8F%99%EA%B8%B0)

[https://chanspark.github.io/2017/08/07/동기-비동기-호출.html](https://chanspark.github.io/2017/08/07/%EB%8F%99%EA%B8%B0-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%98%B8%EC%B6%9C.html)

[https://sudo-minz.tistory.com/21](https://sudo-minz.tistory.com/21)

[https://da-nyee.github.io/posts/operation-system-synchronous-asynchronous/](https://da-nyee.github.io/posts/operation-system-synchronous-asynchronous/)