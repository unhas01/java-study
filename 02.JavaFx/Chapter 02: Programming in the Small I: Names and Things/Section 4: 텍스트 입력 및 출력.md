# Section 4: 텍스트 입력 및 출력

`System.out.print`와 `System.out.println` 함수로 사용자에게 텍스트를 표시하는 것이 매우 쉽다는 것을 보았다 . 그러나 텍스트 출력이라는 주제에 대해서는 할 말이 더 많다. 게다가, 대부분의 프로그램은 프로그램에 입력되는 데이터를 프로그램에 내장하기보다는 런타임(run time)에 사용한다. 그렇기에 출력뿐만 아니라 입력도 할 줄 알아야 한다. 이 절에서는 사용자로부터 데이터를 얻는 방법에 대해 설명하고 있으며, 지금까지 살펴본 것보다 더 자세히 출력에 대해 다루고 있다. 또한 입력과 출력에 파일을 사용하는 것에 대한 내용도 가지고 있다.

<hr>

## 1. 기본 출력 및 형식화된 출력
가장 기본적인 출력 함수는 `System.out.print(x)`이며, 여기서 `x`는 모든 자료형의 값이나 표현식이 될 수 있다. `x`라는 매개변수가 이미 문자열이 아닌 경우 String 자료형의 값으로 변환된 다음 문자열이 **표준 출력(standard output)** 이라 불리는 목적지(destination)로 출력된다. (일반적으로 이는 문자열이 사용자에게 표시되는 것을 의미한다; 하지만 GUI 프로그램에서는 일반 사용자가 볼 수 없을 법한 위치로 출력한다. 게다가 표준 출력은 다른 출력 목적지로 쓰도록 "재연결될(be redirected)" 수 있다. 그럼에도 불구하고, 현재 우리가 함께 작업하고 있는 프로그램의 종류에 있어서, `System.out`의 목적은 사용자에게 텍스트를 표시하는 것이다.)

`System.out.println(x)`은 `System.out.print`와 동일한 텍스트를 출력하지만, 줄바꿈(line feed)에 의해 해당 텍스트 뒤를 이어가는데, 이는 후속된 출력이 다음 행에 있음을 의미한다. 줄바꿈만 출력하는, `System.out.println()`이라는 매개변수 없는 해당 함수를 사용할 수 있다. `System.out.println(x)`은 다음과 같다는 점에 주의하자:

```java
System.out.print(x);
System.out.println();
```

`System.out.print`는 실수의 소수점 아래 숫자를 가능한 한 출력한다는 점을 인식해왔을 것이다. 예를 들어 π은 3.1415926535899793으로 출력되며, 돈을 나타내야 하는 숫자는 1050.0 또는 43.575로 출력될 수 있다. 예를 들어 3.14159, 1050.00 및 43.58과 같은 숫자의 출력을 선호할 지도 모르겠다. 자바에는 "형식화된 출력(formatted output)"이란 기능이 있어 실수 및 다른 값들이 인쇄되는 방식을 쉽게 제어할 수 있다. 상당한 포맷팅 옵션을 사용 가능하다. 여기서는 가장 단순하고 가장 일반적으로 사용되는 몇 가지 가능성만을 다룰 것이다.

`System.out.printf` 기능은 형식화된 출력을 생성하는 데 사용할 수 있다. ("인쇄 형식(print formatted)"을 의미하는 "printf"라는 이름은 이러한 종류의 출력이 유래된 C 및 C++ 프로그래밍 언어에서 가져온 것이다.) `System.out.printf`는 하나 이상의 매개변수를 사용한다. 첫 번째 매개변수는 출력의 형식을 지정하는 String 이다. 이 매개변수를 **형식 문자열(format string)** 이라고 한다. 나머지 매개변수들은 출력되어야 할 값을 명시한다. 여기 1달러 금액(amount)을 적절한 형식으로 숫자를 인쇄하는 문장이 있는데, 여기서 `amount`는 double 자료형의 변수다.

```java
System.out.printf( "%1.2f", amount );
```

값에 대한 출력 형식은 형식 문자열의 **형식 지정자(format specifier)** 에 의해 주어진다. 이 예제에서 형식 지정자는 %1.2f이다. 형식 문자열(여기서 다루는 간단한 경우)에는 출력될 각각의 값에 대해 하나의 형식 지정자가 포함되어 있다. 일반적인 형식 지정자는 `%d`, `%12d`, `%10s`, `%1.2f`, `%15.8e` 및 `%1.8g`이다. 모든 형식 지정자는 백분율 기호(%)로 시작하고 한 글자로 끝나며, 그 사이에 약간의 추가적인 형식 정보가 있을 수 있다. 글자는 발생할 출력 유형을 명시한다. 예를 들어, `%d` 및 `%12d`에서 "d"는 정수가 쓰여질 것을 명시한다. `%12d`의 "12"는 출력에 사용할 최소 공백 수를 지정한다. 출력될 정수가 12칸 미만을 차지하면 정수 앞에 추가적인 공백이 더해져 총 12칸이 나오도록 한다. 이를 출력이 "12칸의 필드에서 오른쪽 정렬이다"라고 한다. 매우 큰 값은 12칸의 공백에 강제되지는 않는다; 값이 12자리 이상이면, 추가적인 공백은 없이 모든 자릿수가 인쇄된다. `%d` 지정자는 `%1d`과 동일한 것을 의미한다 — 즉, 정수는 필요한 만큼의 공간을 사용하여 인쇄된다. (그런데 "d"는 "십진수(demical)" — 즉 밑이 10 — 를 의미한다. "d"를 "x"로 대체하여 16진수 형식의 정수 값을 출력할 수 있다.)

