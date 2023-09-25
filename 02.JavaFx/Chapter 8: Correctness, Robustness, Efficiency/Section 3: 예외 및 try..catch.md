# 예외 및 try..catch

이상적인 상황에서 프로그램이 작동하도록 하는 것은 일반적으로 프로그램을 견고하게 만드는 것 보다 훨씬 쉽다. 견고한 프로그램은 비정상적이거나 "예외적인" 상황에서 충돌 없이 살아남을 수 있다. 견고한 프로그램을 작성하는 한 가지 접근 방식은 발생할 수 있는 문제를 예측하고 가능한 각 문제에 대한 테스트를 프로그램에 포함시키는 것이다. 예를 들어, i가 배열 A에 대해 선언된 인덱스 범위 내에 있지 않을 때 배열 요소 A[i]를 사용하려고 하면 프로그램이 중단된다. 견고한 프로그램은 잘못된 인덱스에 가능성을 예상하고 이를 방지해야 한다. 이를 수행하는 한 가지 방법은 인덱스가 올바른 범위에 있음을 보장하는 방식으로 프로그램을 작성하는 것읻. 또 다른 방법은 배열에서 인덱스 값을 사용하기 전에 인덱스 값이 유효한지 테스트하는 것이다.

```java
if (i < 0 || i >= A.length) {
    ...
} else {
    ...     
}
```

이 접근 ㅂ아식에는 몇 가지 문제가 있다. 잘못될 수 있는 모든 상황을 예측하는 것은 어렵고 때로는 불가능하다. 오류가 감지되면 어떻게 해야 할지 항상 명확한 것은 아니다. 더욱이, 가능한 모든 문제를 예상하려고 하면 간단한 알고리즘이 if문으로 뒤엉키게 될 수 있다.


## 1. 예외 및 예외 클래스

섹션 3.7에서 Java가 프로그램이 실행되는 동안 발생할 수 있는 오류를 처리하기 위해 더 깔끔하고 구조화된 대체 기술을 제공한다는 것을 확인했다. 이 기술을 **예외 처리(exception handling)** 라고 한다. "예외"라는 단어는 "오류"보다 더 일반적인 의미를 갖는다. 여기에는 프로그램이 실행될 때 발생하는 모든 상황이 포함되며, 이는 프로그램의 정상적인 제어 흐름에 대한 예외로 처리된다. 예외는 오류일 수도 있도 우아한 알고리즘을 복잡하게 만들고 싶지 않은 특별한 경우일 수도 있다.

프로그램 실행 중에 예외가 발생하면 예외가 **발생(thrown)** 한다고 말한다. 이런 일이 발생하면 프로그램의 정상적인 흐름이 궤도에서 벗어나 프로그램이 충돌할 위험이 있다. 그러나 어떤 방식으로든 예외를 **포착(caught)** 하고 처리하면 충돌을 피할 수 있다. 예외는 프로그램의 한 부분에서 발생하고 다른 부분에서 포착될 수 있다. 포착되지 않은 예외는 일반적으로 프로그램 충돌을 유발한다. 

그런데 Java 프로그램은 Java 인터프리터에 의해 실행되기 때문에 프로그램이 충돌한다는 것은 단순히 프로그램이 비정상적으로 조기에 종료된다는 의미이다. 이는 Java 인터프리터가 충돌한다는 의미는 아니다. 실제로 인터프리터는 프로그램에서 포착하지 못한 모든 예외를 포착한다. 인터프리터는 프로그램을 종료하며 응답한다. 다른 많은 프로그래밍 언어에서는 충돌한 ㅍ로그램이 전체 시스템을 충돌시키고 다시 시작할 때까지 컴퓨터를 정지시키는 경우가 있다. Java를 사용하면 이러한 시스템 충돌이 불가능해야 한다. 즉, 충돌이 발생하면 자신의 프로그램이 아닌 시스템을 비난하는 만족감을 느낄 수 있다.

