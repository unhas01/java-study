# 네트워킹

프로그램에 관한 한 네트워크는 입력 데이터의 또 다른 가능한 소스이자 데이터가 출력될 수 있는 또 다른 장소일 뿐이다. 네트워크 파일만큼 작업하기 쉽지 않기 때문에 상황이 지나치게 단순화된다. 그러나 Java에서는 입력 스트림과 출력 스트림을 사용하여 사용자와 통신하거나 파일 작업을 하는 데 이러한 스트림을 사용할 수 있는 것처럼 네트워크 통신을 수행할 수 있다. 그럼에도 불구하고, 두 대의 컴퓨터 사이에 네트워크 연결을 여는 것은 약간 까다롭다. 두 대의 컴퓨터가 관련되어 있고 연결을 여는 데 어떻게든 동일해야 하기 때문이다. 그리고 각 컴퓨터가 서로 데이터를 보낼 수 있으면 통신을 동기화하는 것이 문제가 될 수 있다. 그러나 기본 사항은 다른 형태의 I/O와 동일하다

표준 Java 패키지 중 하나는 `java.net`이다. 이 패키지에는 네트워킹에 사용할 수 있는 여러 클래스가 포함되어 있다. 두 가지 다른 스타일의 네트워크 I/O가 지원된다. 그 중 상당히 높은 수준의 것 중 하나는 World Wide Web을 기반으로 하며 사용자가 볼 페이지를 다운로드할 때 웹 브라우저에서 사용하는 일종의 네트워크 통신 기능을 제공한다. 이 네트워킹 스타일의 기본 클래스는 `java.nt.URL` 및 `java.net.URLconnection`이다. URL 유형의 객체는 HTML 문서나 기타 리소스의 주소인 Universal Resource Locator의 추상적 표현이다. URLConnection은 그러한 리소스에 대한 네트워크 연결을 나타낸다.

더 일반적이고 더 중요한 I/O의 두번째 스타일은 네트워크를 더 낮은 수준에서 본다. 이는 소켓의 아이디어를 기반으로 한다. 소켓은 프로그램이 네트쿼으의 다른 프로그램과 연결을 설정하는 데 사용된다. 네트워크를 통한 통신에는 통신에 관련된 각 컴퓨터에 하나씩 두 개의 소켓이 포함된다. Java는 `java.net.Socket` 이라는 클래스를 사용하여 네트워크 통신에 사용되는 소켓을 나타낸다. "소켓"이라는 용어는 아마도 네트워크에 대한 연결을 설정하기 위해 물리적으로 컴퓨터에 전선을 연결하는 이미지에서 유래했을 것이다. 그러나 여기에서 사용된 용어인 소켓은 단순히 컴퓨터에 속하는 객체라는 점을 이해하는 것이 중요하다. 특히, 프로그램은 동시에 여러 개의 소켓을 가질 수 있으며, 각 소켓은 네트워크의 다른 컴퓨터에서 실행되는 다른 프로그램에 연결되거나 심지어 동일한 컴퓨터에서 실행될 수도 있다. 이러한 모든 연결은 동일한 물리적 네트워크 연결을 사용한다.

이 섹션에서는 이러한 기본 네트워킹 클래스에 대해 간략하게 소개하고 입력 및 출력 스트림과 어떻게 관련되는지 보여준다.

## 1. URL과 URLConnection

URL 클래스는 World Wide Web의 리소스를 나타내는 데 사용된다. 모든 리소스에는 해당 리소스를 고유하게 식별하고 웹 브라우저가 네트워크에서 리소스를 찾아 검색할 수 있는 충분한 정보를 포함하는 주소가 있다. 주소를 "url" 또는 "universal resource locator"라고 한다. (URL은 실제로 웹 이외의 다른 소스의 리소스를 참조할 수 있다. 결국 URL은 "보편적(universal)"이다.)

URL 클래스에 속하는 객체는 이러한 주소를 나타낸다. URL 객체가 있으면 이를 사용하여 해당 주소의 리소스에 대한 URLConnection을 열 수 있다. URL은 일반적으로 `https://math/hws.edu/eck/index.html` 과 같은 문자열로 지정된다. 상대(relative) URL도 있다. 상대 URL은 **base** 혹은 **context** 라고 불리며 다른 URL의 위치를 기준으로 리소스의 위치를 지정한다. 예를 들어, 컨텍스트가 `https://math/hws/edu/eck/` URL로 제공되면 불완전한 상대 URL `index.html`은 실제로 다음을 참조한다. `https://math/hws.edu/eck/index.html`

클래스 URL의 객체는 단순한 문자열이 아니라 URL의 문자열 표현으로 구성될 수 있다. URL 객체는 컨텍스트를 나타내는 다른 URL 객체와 해당 컨텍스트에 상대적인 URL을 지정하는 문자열로 구성될 수도 있다. 이 생성자에는 프로토타입이 있다.

```java
public URL(String urlName) throws MalformedURLException

public URL(URL context, String relativeName) throws MalformedURLException
```

지정된 문자열이 합법적인 URL을 나타내지 않는 경우 이러한 생성자는 MalformedURLException 유형의 예외를 발생시킨다. MalformedURLException 클래스는 IOException의 하위 클래스이며 필수 예외 처리가 필요하다.