형식 지정자의 끝에 있는 문자 "s"는 모든 자료형의 값에 사용될 수 있다. 이는 값이 형식화되지 않은 출력에서처럼 기본 형식으로 출력되어야 한다는 것을 의미한다. `%20s`의 "20"과 같은 숫자를 추가하여 문자의 (최소) 수를 명시할 수 있다. "s"는 "문자열(string)"을 의미하며, String 자료형의 값에 대하여 사용할 수 있다. 이는 다른 자료형의 값에도 사용할 수 있다; 이 경우 일반적인 방법으로 해당 값이 String 값으로 변환된다.

double 자료형의 값에 대한 형식 지정자는 더욱 복잡하다. `%1.2f`에서와 같이 "f"는 소수점 뒤에 숫자가 있는 "부동 소수점(floating-point)" 형태로 숫자를 출력하는 데 사용된다. `%1.2f`에서 "2"는 소수점 뒤에 사용할 자릿수를 명시한다. "1"은 출력할 문자의 (최소) 수를 지정한다; "1"은 해당 위치에서 실제로 필요한 수만큼의 문자가 사용되어야 함을 사실상 의미한다. 마찬가지로, `%12.3f`는 12칸의 필드에서 오른쪽 정렬된, 소수점 뒤에 3자리의 수를 지닌 부동 소수점 형식을 명시한다.

"6.00221415 곱하기 10의 23승"을 나타내는 6.00221415e23과 같이 매우 크거나 매우 작은 숫자는 지수 형식으로 써야 한다. `%15.8e`와 같은 형식 지정자는 출력을 지수 형식으로 지정하며, "8"은 소수점 이후 사용할 자릿수를 나타낸다. "e" 대신 "g"를 사용하면 출력은 매우 작은 값과 매우 큰 값의 경우에는 지수 형식이고 다른 값의 경우에는 부동 소수점 형식이 된다. `%1.8g`에서 8은 소수점 앞의 숫자와 소수점 뒤의 숫자를 모두 포함하여 답안의 총 자릿수를 제공한다.

숫자 출력의 경우 형식 지정자에 쉼표(",")가 포함될 수 있으며, 이로 인해 숫자의 자릿수가 그룹으로 분리되어 큰 숫자를 쉽게 읽을 수 있다. 미국에서는 3자리 그룹이 쉼표로 구분된다. 예를 들어 x가 10억일 경우 `System.out.printf("%,d", x)`는 1,000,000,000,000을 출력한다. 다른 국가에서는 구분 문자 및 그룹당 자릿수가 다를 수 있다. 쉼표는 형식 지정자의 시작 부분, 필드 너비의 앞에 와야 한다; 예를 들자면: `%,12.3f`. 출력을 오른쪽이 아닌 왼쪽 정렬로 지정하려면 형식 지정자의 시작 부분에 뺄셈 기호를 추가하라: 예컨대, `%-20s`.

형식 지정자 외에도 `printf`의 형식 문자열에는 다른 문자가 포함될 수 있다. 이 여분의 문자는 출력에 그대로 복사될 뿐이다. 이것은 출력 문자열의 중간에 값을 삽입하는 편리한 방법이 될 수 있다. 예를 들어 x와 y가 int 자료형의 변수라면 다음과 같이 할 수 있다:

```java
System.out.printf("The product of %d and %d is %d", x, y, x*y);
```

이 문장이 실행되면 `x`의 값은 문자열의 첫 번째 `%d`에, `y`의 값은 두 번째 `%d`에, 표현식 `x*y`의 값은 세 번째에서 대체되므로 출력은 "The product of 17 and 42 is 714" (인용 부호는 출력에 포함되지 않는다!)와 같은 것이 된다.

백분율 기호를 출력하려면 형식 문자열에서 형식 지정자 `%%`를 사용하라. `%n`을 사용하여 줄바꿈을 출력할 수 있다. 또한 평소와 같이 문자열에서 백슬래시 `\ `를 사용하여 탭 및 쌍따옴표 문 자와 같은 특수 문자를 출력할 수 있다.

<hr>

## 2. 첫 번째 텍스트 입력 예제

어떤 이해할 수 없는 이유 때문에, 자바는 전통적으로 프로그램의 사용자가 입력한 데이터를 읽는 것을 어렵게 만들었다. 서브루틴 `System.out.print`를 사용하여 사용자에게 출력을 표시할 수 있다는 것을 이미 보았을 것이다. 이 서브루틴은 `System.out`이라고 불리는 미리 정의된 객체의 일부분이다. 이 객체의 목적은 정확하게 사용자에게 출력을 표시하는 것이다. 이에 상응하는 객체로 사용자에 의한 데이터 입력을 읽기 위해서 존재하는 `System.in`이 있지만, 이는 그저 매우 원시적인 입력 설비만을 제공하며, 이를 효과적으로 사용하기 위해서는 다소의 고급 자바 프로그래밍 기술이 필요하다.