예외는 예외를 포착하고 처리하는 데 사용되는 `try..catch`문과 함께 섹션 3.7에서 소개되었다. 그러나 해당 섹션에서는 `try..catch`의 전체 구문이나 예외의 전체 복잡성을 다루지 않았다.

---

예외가 발생하면 실제로 "던지는" 것은 객체이다. 이 객체는 예외가 발생한 지점부터 예외가 포착되어 처리되는 지점까지 정보(인스턴스 변수)를 전달할 수 있다. 이 정보에는 예외가 발생했을 때 실행 중이던 서브 루틴 목록의 **서브 루틴 호출 스택(subroutine call stack)** 이 항상 포함된다. (하나의 서브루틴이 다른 서브루틴을 호출할 수 있으므로 여러 서브 루틴이 동시에 활성화될 수 있다.) 일반적으로 예외 객체에는 예외를 발생시킨 원일을 설명하는 오류 메시지도 포함되며 다른 데이터도 포함될 수 있다. 모든 예외 객체는 표준 클래스 `java.lang.Throwable`의 하위 클래스에 속해야 한다. 일반적으로 각각의 서로 다른 유형의 예외는 자체 `Throwable` 하위 클래스로 표시되며 이러한 하위 클래스는 다양한 유형의 예외 간의 관계를 보여주는 상당히 복잡한 클래스 계층 구조로 배열된다. `Throwable`에는 `Error`, `Exception` 이라는 두 개의 직접적인 하위 클래스가 있다. 이 두 하위 클래스에는 미리 정의된 다른 하위 클래스도 많이 있다. 또한 프로그래머는 새로운 유형의 예외를 나타내는 새로운 예외 클래스를 만들 수 있다.

`Error` 클래스의 서브 클래스 대부분은 합리적인 처리 방법이 없기 때문에 일반적으로 프로그램 종료를 초래하는 Java 가상 머신 내의 심각한 오류를 나타낸다. 일반적으로 이러한 오류를 포착하고 처리하려고 해서는 안된다. 예를 들어 Java 가상 머신이 컴파일된 Java 클래스를 포함해야 하는 파일에서 일종의 불법 데이터를 발견할 때 발생하는 `ClassFormatError`가 있다. 해당 클래스가 프로그램의 일부로 로드된 경우에는 실제로 프로그램을 진행할 방법이 없다.

반면에 `Exception` 클래스의 하위 클래스는 포착되어야 하는 예외를 나탄내다. 대부분의 경우 이는 자연스럽게 "오류"라고 부를 수 있는 예외이지만 프로그래머가 예상하고 합리적인 방식으로 응답할 수 있는 프로그램이나 입력 데이터의 오류이다. (그러나, 발생할 수 있는 모든 오류를 잡아서 프로그램이 충돌하지 않도록 여기에 뭔가를 넣을 테니 라고 말하는 유혹을 피해야 한다. 오류에 응답하려면 그냥 프로그램이 충돌하도록 두는 것이 가장 좋다. 계속하려고 하면 아마도 나중에 더 나쁜 일만 초래할 것이다. 최악의 경우 프로그램이 답을 알려주지 않고 잘못된 답을 제공하는 것이다.)