유효한 URL 객체가 있으면 `openConnection()` 메서드를 호출하여 연결을 설정할 수 있다. 이 메서드는 URLConnection을 반환한다. 그러면 URLConnection 객체를 사용하여 URL이 나타내는 리소스에서 데이터를 읽기 위한 InputStream을 생성할 수 있다. `getInputStream()` 메서드를 호출하여 수행된다.

```java
URL url = new URL(urlAddressString);
URLConnection connection = url.openConnection();
InputStream in = connection.getInputStream();
```

`openConnection()` 및 `getInputStream()` 메서드는 모두 IOException 유형의 예외를 발생시킬 수 있다. InputStream이 생성되면 BufferedReader와 같은 다른 입력 스트림 유형으로 래핑하거나 Scanner르르 사용하는 등 일반적인 방법으로 읽을 수 있다. 물론 스트림에서 읽으면 예외가 발생할 수 있다.

URLConnection 클래스의 다른 유용한 인스턴스 메서드 중 하나는 URL에서 사용할 수 있는 정보 유형을 설명하는 문자열을 반환하는 `getContentType()`이다. 정보 유형을 아직 알 수 없거나 유형을 확인할 수 없는 경우 반환 값은 `null`일 수 있다. 입력 스트림이 생성될 때까지 해당 유형을 사용하지 못할 수 있으므로 일반적으로 `getInputStream()` 후에 `getContentType()`을 호출해야 한다. `getContentType()`에서 반환된 문자열은 MIME 유형이라는 형식이다. MIME 유형에는 "text/plain", "text/html", "image/jpeg", "image/png" 등이 포함된다. 모든 MIME 유형에는 "text', "image"와 같은 일반 유형과 "html" 또는 "png"와 같은 일반 범주 내의 보다 구체적인 유형이라는 두 부분이 포함된다. 예를 들어 텍스트 데이터에만 관심이 있는 경우 `getContentType()`에서 반환된 문자열이 "text"로 시작하는지 확인할 수 있다. 

이 모든 것을 사용하여 URL에서 데이터를 읽는 간단한 예를 살펴본다. 이 서브투린은 지정된 URL에 대한 연결을 열고 URL의 데이터 유형이 텍스트인지 확인한 다음 텍스트를 화면에 복사한다. 이 서브 루틴의 많은 작업에서는 예외가 발생할 수 있다. 서브 루틴이 `throws IOException`을 선언하고 오류가 발생할 때 무엇을 할지 결정하도록 메인 프로그램에 맡기는 방식으로 처리된다. 

```java
static void readTextFromURL( String urlString ) throws IOException {
    
    /* URL에 대한 연결을 열고 입력 스트림을 얻습니다.
       URL에서 데이터를 읽는 데 사용됩니다. */
    URL url = new URL(urlString);
    URLConnection connection = url.openConnection();
    InputStream urlData = connection.getInputStream();
    
    /* 콘텐츠가 텍스트 유형인지 확인하세요. 메모:
        Connection.getContentType() 메소드를 호출해야 합니다.
        getInputStream() 이후. */
    String contentType = connection.getContentType();
    System.out.println("Stream opened with content type: " + contentType);
    System.out.println();
    if (contentType == null || contentType.startsWith("text") == false)
        throw new IOException("URL does not seem to refer to a text file.");
    System.out.println("Fetching content from " + urlString + " ...");
    System.out.println();

    /* 입력 스트림에서 화면으로 텍스트 줄을 복사합니다.
       파일 끝이 발생했습니다(또는 오류가 발생했습니다). */
    BufferedReader in;  // 연결의 입력 스트림에서 읽어옵니다.
    in = new BufferedReader( new InputStreamReader(urlData) );
    
    while (true) {
        String line = in.readLine();
        if (line == null)
            break;
        System.out.println(line);
    }
    in.close();
}
```