자바 5.0은 마침내 새로운 Scanner 클래스로 입력을 조금 더 쉽게 하였다. 하지만 이 클래스를 이용하려면 객체지향 프로그래밍에 대한 지식이 필요하기 때문에, 이 과정의 출발점인 이곳에서는 이를 사용하기가 이상적이진 않다. 자바 6는 사용자와 통신하기 위해 Console 클래스를 도입했지만 Console 자체에 고유한 문제가 있다. (이를 항상 사용할 수 있는 것은 아니며, 숫자가 아닌 문자열만 읽을 수 있다.) 게다가, 필자의 견해로는 Scanner 와 Console 은 여전히 제대로 되어 있지 않다. 그럼에도 불구하고, 지금 이를 사용하기 시작할 경우에 대비하여 이 절의 끝부분에서 Scanner 를 간략히 소개하겠다. 그러나, 필자 고유의 텍스트 입력 버전부터 시작할 것이다.

다행히 언어의 표준 부분에서는 이용할 수 없는 서브루틴을 제공하는 새로운 클래스를 만들어 자바를 **확장(extend)** 할 수 있다. 새로운 클래스를 사용할 수 있게 되면, 클래스에 포함된 서브루틴은 내장된(built-in) 루틴과 정확히 같은 방식으로 사용할 수 있다. 이러한 방식을 따라, 필자는 사용자가 입력한 값을 읽기 위한 서브루틴을 정의하는 TextIO라는 이름의 클래스를 작성했다. 이 클래스의 서브루틴은 Scanner 를 사용하거나 `System.in`을 직접 사용하는 데 필요한 자바의 발전된 측면을 알지 못한 채로, 표준 입력 객체인 `System.in`으로부터 입력을 얻을 수 있게 해준다.

TextIO 는 textio 라는 이름의 "패키지(package)"로 정의된다. 즉 `TextIO.java` 파일을 찾을 때, textio 라는 이름의 폴더 안에서 이를 찾을 수 있다. 게다가, 이는 TextIO 를 사용하는 프로그램은 반드시 textio 패키지에서 이를 "가져와야(import)" 함을 의미한다. 이 작업은 import 명령으로 수행된다.

```java
import textio.TextIO;
```

이 명령은 프로그램을 시작하는 "public class"보다 먼저 나타나야 한다. 자바 표준 클래스의 대부분은 패키지로 정의되어 있으며 이와 같은 방법에 따라 프로그램으로 가져온다.

TextIO 클래스를 사용하려면, 프로그램이 해당 클래스를 사용할 수 있는지 확인해야 한다. 이것은 사용하고 있는 자바 프로그래밍 환경에 달려있다. 일반적으로는 기본 프로그램이 포함되어 있는 폴더와 동일한 곳에 textio 폴더를 추가하기만 하면 된다. 이 폴더에는 TextIO.java 파일이 포함되어 있다. TextIO 사용 방법에 대한 자세한 내용은 제6장을 보라.

TextIO 클래스의 입력 루틴은 정적 멤버 함수(static member function)이다. (정적 멤버 함수는 이전 장에서 소개되었다.) 사용자가 입력한 정수를 프로그램이 읽도록 한다고 가정하자. TextIO 클래스에는 `getlnInt`라는 정적 멤버 함수가 포함되어 있으며 이러한 목적으로 사용할 수 있다. TextIO 클래스에 이 함수가 포함되어 있으므로, 프로그램에서 `TextIO.getlnInt`처럼 이를 참조해야 한다. 해당 함수에는 매개변수가 없으므로 함수에 대한 완전한 호출은 "`TextIO.getlnInt()`" 형식을 취한다. 이 함수 호출은 사용자가 입력한 int 값을 나타내며, 반환된 값을 변수에 할당하는 등의 작업을 수행해야 한다. 예를 들어 `userInput`이 int 자료형의 변수("`int userInput;`" 선언문으로 만들어진)인 경우, 다음 할당문을 사용할 수 있다:

```java
userInput = TextIO.getlnInt();
```

컴퓨터는 해당 문장을 실행할 때, 사용자가 정수 값을 입력할 때까지 대기할 것이다. 프로그램을 계속하려면 사용자가 번호를 입력하고 리턴(return)을 눌러야 한다. 사용자가 입력한 값은 함수에 의해 반환되며 변수 `userInput`에 저장된다. 다음은 `TextIO.getlnInt`를 사용하여 사용자가 입력한 번호를 읽은 다음 해당 번호의 정사각형을 출력하는 전체 프로그램이다. 첫 번째 줄에 있는 `import` 명령에 유의하라:

