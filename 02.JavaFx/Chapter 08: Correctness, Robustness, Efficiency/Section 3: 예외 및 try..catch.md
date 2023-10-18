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

프로그램이 의도적으로 예외를 발생시키는 것이 타당한 경우가 있다. 이는 프로그램이 일종의 예외 또는 오류 조건을 발견했지만 문제가 발견된 시점에서 오류를 처리할 합리적인 방법이 없는 경우이다. 프로그램은 프로그램의 다른 부분이 예외를 포착하고 처리할 것이라는 희망으로 예외를 발생시킬 수 있다. 이는 `throw` 문을 사용하여 수행할 수 있다. 섹션 4.3.8에서 이미 확인했다. 

```java
throw exception-object;
```

**exception-object**는 `Throwable`의 하위 클래스 중 하나에 속하는 객체여야 한다. 일반적으로 `Exception`의 하위 클래스 중 하나에 속한다. 대부분의 경우 `new` 연산자를 사용하여 새로 생성된 객체이다. 

```java
throw new ArithmeticException("Division by zero");
```

생성자의 매개 변수는 예외 객체의 오류 메시지가 된다. `e`가 객체를 참조하는 경우 `e.getMessage()`를 호출하여 오류 메시지를 검색할 수 있다. 그러면 프로그래머가 굳이 예외를 발생시켜야 하는 이유는 무엇인가? `int` 유형인 경우 0으로 나누면 실제로 `ArithmeticException`가 발생한다. 그러나 부동 소수점 숫자를 사용한 산술 연산에서는 예외가 발생하지 않는다. 대신 특수 값 `Double.NaN`이 사용된다. 불법적인 작업의 결과를 나타내는 데 사용된다. 어떤 상황에서든 실수를 0으로 나눌 때 `ArithmeticException`을 발생시키는 것을 선호할 수 있다.

예외는 시스템이나 `throw` 문에 의해 발생할 수 있다. 두 경우 모두 예외는 정확히 동일한 방식으로 처리된다. `try` 문 내에서 예외가 발생했다고 가정한다. 해당 `try`문에 해당 유형의 예외를 처리하는 `catch`절이 있으면 컴퓨터는 `catch` 절로 점프하여 실행한다. 예외가 **처리(handled)된다.** 예외를 처리한 후 컴퓨터는 `try`문의 `finally`(있는 경우)절을 실행한다. 그런 다음 `try` 다음에 이어지는 나머지 프로그램에서 정상적으로 계속된다. 예외가 즉시 발견되어 처리되지 않으면 예외 처리가 계속된다.

서브 루틴 실행 중에 예외가 발생하고 동일한 서브 루틴에서 예외가 처리되지 않으면 서브 루틴이 종료된다. (보류 중인 `finally`절이 실행 된 후) 그런 다음 해당 서브 루틴을 호출한 루틴이 예외를 처리할 기회를 얻는다. 즉, 적절한 `catch` 절이 있는 `try`문 내에서 서브루틴이 호출된 경우 해당 `catch`절이 실행되고 프로그램은 거기서부터 정상적으로 계속된다. 다시 말하자면, 두 번째 루틴이 예외를 처리하지 않으면 두 번째 루틴도 종료되고 이를 호출한 루틴이 예외가 발생하면 다음 샷을 얻는다. 예외는 처리되지 않고 전체 서브 루틴 호출 체인을 통과하는 경우에만 프로그램을 중단시킨다. 이를 "호출 스택 해제(unwinding the call stack)"라고 한다.

예외를 생성할 수 있는 서브 루틴은 루틴 헤더에 `throws exception-class-name` 절을 추가해 이 사실을 알릴 수 있다.

```java
/**
 * 이차 방정식의 두 근 중 더 큰 값을 반환합니다.
 * A*x*x + B*x + C = 0(근이 있는 경우). A == 0 또는
 * 판별식 B*B - 4*A*C가 음수인 경우 예외
 * IllegalArgumentException 유형이 발생합니다.
 */
static public double root( double A, double B, double C ) 
                              throws IllegalArgumentException {
    if (A == 0) {
        throw new IllegalArgumentException("A can't be zero.");
    }
    else {
        double disc = B*B - 4*A*C;
        if (disc < 0)
            throw new IllegalArgumentException("Discriminant < zero.");
        return  (-B + Math.sqrt(disc)) / (2*A);
    }
}
```