이 서브 루틴을 사용하는 전체 프로그램은 [FetchURL.java](https://math.hws.edu/javanotes/source/chapter11/FetchURL.java) 파일에서 찾을 수 있다. 프로그램을 실행할 때 명령줄에서 URL을 지정할 수 있다. 그렇지 않은 경우 URL을 입력하라는 메시지가 표시된다. 이 프로그램의 경우 URL은 웹의 리소스를 참조하는 URL의 경우 "http://" 또는 "https://"로 시작할 수 있고, 웹의 파일을 참조하는 URL의 경우 "file://"가 URL 시작 부분에 추가된다. 웹사이트에서 이 교과서의 첫 페이지를 가져오려면 URL `math.hws.edu/javanotes`를 사용하여 프로그램을 사용해 보자. 발생할 수 있는 다양한 오류를 확인하려면 잘못된 입력을 사용해 보자.


## 2. TCP/IP와 Client/Server

인터넷을 통한 통신은 TCP/IP라고 하는 **전송 제어 프토토콜(Transmission Control Protocol)** 과 **인터넷 프로토콜(Internet Protocol)** 이라는 한 쌍의 프로토콜을 기반으로 한다. (실제로 특정 어플리케이션에서는 TCP 대신 사용할 수 있는 UDP라는 보다 기본적인 통신 프로토콜이 있다. UDP는 Java에서 지원되지만 이 논의에서는 안정적인 양방향을 제공하는 TCP/IP를 고수한다.)

두 프로그램이 TCP/IP를 사용하여 통신하려면 이 섹션 앞부분에서 설명한 대로 각 프로그램이 소켓을 만들고 해당 소켓을 연결해야 한다. 이러한 연결이 이루어지면 입력 스트림과 출력 스트림을 사용하여 통신이 이루어진다. 각 프로그램에는 자체 입력 스트림과 자체 출력 스트림이 있다. 한 프로그램이 출력 스트림과 출력 스트림을 사용하여 통신이 이루어진다. 한 프로그램이 출력 스트림에 쓴 데이터는 다른 컴퓨터로 전송된다. 그곳에서 네트워크 연결의 다른 쪽 끝에서 프로그램의 입력 스트림으로 들어간다. 해당 프로그램이 입력 스트림에서 데이터를 읽을 때 네트워크를 통해 전송된 데이터를 수신한다.

그렇다면 가장 어려운 부분은 우선 네트워크 연결을 만드는 것이다. 두 개의 소켓이 관련된다. 작업을 시작하려면 하나의 프로그램이 다른 소켓에서 연결 요청이 들어올 때까지 수동적으로 기다리는 소켓을 만들어야 한다. 대기 소켓은 연결을 수신하고 있다고 가정한다. 연결 예정의 반대편에서 다른 프로그램은 청취 소켓에 연결 요청을 보내는 소켓을 만든다. 청취 소켓이 연결 요청을 수신하면 응답하고 연결이 설정된다. 이 작업이 완료되면 각 프로그램은 연결을 통해 데이터를 전송하기 위한 입력 스트림과 출력 스트림을 얻을 수 있다. 한 프로그램 또는 다른 프로그램이 연결을 닫을 때까지 이러한 스트림을 통해 통신이 이루어진다. 

청취 소켓을 생성하는 프로그램을 **서버(server)** 라고 하며, 소켓을 **서버 소켓(server socket)** 이라고 한다. 서버에 연결하는 프로그램을 **클라이언트(client)** 라고 하며, 연결을 위해 사용되는 소켓을 **클라이언트 소켓(client socket)** 이라고 한다. 아이디어는 서버가 네트워크 어딘가에 있고 일부 클라이언트의 연결 요청을 기다리고 있다는 것이다. 서버는 일종의 서비스를 제공하는 것으로 생각할 수 있으며 클라이언트는 서버에 연결하여 해당 서비스에 엑세스한다. 이를 네트워크 통신의 **클라이언트/서버 모델** 이라고 한다. 많은 실제 응용프로그램에서 서버 프로그램은 동시에 여러 클라이언트에 대한 연결을 제공할 수 있다. 클라이언트가 해당 유형의 서버에 있는 청취 소켓에 연결되면 해당 소켓은 청취를 중지하지 않는다. 대신 클라이언트가 서비스되는 동시에 추가 클라이언트 연결 요청을 계속 수신한다. 그러나 여러 클라이언트를 동시에 연결하려면 스레드를 사용해야 한다.

이 섹션 시작 부분에서 논의된 URL 클래스는 필요한 네트워크 통신을 수행하기 위해 배후에서 클라이언트 소켓을 사용한다. 해당 연결의 반대편에서는 URL 객체의 연결 요청을 받아들이고 서버 컴퓨터의 특정 파일에 대한 해당 객체의 요청을 읽고 해당 파일의 내용을 네트워크를 통해 다시 전송하여 응답하는 서버 프로그램이 있다. 데이터를 전송한 후 서버는 연결을 닫는다.

---

클라이언트 프로그램은 네트워크에 있는 모든 컴퓨터 중에서 통신하려는 컴퓨터를 지정할 수 있는 방법이 있어야 한다. 인터넷상의 모든 컴퓨터네은 이를 식별하는 IP주소가 있다. 많은 컴퓨터는 `math.hws.ede` 또는 `www.google.com`와 같은 도메인 이름으로 참조될 수도 있다. 기존 (IPv4) IP 주소는 32비트 정수이다. 일반적으로 `64.123.144.237`과 같은 소위 "점으로 구분된 10진수" 형식으로 작성된다. 여기서 주소의 네 숫자는 각각 0에서 255까지 범위의 8비트 정수를 나타낸다. 인터넷 프로토콜의 새로운 버전 IPv6가 소개되고 있다. IPv6 주소는 128비트 정수이며 일반적으로 16진수 형식으로 작성된다.(일부 콜론 및 추가 정보가 포함될 수 있다.) 두 가지 형식 중 하나의 IP주고가 표시될 수 있다.

컴퓨터는 여러 개의 IP 주소를 가질 수 있으며 IPv4 및 IPv6 주소를 모두 가질 수 있다. 일반적으로 이들 중 하나는 프로그램이 동일한 컴퓨터에 있는 다른 프로그램과 통신하려고 할 때 사용할 수 있는 **루프백 주소(loopback address)** 이다. 루프백 주소는 IPv4 주소 127.0.0.1을 가지며 일반적으로 도메인 이름 **localhost**를 사용하여 참조할 수도 있다. 또한 물리적 네트워크 연결과 연결된 IP 주소가 하나 이상 있을 수 있다. 귀하의 컴퓨터에는 컴퓨터의 IP주소를 표시하는 유틸리티가 있을 수 있다. 동일한 작을 수행하는 작은 Java 프로그램 [ShowMyNetwork.java](https://math.hws.edu/javanotes/source/chapter11/ShowMyNetwork.java)를 작성했다. 출력은 다음과 같다.

```java
wlo1 :  /2603:7080:7f41:f400:441a:459f:2c62:c349%wlo1  /192.168.0.2  
lo :  /0:0:0:0:0:0:0:1%lo  /127.0.0.1
```

각 줄의 첫 항목은 네트워크 인터페이스 이름으로, 이는 컴퓨터 운영체제에만 실제로 의미가 있다. 동일한 줄에는 해당 인터페이스의 IP주소도 포함된다. 이 예에서 `lo`는 평소와 같이 IPv4 주소 127.0.0.1을 갖는 루프백 주소를 나타낸다. 여기서 가장 중요한 숫자는 `192.168.0.2`이며, 이는 네트워크를 통한 통신에 사용할 수 있는 IPv4 주소이다. 출력의 다른 숫자는 IPv6 주소이지만 IPv4 주소는 사람이 사용하기 더 쉽다.

이제 단일 컴퓨터에는 동시에 네트워크 통신을 수행하는 여러 프로그램이 있을 수도 있고, 하나의 프로그램이 여러 다른 컴퓨터와 통신할 수도 있다. 이러한 가능성을 허용하기 위해 네트워크 연결에는 실제로 IP 주소와 포트 번호가 결합되어 있다. 포트 번호는 단지 16비트 양의 정수이다. 서버는 단순히 연결을 수신하는 것이 아니라 특정 포트의 연결을 수신한다. 잠재 클라이언트는 서버가 실행 중인 컴퓨터의 인터넷 주소와 서버가 수신 대기 중인 포트 번호를 모두 알아야 한다. 예를 들어 웹 서버는 일반적으로 포트 80에서 연결을 수신한다. 다른 표준 인터넷 서비스에도 표준 포트 번호가 있다. (표준 포트 번호는 모두 1024보다 작은 번호로 특정 서비스를 위해 예약되어 있다.)

## 3. Java의 소켓

TCP/IP 연결을 위해 `java.net` 패키지는 ServerSocket과 Socket 이라는 두 가지 클래스를 제공한다. ServerSocket은 클라이어트의 연결 요청을 기다리는 청취 소켓을 나타낸다. 소켓은 실제 네트워크 연결의 한 끝점을 나타낸다. 소켓은 서버에 연결 요청을 보내는 클라이언트 소켓일 수 있다. 그러나 클라이언트의 연결 요청을 처리하기 위해 서버에서 소켓을 생성할 수도 있다. 이를 통해서 서버는 여러 솟켓을 생성하고 여러 연결을 처리할 수 있다. ServerSocket 자체는 연결에 참여하지 않는다. 단지 연결 요청을 듣고 생성한다. 실제 연결을 처리하는 소켓이다.

ServerSocket 객체를 생성할 때 서버가 수신할 포트 번호를 지정해야 한다.

```java
public ServletSocket(int port) throws IOException
```

포트 번호는 0에서 65535 사이에 있어야 하며 일반적으로 1024보다 커야 한다. 1024보다 작은 포트 번호가 지정되면 생성자가 SecurityException을 발생시킬 수 있다. 예를 들어, 지정된 포트 번호가 이미 사용 중인 경우에도 IOException이 발생할 수 있다.

ServerSocket이 생성되자마자 연결 요청 수신 대기가 시작된다. ServerSocket 클래스의 `accept()` 메서드는 이러한 요청을 수락하고 클라이언트와 연결을 설정한 다음 클라이언트와의 통신에 사용할 수 있는 소켓을 반환한다. 

```java
public Socket accept() throws IOException
```

`accept()` 메서드를 호출하면 연결 요청이 수신될 때까지 반환되지 않는다. 해당 메서드는 연결을 기다리는 동한 **차단(block)** 된다고 한다. `accept()`를 반복적으로 호출할 수 있다. 여러 연결 요청을 수락한다. ServerSocket은 `close()` 메서드를 사용하여 닫힐 때까지, 오류가 발생할 때 까지 또는 프로그램이 어떤 방식으로 종료될 때까지 계속해서 연결을 수신한다. 

서버가 포트 1728에서 수신 대기하고 프로그램이 실행되는 동안 계속해서 연결을 수락하기를 원한다고 가정해 본다. 하나의 클라이언트와의 통신을 처리하기 위해 `provideService(socket)` 메소드를 작성했다고 가정한다. 그러면 서버 프로그램의 기본 형식은 다음과 같다.

```java
try {
    ServerSocket server = new ServerSocket(1728);
    while (true) {
        Socket connection = server.accept();
        provideService(connection);
    }
}
catch (IOException e) {
    System.out.println("Server shut down with error: " + e);
}
```

클라이언트 측에서는 Socket 클래스의 생성자를 사용하여 클라이언트 소켓이 생성된다. 알려진 컴퓨터와 포트의 서버에 연결하려면 생성자를 사용한다.

```java
public Socket(String computer, int port) throws IOException
```

첫 번째 매개 변수는 IP 주소 혹은 도메인 이름일 수 있다. 이 생성자는 연결이 설정되거나 오류가 발생할 때까지 차단된다. 

연결된 소켓이 있으면 생성 방법에 관계없이 소켓 메서드 `getInputStream()` 및 `getOutputStream()`을 사용하여 연결을 통한 통신에 사용할 수 있는 스트림을 얻을 수 있다. 이러한 메서드는 각각 InputStream 및 OutputStream 유형의 객체를 반환한다. 이 모든 것을 염두에 두고 클라이언트 연결 작업 방법의 개요는 다음과 같다.

```java
/**
 * 지정된 서버 컴퓨터에 대한 클라이언트 연결을 열고
 * 서버의 포트 번호를 확인한 후 통신을 수행합니다.
 * 연결.
 */
void doClientConnection(String computerName, int serverPort) {
    Socket connection;
    InputStream in;
    OutputStream out;
    try {
        connection = new Socket(computerName,serverPort);
        in = connection.getInputStream();
        out = connection.getOutputStream();
    }
    catch (IOException e) {
        System.out.println(
                "Attempt to create connection failed with error: " + e);
        return;
    }
    
    // Use the streams, in and out, to communicate with the server.

    try {
        connection.close();
    }
    catch (IOException e) {
    }
}
```

이 모든 것이 네트워크 통신을 실제보다 더 쉽게 만든다. 네트워크가 완전히 신뢰할 수 있다면 제가 설명한 것처럼 모든 것이 거의 쉬울 것이다. 하지만 문제는 네트워크와 사람의 실수를 처리할 수 있는 강력한 프로그램을 작성하는 것이다. 여기서 다룬 네트워크 프로그래밍의 기본 아이디를 제공하여 몇 가지 간단한 네트워크 어플리케이션을 작성하는 데 충분하다. 

## 4. 간단한 클라이언트/서버

첫 번째 예는 두 개의 프로그램으로 구성된다. 프로그램의 소스 코드 파일은 [DateClient.java](https://math.hws.edu/javanotes/source/chapter11/DateClient.java) 및 [DateServer.java](https://math.hws.edu/javanotes/source/chapter11/DateServer.java)이다. 하나는 간단한 네트워크 클라이언트이고 다른 하나는 일치하는 서버이다. 클라이언트는 서버에 연결하고, 서버에서 한 줄의 텍스트를 읽고, 해당 텍스트를 화면에 표시한다. 서버가 보내는 텍스트는 서버가 실행되는 컴퓨터의 현재 날짜와 시간으로 구성된다. 연결을 열려면 클라이언트는 서버가 실행 중인 컴퓨터와 서버가 수신 대기 중인 포트을 알아야 한다. 서버는 포트 번호 32007에서 수신대기한다. 서버와 클라이언트가 동일한 포트를 사용하는 한 포트 번호는 1025에서 65535 사이일 수 있다. 1~1024 사이의 포트 번호는 표준 서비스용으로 예약되어 있으므로 다른 서버에서 사용하면 안된다. 서버가 실행 중인 컴퓨터와 이름이나 IP 번호를 명령줄 인수로 지정할 수 있다. 

```java
import java.net.*;
import java.util.Scanner;
import java.io.*;

/**
 * 이 프로그램은 지정된 컴퓨터에 대한 연결을 엽니다.
 *를 첫 번째 명령줄 인수로 사용합니다. 명령줄이 없는 경우
 * 인수가 주어지면 사용자에게 컴퓨터를 요청합니다
 * 연결합니다. 연결은 다음과 같습니다.
 * LISTENING_PORT에 의해 지정된 포트. 프로그램은 하나를 읽습니다
 * 연결의 텍스트 줄을 닫은 다음
 * 연결. 읽은 텍스트를 표시합니다.
 * 표준 출력. 이 프로그램은 다음과 함께 사용하도록 만들어졌습니다.
 * 현재 데이터를 보내는 서버 프로그램 DateServer
 * 서버가 실행되는 컴퓨터의 날짜 및 시간입니다.
 */
public class DateClient {

    public static final int LISTENING_PORT = 32007;

    public static void main(String[] args) {

        String hostName;         // 연결할 서버 컴퓨터의 이름입니다.
        Socket connection;       // 서버와 통신하기 위한 소켓입니다.
        BufferedReader incoming; // 연결에서 데이터를 읽어옵니다.

        /* 명령줄에서 컴퓨터 이름을 가져옵니다. */

        if (args.length > 0)
            hostName = args[0];
        else {
            Scanner stdin = new Scanner(System.in);
            System.out.print("Enter computer name or IP address: ");
            hostName = stdin.nextLine();
        }

        /* 연결한 다음 텍스트 줄을 읽고 표시합니다. */

        try {
            connection = new Socket( hostName, LISTENING_PORT );
            incoming = new BufferedReader(
                    new InputStreamReader(connection.getInputStream()) );
            String lineFromServer = incoming.readLine();
            if (lineFromServer == null) {
                //incoming.readLine()의 null은 다음을 나타냅니다.
                // 스트림의 끝을 발견했습니다.
                throw new IOException("Connection was opened, " +
                        "but server did not send any data.");
            }
            System.out.println();
            System.out.println(lineFromServer);
            System.out.println();
            incoming.close();
        }
        catch (Exception e) {
            System.out.println("Error:  " + e);
        }

    }
    
}
```

서버와의 모든 통신은 `try..catch` 문에서 수행된다. 이렇게 하면 연결이 열리거나 닫힐 때 그리고 입력 스트림에서 데이터를 읽을 때 생성될 수 있는 IOException을 포착한다. 연결의 입력 스트림은 텍스트 한 줄을 쉽게 읽을 수 있게 해주는 `readLine()` 메서드가 있는 BufferedReader로 래핑된다. 해당 메서드가 `null`을 반환하는 경우 서버가 데이터를 전송하지 않고 연결을 종료했음을 의미할 수 있다.

이 프로그램이 오류 없이 실행되기 위해서는 클라이언트가 연결을 시도하는 컴퓨터에서 서버 프로그램이 실행되고 있어야 한다. 그런데 클라이언트와 서버 프로그램을 동일한 컴퓨터에서 실행하는 것이 가능하다. 예를 들어 두 개의 명령 창을 열고 한 창에서 서버를 시작한 다음 다른 창에서 클라이언트를 실행할 수 있다. 이러한 작업을 더 쉽게 하기 위해 대부분의 컴퓨터는 도메인 이름 `localhost`와 IP 주소 `127.0.0.1`을 "이 컴퓨터"를 참조하는 것으로 인식한다.

---

DateClient 클라이언트 프로그램에 해당하는 서버 프로그램은 DateServer이다. DateServer 프로그램은 포트 32007에서 연결 요청을 수신하기 위해 ServerSocket을 생성한다. 수신 소켓이 생성된 후 서버는 연결을 수락하고 처리하는 무한 루프에 들어간다. 이는 프로그램이 어떤 방식으로든 종료될 때 까지 계속된다. 예를 들어 서버가 실행중인 명령 창에 `contorl-c`를 입력하는 것이다. 클라이언트로부터 연결 요청이 수신되면 서버는 연결을 처리하기 위해 서브루틴을 호출한다. 서브 루틴에서 모든 예외를 발생하는 일이 포착되어 서버가 충돌하지 않는다. 어떤 이유로 한 클라이언트에 대한 연결이 실패했다고 해서 서버가 종료되어야 한다는 의미는 아니다. 오류는 클라이언트의 잘못일 수 있다. 연결 처리 서브 루틴은 연결을 통해 데이터를 전송하기 위한 PrintWriter를 생성한다. 현재 날짜와 시간을 이 스트림에 쓴 다음 연결을 닫는다. 

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
 * 예를 들어). 이 서버는 각 연결을 처리합니다.
 * 별도의 스레드를 생성하지 않고 받은 그대로
 * 연결을 처리합니다.
 */
public class DateServer {

    public static final int LISTENING_PORT = 32007;

    public static void main(String[] args) {

        ServerSocket listener;  // 들어오는 연결을 수신합니다.
        Socket connection;      // 연결 프로그램과의 통신을 위해.

        
        /* 연결을 영원히 허용하고 처리하거나 오류가 발생할 때까지 처리합니다.
           (연결된 기기와 통신 중 발생하는 오류에 유의하세요.
           프로그램은 sendDate() 루틴에서 포착되어 처리됩니다.
           서버가 충돌하지 않습니다.) */
        try {
            listener = new ServerSocket(LISTENING_PORT);
            System.out.println("Listening on port " + LISTENING_PORT);
            while (true) {
                // 다음 연결 요청을 수락하고 처리합니다.
                connection = listener.accept();
                sendDate(connection);
            }
        }
        catch (Exception e) {
            System.out.println("Sorry, the server has shut down.");
            System.out.println("Error:  " + e);
            return;
        }

    }


    /**
     * 클라이언트라는 매개변수는 이미 다른 소켓에 연결되어 있는 소켓입니다.
     * 프로그램. 연결에 대한 출력 스트림을 가져오고 현재 시간을 보냅니다.
     * 연결을 닫습니다.
     */
    private static void sendDate(Socket client) {
        try {
            System.out.println("Connection from " +
                    client.getInetAddress().toString() );
            Date now = new Date();  // 현재 날짜와 시간입니다.
            PrintWriter outgoing;   // 데이터 전송을 위한 스트림입니다.
            outgoing = new PrintWriter( client.getOutputStream() );
            outgoing.println( now.toString() );
            outgoing.flush();  // 실제로 데이터가 전송되는지 확인하세요!
            client.close();
        }
        catch (Exception e){
            System.out.println("Error: " + e);
        }
    } 
    
}
```

명령줄 인터페이스에서 DateServer를 실행하면 연결 요청을 기다리고 수신되는 대로 보고한다. DateServer 서비스를 컴퓨터에서 영구적으로 사용할 수 있도록 하려면 프로그램이 **데몬(daemon)** 으로 실행된다. 데몬은 사용자와 관계없이 컴퓨터에서 지속적으로 실행되는 프로그램이다. 컴퓨터가 부팅되자마자 자동으로 데몬을 시작하도록 컴퓨터를 구성할 수 있다. 그런 다음 컴퓨터가 다른 목적으로 사용되는 동안에도 백그라운드에서 실행된다. 예를 들어, World Wide Web에서 페이지를 사용할 수 있게 만드는 컴퓨터는 웹 페이지에 대한 요청을 수신하고 페이지를 전송하여 응답하는 데몬을 실행한다. 이는 DateServer의 향상된 아날로그 프로그램일 뿐이다. 그러나 프로그램을 데몬으로 설정하는 방법에 대한 질문은 여기서 다루고 싶은 것이 아니다. 테스트 목적으로 프로그램을 직접 시작하는 것은 쉽지만, 예제는 심각한 서버로 실행될 만큼 충분히 강력하거나 모든 기능을 갖추고 있지 않다.

`outgoing.println()`을 호출하여 클라이언트에 데이터 라인을 보낸 후 서버 프로그램은 `outgoing.flush()`를 호출한다. `flush()` 메서드는 모든 출력 스트림 클래스에서 사용할 수 있다. 이를 호출하면 스트림에 기록된 데이터가 실제로 대상으로 전송된다. 일반적으로 출력 스트림을 사용하여 네트워크 연결을 통해 데이터를 보낼 때마다 이 함수를 호출해야 한다. 그렇게 하지 않으면 전송할 데이터가 상당히 많아질 때까지 스트림이 데이터를 수집할 가능성이 있다. 이는 효율성을 위해 수행되지만 클라이언트가 전송을 기다리고 있을 때 허용할 수 없는 지연이 발생할 수 있다. 소켓이 닫힐 때 일부 데이터가 전송되지 않은 상태로 남아 있을 수도 있으므로 특히 `flush()`를 호출하는 것이 중요하다. 연결을 닫기 전에, 이는 Java 구현이 다르게 동작할 수 있는 불행한 사례 중 하나이다. 출력 스트림을 플러시 하지 못하면 네트워크 응용 프로그램이 일부 컴퓨터 유형에서는 작동하지만 다른 컴퓨터에서는 작동하지 않을 수 있다.

## 5. 간단한 네트워크 채팅

예를 들어, DateServer에서 서버는 정보를 전송하고 클라이언트는 이를 읽는다. 클라이언트와 서버 간 양방향 통신도 가능하다. 첫 버째 예로, 연결의 각 끝에서 사용자가 다른 사용자에게 메시지를 보낼 수 있도록 하는 클라이언트와 서버를 살펴본다. 이 프로그램은 사용자가 메시지를 입력하는 명령줄 환경에서 작동한다. 이 예에서 서버는 단일 클라이언트의 연결을 기다린 다음 다른 클라이언트가 연결할 수 없도록 해당 리스너를 닫는다. 클라이언트와 서버가 연결된 후에는 연결의 양쪽 끝이 거의 동일한 방식으로 작동한다. 클라이언트측 사용자가 메시지를 입력하면 서버로 전송되어 해당 사용자에게 표시된다. 그런 다음 서버 사용자는 클라이언트에 전송되는 메시지를 입력한다. 그런 다음 클라이언트 사용자능 다른 메시지를 입력한다. 이는 메시지를 묻는 메시지가 표시될 때 한 사용자 또는 다른 사용자가 "quit"를 입력할 때까지 계속된다. 그런 일이 발생하면 연결이 닫히고 두 프로그램이 모두 종료된다. 클라이언트 프로그램과 서버 프로그램은 매우 유사하다. 연결을 여는 기술은 다르며 클라이언트는 첫 번째 메시지를 보내도록 프로그래밍되고 서버는 첫 번째 메시지를 수신하도록 프로그래밍 되어있다. 클라이언트 및 서버 프로그램은 파일에서 찾을 수 있다. [CLChatClient](https://math.hws.edu/javanotes/source/chapter11/CLChatClient.java), [CLChatServer](https://math.hws.edu/javanotes/source/chapter11/CLChatServer.java)


```java
import java.net.*;
import java.util.Scanner;
import java.io.*;

/**
 * 이 프로그램은 간단한 명령줄 인터페이스 채팅 프로그램의 한쪽 끝입니다.
 * CLChatClient의 연결을 기다리는 서버 역할을 합니다.
 * 프로그램. 서버가 수신하는 포트는 다음과 같이 지정할 수 있습니다.
 * 명령줄 인수. 그렇지 않은 경우 다음에서 지정한 포트는
 * 상수 DEFAULT_PORT가 사용됩니다. 포트 번호가 0인 경우
 *를 지정하면 서버는 사용 가능한 모든 포트에서 수신 대기합니다.
 * 본 프로그램은 1개의 연결만 지원합니다. 연결이 되자마자
 * 열리면 청취 소켓이 닫힙니다. 연결의 두 끝
 * 각각은 상대방에게 HANDSHAKE 문자열을 보내서 양쪽 끝이 확인할 수 있도록 합니다.
 * 상대방의 프로그램이 올바른 유형인지 확인합니다. 그러면 연결된
 * 프로그램은 서로 번갈아 메시지를 보냅니다. 클라이언트는 항상 보냅니다.
 * 첫 번째 메시지. 양쪽 끝의 사용자는 다음 방법으로 연결을 닫을 수 있습니다.
 * 메시지를 묻는 메시지가 나타나면 "quit" 문자열을 입력합니다. 첫 번째
 * 연결을 통해 전송된 문자열의 문자는 0 또는 1이어야 합니다. 이것
 * 문자는 명령으로 해석됩니다.
 */
public class CLChatServer {

    /**
     * 명령줄에 아무것도 지정되지 않은 경우 수신할 포트입니다.
     */
    static final int DEFAULT_PORT = 1728;

    /**
     * 악수 문자열. 연결의 각 끝은 이 문자열을
     * 기타 연결이 열린 직후. 이는 다음을 확인하기 위해 수행됩니다.
     * 연결 반대편의 프로그램은 CLChat 프로그램입니다.
     */
    static final String HANDSHAKE = "CLChat";

    /**
     * 이 문자는 전송되는 모든 메시지 앞에 추가됩니다.
     */
    static final char MESSAGE = '0';

    /**
     * 이 문자는 사용자가 종료할 때 연결된 프로그램으로 전송됩니다.
     */
    static final char CLOSE = '1';


    public static void main(String[] args) {

        int port;   // 서버가 수신 대기하는 포트입니다.

        ServerSocket listener;  // 연결 요청을 수신합니다.
        Socket connection;      // 클라이언트와의 통신을 위해.

        BufferedReader incoming;  // 클라이언트로부터 데이터를 수신하기 위한 스트림입니다.
        PrintWriter outgoing;     // 클라이언트에 데이터를 전송하기 위한 스트림입니다.
        String messageOut;        // 클라이언트에게 보낼 메시지입니다.
        String messageIn;         // 클라이언트로부터 받은 메시지입니다.

        Scanner userInput;        // 읽기용 System.in 래퍼
                                    // 사용자로부터의 입력 라인.
        
        /* 먼저 명령줄에서 포트 번호를 가져옵니다.
            또는 아무것도 지정되지 않은 경우 기본 포트를 사용합니다. */
        if (args.length == 0)
            port = DEFAULT_PORT;
        else {
            try {
                port= Integer.parseInt(args[0]);
                if (port < 0 || port > 65535)
                    throw new NumberFormatException();
            }
            catch (NumberFormatException e) {
                System.out.println("Illegal port number, " + args[0]);
                return;
            }
        }

        /* 연결 요청을 기다립니다. 도착하면 닫으세요
           청취자를 아래로 내립니다. 통신을 위한 스트림 만들기
           그리고 악수를 교환합니다. */
        try {
            listener = new ServerSocket(port);
            System.out.println("Listening on port " + listener.getLocalPort());
            connection = listener.accept();
            listener.close();
            incoming = new BufferedReader(
                    new InputStreamReader(connection.getInputStream()) );
            outgoing = new PrintWriter(connection.getOutputStream());
            outgoing.println(HANDSHAKE);  // 클라이언트에게 핸드셰이크를 보냅니다.
            outgoing.flush();
            messageIn = incoming.readLine();  // 클라이언트로부터 핸드셰이크를 받습니다.
            if (! HANDSHAKE.equals(messageIn) ) {
                throw new Exception("Connected program is not a CLChat!");
            }
            System.out.println("Connected.  Waiting for the first message.");
        }
        catch (Exception e) {
            System.out.println("An error occurred while opening connection.");
            System.out.println(e.toString());
            return;
        }

        /* 한쪽이 연결될 때까지 연결의 다른 쪽 끝과 메시지를 교환합니다.
           또는 다른 하나는 연결을 닫습니다. 이 서버 프로그램은
           클라이언트의 첫 번째 메시지. 그 후 메시지가 번갈아 나타납니다.
           엄격하게 앞뒤로. */
        
        try {
            userInput = new Scanner(System.in);
            System.out.println("NOTE: Enter 'quit' to end the program.\n");
            while (true) {
                System.out.println("WAITING...");
                messageIn = incoming.readLine();
                if (messageIn.length() > 0) {
                    // 메시지의 첫 번째 문자는 명령입니다. 만약에
                    // 명령이 CLOSE이면 연결이 닫힙니다.  
                    // 그렇지 않으면 명령 문자를 제거합니다.
                    // 메시지를 보내고 진행합니다.
                    if (messageIn.charAt(0) == CLOSE) {
                        System.out.println("Connection closed at other end.");
                        connection.close();
                        break;
                    }
                    messageIn = messageIn.substring(1);
                }
                System.out.println("RECEIVED:  " + messageIn);
                System.out.print("SEND:      ");
                messageOut = userInput.nextLine();
                if (messageOut.equalsIgnoreCase("quit"))  {
                    // 사용자가 종료를 원합니다. 상대방에게 알린다
                    // 연결을 종료한 후 연결을 닫습니다.n close the connection.
                    outgoing.println(CLOSE);
                    outgoing.flush();  // 데이터가 전송되었는지 확인하세요!
                    connection.close();
                    System.out.println("Connection closed.");
                    break;
                }
                outgoing.println(MESSAGE + messageOut);
                outgoing.flush(); // 데이터가 전송되었는지 확인하세요!
                if (outgoing.checkError()) {
                    throw new IOException("Error occurred while transmitting message.");
                }
            }
        }
        catch (Exception e) {
            System.out.println("Sorry, an error has occurred.  Connection lost.");
            System.out.println("Error:  " + e);
            System.exit(1);
        }

    }  
    
}
```

이 프로그램은 DateServer 보다 조금 더 강력하다. 우선, 연결을 시도하는 클라이언트가 실제로 CLChatClient 프로그램인지 확인하기 위해 **핸드셰이크(handshake)** 를 사용한다. 핸드셰이크는 실제 데이터가 전송되기 전에 연결 설정의 일부로 클라이언트와 서버 간에 전송되는 정보이다. 이 경우 연결의 각 측은 자신을 식별하기 위해 문자열을 상대방에게 보낸다. 핸드셰이크는 CLChatClient 와 CLChatServer 간의 통신을 위해 구성한 **프로토콜(protocol)** 의 일부이다. 프로토콜은 연결을 통해 어떤 데이터와 메시지가 교환될 수 있는지, 어떻게 표현되어야 하는지, 어떤 순서로 전송될 수 있는지에 대한 자세한 사양이다. 클라이언트/서버 애플리케이션을 디자인할 때 프로토콜 디자인은 중요하다. `CLChat` 프로토콜 의 또 다른 측면은 핸드셰이크 후에 연결을 통해 전송되는 모든 텍스트 줄이 명령 역할을 하는 문자로 시작된다는 것이다. 문자가 0이면 줄의 나머지 부분은 한 사용자가 다른 사용자에게 보내는 메시지이다. 문자가 1이면 해당 줄은 사용자가 "quit" 명령을 입력했으며 연결이 종료됨을 나타낸다.