```java
import textio.TextIO;

/**
* A program that reads an integer that is typed in by the
* user and computes and prints the square of that integer.
  */
  public class PrintSquare {

  public static void main(String[] args) {

       int userInput;  // The number input by the user.
       int square;     // The userInput, multiplied by itself.

       System.out.print("Please type a number: ");
       userInput = TextIO.getlnInt();
       square = userInput * userInput;

       System.out.println();
       System.out.println("The number that you entered was " + userInput);
       System.out.println("The square of that number is " + square);
       System.out.println();

  } // end of main()

} // end of class PrintSquare
```

이 프로그램을 실행하면 "Please type a number:"라는 메시지가 표시되며, 번호 후에 캐리지 리턴(carriage return)을 포함한 응답을 입력할 때까지 프로그램이 중지된다. 입력을 읽기 전에 사용자에게 질문이나 다른 프롬프트(prompt)를 출력하는 것이 좋은 스타일이라는 것을 유의하라. 그렇지 않으면, 사용자는 컴퓨터가 무엇을 기다리고 있는지, 심지어 사용자가 무엇을 하기를 기다리고 있는지 정확히 알 방법이 없을 것이다.

## 3. 기본적인 TextIO 입력 함수
**TextIO**에는 다양한 자료형의 값을 입력하는 다양한 함수들이 포함되어 있다. 가장 많이 사용할 법한 함수들은 다음과 같다:

```java
j = TextIO.getlnInt();     // int 자료형 값 읽기.
y = TextIO.getlnDouble();  // double 자료형 값 읽기.
a = TextIO.getlnBoolean(); // boolean 자료형 값 읽기.
c = TextIO.getlnChar();    // char 자료형 값 읽기.
w = TextIO.getlnWord();    // String 자료형 값인 하나의 "단어(word)" 읽기.
s = TextIO.getln();        // 전체 입력 행을 String 자료형으로 읽기.
```

이러한 문장이 허용되려면 각 할당문 왼쪽의 변수는 이미 선언되어야 하며 오른쪽의 함수에 의해 반환된 것과 동일한 자료형이어야 한다. 이러한 함수에는 매개변수가 없다는 점에 주의하라. 반환되는 값은 프로그램이 실행될 때 사용자가 입력한 프로그램 외부에서 온다. 프로그램에서 사용할 수 있도록 해당 데이터를 "캡처(capture)"하려면 함수의 반환 값을 변수에 할당해야 한다. 그런 다음 변수의 이름을 사용하여 사용자의 입력 값을 참조할 수 있다.

이러한 함수 중 하나를 호출하면 올바른 유형의 허용된 값을 반환할 것이라는 점이 보장된다. 예를 들어 사용자가 허용되지 않는 값을 — 예를 들어, int가 요청되었는데 사용자가 숫자가 아닌 문자로 입력하거나 int 자료형의 변수에 저장할 수 있는 값의 허용된 범위를 벗어나는 숫자를 — 입력으로써 타이핑한 경우, 컴퓨터는 사용자에게 값을 재입력하도록 요청하며, 프로그램은 사용자가 입력한 최초의, 허용되지 않은 값을 절대 보지 않게 된다. `TextIO.getlnBoolean()`의 경우, 사용자는 true, false, t, f, yes, no, y, n, 1, 또는 0 중 하나를 입력하는 것이 허용된다. 게다가, 대문자나 소문자를 사용할 수 있다. 어쨌든 사용자의 입력은 참/거짓 값으로 해석된다. `TextIO.getlnBoolean()`을 사용하여 Yes/No 질문에 대한 사용자의 응답을 읽는 것이 편리하다.

String 자료형을 반환하는 두 가지 입력 함수가 있다는 것을 알 것이다. 첫 번째인 `getlnWord()`는 공백이 아닌 문자로만 구성된 문자열을 반환한다. 이것이 호출되면 사용자가 입력한 임의의 공백과 캐리지 리턴(carriage return)을 건너뛴다. 그 후 다음의 공백이나 캐리지 리턴이 나올 때까지 공백이 아닌 문자를 읽는다. 읽어들인 공백이 아닌 문자로 구성된 String 을 반환하는 것이다. 두 번째 입력 함수인 `getln()`은 단순히 다음의 공백부터 캐리지 리턴까지 포함하여 사용자가 입력한 모든 문자로 구성된 문자열을 반환한다. 이는 입력 텍스트의 전체 행을 얻는 것이다. 캐리지 리턴 자체는 입력 문자열의 일부로 반환되지 않고, 컴퓨터에 의해 읽힌 후 폐기된다. `TextIO.getln()`에 의해 반환된 String 자료형은 문자를 전혀 가지지 않는 "**빈 문자열(empty string)**"일 수도 있다는 점에 유의하라. 사용자가 다른 내용을 먼저 입력하지 않고 단순히 리턴을 누르면 이러한 반환 값을 얻을 수 있다.

`TextIO.getln()`은 값을 읽기 전에 공백이나 개행(end-of-line)을 건너뛰지 **않는다**. 그러나 입력 함수 `getlnInt()`, `getlnDouble()`, `getlnBoolean()` 및 `getlnChar()`는 값을 읽기 전에 입력된 공백과 캐리지 리턴을 생략한다는 점에서 `getlnWord()`처럼 동작한다. 이러한 함수 중 하나가 개행을 건너뛰게 되면, '?'를 출력하여 사용자에게 더 많은 입력이 예정되어 있음을 알린다.