이전 섹션에서 설명한 대로 이 서브 루틴의 계산에는 `A != 0` 및 `B*B-4*A*C >= 0` 조건이 있다. 이러한 전제 조건 중 하나가 위반하면 서브 루틴에서 `IllegalArgumentException` 유형의 예외가 발생한다. 서브 루틴에서 잘못된 조건이 발견되면 예외를 던지는 것이 합리적인 응답인 경우가 많다. 서브 루틴을 호출한 프로그램이 오류를 처리하는 좋은 방법을 알고 있는 경우 예외를 포착할 수 있다. 그렇지 않으면 프로그램이 중단되고 프로그래머는 프로그램을 수정해야 한다는 것을 알게 된다.

서브 루틴 헤더의 `throws` 절은 쉼표로 구분하여 여러 가지 다른 유형의 예외를 선언할 수 있다. 

```java
oid processArray(int[] A) throws NullPointerException, 
                                         ArrayIndexOutOfBoundsException { ...
```


## 4. 필수 예외 처리

앞의 예에서 서브 루틴 `root()`가 `IllegalArgumentException`을 발생시킬 수 있다고 선언하는 것은 이 루틴의 잠재적인 독자를 위한 예의일 뿐이다. 이는 `IllegalArgumentException` 처리가 "필수"가 아니기 때문이다. 루틴은 가능성을 알리지 않고 `IllegalArgumentException`을 발생시킬 수 있다. 프로그래머가 `NullPointerException` 유형의 예외를 포착하거나 무시하도록 선택할 수 있는 것처럼 해당 루틴을 호출하는 프로그램은 자유롭게 예외를 포착하거나 무시할 수 있다.

필수 처리가 필요한 예외 클래스의 경우 상황이 다르다. 서브 루틴이 그러한 예외를 발생시킬 수 있는 경우 해당 사실은 루틴 정의의 `throws`절에서 발표되어야 한다. 그렇게 하지 못하면 컴파일러에서 보고되는 구문 오류이다. 필수 처리가 필요한 예외를 **checked exceptions** 이라고 한다. 컴파일러는 그러한 예외가 프로그램에 의해 처리되는지 확인한다.

서브루틴 본문의 일부 명령문이 필수 처리가 필요한 checked exception를 생성할 수 있다고 가정한다. 명령문은 예외를 직접 발생시키는 `throw`문 일수도 있고, 예외를 발생시킬 수 있는 서브 루틴에 대한 호출일 수도 있다. 두 경우 모두 예외를 **처리해야 한다.** 이는 두 가지 방법 중 하나로 수행할 수 있다. 첫 번째 방법은 예외를 처리하는 `catch`절이 있는 `try`문에 명령문을 배치하는 것이다. 이 경우 예외는 서브 루틴 내에서 처리되므로 서브 루틴 호출자는 예외를 볼 수 없다. 두 번째 방법은 서브 루틴이 예외를 발생시킬 수 있다고 선언하는 것이다. 이것은 `throws`을 추가하여 수행된다. 절을 서브루틴 헤더에 추가하여 서브 루틴이 실행될 때 예외가 생성될 수 있다는 가능성을 호출자에게 경고한다. 호출자는 차례로 `try`문에서 예외를 처리하거나 헤더에 예외를 선언해야 한다.

`Error` 또는 `RuntimeException`의 하위 클래스가 아닌 모든 예외 클래스에는 예외 처리가 필수이다. 이러한 checked exception은 일반적으로 프로그래머가 제어할 수 없는 조건을 나타낸다. 예를 들어 잘못된 입력이나 사용자가 취한 불법적인 작업을 나타낼 수 있다. 이러한 오류를 피할 수 있는 방법은 없으므로 이를 처리할 수 있는 견고한 프로그램을 준비해야 한다. Java 설계에서는 프로그래머가 이러한 오류의 가능성을 무시하는 것이 불가능하다.

checked exception 중에서 Java의 입력/출력 루틴을 사용할 때 발생할 수 있는 몇 가지 예외가 있다. 즉, 예외 처리에 대해 이해하지 못하면 이러한 루틴을 사용할 수도 없다.


## 5. 예외가 있는 프로그래밍

강력한 프로그램을 작성하는 데 예외를 사용할 수 있다. 이는 견고성에 대한 조직적이고 구조화된 접근 방식을 제공한다. 예외가 없으면 가능한 다양한 오류 조건을 테스트하는 `if`문으로 인해 프로그램이 복잡해질 수 있다. 예외를 제외하면 모든 일반적인 경우를 처리하는 알고리즘을 깔끔하게 구현하는 것이 가능해진다. 예외적인 경우는 `try`문의 `catch`절 같이 다른 곳에서 처리될 수 있다.

