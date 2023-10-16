# 스레드와 네트워킹

이전 장에서 우리는 네트워크 프로그래밍의 몇 가지 예를 살펴보았다. 이러한 예제에서는 네트워크 연결을 만들고 이를 통해 통신하는 방법을 보여 주었지만 네트워크 프로그래밍의 기본 특성 중 하나인 네트워크 통신이 비동기식이라는 사실을 다루지 않았다. 네트워크 연결의 한쪽 끝에 있는 프로그램의 관점에서 메시지는 언제든지 연결의 다른 쪽에서 도착할 수 있다. 메시지 도착은 이벤트이다. 이는 메시지를 수신하는 프로그램의 제어를 받지 않는다. 아마도 이벤트 지향 네트워킹 API는 네트워크 통신의 비동기적 특성을 처리하는 데 좋은 접근 방식이 될 수 있지만 Java에서는 이러한 접근 방식이 사용되지 않는다. 대신 Java의 네트워크 프로그래밍은 일반적으로 스레드를 사용한다. 

## 1. Blocking I/O 문제

섹션 11.4에서 다룬 것처럼 네트워크 프로그래밍은 소켓을 사용한다. 여기서 용어를 사용하는 의미에서 소켓은 네트워크 연결의 한쪽 끝을 나타낸다. 모든 소켓에는 연관된 입력 스트림과 출력 스트림이 있다. 연결의 한쪽 끝에서 출력 스트림에 기록된 데이터는 네트워크를 통해 전송되고 다른 쪽 끝의 입력 스트림에 나타난다. 

소켓의 입력 스트림에서 데이터를 읽으려는 프로그램은 해당 입력 스트림의 입력 방법 중 하나를 호출한다. 입력 방법이 호출되기 전에 데이터가 이미 도착했을 수도 있다. 이 경우 입력 방법은 데이터를 검색하고 즉시 반환한다. 그러나 입력 방법은 연결의 반대편에서 데이터가 도착할 때까지 기다려야 할 가능성이 높다. 데이터가 도착할 때까지 입력 방법과 이를 호출한 스레드가 차단된다.

소켓의 출력 스트림에 있는 출력 방법을 차단하는 것도 가능하다. 이는 프로그램이 네트워크를 통해 데이터를 전송할 수 있는 것 보다 더 빨리 데이터를 소켓에 출력하려고 시도하는 경우 발생할 수 있다. (약간 복잡하다. 소켓은 네트워클르 통해 전송될 것으로 예상되는 데이터를 보유하기 위해 "버퍼"를 사용한다. 버퍼는 대기열처럼 사용되는 메모리 블록일 뿐이다. 출력 방법은 해당 데이터를 버퍼에 삭제한다. 하위 수준 소프트웨어는 버퍼에서 데이터를 제거하고 이를 네트워크를 통해 전송한다. 버퍼가 가득 차면 버퍼에서 공간을 사용할 수 있을 때까지 출력 방법이 차단된다. 출력 방법이 반환될 때 데이터가 네트워크를 통해 나간것을 의미하는 것은 아니다. 이는 단지 데이터가 버퍼로 들어가고 나중에 전송되도록 예약되었음을 의미한다.)

네트워크 통신에서는 **blocking I/O** 를 사용한다고 한다. 네트워크 입력 및 출력 작업이 무기한 차단될 수 있기 때문이다. 네트워크를 사용하는 프로그램은 이러한 차단을 처리할 준비가 되어 있어야 한다. 어떤 경우에는 프로그램이 다른 모든 처리를 종료하고 입력을 기다리는 것이 허용된다. 그러나 스레드는 프로그램의 일부 부분이 다른 부분이 차단된 상태에서 유용한 작업을 계속할 수 있도록 한다. 서버에 요청을 보내는 네트워크 클라이언트 프로그램은 서버의 응답을 기다리는 동안 다른 작업을 수행할 수 없는 경우 단일 스레드로 작업을 수행할 수 있다. 반면에 네트워크 서버 프로그램은 일반적으로 동시에 여러 클라이언트에 연결할 수 있다. 클라이언트로부터 데이터가 도착하기를 기다리는 동안 서버는 다른 클라이언트와 통신하는 등 다른 작업도 수행할 수 있다. 서버가 다른 스레드를 사용하여 다른 클라이언트와의 통신을 처리하는 경우 한 클라이언트의 I/O가 차단된다는 사실로 인해 서버가 다른 클라이언트와 통신하는 것이 중단되지는 않는다.

I/O 차단을 처리하기 위해 스레드를 사용하는 것은 계산 속도를 높이기 위해 스레드를 사용하는 것과 근본적으로 다르다는 점을 이해하는 것이 중요하다. 섹션 12.3.2에서 속도 향상을 위해 스레드를 사용할 때 사용 가능한 각 프로세서에 대해 하나의 스레드를 사용하는 것이 합리적이었다. 하나의 프로세서만 사용할 수 있는 경우 둘 이상의 스레드를 사용해도 속도가 전혀 향상되지 않는다.. 실제로 스레드 생성 및 관리와 관련된 추가 오버헤드로 인해 속도가 느려질 수 있다.

반면에 I/O를 차단하는 경우 프로세서 수보다 더 많은 스레드를 갖는 것이 합리적일 수 있다. 특정 시간에 많은 스레드가 차단될 수 있기 떄문이다. 차단되지 않은 활성 스레드만 처리 시간을 두고 경쟁한다. 이상적인 경우, 모든 프로세서를 계속 사용하게 하려면 프로세서당 하나의 활성 스레드를 갖고 싶을 것이다. (실제로는 활성 스레드 수의 시간 경과에 따른 변화를 허용하기 위해 평균적으로 그보다 약간 적다.) 예를 들어, 네트워크 서버 프로그램에서 스레드는 일반적으로 가장 많은 시간을 소비한다. I/O 작업이 완료되기를 기다리는 동안 차단된 시간이다. 스레드가 차단되는 경우 프로세서 수보다 약 10배 많은 스레드가 있어야 한다. 따라서 단일 프로세서만 있는 컴퓨터에서도 서버 프로그램은 많은 수의 스레드를 효과적으로 활용할 수 있다.

## 2. 비동기 네트워크 채팅 프로그램