게다가, 사용자가 입력 값 뒤의 행에 여분의 문자들을 입력하면, **행 끝의 캐리지 리턴과 함께 여분의 문자들 모두가 폐기된다**. 프로그램이 다른 입력 함수를 실행하면, 사용자는 이전 행에서 값을 하나 이상 입력했더라도 다른 입력 행에 타이핑을 해야 한다. 사용자의 입력 내용을 무시하는 것이 좋은 생각처럼 여겨지지 않을 수도 있지만, 대부분의 프로그램에서 이렇게 하는 것이 가장 안전한 방식으로 밝혀졌다.

입출력에 대한 TextIO 를 사용하면서, 이제부터 제2장 제2절의 투자 총액을 계산하는 프로그램을 개선할 수 있다. 우리는 투자의 초기 자산과 이자율에 사용자 유형을 적용할 수 있다. 그 결과물은 훨씬 더 유용한 프로그램이다 — 무엇보다, 이를 한 번 이상 실행하는 것이기 때문이다! 이 프로그램은 정확한 형식으로 통화액을 출력하기 위해 형식화된 출력을 사용한다는 점에 유의하라.

```java
import textio.TextIO;

/**
* This class implements a simple program that will compute
* the amount of interest that is earned on an investment over
* a period of one year.  The initial amount of the investment
* and the interest rate are input by the user.  The value of
* the investment at the end of the year is output.  The
* rate must be input as a decimal, not a percentage (for
* example, 0.05 rather than 5).
*/
public class Interest2 {

public static void main(String[] args) {

      double principal;  // The value of the investment.
      double rate;       // The annual interest rate.
      double interest;   // The interest earned during the year.

      System.out.print("Enter the initial investment: ");
      principal = TextIO.getlnDouble();

      System.out.print("Enter the annual interest rate (as a decimal): ");
      rate = TextIO.getlnDouble();

      interest = principal * rate;       // Compute this year's interest.
      principal = principal + interest;  // Add it to principal.

      System.out.printf("The amount of interest is $%1.2f%n", interest);
      System.out.printf("The value after one year is $%1.2f%n", principal);

} // end of main()

} // end of class Interest2
```

(각각의 데이터 자료형에 관한 별개의 입력 루틴이 있는 반면, 모든 유형의 데이터 값을 출력할 수 있는 `System.out.println`이라는 출력 루틴이 왜 하나뿐인지 의문스러울 수 있다. 출력 함수의 경우, 매개변수를 보면 어떤 자료형의 값이 출력되고 있는지를 컴퓨터에게 알릴 수 있다. 그러나 입력 루틴에는 매개변수가 없으므로, 다른 입력 루틴들은 오로지 다른 이름들을 가지는 방법에 의해서만 구별될 수 있다.)

<hr>

## 4. File I/O 소개
`System.out`은 "표준 출력"이라고 알려진 출력 목적지로 출력을 전송한다. 그러나 표준 출력은 하나의 가능한 출력 목적지일 뿐이다. 예를 들어 사용자의 하드 드라이브에 저장된 **파일(file)** 에 데이터를 쓸 수 있다. 물론 이에 대한 장점은 심지어 프로그램이 종료된 후에도 데이터가 파일에 저장되고, 사용자가 파일을 인쇄하며, 다른 사람에게 이메일로 보내고, 다른 프로그램으로 편집하는 등의 작업을 할 수 있다는 것이다. 마찬가지로 `System.in`은 입력 데이터를 위한 하나의 가능한 소스만 가지고 있다.

TextIO 는 파일에 데이터를 쓰고 파일에서 데이터를 읽을 수 있는 기능을 가지고 있다. TextIO 는 출력 함수인 `TextIO.put`, `TextIO.putln`, `TextIO.putf`를 포함한다. 일반적으로 이러한 기능은` System.out.print`, `System.out.println` 및 `System.out.printf`와 동일하게 작동하며, 이들은 상호 교환적이다. 그러나 파일 및 다른 목적지로 텍스트를 출력하는 데도 이들을 사용할 수 있다.

`TextIO.put`, `TextIO.putln` 또는 `TextIO.putf`를 사용하여 출력을 작성하면, **현재 출력 목적지(current output destination)** 로 출력이 전송된다. 일반적으로, 현재 출력 목적지는 표준 출력이다. 그러나 TextIO 에는 현재 출력 목적지를 **변경**하는 데 사용할 수 있는 서브루틴이 있다. 예를 들어 "result.txt"라는 이름의 파일을 쓰려면 다음 문장을 사용할 것이다:

```java
TextIO.writeFile("result.txt");
```

이 문장이 실행된 후, TextIO 출력문의 어떤 출력이든 표준 출력 대신 "result.txt"라는 파일로 전송된다. 해당 파일이 아직 존재하지 않으면 생성될 것이다. 같은 이름의 파일이 이미 있는 경우, 이전 내용은 경고 없이 지워진다는 점에 유의하라!