프로그램에서 예외 상황이 발생하고 이를 즉시 처리할 수 없는 경우 프로그램에서 예외가 발생할 수 있다. 어떤 경우에는 `IllegalArgumentException` 또는 `IOException`과 같은 Java의 사전 정의된 클래스 중 하나에 속하는 예외를 발생시키는 것이 합리적이다. 그러나 예외 조건을 적절하게 나타내는 표준 클래스가 없는 경우 프로그래머는 새 예외 클래스를 정의할 수 있다. 새 클래스는 표준 클래스 `Throwable` 또는 해당 하위 클래스 중 하나를 확장해야 한다. 일반적으로 프로그래머가 필수 예외 처리를 요구하지 않으려면 새 클래스는 `RuntimeException`을 확장한다. 새로운 checked exception를 생성하려면 필수 처리가 필요한 경우 프로그래머는 `Exception`의 하위 클래스를 확장하거나 `Exception` 자체를 확장할 수 있다. 

예를 들어 다음은 `Exception`을 확장하는 클래스이므로 예외 처리가 필수이다.

```java
public class ParseError extends Exception {
    pulbic ParseError(String message) {
        super(message);
    }
}
```

클래스에는 지정된 오류 메시지가 포함된 `ParseError` 객체를 생성할 수 있게 해주는 생성자만 포함되어 있다. `super(message)` 문은 슈퍼 클래스의 생성자 `Exception`을 호출한다. 물론 슈퍼 클래스에서 `getMessage()`, `printStackTrace()` 루틴을 상속한다. `e`가 `ParseError` 유형의 객체를 참조하는 경우 `e.getMessage()` 함수 호출은 생성자에 지정된 오류 메시지를 검색한다. 하지만 `ParseError` 클래스의 핵심은 단순히 존재한다는 것이다. `ParseError` 유형의 객체가 던져지면 특정 유형의 오류가 발생함을 나타낸다. 

프로그램에서 `throw`문을 사용하여 `ParseError` 유형의 오류를 발생시킬 수 있다. `ParseError` 객체의 생성자는 오류 메시지를 지정해야 한다.

```java
throw new ParseError("Encountered an illegal negative number.");
```

또는 

```java
throw new ParseError("The word '" + word + "' is not a valid file name.");
```

`ParseError`는 `Exception`의 하위 클래스로 정의되므로 checked exception이다. 오류를 포착하는 `try`문에서 `throw` 문이 발생하지 않으면 서브 루틴 헤더에 `throws ParseError` 절을 추가하여 에러를 던질 수 있음을 선언해야 한다.

```java
void getUserData() throws ParseError {

}
```

`ParseError`가 `Exception` 대신 `RuntimeException`의 하위 클래스로 정의된 경우 이는 필요하지 않다. 이 경우 `ParseError`는 예외를 확인하지 않기 때문이다.

`ParseError`를 처리하려는 루틴은 `catch` 절과 함께 사용할 수 있다.

```java
try {
    getUserData();
    processUserData();
}
catch (ParseError pe) {
    // handle the error    
}
```

`ParseError`는 `Exception`의 하위 클래스 이므로 `catch`절은 `Exception` 유형의 다른 객체와 함께 포착된다.

```java
class ShipDestroyed extends RuntimeException { 
    Ship ship;  // Which ship was destroyed.
    int where_x, where_y;  // Location where ship was destroyed.
    ShipDestroyed(String message, Ship s, int x, int y) {
        super(message);
        ship = s;
        where_x = x;
        where_y = y;
    }
}
```

여기서 `ShipDestroyed` 객체에는 오류 메시지와 파괴된 선박에 대한 일부 정보가 포함되어 있다. 

```java
if (userShip.isHit()) 
    throw new ShipDestroyed("공격당함", userShip, xPos, yPos);
```

`ShipDestroyed` 객체가 나타내는 조건은 오류로 간주되지 않을 수도 있다. 이는 게임의 정상적인 흐름을 방해할 것으로 예상되는 것일 수도 있다. 때때로 예외를 사용하여 이러한 중단을 깔끔하게 처리할 수 있다.

---