`Exception` 클래스에는 자체 하위 클래스인 `RuntimeException`이 있다. 이 클래스는 이전 섹션에서 다룬 모든 예외를 포함하여 많은 일반적인 예외를 함께 그룹화한다. 예를 들어  `IllegalArgumentException`, `NullPointerException`는 `RuntimeException`의 하위 클래스이다. `RuntimeException`은 일반적으로 프로그래머가 수정해야 하는 프로그램 버그를 나타낸다. `RuntimeExceptions` 및 `Errors`는 프로그램이 이러한 문제가 발생할 가능성을 간단히 무시할 수 있다는 속성을 공유한다. (여기서 "무시"한다는 것은 예외가 발생할 경우 프로그램이 충돌하도록 놔두는 것을 의미한다.) 예를 들어, 프로그램은 가능한 `ArrayIndexOutOfBoundsException`을 포착하기 위한 준비 없이 A[i]와 같은 배열 참조를 사용할 때마다 이를 수행한다. `Error`, `RuntimeException 및 해당 하위 클래스 이외의 다른 모든 예외 클래스의 경우 예외 처리는 "필수(mandatory)"이다.

다음 다이어그램은 `Throwable` 클래스와 해당 하위 클래스 중 일부를 보여주는 클래스 계층 구조이다. 필수 예외 처리가 필요한 클래스는 빨간색이다.

![Throwable 계층](./images/throwable%20계층구조.png)

`Throwable` 클래스에는 모든 예외 객체와 함께 사용할 수 있는 여러 인스턴스 메서드가 포함되어 있다. `e`가 `Throwable` 유형 또는 해당 하위 클래스 중 하나인 경우 `e.getMessage()`는 예외를 설명하는 문자열을 반환하는 함수이다. 객체의 문자열 표현이 필요할 때마다 시스템에서 사용되는 `e.toString()` 함수는 에외가 속한 클래스의 이름과 함께 반환되는 동일한 문자열을 포함하는 문자열을 반환한다. `e.printStackTrace()` 메서드는 예외가 발생했을 때 어떤 서브 루틴이 활성화되었는지 알려주는 스택 추적을 표준 출력을 기록한다. 스택 추적은 문제의 원인을 파악하려고 할 때 매우 유용하게 할 수 있다. 스택 추적의 정보는 프로그램에서 예외가 발생한 위치를 정확하게 알려준다. (프로그램에서 예외가 포착되지 않으면 예외에 대한 기본 응답이 스택 추적을 표준 출력으로 인쇄한다.)

## 2. try 문

Java 프로그램에서 예외를 포착하려면 `try`문이 필요하다. 섹션 3.7절 부터 이러한 문을 사용해 왔지만 `try` 문의 전체 구문은 거기에 제시된 것보다 더 복잡하다. 지금 까지는 다음처럼 사용했다.

```java
try {
    double determinant = M[0][0]*M[1][1] - M[0][1]*M[1][0];
    System.out.println("The determinant of M is " + determinant);
}
catch ( ArrayIndexOutOfBoundsException e ) {
    System.out.println("M is the wrong size to have a determinant.");
    e.printStackTrace();
}
```

여기서 컴퓨터는 "`try`"라는 단어 뒤에 오는 명령문 블록을 실행하려고 시도한다. 이 블록을 실행하는 동안 예외가 발생하지 않으면 문의 "`catch`" 부분은 무시된다. 그러나 `ArrayIndexOutOfBoundsException` 유형의 예외가 발생하면 컴퓨터는 즉시, `try`문의 `catch` 절로 점프한다. 이 명령문 블록은 `ArrayIndexOutOfBoundsException`에 대한 **예외 처리기(exception handler)** 라고 한다. 이러한 방식으로 예외를 처리하면 프로그램이 중단되는 것을 방지할 수 있다. `catch`의 절 시작 전에 변수 `e`에 예외를 나타내는 객체가 할당된다.

그러나 `try`문의 전체 구문에는 다양한 옵션이 있다. 그것들을 살펴보는 데는 시간이 좀 걸릴 것이다. 우선 `try..catch` 문에는 두 개 이상의 `catch` 절이 있을 수 있다. 이렇게 하면 하나의 `try`문으로 여러 가지 다른 유형의 예외를 포착할 수 있다. 위의 예에서는 `ArrayIndexOutOfBoundsException` 외에도 M값이 `null` 인 경우 발생할 수 있는 `NullPointerException`가 있다. 


```java
try {
    double determinant = M[0][0]*M[1][1] - M[0][1]*M[1][0];
    System.out.println("The determinant of M is " + determinant);
}
catch ( ArrayIndexOutOfBoundsException e ) {
    System.out.println("M is the wrong size to have a determinant.");
}
catch ( NullPointerException e ) {
    System.out.print("Programming error!  M doesn't exist." + );
}
```

여기서 컴퓨터는 `try`절의 명령문을 실행하려고 시도한다. 오류가 발생하지 않으면 두 `catch` 절 모두 건너뛴다. `ArrayIndexOutOfBoundsException`가 발생하면 컴퓨터는 첫 `catch` 절의 본문을 실행하고 두 번째 `catch` 절을 건너 뛴다. `NullPointerException`이 발생하면 두 번째 `catch` 절로 점프하여 이를 실행한다.

`ArrayIndexOutOfBoundsException`, `NullPointerException`는 모두 `RuntimeException`의 하위 클래스이다. 단일 `catch` 절로 처리할 수 있다.

```java
try {
    double determinant = M[0][0]*M[1][1] - M[0][1]*M[1][0];
    System.out.println("The determinant of M is " + determinant);
}
catch ( RuntimeException err ) {
    System.out.println("Sorry, an error has occurred.");
    System.out.println("The error was: " + err);
}
```

이 `try` 문의 `catch` 절은 `RuntimeException` 클래스 또는 해당 하위 클래스에 속하는 모든 예외를 포착한다. 이는 예외 클래스가 클래스 계층 구조로 구성되는 이유를 보여준다. 이를 통해 특정 유형의 예외만 포착하도록 네트를 좁게 캐스팅할 수 있는 옵션이 제공된다. 또는 광범위한 예외 클래스를 포착하기 위해 광범위하게 그물을 던질 수 있다. 서브 클래싱으로 인해 `try`문에 여러 개의 `catch` 절이 있는 경우 지정된 예외가 해당 `catch` 절 중 여러 개와 일치할 수 있다. 예를 들어 `NullPointerException` 유형의 예외는 다음에 대한 `catch` 절과 일치하다. (`NullPointerException` , `RuntimeException` , `Exception` 또는 `Throwable` ) 이 경우 예외와 일치하는 첫 번째 `catch` 절만 실행된다.

물론 `RuntimeException`을 포착하면 우리가 관심 있는 두 가지 예외 유형보다 더 많은 유형의 예외를 포착할 수 있다. 단일 `catch` 절에 여러 특정 예외 유형을 결합하는 것이 가능하다.

```java
try {
    double determinant = M[0][0]*M[1][1] - M[0][1]*M[1][0];
    System.out.println("The determinant of M is " + determinant);
}
catch ( NullPointerException | ArrayIndexOutOfBoundsException err ) {
    System.out.println("Sorry, an error has occurred.");
    System.out.println("The error was: " + err);
}
```

여기서는 두 가지 예외 유형이 or 연산자인  "`|`"와 결합한다. 이 예에서는 `NullPointerException` 또는 `ArrayIndexOutOfBoundsException` 유형의 오류를 포착하며 다른 유형은 포착하지 않는다.

여기서 사용하는 예는 현실적이지 않는다. 왜냐하면 널 포인터와 잘못된 배열 인덱스를 방지하기 위해 예외 처리를 사용할 가능성이 거의 없기 때문이다. 이는 예외 처리보다 신중한 프로그래밍이 더 나은 경우이다. 프로그램이 M배열에 `null`이 아닌 합리적인 값을 할당하는지 확인하라. 배열을 사용하려고 할 때마다 Java 설계자가 `try..catch` 문을 설정하도록 강요한다면 분명히 분개할 것이다. 이것이 잠재적인 `RuntimeException`을 처리하는 이유이다. 필수는 아니다. 잘못될 수 있는 일이 너무 많다.

---

아직 `try` 문의 구문을 완전히 지정하지 않았다. 다음 변형은 `try` 문 끝에 `finally` 절이 있을 가능성이다.

```
try {
    statements
}
optional-catch-clauses
optional-finally-clause
```

`catch` 절도 선택 사항으로 나열되어 있다. `try` 문에는 0개 이상의 `catch` 절과 선택적으로 `finally` 절이 포함될 수 있다. `try` 문에는 둘 중 하나가 포함되어야 한다. 즉, `try` 문은 `finally` 절이나 하나 이상의 `catch` 절 또는 둘 다 가질 수 있다. 

```
catch ( exception-class-names variable-name ) {
    statements
}
```

여기서 예외 클래스 이름은 단일 예외 클래스 이거나 "`|`"로 구분된 여러 클래스일 수 있다. 

```
finally {
    statements
}
```

`finally` 절의 의미는 예외 발생 여부와 발생한 예외의 포착 여부에 관계없이 `finally` 절의 문 블록이 `try` 문 실행의 마지막 단계로 실행되도록 보장된다는 것이다. 그리고 처리한다. `finally` 절은 어떤 상황에서도 생략해서는 안되는 필수적인 정리를 수행하기 위한 것이다. 이러한 정리 유형의 한가지 예는 네트워크 연결을 닫는 것이다. 이 경우 실제 프로그래밍을 살펴볼 만큼 네트워킹에 대해 아직 충분히 알지 못하더라고 몇 가지 의사 코드를 고려해 볼 수 있다.

```
try {
   open a network connection
   communicate over the connection
}
catch ( IOException e ) {
   report the error
}
finally {
   if the connection was successfully opened
      close the connection
}
```

`finally` 절은 통신 중에 오류를 발생하는지 여부에 관계없이 네트워크 연결이 확실히 닫히도록 보장한다. 이 예제의 의사코드는 리소스를 강력하게 획득하고 리소스를 사용한 다음, 리소스를 해제하는 데 사용할 수 있는 일반적인 패턴을 따른다.


---

리소스를 획득한 다음 리소스를 사용하고 리소스를 해제하는 패턴은 매우 일반적이다. 리소스를 얻는 동안 오류가 발생하지 않은 경우에만 리소스를 해제할 수 있다. 그리고, 성공적으로 획득했다면, 사용 중 오류가 발생하는지 여부에 관계없이 닫아야 한다. 이 패턴은 매우 일반적이어서 `try`문 구문의 마지막 옵션으로 이어진다. 이 옵션을 사용하면 리소스를 얻는 데 코드만 필요하며 리소스 공개에 대해 걱정할 필요가 없다. 이는 `try` 문이 끝나면 자동으로 발생한다.

이것이 작동하려면 맬개 변수 없이 `close()`라는 단일 메서드를 정의하는 `AutoCloseable` 이라는 인터페이스를 구현하는 객체를 리소스를 표현해야 한다. 파일 및 네트워크 연결과 같은 것을 나타내는 표준 Java 클래스는 이미 `AutoCloseable`을 구현한다. 섹션 2.4.6에 소개된 `Scanner` 클래스도 마찬가지이다. 해당 섹션에서 `Scanner`를 사용하여 `System.in`에서 읽는 방법을 보여줬다. 스캐너를 사용한 후에는 닫는 것이 좋은 형태라고 생각된다. 

```java
try( Scanner in = new Scanner(System.in) ) {
    // Use the Scanner to read from standard input
}
catch (Exception e) {
    // ... some error occurred while using the Scanner
}
```

Scanner를 할당하는 명령문은 `try` 단어 뒤에 괄호 안에 표시된다. 명령문은 변수 초기화를 포함하는 변수 선언 형식이어야 한다. 변수는 `try` 문에 대해 로컬이다. 이 예에서는 Scanner 문이 종료되는 한 시점에서 `in.close()`를 확실히 호출할 것이라는 점을 확신할 수 있다. 성공적으로 초기화된다.

## 3. 예외 던지기