`TextIO.writeFile`을 호출하면 TextIO 는 파일을 기억하고 `TextIO.put` 또는 다른 출력 함수의 출력을 자동으로 해당 파일로 전송한다. 표준 출력에 쓰는 쪽으로 다시 돌아가고 싶다면, 다음과 같이 호출할 수 있다:

```java
TextIO.writeStandardOutput();
```

다음은 사용자에게 몇 가지 질문을 하고 사용자의 응답을 "profile.txt"라는 파일에 출력하는 간단한 프로그램이다. 이는 예제로써 파일뿐만 아니라 표준 출력에 대한 출력을 위해 TextIO 를 사용하지만, `System.out` 또한 표준 출력에 관한 출력을 위해 사용될 수 있을 것이다.

```java
import textio.TextIO;

public class CreateProfile {

    public static void main(String[] args) {

        String name;     // The user's name.
        String email;    // The user's email address.
        double salary;   // the user's yearly salary.
        String favColor; // The user's favorite color.

        TextIO.putln("Good Afternoon!  This program will create");
        TextIO.putln("your profile file, if you will just answer");
        TextIO.putln("a few simple questions.");
        TextIO.putln();

        /* Gather responses from the user. */

        TextIO.put("What is your name?           ");
        name = TextIO.getln();
        TextIO.put("What is your email address?  ");
        email = TextIO.getln();
        TextIO.put("What is your yearly income?  ");
        salary = TextIO.getlnDouble();
        TextIO.put("What is your favorite color? ");
        favColor = TextIO.getln();

        /* Write the user's information to the file named profile.txt. */

        TextIO.writeFile("profile.txt");  // subsequent output goes to file
        TextIO.putln("Name:            " + name); 
        TextIO.putln("Email:           " + email);
        TextIO.putln("Favorite Color:  " + favColor);
        TextIO.putf( "Yearly Income:   %,1.2f%n", salary);

        /* Print a final message to standard output. */

        TextIO.writeStandardOutput();
        TextIO.putln("Thank you.  Your profile has been written to profile.txt.");

    }

}
```

대부분의 경우에 있어, 출력에 사용할 파일을 사용자가 선택하도록 하고 싶을 것이다. 사용자에게 파일 이름을 입력하도록 요청할 수 있지만, 이는 오류를 범하기 쉬우며(error-prone), 사용자는 파일 대화상자(dialog box)에서 파일을 선택하는 데 더욱 익숙하다. 다음 문장

```java
TextIO.writeUserSelectedFile();
```

은 사용자가 출력 파일을 명시할 수 있도록 하는 전형적인 GUI 파일 선택 대화상자를 연다. 이것은 또한 사용자가 기존 파일을 대체하려고 할 경우 사용자에게 경고할 수 있다는 이점이 있다. 사용자는 파일을 선택하지 않고 대화 상자를 취소할 수 있는 것이다. `TextIO.writeUserSelectedFile`은 boolean 값을 반환하는 함수다. 반환 값은 사용자가 파일을 선택한 경우 `true`이고, 사용자가 대화 상자를 취소한 경우 `false`이다. 실제로 파일에 쓸 것인지 아닌지를 알아야 할 경우 프로그램이 해당 반환 값을 확인할 수 있다.

<hr>

TextIO 는 또한 표준 입력에서 읽는 대신 파일에서 읽을 수 있다. TextIO 의 다양한 "get" 함수에 관한 입력 소스(input source)를 명시할 수 있다. 기본 입력 소스는 표준 입력(standard input)이다. ₩`TextIO.readFile("data.txt")` 문장을 사용하여 "data.txt"라는 이름의 파일에서 읽거나, `TextIO.readUserSelectedFile()`이라고 하여 GUI 스타일 대화상자를 사용하여 사용자가 입력 파일을 선택하도록 할 수 있다. 이 작업을 완료한 후에는, 사용자가 입력하는 대신 파일로부터 모든 입력이 나온다. `TextIO.readStandardInput()`을 사용하여 사용자의 입력을 읽는 쪽으로 돌아갈 수 있다.

프로그램이 표준 입력에서 읽을 때, 사용자는 입력의 오류를 수정할 기회를 얻는다. 프로그램이 파일에서 읽고 있을 때는 이러한 것이 불가능하다. 프로그램에서 파일을 읽으려고 할 때 허용되지 않는 데이터가 발견되면 프로그램을 멈추는 오류가 발생한다. (나중에, 그러한 오류들을 "포착(catch)"하고 복구하는 것이 가능하다는 것을 알게 될 것이다.) 더 드물기는 하지만, 파일에 쓸 때 오류가 발생할 수도 있다.

자바의 입력/출력에 대한 완전한 이해에는 객체 지향 프로그래밍에 대한 지식이 필요하다. 이후 제11장에서 이 주제로 돌아갈 것이다. TextIO 클래스에 있는 파일 I/O 성능은 비교적 원시적이다. 그럼에도 불구하고, 이는 많은 어플리케이션에 충분하며, 파일들에 대한 약간의 경험을 더 빨리 얻을 수 있게 할 것이다.