네트워크 통신을 위해 스레드를 사용하는 첫 번째 예로 GUI 채팅 프로그램을 고려한다.

섹션 11.4.5의 command-line 채팅 프로그램 [CLChatClient.java](https://math.hws.edu/javanotes/source/chapter11/CLChatClient.java) 및 [CLChatServer.java](https://math.hws.edu/javanotes/source/chapter11/CLChatServer.java)는 커뮤니케이션을 위해 직접적인 단계별 프로토콜을 사용한다. 연결의 한 쪽에서 사용자가 메시지를 입력한 후 사용자는 연결의 다른 쪽에서 응답을 기다려야 한다. 비동기 채팅 프로그램이 훨씬 더 좋을 것이다. 이러한 프로그램에서 사용자는 응답을 기다리지 않고 계속해서 줄을 입력하고 메시지를 보낼 수 있다. 상대방으로부터 비동기적으로 도착하는 메시지는 도착하자마자 표시된다. 명령줄 인터페이스에서는 이 작업을 수행하기가 쉽지 않지만 그래픽 사용자 인터페이스에서는 자연스러운 응용 프로그램이다. GUI 채팅 프로그램의 기본 아이디어는 연결의 반대편에서 도착하는 메시지를 읽는 작업을 수행하는 스레드를 생성하는 것이다. 메시지가 도착하자마자 사용자에게 표시된다. 그 다음에, 메시지 읽기 스레드는 다음 수신 메시지가 도착할 때까지 차단된다. 그러나 차단되는 동안 다른 스레드는 계속 실행될 수 있다. 특히 사용자 작업에 응답하는 이벤트 처리 스레드는 계속 실행된다. 해당 스레드는 사용자가 메시지를 생성하자마자 보내는 메시지를 보낼 수 있다. 

GUIChat 프로그램은 연결의 클라이언트 측 또는 서버 측 역할을 할 수 있다. 프로그램에는 사용자가 들어오는 연결 요청을 수신하는 서버 소켓을 생성하기 위해 클릭할 수 있는 "포트 수신 대기" 버튼이 있다. 이렇게 하면 프로그램이 서버 역할을 하게 된다. 또한 사용자가 연결 요청을 보내기 위해 클릭할 수 있는 "연결" 버튼도 있다. 이렇게 하면 프로그램이 클라이언트 역할을 하게 된다. 평소와 같이 서버는 지정된 포트 번호를 수신한다. 클라이언트는 서버가 실행 중인 컴퓨터와 서버가 수신 대기 중인 포트를 알아야 한다. 사용자가 이 정보를 입력할 수 있는 `GUIChat` 창에 입력 상자가 있다.

두 개의 `GUIChat` 창 사이에 연결이 설정되면 각 사용자는 다른 사용자에게 메시지를 보낼 수 있다. 창에는 사용자가 메시지를 입력하는 입력 상자가 있다. Return 키를 누르면 메시지가 전송된다. 이는 메시지 전송이 사용자 작업에 의해 생성된 이벤트에 대한 응답으로 일반적인 이벤트 처리 스레드에 의해 처리됨을 의미한다. 메시지는 들어오는 메시지를 기다리는 별도의 스레드에 의해 수신된다. 이 스레드는 메시지 도착을 기다리는 동안 차단된다. 메시지가 도착하면 해당 메시지를 사용하제에 표시된다. 창에는 네트워크 연결에 대한 기타 정보와 함께 들어오고 나가는 메시지를 모두 표시하는 대규모 기록 영역이 포함되어 있다.

소스 코드를 컴파일 하고 프로그램을 사용해 보기를 바란다. 단일 컴퓨터에서 이를 시도하려면 해당 컴퓨터에서 두 개의 프로그램 복사본을 실행하고 "localhost" 또는 "127.0.0.1"을 컴퓨터 이름으로 사용하여 한 프로그램 창과 다른 프로그램 창을 연결하면 된다. 또한 소스 코드를 읽어 보시기 바란다. 여기서는 그 중 일부에 대해서만 논의한다.

프로그램은 대부분의 네트워크 관련 작업을 처리하기 위해 중첩 클래스인 ConnectionHandler를 사용한다. ConnectionHandler는 Thread의 하위 클래스이다. ConnectionHandler 스레드는 네트워크 연결을 열고 연결이 열린 후 들어오는 메시지를 읽는 일을 담당한다. 연결 열기 코드를 별도의 스레드에 배치하여 연결이 열리는 동안 GUI가 차단되지 않도록 한다. (수신 메시지를 읽는 것과 마찬가지로 연결을 여는 것은 완료하는 데 시간이 걸릴 수 있는 차단 작업이다.) 프로그램이 서버로 작동할 때와 클라이언트로 작동할 때 연결 열기를 처리한다. 사용자가 "수신" 버튼이나 "연결" 버튼을 클릭하여 스레드가 생성된다. "듣기" 버튼을 누르면 스레드가 서버 역할을 하고, "연결" 버튼을 누르면 스레드가 클라이언트 역할을 한다. 이 두가지 경우를 구별하기 위해 ConnectionHandler 클래스에는 아래와 같은 두 개의 생성자가 있다. `postMessage()` 메서드는 사용자가 볼 수 있는 창의 기록 영역에 메시지를 게시한다.

```java
/**
 * 지정된 포트에서 연결을 수신합니다. 생성자
 * 어떤 네트워크 작업도 수행하지 않습니다. 그것은 단지 일부를 설정합니다
 * 인스턴스 변수를 선택하고 스레드를 시작합니다. 참고
 * 스레드는 하나의 연결만 수신합니다.
 * 서버 소켓을 닫습니다.
 */
ConnectionHandler(int port) {  // "서버" 역할을 합니다.
    state = ConnectionState.LISTENING;
    this.port = port;
    postMessage("\nLISTENING ON PORT " + port + "\n");
    try { setDaemon(true); }
    catch (Exception e) {}
    start();
}

/**
 * 지정된 컴퓨터 및 포트에 대한 연결을 엽니다. 생성자
 * 어떤 네트워크 작업도 수행하지 않습니다. 그것은 단지 일부를 설정합니다
 * 인스턴스 변수를 선택하고 스레드를 시작합니다.
 */
ConnectionHandler(String remoteHost, int port) {  // "클라이언트" 역할을 합니다.
    state = ConnectionState.CONNECTING;
    this.remoteHost = remoteHost;
    this.port = port;
    postMessage("\nCONNECTING TO " + remoteHost + " ON PORT " + port + "\n");
    try { setDaemon(true); }
    catch (Exception e) {}
    start();
}
```

여기서 `state`는 열거형에 의해 유형이 정의되는 인스턴스 변수이다.

```java
enum ConnectionState { LISTENING, CONNECTING, CONNECTED, CLOSED };
```

이 `enum`의 값은 네트워크 연결의 다양한 가능한 상태를 나타낸다. 다양한 이벤트에 대한 응답은 이벤트가 발생할 때 연결 상태에 따라 달라질 수 있으므로 네트워크 연결을 상태 머신으로 처리하는 것이 유용한 경우가 많다. 상태 변수를 `LISTENING` 또는 `CONNECTING`으로 설정하면 스레드가 연결을 설정할 때 서버로 작동할 지 아니면 클라이언트로 작동할지를 알려준다.

스레드가 시작되면 다음 `run()` 메서드를 실행한다.

```java
/**
 * 스레드에 의해 실행되는 run() 메소드. 그것은
 * 클라이언트 또는 서버로 연결(어느 것에 따라 다름)
 * 생성자가 사용되었습니다).
 */
public void run() {
    try {
        if (state == ConnectionState.LISTENING) {
            // 서버로 연결을 엽니다.
            listener = new ServerSocket(port);
            socket = listener.accept();
            listener.close();
        } else if (state == ConnectionState.CONNECTING) {
        // 클라이언트로 연결을 엽니다.
            socket = new Socket(remoteHost,port);
        }
        connectionOpened();  // 연결을 사용하도록 설정합니다(포함
                            // 읽기를 위해 BufferedReader를 생성합니다.
                            // 들어오는 메시지).
        while (state == ConnectionState.CONNECTED) {
            // 반대편에서 한 줄의 텍스트를 읽습니다.
            // 연결하고 이를 사용자에게 보고합니다.
            String input = in.readLine();
            if (input == null)
                connectionClosedFromOtherSide(); // 소켓을 닫고 사용자에게 보고합니다.
            else
                received(input);  // Report message to user.
        }
    } catch (Exception e) {
        // 에러 발생됨. 사용자에게 신고하되 신고하지 않음
        // 연결이 종료된 경우(오류가 발생했기 때문에)
        // 다음과 같은 경우에 생성되는 예상 오류일 수 있습니다.
        // 소켓이 닫힙니다.)
        if (state != ConnectionState.CLOSED)
            postMessage("\n\n ERROR:  " + e);
    }
    finally {  // 스레드를 종료하기 전에 정리합니다.
        cleanUp();
    }
}
```

이 메서드는 일부 작업을 수행하기 위해 여러 가지 다른 메서드를 호출하지만 작동 방식에 대한 일반적인 개요를 볼 수 있다. 서버 또는 클라이언트로 연결을 연 후 `run()` 메서드는 연결이 닫힐 때까지 연결의 반대쪽에서 메시지를 수신하고 처리하는 `while` 루프에 들어간다. 연결을 닫는 방법을 이해하는 것이 중요하다. `GUIChat` 창에는 사용자가 클릭하여 연결을 닫을 수 있는 "연결 끊기" 버튼이 있다. 프로그램은 연결을 나타내는 소켓을 닫고 연결 상태를 `CLOSED`로 설정하여 이 이벤트에 응답한다. 이런 일이 발생하면 연결 처리 스레드가 수신 메시지를 기다리는 메서드 `in.readLine()`에서 차단될 가능성이 높다. GUI 스레드에 의해 소켓이 닫히면 이 메서드는 실패하고 예외가 발생한다. 이 예외로 인해 스레드가 종료된다. (소켓이 닫힐 때 연결 처리 스레드가 `in.readLine()` 호출 사이에 있으면 연결 상태가 `CONNECTED`에서 `CLOSED`로 변경되므로 `while` 루프가 종료된다.) 창을 닫으면 연결도 닫힌다.

연결 반대편의 사용자가 연결을 닫는 것도 가능하다. 그런 일이 발생하면 들어오는 메시지의 스트림이 종료되고 연결 이쪽의 `in.readLine()`은 스트림 끝을 나타내고 원격 장치에 의해 연결이 닫혔다는 신호 역할을 하는 `null` 값을 반환한다.

`GUIChat` 코드를 최종적으로 살펴보려면 메시지를 보내고 받는 메서드를 고려해라. 이러한 메서드는 다른 스레드에서 호출된다. `send()` 메서드는 사용자 작업에 대한 응답으로 _이벤트 처리 스레드(event-handling thread)_ 에 의해 호출된다. 그 목적은 원격 사용자에게 메시지를 전송하는 것이다. (가능성은 낮지만 소켓의 출력 버퍼가 가득 차면 데이터 출력 작업이 차단될 수 있다고 생각할 수 있다. 보다 정교한 프로그램에서는 다른 스레드를 사용하여 나가는 메시지를 전송함으로써 이러한 가능성을 고려할 수 있다.) `send()` 메서드는 PrintWriter 변수 `out`를 사용하고 소켓의 출력 스트림에 쓴다. 이 메서드를 동기화하면 전송 작업 중에 연결 상태가 변경되는 것을 방지할 수 있다. 이 메서드를 동기화하면 전송 작업 중에 연결 상태가 변경되는 것을 방지할 수 있다.

```java
/**
 * 연결 상대방에게 메시지를 보내고 게시
 * 성적표에 메시지를 보냅니다. 이는 다음과 같은 경우에만 호출되어야 합니다.
 * 연결 상태는 ConnectionState.CONNECTED입니다. 그것이 호출되는 경우
 * 다른 경우에는 무시됩니다.
 */
synchronized void send(String message) {
    if (state == ConnectionState.CONNECTED) {
        postMessage("SEND:  " + message);
        out.println(message);
        out.flush();
        if (out.checkError()) {
            postMessage("\nERROR OCCURRED WHILE TRYING TO SEND DATA.");
            close();  // Closes the connection.
        }
    }
}
```

```java
/**
 * 이것은 메시지가 수신될 때 run() 메소드에 의해 호출됩니다.
 * 연결의 반대편. 메시지는 다음 위치에 게시됩니다.
 * 기록, 그러나 연결 상태가 CONNECTED인 경우에만 해당됩니다. (이것
 * 사용자가 클릭한 후에 메시지가 수신될 수 있기 때문입니다.
 * "연결 끊기" 버튼; 해당 메시지는 사용자가 볼 수 없어야 합니다.
 * 사용자.)
 */
synchronized private void received(String message) {
    if (state == ConnectionState.CONNECTED)
        postMessage("RECEIVE:  " + message);
}
```


## 3. 스레드 네트워크 서버

스레드는 네트워크 서버 프로그램에서 자주 사용된다. 이를 통해 서버는 동시에 여러 클라이언트를 처리할 수 있다. 클라이언트가 장기간 연결을 유지할 수 있으면 다른 클라이언트는 서비스를 기다릴 필요가 없다. 각 고객과의 상호 작용이 매우 짧을 것으로 예상되더라도 항상 그럴 것이라고 가정할 수는 없다. 서버가 예상하는 데이터를 전송하지 않고 연결을 유지하는 클라이언트가 오작동할 가능성을 허용해야 한다. 이로 인해 스레드가 무기한 중단될 수 있지만 스레드 서버에는 다른 클라이언트와 함께 계속될 수 있는 다른 스레드가 있다.

섹션 11.4.4의 [DateServer.java](https://math.hws.edu/javanotes/source/chapter11/DateServer.java) 샘플 프로그램은 매우 간단한 네트워크 서버 프로그램이다. 스레드를 사용하지 않으므로 서버는 다른 클라이언트의 연결을 수락하기 전에 하나의 클라이언트로 완료해야 한다. `DateServer`를 스레드 서버로 전환하는 방법을 살펴본다. (이 서버는 너무 단순해서 그렇게 하는 것이 큰 의미가 없다. 그러나 더 복잡한 서버에도 동일한 기술이 작동할 수 있다. 예를 들어 문제 12.5를 참조하세요. 또한 클라이언트 프로그램 [DateClient.java](https://math.hws.edu/javanotes/source/chapter11/DateClient.java)에 유의 하세요. 서버용 클라이언트를 구현하는 클라이언트가 하나의 연결만 사용하므로 스레드를 사용할 필요가 없다. 원래 클라이언트 프로그램은 새 버전의 서버에서 작동한다.)

첫 번째 시도로 [DateServerWithThreads.java](https://math.hws.edu/javanotes/source/chapter12/DateServerWithThreads.java)를 고려해보자. 이 샘플 프로그램은 서브 루틴을 호출하여 연결 자체를 처리하는 대신 연결 요청이 수신될 때 마다 새 스레드를 생성한다. 메인 프로그램은 단순히 스레드를 생성하고 연결을 스레드에 전달한다. 이 작업은 시간이 거의 걸리지 않으며 특히 차단되지 않는다. 스레드의 `run()` 메서드는 원래 프로그램에서 처리하는 것과 정확히 동일한 방식으로 연결을 처리한다. 이것은 프로그래밍하기 전혀 어렵지 않다. 여기에 빨간색으로 표시된 중요한 변경 사항이 있는 새 버전의 프로그램이 있다. 연결 스레드의 생성자는 거의 수행하지 않으며 특히 차단할 수 없다. 생성자는 메인 스레드에서 실행되므로 이는 매우 중요하다.

```java
import java.net.*;
import java.io.*;
import java.util.Date;

/**
 * 본 프로그램은 연결 요청을 받는 서버입니다.
 * LISTENING_PORT 상수로 지정된 포트입니다. 언제
 * 연결이 열리면 프로그램은 현재 시간을 다음으로 보냅니다.
 * 연결된 소켓. 프로그램은 계속해서 수신됩니다.
 * 연결이 종료될 때까지 연결을 처리합니다(Control-C에 의해,
 * 예를 들어).
 *
 * 이 버전의 프로그램은 다음에 대한 새 스레드를 생성합니다.
 * 모든 연결 요청.
 */
public class DateServerWithThreads {

    public static final int LISTENING_PORT = 32007;

    public static void main(String[] args) {

        ServerSocket listener;  // 들어오는 연결을 수신합니다.
        Socket connection;      // 연결 프로그램과의 통신을 위해.

        /* 연결을 영원히 허용하고 처리하거나 오류가 발생할 때까지 처리합니다. */
        
        try {
            listener = new ServerSocket(LISTENING_PORT);
            System.out.println("Listening on port " + LISTENING_PORT);
            while (true) {
                // 다음 연결 요청을 수락하고 이를 처리 할 스레드를 생성합니다 .
                connection = listener.accept();
                ConnectionHandler handler = new ConnectionHandler(connection);
                handler.start();
            }
        }
        catch (Exception e) {
            System.out.println("Sorry, the server has shut down.");
            System.out.println("Error:  " + e);
            return;
        }

    }


    /**
     * 하나의 클라이언트 연결을 처리하는 스레드를 정의합니다.
     */
    private static class ConnectionHandler extends Thread { 
        Socket client; // The connection to the client.
        ConnectionHandler(Socket socket) {
            client = socket;
        }
        public void run() {
            // (원래 DateServer 프로그램에서 복사한 코드)
            String clientAddress = client.getInetAddress().toString();
            try {
                System.out.println("Connection from " + clientAddress );
                Date now = new Date();  // 현재 날짜와 시간입니다.
                PrintWriter outgoing;   // 데이터 전송을 위한 스트림입니다.
                outgoing = new PrintWriter( client.getOutputStream() );
                outgoing.println( now.toString() );
                outgoing.flush();  // 실제로 데이터가 전송되는지 확인하세요!
                client.close();
            }
            catch (Exception e){
                System.out.println("Error on connection with: "
                        + clientAddress + ": " + e);
            }
        }
    }
    
}
```

한 가지 흥미로운 변경 사항은 `run()` 메서드 끝에 있는데, 오류 메시지 출력에 `clientAddress`를 추가했다. 오류 메시지가 어떤 연결을 참조하는지 식별하기 위해 이 작업을 수행했다. 스레드는 병렬로 실행되므로 서로 다른 스레드의 출력이 다양한 순서로 섞일 수 있다. 동일한 스레드의 메시지가 반드시 출력에 함께 표시되는 것은 아니다. 다른 스레드의 메시지로 구분될 수 있다. 이것은 스레드로 작업할 때 염두에 두어야 할 복잡한 문제 중 하나일 뿐이다.

## 4. 스레드 풀 사용

모든 연결에 대해 새 스레드를 생성하는 것은 그리 효율적이지 않다. 특히 연결의 수명이 일반적으로 매우 짧은 경우에는 더욱 그렇다. 다행스럽게도 스레드 풀이라는 대안이 있다.

[DateServerWithThreadPool.java](https://math.hws.edu/javanotes/source/chapter12/DateServerWithThreadPool.java)는 스레드 풀을 사용하는 서버의 향상된 버전이다. 풀의 각 스레드는 무한 루프에서 실행된다. 루프를 통과할 때마다 하나의 연결을 처리한다. 메인 프로그램이 스레드에 연결을 보낼 수 잇는 방법이 필요하다. 그러한 목적으로 `ConnectionQueue`라는 이름의 blocking queue (섹션 12.3.3)을 사용한다. 연결 처리 스레드(connection-handling thread)는 이 대기열에서 연결을 가져온다. 차단 대기열이므로 스레드는 대기열이 비어 있으면 차단되고 대기열에서 연결을 사용할 수 있게 되면 깨어난다. 다른 동기화나 통신 기술은 필요하지 않다. 모두 차단 대기열에 내장되어 있다. 연결 처리 스레드에 대한 `run()` 메서드는 다음과 같다.

```java
public void run() {
    while (true) {
        Socket client;
        try {
            client = connectionQueue.take();  // 항목을 사용할 수 있을 때까지 차단합니다.
        }
        catch (InterruptedException e) {
            continue; // (중단되면 while 루프의 시작으로 돌아갑니다.)
        }
        String clientAddress = client.getInetAddress().toString();
        try {
            System.out.println("Connection from " + clientAddress );
            System.out.println("Handled by thread " + this);
            Date now = new Date();  // 현재 날짜와 시간입니다.
            PrintWriter outgoing;   // 데이터 전송을 위한 스트림입니다.
            outgoing = new PrintWriter( client.getOutputStream() );
            outgoing.println( now.toString() );
            outgoing.flush();  // 실제로 데이터가 전송되는지 확인하세요!
            client.close();
        } catch (Exception e){
            System.out.println("Error on connection with: " 
                    + clientAddress + ": " + e);
        }
    }
}
```

그 동안 메인 프로그램은 연결이 허용되고 대기열에 추가되는 무한 루프에서 실행된다.

```java
while (true) {
        // 다음 연결 요청을 수락하고 대기열에 넣습니다.
    connection = listener.accept();
    try {
        connectionQueue.put(connection); // 큐가 가득 차면 차단됩니다.
    } catch (InterruptedException e) {
    }
}
```

이 프로그램의 큐는 ArrayBlockingQueue<Socket> 유형이다. 따라서 용량이 제한되어 있으며 대기열이 가득 차면 대기열에 대한 `put()` 작업이 차단된다. 하지만 잠시, 메인 프로그램이 차단되는 것을 피하고 싶지 않나?? 메인 프로그램이 차단되면 서버는 더 이상 연결을 허용하지 않으며 연결을 시도하는 클라이언트는 계속 대기한다. 용량이 무제한인 LinkedBlockingQueue<Socket>을 사용하는 것이 더 나을까??

실제로 차단 대기열의 연결은 어쨋든 기다리고 있다. 그들은 서비스를 받고 있지 않다. 대기열이 비합리적으로 길어지면 대기열의 연결은 비합리적인 시간 동안 기다려야 한다. 대기열이 계속해서 무한정 증가한다면 이는 서버가 연결 요청을 처리할 수 있는 것보다 더 빠르게 연결 요청을 받고 있다는 의미이다. 이는 여러 가지 이유로 발생할 수 있다. 귀하의 서버가 받는 트래픽 양을 처리할 만큼 강력하지 않을 수도 있다. 새서버를 구입해야 한다. 혹은 스레드 풀에 서버를 완전히 활용하기에 충분한 스레드가 없을 수도 있다. 서버의 기능에 맞게 스레드 풀의 크기를 늘려야 한다. 아니면 서버가 "서비스 거부" 공격을 받고 있을 수도 있다.

어쨋든 용량이 제한된 ArrayBlockingQueue가 올바른 선택이다. 대기열의 연결이 서비스를 위해 너무 오래 기다릴 필요가 없도록 대기열은 충분히 짧아야 한다. 실제 서버에서는 대기열의 크기와 스레드 풀의 스레드 수를 조정하여 서버가 실행 중인 특정 하드웨어 및 네트워크와 클라이언트 요청의 특성을 고려하여 서버를 "조정"해야 한다. 일반적으로 처리된다. 최적의 튜닝은 일반적으로 어려운 문제이다. 

그런데 일이 잘못될 수 있는 또 다른 방법이 있다. 서버가 클라이언트로부터 일부 데이터를 읽어야 하는데 클라이언트가 어떤 데이터도 보내지 않는다고 가정해 본다. 데이터를 읽으려는 스레드는 입력을 기다리면서 무기한 차단될 수 있다. 스레드 풀을 사용하는 경우 풀의 모든 스레드에서 이런 일이 발생할 수 있다. 이 경우 더 이상 처리가 이루어질 수 없다. 이 문제에 대한 해결책은 연결이 과도한 시간 동안 비활성 상태인 경우 연결이 "시간 초과"되도록 하는 것이다. 일반적으로 각 연결 스레드는 클라이언트로부터 마지막으로 데이터를 수신한 시간을 추적한다. 서버는 주기적으로 깨어나 각 연결 스레드를 확인하여 얼마나 오랫동안 비활성 상태였는지 확인하는 또 다른 스레드를 실행한다. 너무 오랫동안 입력을 기다린 연결 스레드가 종료되고 그 자리에 새 스레드가 시작된다. 제한 시간이 얼마나 길어야 하는지에 대한 질문은 또 다른 어려운 튜닝 문제이다.

## 5. 분산 컴퓨팅

우리는 스레드를 사용하여 여러 프로세서가 함께 작업하여 일부 작업을 완료하는 병렬 처리를 수행하는 방법을 살펴보았다. 지금까지 우리는 모든 프로세서가 하나의 다중 프로세서 컴퓨터 안에 있다고 가정했다. 그러나 해당 컴퓨터가 통신할 수 있는 네트워크에 연결되어 있는 한, 다른 컴퓨터에 있는 프로세서를 사용하여 병렬 처리를 수행할 수도 있다. 여러 대의 컴퓨터가 작업을 수행하고 네트워크를 통해 통신하는 이러한 유형의 병렬 처리를 **분산 컴퓨팅(distributed computing)** 이라고 한다.

어떤 의미에서 전체 인터넷은 거대한 분산 컴퓨팅이지만 여기서는 네트워크의 컴퓨터가 일부 컴퓨팅 문제를 해결하기 위해 어떻게 협력할 수 있는지에 관심이 있다. Java의 분산 컴퓨팅에 사용할 수 있는 몇 가지 접근 방식이 있다. Java의 표준 부분인 일반적인 기술 중 하나는 RMI(Remote Method Invocation)이다. RMI를 사용하면 한 컴퓨터에서 실행되는 프로그램이 다른 컴퓨터에 존재하는 객체의 메서드를 호출할 수 있다. 이를 통해 프로그램의 다른 부분이 다른 컴퓨터에서 실행되는 객체 지향 프로그램을 설계할 수 있다. 네트워킹의 일반적인 경우와 마찬가지로 서비스를 찾는 문제가 있다. (이 경우 "서비스"는 네트워크를 통해 호출할 수 있는 객체를 의미) 즉, 한 컴퓨터에서 서비스가 있는 컴퓨터와 수신 대기 중인 포트를 어떻게 알 수 있나?? RMI는 다른 컴퓨터에서 사용할 수 있는 서비스 목록을 유지하는 알려진 위치에서 실행되는 서버 프로그램인 "요청 브로커(request broker)"를 사용하여 이 문제를 해결한다. 서비스를 제공하는 컴퓨터는 해당 서비스를 요청 브로커에 등록한다. 서비스가 필요한 컴퓨터는 브로커의 위치를 알아야 하며, 브로커에 접속하여 어떤 서비스를 사용할 수 있는지, 어디에 있는지 알아본다.

RMI는 사용하기가 쉽지 않은 복잡한 시스템이다. 여기서는 Java 표준 네트워크 API의 일부이기 때문에 언급하지만 더 이상 논의하지 않는다. 대신, 기본적인 네트워킹만 사용하는 비교적 간단한 분산 컴퓨팅 데모를 살펴본다.

우리가 고려할 문제는 [MultiprocessingDemo1.java](https://math.hws.edu/javanotes/source/chapter12/MultiprocessingDemo1.java)에 대해 섹션 12.2, 12.3에서 사용된 복잡한 이미지의 계산의 동일한 문제이다. 그러나 이 경우 해당 프로그램은 GUI 프로그램이 아니므로 화면에 이미지가 표시되지 않는다. 계산은 가장 간단한 유형의 병렬 프로그래밍을 사용하는 것으로, 문제를 작업 간 통신 없이 독립적으로 수행할 수 있는 작업으로 나눌 수 있다. 이러한 유형의 문제에 분산 컴퓨팅을 적용하기 위해 문제를 작업으로 나누고 해당 작업을 네트워크를 통해 실제 작업을 수행하는 "작업자" 프로그램으로 보내는 하나의 "마스터" 프로그램을 사용할 수 있다. 작업자 프로그램은 결과를 마스터 프로그램으로 다시 보내며 마스터 프로그램은 모든 작업의 결과를 전체 문제의 솔루션으로 결합한다. 이러한 맥락에서 작업자 프로그램은 종종 "노예"라 불린다. 분산 컴퓨팅에 대한 **마스터/슬레이브(master/slave)** 접근방식이다.

데모 프로그램은 세 가지 소스 코드 파일로 정의된다. [CLMandelbrotMaster.java](https://math.hws.edu/javanotes/source/chapter12/CLMandelbrotMaster.java)는 마스터 프로그램을 정의한다. [CLMandelbrotWorker.java](https://math.hws.edu/javanotes/source/chapter12/CLMandelbrotWorker.java)는 작업자 프로그램을 정의한다. CLMandelbrotTask.java는 작업자가 수행하는 개별 작업을 나타내는 클래스를 정의한다. 마스터는 전체 문제를 작업 모음으로 나눈다. 작업을 실행하고 결과를 마스터에게 다시 보낼 작업자에게 해당 작업을 배포한다. 마스터는 모든 개별 작업의 결과를 전체 문제에 적용한다.

데모를 실행하려면 먼저 여러 컴퓨터에서 `CLMandelbrotWorker` 프로그램을 시작해야 한다. 이 프로그램은 CLMandelbrotTask를 사용하므로 두 클래스 파일 `CLMandelbrotWorker.class` 및 `CLMandelbrotTask.class`가 작업가 컴퓨터에 있어야 한다. 그런 다음 마스터 컴퓨터에서 `CLMandelbrotMaster`를 실행할 수 있다. 마스터 프로그램에는 CLMandelbrotTask 클래스도 필요하다. `CLMandelbrotMaster`에 대한 명령줄 인수로 각 작업자 컴퓨터의 호스트 이름 또는 IP 주소를 지정해야 한다. 작업자 프로그램은 마스터 프로그램의 연결 요청을 수신하고 마스터 프로그램은 해당 요청을 보낼 위치를 알려주어야 한다. 예를 들어 작업자 프로그램이 IP주소가 172.21.7.101, 172.21.7.102 및 172.21.7.103인 세 대의 컴퓨터에서 실행 중인 경우 다음 명령을 사용하여 실행할 수 있다.

```console
java  CLMandelbrotMaster  172.21.7.101  172.21.7.102  172.21.7.103
```

동일한 컴퓨터에서 `CLMandelbrotWorker`의 여러 복사본을 실행할 수 있지만 서로 다른 포트에서 네트워크 연결을 수신해야 한다. `CLMandelbrotMaster`와 동일한 컴퓨터에서 `CLMandelbrotWorker`를 실행할 수도 있다. 컴퓨터에 여러 개의 프로세ㅔ서가 있는 경우 이 작업을 수행하면 속도가 약간 향상될 수도 있다. 자세한 내용은 프로그램 소스 코드 파일의 주석을 참조하세요. 그러나 여기에는 동일한 컴퓨터에서 마스터 프로그램과 작업자 프로그램의 두 복사본을 실행하는 데 사용할 수 있는 몇 가지 명령이 있다. 별도의 명령 창에서 다음 명령을 제공한다.

```console
java  CLMandelbrotWorker                             (Listens on default port)
   
java  CLMandelbrotWorker  2501                       (Listens on port 2501)
   
java  CLMandelbrotMaster  localhost  localhost:2501
```

`CLMandelbrotMaster`가 실행될 때마다 정확히 동일한 문제를 해결한다. 

다양한 수의 작업자 프로그램으로 `CLMandelbrotMaster`를 실행하여 문제를 해결하는 데 필요한 시간이 작업자 수에 따라 어떻게 달라지는지 확인할 수 있다. (마스터 프로그램이 종료된 후에도 작업자 프로그램은 계속 실행되므로 작업자를 다시 시작할 필요 없이 마스터 프로그램을 여러 번 실행할 수 있다.)
 또한 명령줄 인수 없이 `CLMandelbrotMaster`를 실행하면 전체 문제가 해결된다. 따라서 분산 컴퓨팅을 사용하지 않고도 문제를 해결하는 데 시간이 얼마나 걸리는지 확인할 수 있다. 매우 오래되고 느린 컴퓨터에서 실행한 시험에서는 `CLMandelbrotMaster`는 문제를 스스로 해결하기 위해 40초가 걸렸다. 작업자 한 명만 사용하면 43초가 걸렸다. 추가 시간은 네트워크 사용과 관련된 추가 작업을 나타낸다. 네트워크 연결을 설정하고 네트워크를 통해 메시지를 보내는 데 시간이 걸린다. 서로 다른 컴퓨터에 있는 두 작업자를 사용하여 문제는 22초만에 해결되었다. 이 경우 각 작업자는 작업의 절반 정도를 수행하고 계산이 병렬로 수행되므로 작업이 약 절반의 시간에 완료되었다. 작업자 수가 많아지면서 시간은 계속해서 줄었들었지만 어느 정도까지만 그렇다. 마스터 프로그램 자체에는 작업자 수에 관계없이 일정량의 작업이 있어며, 문제를 해결하는 데 걸리는 총 시간은 마스터 프로그램이 해당 역할을 수행하는 데 걸리는 시간보다 결코 작을 수 없다. 이 경우 최소 시간은 5초 정도였던 것 같다.

---

이 분산 어플리케이션이 어떻게 프로그래밍되는지 살펴본다. 마스터 프로그램은 전체 문제를 일련의 작업으로 나눈다. 각 작업은 CLMandelbrotTask 유형의 객체로 표시된다. 이러한 작업은 작업자 프로그램에 전달되어야 하며 작업자 프로그램은 결과를 다시 보내야 한다. 이 통신에는 일부 프로토콜이 필요하다. 나는 문자 스트림을 사용하기로 결정했다. 마스터는 작업을 텍스트 줄로 인코딩하여 작업자에게 전송한다. 작업자는 텍스트를 (CLMandelbrotTask 유형의 개체로) 디코딩하여 어떤 작업을 수행해야 하는지 알아보세요. 그것은 할당된 작업을 수행한다. 그것은 결과를 다른 텍스트 라인으로 인코딩하여 마스터 프로그램으로 보낸다. 모든 작업이 완료되고 결과가 결합되면 문제가 해결되었다. 

`CLMandelbrotWorker`는 하나의 작업뿐만 아니라 일련의 작업도 받는다. 작업을 완료하고 결과를 다시 보낼 때마다 새 작업이 할당된다. 모든 작업이 완료된 후 작업자는 연결을 닫으라는 "close" 명령을 받는다. CLMandelbrotWorker.java에서 이 모든 작업은 이미 마스터 프로그램에 열려 있는 연결을 처리하기 위해 호출되는 `handlerConnection()`이라는 메서드에서 수행된다. `readTask()` 메서드를 사용하여 마스터로부터 수신한 작업을 디코딩하고 `writeResults()` 메서드를 사용하여 마스터로 다시 전송할 작업 결과를 인코딩한다. 또한 발생하는 모든 오류를 처리해야 한다.

```java
private static void handleConnection(Socket connection) {
    try {
        BufferedReader in = new BufferedReader(
                new InputStreamReader( connection.getInputStream()) );
        PrintWriter out = new PrintWriter(connection.getOutputStream());
        while (true) {
            String line = in.readLine();  // 마스터로부터의 메시지.
            if (line == null) {
                // 스트림 끝이 발생했습니다. -- 발생해서는 안 됩니다.
                throw new Exception("Connection closed unexpectedly.");
            }
            if (line.startsWith(CLOSE_CONNECTION_COMMAND)) {
                // 연결의 정상적인 종료를 나타냅니다.
                System.out.println("Received close command.");
                break;
            } else if (line.startsWith(TASK_COMMAND)) {
                // 이 작업자가 속한 CLMandelbrotTask를 나타냅니다.
                // 수행해야 합니다..
                CLMandelbrotTask task = readTask(line);  // 메시지를 디코딩합니다.
                task.compute();  // 작업을 수행합니다.
                out.println(writeResults(task));  // 결과를 다시 보냅니다.
                out.flush();  // 데이터가 즉시 전송되는지 확인하세요!
            } else {
                // 다른 메시지는 프로토콜의 일부가 아닙니다.
                throw new Exception("Illegal command received.");
            }
        }
    } catch (Exception e) {
        System.out.println("Client connection closed with error " + e);
    } finally {
        try {
            connection.close();  // 소켓이 닫혀 있는지 확인합니다.
        }
        catch (Exception e) {
        }
    }
}
```

이 메서드는 별도의 스레드에서 실행되지 않는다. 작업자는 한 번에 한 가지 작업만 수행하며 다중 스레드가 필요하지 않다.

마스터 프로그램인 CLMandelbrotMaster.java로 전환하면 더 복잡한 상황에 직면하게 된다. 마스터 프로그램은 여러 네트워크 연결을 통해 여러 작업자와 통신해야 한다. 이를 달성하기 위해 마스터 프로그램은 각 작업자와의 통신을 관리하는 하나의 스레드로 구성된 다중 스레드이다. `main()` 루틴의 의사코드 개요는 매우 간단하다.

```
create the tasks that must be performed and add them to a queue
if there are no command line arguments {
      // The master program does all the tasks itself.
   Remove each task from the queue and perform it.
}
else {
      // The tasks will be performed by worker programs.
   for each command line argument:
      Get information about a worker from command line argument.
      Create and start a thread to send tasks to workers.
   Wait for all threads to terminate.
}
// All tasks are now complete (assuming no error occurred).


수행해야 하는 작업을 생성하고 대기열에 추가합니다.
명령줄 인수가 없는 경우 {
      // 마스터 프로그램은 모든 작업을 자체적으로 수행합니다.
   대기열에서 각 작업을 제거하고 수행합니다.
}
또 다른 {
      // 작업은 작업자 프로그램에 의해 수행됩니다.
   각 명령줄 인수에 대해 다음을 수행합니다.
      명령줄 인수에서 작업자에 대한 정보를 가져옵니다.
      스레드를 생성하고 시작하여 작업자에게 작업을 보냅니다.
   모든 스레드가 종료될 때까지 기다립니다.
}
// 이제 모든 작업이 완료되었습니다(오류가 발생하지 않았다고 가정).
```

`tasks` 이름의 작업은 ConcurrentBlockingQueue<CLMandelbrotTask> 유형 변수에 배치된다. 통신 스레드는 이 대기열에서 작업을 가져와 작업자 프로그램으로 보낸다. `task.poll()` 메서드는 대기열에서 작업을 제거하는 데 사용된다. 큐가 비어 있으면 모든 작업이 할당되었고 통신 스레드가 종료될 수 있다는 신호 역할을 하는 `null`을 반환한다.

스레드의 작업은 일련의 작업을 작업자 스레드로 보내고 작업자가 다시 보내는 결과를 받는 것이다. 스레드는 처음에 연결을 여는 일도 담당한다. 스레드에 의해 실행되는 프로세스에 대한 의사코드 개요는 다음과 같다.

```
Create a socket connected to the worker program.
Create input and output streams for communicating with the worker.
while (true) {
   Let task = tasks.poll().
   If task == null
      break;  // All tasks have been assigned.  
   Encode the task into a message and transmit it to the worker.
   Read the response from the worker.
   Decode and process the response.
}
Send a "close" command to the worker.
Close the socket.

작업자 프로그램에 연결된 소켓을 만듭니다.
작업자와 통신하기 위한 입력 및 출력 스트림을 만듭니다.
동안 (참) {
   작업 =tasks.poll()이라고 둡니다.
   작업 == null인 경우
      부서지다; // 모든 작업이 할당되었습니다.  
   작업을 메시지로 인코딩하여 작업자에게 전송합니다.
   작업자의 응답을 읽어보세요.
   응답을 디코딩하고 처리합니다.
}
작업자에게 "닫기" 명령을 보냅니다.
소켓을 닫습니다.
```

이것은 괜찮을 것이다. 그러나 몇 가지 미묘한 점이 있다. 우선, 스레드는 네트워크 오류를 처리할 준비가 되어 있어야 한다. 예를 들어 작업자가 예기치 않게 종료될 수 있다. 그런나 그런 일이 발생하더라고 다른 작업자가 여전히 사용 가능하다면 마스터 프로그램은 계속될 수 있다. (프로그램을 실행할 때 다음을 시도해 볼 수 있다. `control-c`를 사용하여 작업자 프로그램 중 하나를 중지한다. 그리고 마스터 프로그램이 여전히 성공적으로 완료되는지 관찰한다.) 스레드가 작업을 수행하는 동안 오류가 발생하면 어려움이 발생한다. 문제가 전체적으로 완료되면 해당 작업을 다른 작업자에게 다시 할당해야 한다. 완료되지 않은 작업을 작업 목록에 다시 넣어서 이 문제를 처리한다. (안타깝게도 내 프로그램은 가능한 모든 오류를 처리하지 못한다. 마지막 작업자 스레드가 실패하면 완료되지 않은 작업을 대신할 사람이 아무도 남지 않게 된다. 또한 실제로 오류가 발생하지 않고 네트워크 연결이 무기한 "중지"되면 내 프로그램이 또한 결코 도착하지 않는 작업자의 응답을 기다리면서 중단된다. 보다 강력한 프로그램에는 문제를 감지하고 작업을 재할당하는 방법이 있을 것이다.)


위에서 설명한 절차의 또 다른 결함은 마스터 프로그램의 스레드가 작업자의 응답을 처리하는 동안 작업자 프로그램을 idle 상태로 남겨둔다는 것이다. 이전 작업의 응답을 처리하기 전에 작업자에게 새 작업을 전달하는 것이 좋을 것이다. 이렇게 하면 작업자가 바쁘게 유지되고 두 작업이 순차적이 아닌 동시에 진행될 수 있다. (이 예에서 응답을 처리하는 데 걸리는 시간이 너무 짧아서 응답이 완료되는 동안 작업자를 기다리게 하는 것은 큰 차이가 없을 것ㅇ다. 그러나 일반적인 원칙에 따르면 알고리즘에서 가능한 많은 병렬성을 갖는 것이 바람직하다.) 이를 고려하여 절차를 수정할 수 있다.


```
try {
    Create a socket connected to the worker program.
    Create input and output streams for communicating with the worker.
    Let currentTask = tasks.poll()
    if (currentTask != null)
        Encode currentTask into a message and send it to the worker.
    while (currentTask != null) {
        Read the response from the worker.
        Let nextTask = tasks.poll().
                If nextTask != null {
            // Send nextTask to the worker before processing the
            // response to currentTask.
            Encode nextTask into a message and send it to the worker.
        }
        Decode and process the response to currentTask.
                currentTask = nextTask.
    }
    Send a "close" command to the worker.
    Close the socket.
}
catch (Exception e) {
    Put uncompleted task, if any, back into the task queue.
}
finally {
    Close the connection.
}
```

이 모든 것이 어떻게 Java로 변환되는지 보려면 CLMandelbrotMaster.java의 WorkerConnection 중첩 클래스를 확인하세요.