예외를 발생시키는 기능은 둘 이상의 프로그램에서 사용되는 범용 메서드와 클래스를 작성하는 데 특히 유용하다. 이 경우, 메서드나 클래스를 작성하는 사람은 해당 메서드나 클래스가 어떻게 사용될지 정확히 알 수 없기 때뭉네 오류를 처리할 합리적인 방법이 없는 경우가 많다. 이러한 상황에서 프로그래머는 종종 오류 메시지를 인쇄하고 앞서 나가고 싶은 유혹을 느끼지만, 이는 나중에 예측할 수 없는 결과를 초래할 수 있으므로 거의 만족스럽지 않다. 오류 메시지를 인쇄하고 프로그램을 종료하는 것은 프로그램이 오류를 처리할 기회를 주지 않기 때문에 거의 나쁜 일이다.

메서드를 호출하거나 클래스를 사용하는 프로그램은 오류가 발생했음을 알아야 한다. 예외를 지원하지 않은 언어에서 유일한 대안은 일부 특수 값을 반환하거나 일부 전역 변수의 값을 설정하여 오류가 발생했음을 나타내는 것이다. 섹션 8.2.2의 `readMeasurement()` 함수는 사용자의 입력이 잘못된 경우 -1을 반환한다. 그러나 이는 주 프로그램이 반환 값을 테스트하는 데 방해가 되는 경우에만 유용하다. 서브루틴이 호출될 때 마다 특별한 반환 값을 확인하는 것을 게으르게 만드는 것은 매우 쉽다. 그리고 이 경우에는 -1을 사용한다. 오류가 발생했다는 신호로 인해 음수 측정이 불가능해진다. 예외는 오류가 발생할 때 서브루틴이 반응하느 ㄴ더 깔끔한 방법이다.

오류를 알리기 위해 특별한 반환 값 대신 예외를 사용하도록 `readMeasurement()` 함수를 수정하는 것은 쉽ㄴ다. 수정된 서브 루틴은 사용자 입력이 잘못된 경우 `ParseError`를 발생시킨다. 여기서 `ParseError`는 위에 정의된 `Exception`의 하위 클래스이다.

```java
public class A {

    /**
     * 한 줄의 입력에서 사용자의 입력 측정값을 읽습니다.
     * 전제 조건: 입력 라인이 비어 있지 않아야 합니다.
     * 사후조건 : 사용자의 입력이 정당한 경우 측정
     *는 인치로 변환되어 반환됩니다.
     * @throws 사용자의 입력이 올바르지 않으면 @throws ParseError가 발생합니다.
     */
    static double readMeasurement() throws ParseError {
        
        double inches;  // 사용자가 측정한 총 인치 수입니다.
        double measurement;  // 한 번의 측정, "12마일"의 12와 같은 것입니다.
        String units;       // 측정을 위해 지정된 단위, "마일"과 같은 것입니다.
        char ch;  // 사용자 입력에서 다음 문자를 엿보는 데 사용됩니다.
        inches = 0;  // 아직 읽은 인치가 없습니다.

        skipBlanks();
        ch = TextIO.peek();
   
        /* 라인에 더 많은 입력이 있는 한 측정값을 읽고
            해당 인치 수를 변수 인치에 추가합니다. 만약
            루프 중에 오류가 감지되면 즉시 서브루틴을 종료합니다.ParseError를 발생시킵니다 
      . */
        while (ch != '\n') {
            /* 다음 측정값과 단위를 가져옵니다. 읽기 전에
            무엇이든 읽을 수 있는 합법적인 값이 있는지 확인하세요. */
            if ( ! Character.isDigit(ch) ) {
                throw new ParseError("Expected to find a number, but found " + ch);
            }
            measurement = TextIO.getDouble();

            skipBlanks();
            if (TextIO.peek() == '\n') {
                throw new ParseError("Missing unit of measure at end of line.");
            }
            units = TextIO.getWord();
            units = units.toLowerCase();

            /* 측정값을 인치로 변환하여 합계에 추가합니다. */
            if (units.equals("inch")
                    || units.equals("inches") || units.equals("in")) {
                inches += measurement;
            }
            else if (units.equals("foot")
                    || units.equals("feet") || units.equals("ft")) {
                inches += measurement * 12;
            }
            else if (units.equals("yard")
                    || units.equals("yards") || units.equals("yd")) {
                inches += measurement * 36;
            }
            else if (units.equals("mile")
                    || units.equals("miles") || units.equals("mi")) {
                inches += measurement * 12 * 5280;
            }
            else {
                throw new ParseError("\"" + units
                        + "\" is not a legal unit of measure.");
            }
     
            /* 줄의 다음 항목이 다음인지 확인하기 위해 미리 살펴보세요. */
            skipBlanks();
            ch = TextIO.peek();

        } 

        return inches;
    } 
}
```