<hr>

## 5. 기타 TextIO 기능
지금까지 본 TextIO 입력 함수는 입력 행에서 하나의 값만 읽을 수 있다. 그러나, 동일한 입력 행에서 둘 이상의 값을 읽기를 원할 때도 있다. 예를 들어, 사용자가 같은 줄에 두 숫자인 42와 17을 입력하기 위해 "42 17"과 같은 것을 타이핑할 수 있기를 원하는 것이다. TextIO 는 이를 위해 다음과 같은 대안적인 입력 함수를 제공한다:

```java
j = TextIO.getInt();     // int 자료형 값을 읽음.
y = TextIO.getDouble();  // double 자료형 값을 읽음.
a = TextIO.getBoolean(); // boolean 자료형 값을 읽음.
c = TextIO.getChar();    // char 자료형 값을 읽음.
w = TextIO.getWord();    // String 자료형 값을 한 "단어(word)"로 읽음.
```

이러한 함수들의 이름은 "getln" 대신 "get"으로 시작한다. "Getln"은 "get line"의 줄임말로, 이름이 "getln"으로 시작하는 함수는 전체 데이터 행을 소모한다는 것을 상기해야 한다. "ln"이 없는 함수는 입력 값을 같은 방법으로 읽지만, **입력 버퍼(input buffer)** 라고 하는 내부 메모리 덩어리에 입력 행의 나머지 부분을 저장한다. 다음에 컴퓨터가 입력 값을 읽기를 원할 때, 이는 사용자에게 입력을 요구하기 전에 입력 버퍼를 들여다볼 것이다. 이를 통해 컴퓨터는 사용자 입력의 한 행에서 여러 값을 읽을 수 있다. 엄밀히 말하면, 컴퓨터는 실제로 **입력 버퍼에서만** 값을 읽는다. 프로그램이 처음으로 사용자의 입력을 읽으려고 할 때, 컴퓨터는 사용자가 입력의 전체 행에 입력을 하는 동안 대기한다. TextIO 는 ("getln" 함수들 중 하나에 의해) 해당 행의 데이터를 읽거나 폐기할 때까지, 입력 버퍼에 해당 행을 저장한다. 사용자는 버퍼가 비어 있을 때만 입력한다.

그런데, 지적할 것은, TextIO 입력 함수는 입력을 살피는 동안 공백과 캐리지 리턴을 건너뛰는 반면, 다른 문자들을 건너뛰지는 **않는다**는 점이다. 예를 들어, 두 개의 int 자료형을 읽으려고 하고 사용자가 "42,17"을 입력하면, 컴퓨터는 첫 번째 숫자를 정확하게 읽지만 두 번째 숫자를 읽으려고 할 때 쉼표(comma)를 보게 될 것이다. 컴퓨터는 이를 오류로 간주하여 사용자가 번호를 다시 입력하도록 강제할 것이다. 한 행에서 여러 개의 숫자를 입력하려면, 쉼표가 아닌 공백으로 이들을 구분하는지를 사용자가 알고 있는지 확인해야 한다. 그 대신으로, 숫자 사이에 쉼표가 필요하게 하고 싶다면 `getChar()`를 사용하여 두 번째 숫자를 읽기 전에 쉼표를 읽으면 된다.

또 다른 문자 입력 함수인 `TextIO.getAnyChar()`도 있는데, 이는 공백이나 캐리지 리턴을 건너뛰지 않는다. 이는 단순히 사용자가 입력한 다음 문자를 읽고 반환하며, 공백이나 캐리지 리턴이라 하더라도 마찬가지다. 사용자가 캐리지 리턴을 입력한 경우 `getAnyChar()`가 반환하는 문자는 특수 줄바꿈(linefeed) 문자인 '`\n`'이다. 입력시 다음 문자를 실제로 읽지 않고도 미리 볼 수 있는 기능인 `TextIO.peek()`도 있다. 당신이 다음 문자를 "엿보기(peek)"한 후에도, 입력으로부터 다음 항목을 읽을 때 그것은 여전히 거기에 있을 것이다. 이렇게 하면 입력에서 무엇이 나타날 지를 미리 내다보고 알 수 있어서, 거기에 무엇이 있느냐에 따라 다른 행동을 취할 수 있다.

TextIO 클래스는 다른 여러 함수를 제공한다. 이에 대해 자세히 알아보려면 소스 코드 파일인 TextIO.java의 설명을 참조하라.

분명하게도, 입력의 의미론(semantics)은 출력의 의미론보다 훨씬 더 복잡하다! 다행스럽게도, 대부분의 응용프로그램에서 이는 실제로 매우 간단하다. 뭔가 화려한 것을 하려면 세부사항만 따라 하면 된다. 특히, 동일한 입력 행으로부터 여러 항목을 읽어오길 정말로 원하는 것이 아니라면, 입력 루틴의 "get" 버전보다 "getln" 버전을 사용할 것을 **강력하게** 조언한다. "getln" 버전이 의미론적으로 훨씬 단순하기 때문이다.

<hr>

## 6. 입력을 위한 Scanner 사용
TextIO 는 사용자로부터 입력을 쉽게 받을 수 있도록 한다. 하지만, 이는 표준 클래스가 아니기 때문에 `TextIO.java`를 사용하는 모든 프로그램에서 이것이 구비되도록 해야 한다는 사실을 기억해야 한다. 입력에 대한 또 다른 선택지는 Scanner 클래스다. Scanner 를 사용하는 한 가지 장점은 그것이 자바의 표준 부분이고 필요할 때 항상 있다는 것이다.

사용자 입력에 Scanner 를 사용하는 것은 그리 어렵지 않고, 몇 가지 좋은 기능도 있지만, Scanner 를 사용하는 것은 제4장과 제5장이 되어서야 소개될 일부 구문을 필요로 한다. 이것이 왜 작동하는지는 설명하지 않고, 여기서는 이것을 어떻게 하는지를 언급할 것이다. 이 시점에서는 모든 구문을 이해하지는 못할 것이다. (Scanner 에 대해서는 제11장 제1절 제5관에서 더 자세히 다룰 것이다.)

우선, Scanner 는 패키지 `java.util`에 정의되어 있으므로, 프로그램 소스 코드 파일의 시작 부분, "public class..." 이전에 가져오기(import) 명령어를 추가해야 한다:
```java
import java.util.Scanner;
```

그런 다음 `main()` 루틴의 시작 부분에 다음 문장을 포함시켜라:

```java
Scanner stdin = new Scanner( System.in );
```

이로써 Scanner 자료형의 `stdin`이라는 변수가 생성된다. (원한다면 해당 변수에 대한 다른 이름을 사용할 수 있다; "stdin"은 "표준 입력(standard input)"을 의미한다.) 그런 다음 프로그램에서 `stdin`을 사용하여 사용자 입력을 읽기 위한 다양한 서브루틴에 접근할 수 있다. 예를 들면, `stdin.nextInt()` 함수는 사용자로부터 int 자료형의 값 하나를 읽어서 이를 반환한다. 이는 다음 두 가지를 제외하고는 `TextIO.getInt()`와 거의 동일하다: 사용자가 입력한 값이 허용된 int가 아닌 경우, `stdin.nextInt()`는 사용자에게 값을 다시 입력하라는 메시지를 표시하기보다는 작동을 멈춘다. 그리고 사용자가 입력한 정수는 공백이나 개행(end-of-line)이 반드시 따라야 하는 반면, `TextIO.getInt()`는 숫자가 아닌 어떤 문자라도 읽기를 중지할 것이다.

`stdin.nextDouble()`, `stdin.nextLong()` 및 `stdin.nextBoolean()`을 포함하여, 다른 자료형의 데이터를 읽는 일에 대응하는 메서드들이 있다. (`stdin.nextBoolean()`은 "true" 또는 "false"만 입력으로 받아들인다.) 이러한 서브루틴은 한 행에서 둘 이상의 값을 읽을 수 있으므로, TextIO 서브루틴의 "getln" 버전들보다는 "get" 버전들과 더 유사하다. `stdin.nextLine()` 메서드는 `TextIO.getln()`과 동일하며, `stdin.next()`는 `TextIO.getWord()`와 마찬가지로 공백이 아닌 문자열을 반환한다.

간단한 예로써, 사용자 입력에 있어 TextIO 대신 Scanner 를 사용하는 버전의 샘플 프로그램 Interest2.java 가 있다:

```java
import java.util.Scanner;

public class Interest2WithScanner {

public static void main(String[] args) {

      Scanner stdin = new Scanner( System.in );  // Create the Scanner.

      double principal;  // The value of the investment.
      double rate;       // The annual interest rate.
      double interest;   // The interest earned during the year.

      System.out.print("Enter the initial investment: ");
      principal = stdin.nextDouble();

      System.out.print("Enter the annual interest rate (as a decimal): ");
      rate = stdin.nextDouble();

      interest = principal * rate;       // Compute this year's interest.
      principal = principal + interest;  // Add it to principal.

      System.out.printf("The amount of interest is $%1.2f%n", interest);
      System.out.printf("The value after one year is $%1.2f%n", principal);

} // end of main()

} // end of class Interest2WithScanner
```

`Scanner`를 가져오고(import) `stdin`을 생성하기 위해 위에 주어진 두 행이 포함된 것을 지적하고자 한다. 또한 `TextIO.getlnDouble()`을 `stdin.nextDouble()`로 대체한 것에 유의하라. (사실 `stdin.nextDouble()`은 "getln" 버전이라기보다 `TextIO.getDouble()`과 정말로 동등하지만, 사용자가 각각의 입력 행에 한 개의 숫자만 입력하는 한 프로그램의 동작에 영향을 미치지는 않을 것이다.)

당분간은 계속하여 입력을 위한 TextIO 를 제공할 것이지만, 이 장의 끝에 있는 연습문제에 대한 온라인 해답편에서 Scanner 를 사용하는 몇 가지 예제를 제시할 것이다. Scanner 에 대한 자세한 내용은 이 교재의 뒷부분에서 다룬다.