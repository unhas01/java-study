# Section 3: 문자열, 클래스, 객체 및 서브루틴

이전의 절에서는 8가지 원시 데이터 자료형과 String 자료형을 소개했다. 원시 자료형과 String 사이에는 근본적인 차이가 있다: String 자료형의 값은 객체다. 제5장 전까지는 객체를 자세히 학습하진 않겠지만, 그것들에 대해 조금이나마 알고, 이들과 밀접하게 관련된 주제인 클래스를 알아보는 것은 유용할 것이다. 이는 단순히 문자열이 유용하기 때문이 아니라, 객체와 클래스가 또 다른 중요한 프로그래밍 개념인 서브루틴을 이해하는 데 필수적이기 때문이다.

## 1. 내장 서브루틴 및 함수
서브루틴(subroutine)은 함께 뭉쳐져(chunked) 이름이 주어진 일련의 프로그램 명령어라는 것을 상기하라. 서브루틴은 어떤 작업을 수행하도록 설계되었다. 프로그램에서 작업을 수행하려면 서브루틴 호출문을 사용하여 서브루틴을 "호출(call)"할 수 있다. 제4장에서는 자기만의 서브루틴 작성법을 배우겠지만, 이미 작성된 서브루틴을 호출하는 것만으로도 프로그램에서 많은 일을 할 수 있다. 자바에서는 모든 서브루틴이 클래스나 객체에 포함되어 있다. 자바 언어의 표준 부분인 일부 클래스에는 사용 가능한, 미리 정의된 서브루틴이 포함되어 있다. String 자료형의 값은 객체로서 그에는 해당 문자열을 조작하는 데 사용할 수 있는 서브루틴이 포함되어 있다. 이 서브루틴들은 자바 언어로 "내장(built into)"된다. 이들이 어떻게 작성됐는지, 어떻게 작동하는지 이해하지 않고도 이 모든 서브루틴을 호출할 수 있다. 정말로 이것이 서브루틴의 핵심이다: 서브루틴은 안에서 무슨 일이 벌어지는지 모르게 사용할 수 있는 "블랙박스"이다.

먼저 클래스의 일부인 서브루틴을 고려해보자. 클래스의 목적 중 하나는 해당 클래스에 포함된 일부 변수와 서브루틴을 그룹화하는 것이다. 이러한 변수와 서브루틴을 클래스의 **정적 멤버(static members)** 라고 한다. 한 가지 예를 보았을 것이다: 프로그램을 정의하는 클래스에서 `main()` 루틴은 클래스의 정적 멤버다. 정적 멤버를 정의하는 클래스 정의 부분은 `public static void main...` 에 있는 단어 "static"과 같이 예약어 "`static`"으로 표시된다.

클래스에 정적 변수 또는 서브루틴이 포함된 경우, 클래스 이름은 변수 또는 서브루틴의 전체 이름의 일부가 된다. 예를 들어, System이라는 이름의 표준 클래스는 `exit`라는 이름의 서브루틴을 포함한다. 프로그램에서 해당 서브루틴을 사용하려면 `System.exit`로 참조해야 한다. 이 전체 이름은 서브루틴을 포함하는 클래스 이름, 그 다음으로 마침표를 포함하는 클래스 이름, 그 다음으로 서브루틴의 이름으로 구성된다. 이 서브루틴은 그 매개변수로 정수를 필요로 하기 때문에, 당신은 실제로 다음과 같은 서브루틴 호출문과 함께 그것을 사용한다:

```java
System.exit(0);
```

`System.exit`을 호출하면 프로그램이 종료되고 자바 가상 기계가 종료된다. `main` 루틴이 끝나기 전에 프로그램을 종료해야 할 이유가 있다면 이를 사용할 수 있다. (매개변수는 프로그램이 종료된 이유를 컴퓨터에 알려준다. 매개변수 값이 0이면 프로그램이 정상적으로 종료되었음을 나타낸다. 다른 값은 오류가 감지되어 프로그램이 종료되었음을 나타내므로, `System.exit(1)`로 호출하여 오류로 인해 프로그램이 종료되고 있음을 나타낼 수 있다. 매개변수는 운영체제로 다시 전송된다; 실제로는, 이 값은 대개 운영체제에 의해 무시된다.)

System 은 자바와 함께 제공되는 많은 표준 클래스 중 하나에 불과하다. 또 다른 유용한 클래스로 Math 를 꼽을 수 있다. 이 클래스는 정적 변수를 포함하는 클래스의 예시를 제시한다: 여기에는 수학 상수 π과 e의 값을 지닌 변수 `Math.PI`와 `Math.E`를 포함한다. Math 는 또한 많은 수의 수학적인 "기능(function)"을 포함한다. 모든 서브루틴은 특정한 작업을 수행한다. 일부 서브루틴의 경우 이 작업은 일부 데이터 값을 계산하거나 검색하는 것이다. 이런 방식의 서브루틴을 **함수(function)** 라고 한다. 우리는 함수가 **값을 반환**한다(returns a value)고 말한다. 일반적으로 반환되는 값은 함수를 호출하는 프로그램에서 어떻게든 사용되도록 되어 있다.

당신은 숫자의 제곱근을 계산하는 수학 함수에 익숙하다. 자바에서 이에 대응하는 함수를 `Math.sqrt`라고 한다. 이 함수는 Math 라는 클래스의 정적 멤버 서브루틴이다. x가 숫자 값이면 `Math.sqrt(x)`는 해당 값의 제곱근을 계산하여 반환한다. `Math.sqrt(x)`는 값을 나타내기 때문에, 다음과 같은 서브루틴 호출문에서 그 자체를 줄에 놓는 것은 타당하지 않는다.

```java
Math.sqrt(x); // 이건 말이 안 된다!
```

결국 이 경우에 함수에 의해 계산된 값으로 컴퓨터가 무엇을 해야 할까? 컴퓨터에게 그 값으로 뭔가를 하라고 말해야 한다. 컴퓨터에게 다음과 같이 이를 표시하도록 지시할 수 있을 것이다:

```java
System.out.print( Math.sqrt(x) );  // x의 제곱근을 표시
```

또는 할당문을 사용하여 시스템에 해당 값을 변수에 저장하도록 지시할 수 있다:

```java
lengthOfSide = Math.sqrt(x);
```

`Math.sqrt(x)` 함수 호출은 double 자료형의 값을 나타내며, 이를 double 자료형의 숫자 리터럴을 사용할 수 있는 어느 곳에서든 사용할 수 있다. 이 공식의 `x`
는 서브루틴에 대한 매개변수를 나타낸다; x는 "x"라는 이름의 변수가 될 수도 있고, 숫자 값을 나타내는 표현으로 대체될 수도 있다. 예를 들어, `Math.sqrt(2)`는 2의 제곱근을 계산하고, `Math.sqrt(a*a+b*b)`는 a와 b가 숫자 변수인 한 허용될 것이다.

Math 클래스에는 많은 정적 멤버 함수가 포함되어 있다. 다음은 이들 중 가장 중요한 몇 가지의 목록이다:

- x의 절대값을 계산하는 `Math.abs(x)`
- 일반적인 삼각함수인 `Math.sin(x)`, `Math.cos(x)` 및 `Math.tan(x)`. (모든 삼각함수의 경우 각도는 도(degree)가 아닌 라디안(radian)으로 측정된다.)
- 역삼각함수인 arcsin, arccos 및 arctan으로, 다음과 같이 표기된다: `Math.asin(x)`, `Math.acos(x)`, `Math.atan(x)`. 반환값은 도가 아닌 라디안으로 표현된다.
- e를 x승한 숫자를 계산하기 위한 지수 함수인 `Math.exp(x)`와 밑이 e인 x의 로그값을 계산하기 위한 자연로그함수 `Math.log(x)`.
- x의 y승을 계산하기 위한 `Math.pow(x,y)`.
- x보다 작거나 같은 가장 가까운 정수 값으로 x를 반올림하는 `Math.floor(x)`. 반환값은 수학적으로는 정수임에도 불구하고 예상한 대로 int 자료형이 아니라 double 자료형의 값으로 반환된다. 예를 들어, `Math.floor(3.76)`는 3.0이고, `Math.floor(-4.2)`는 -5이다. `Math.round(x)` 함수는 x에 가장 가까운 정수를 반환하고, `Math.ceil(x)`은 x를 정수로 반올림한다.("Ceil"은 "바닥(floor)"의 반대인 "천장(ceiling)"의 줄임말이다.)
- `Math.random()`은 0.0 <= `Math.random()` < 1.0을 만족하는 범위 내의 랜덤하게 선택된 double 자료형 값을 반환한다. (컴퓨터는 실제로는 진정한 무작위는 아니지만 대부분의 목적에는 충분히 효과적인 무작위인 이른바 "유사 랜덤(pseudorandom)" 숫자를 계산한다.) 이후의 예제에서 `Math.random`의 많은 사용례를 보게 될 것이다.

이러한 함수의 경우 매개변수의 자료형 — 괄호 안의 x 또는 y — 은 어느 숫자 자료형의 어느 값이든 될 수 있다. 대부분의 함수의 경우, 함수에 의해 반환되는 값은 매개변수의 자료형에 관계없이 double 자료형이다. 그러나 `Math.abs(x)`의 경우 반환되는 값은 x와 동일한 자료형이다; x가 int 자료형인 경우 `Math.abs(x)`도 마찬가지다. 예를 들어, `Math.sqrt(9)`가 double 자료형의 값 3.0인 반면, `M`ath.abs(9)`는 int 자료형의 값인 9이다.

`Math.random()`에는 매개변수가 없다는 점에 유의하라. 괄호 사이에는 아무 것도 없는데도 불구하고, 여전히 괄호는 필요하다. 괄호는 이것이 변수가 아닌 서브루틴임을 컴퓨터에게 알려준다. 매개변수가 없는 서브루틴의 다른 예로는 System 클래스의 `System.currentTimeMillis()` 함수를 들 수 있다. 이 함수가 실행되면 표준화된 기준 시간(standardized base time) (정히 궁금하다면 1970년이 시작된 때이다) 이후 경과한 밀리초 수로 표시되는 현재 시간을 가져온다. 1밀리초는 1000분의 1초다. `System.currentTimeMillis()`의 반환값은 long 자료형(64비트 정수)이다. 이 기능은 컴퓨터가 작업을 수행하는 데 걸리는 시간을 측정하는 데 사용할 수 있다. 작업이 시작된 시간과 작업이 완료된 시간을 기록하고 차이점을 취하기만 하면 된다. 보다 정확한 타이밍을 위해 `System.nanoTime()`을 대신 사용할 수 있다. `System.nanoTime()`은 임의의 시작 시간 이후의 나노초 수를 반환하는데, 여기서 1나노초는 10억분의 1초다. 그러나 나노초까지의 시간이 정말 정확할 것으로 기대해서는 안 된다.

여기 몇 가지 수학적 과제를 수행하고 프로그램이 실행되는 데 걸리는 시간을 보고하는 샘플 프로그램이 있다.

```java
/**
* This program performs some mathematical computations and displays the
* results.  It also displays the value of the constant Math.PI.  It then
* reports the number of seconds that the computer spent on this task.
  */
  public class TimedComputation {

  public static void main(String[] args) {

       long startTime; // Starting time of program, in nanoseconds.
       long endTime;   // Time when computations are done, in nanoseconds.
       long compTime;  // Run time in nanoseconds.
       double seconds; // Time difference, in seconds.

       startTime = System.nanoTime();

       double width, height, hypotenuse;  // sides of a triangle
       width = 42.0;
       height = 17.0;
       hypotenuse = Math.sqrt( width*width + height*height );
       System.out.print("A triangle with sides 42 and 17 has hypotenuse ");
       System.out.println(hypotenuse);

       System.out.println("\nMathematically, sin(x)*sin(x) + "
               + "cos(x)*cos(x) - 1 should be 0.");
       System.out.println("Let's check this for x = 100:");
       System.out.print("      sin(100)*sin(100) + cos(100)*cos(100) - 1 is: ");
       System.out.println( Math.sin(100)*Math.sin(100) 
               + Math.cos(100)*Math.cos(100) - 1 );
       System.out.println("(There can be round-off errors when" 
               + " computing with real numbers!)");

       System.out.print("\nHere is a random number:  ");
       System.out.println( Math.random() );

       System.out.print("\nThe value of Math.PI is ");
       System.out.println( Math.PI );

       endTime = System.nanoTime();
       compTime = endTime - startTime;
       seconds = compTime / 1000000000.0;

       System.out.print("\nRun time in nanoseconds was: ");
       System.out.println(compTime);
       System.out.println("(This is probably not perfectly accurate!");
       System.out.print("\nRun time in seconds was:  ");
       System.out.println(seconds);

  } // end main()

} // end class TimedComputation
```

## 2. 클래스 및 객체
클래스는 정적 변수 및 서브루틴용 용기(container)가 될 수 있다. 그러나 클래스는 다른 목적도 가지고 있다. 이들은 객체를 묘사하는 데 사용된다. 이러한 점에서, 클래스는 int와 double이 자료형인 것과 같은 방식으로 **자료형**이다. 즉, 클래스 이름을 사용하여 변수를 선언할 수 있다. 그러한 변수는 오직 한 가지 자료형의 값만 포함할 수 있다. 이 경우의 값은 **객체(object)** 이다. 객체는 변수와 서브루틴의 모음(collection)이다. 모든 객체는 객체가 어떤 "자료형"인지를 알려주는 관련 클래스를 가지고 있다. 객체의 클래스는 객체가 포함하는 서브루틴과 변수를 명시한다. 동일한 클래스에 의해 정의된 모든 객체는 유사한 변수 및 서브루틴 모음들을 보유한다는 점에서 유사하다. 예를 들어, 객체는 평면에서 점을 나타낼 수 있으며, 그 점의 좌표를 나타내기 위해 x와 y라는 변수를 포함할 수 있다. 모든 점 객체는 x와 y를 가지지만, 다른 점들은 이러한 변수에 대한 다른 값을 가질 것이다. 예를 들어 Point 라는 클래스가 모든 점 객체의 공통 구조를 정의하기 위해 존재할 수 있으며, 그러한 모든 객체는 Point 자료형의 값이 된다.

다른 예로 `System.out.println`을 다시 살펴보자. System 은 클래스, `out`은 해당 클래스 내의 정적 변수다. 하지만, `System.out`의 값은 **객체**이고, `System.out.println`은 실제로 객체 `System.out`에 포함되어 있는 서브루틴의 전체 이름이다. 현 시점에서는 이를 이해할 필요가 없지만, `System.out`에 의해 지시되는 객체는 PrintStream 클래스의 객체이다. PrintStream 은 자바의 또 다른 클래스로 자바의 표준 부분이다. PrintStream 자료형의 객체는 어느 것이든 정보가 출력될 수 있는 목적지(destination)이다; PrintStream 자료형의 **모든 객체**는 그 목적지로 정보를 전송하는 데 사용할 수 있는 `println` 서브루틴을 가지고 있다. 객체 `System.out`은 단지 하나의 가능한 목적지일 뿐이고, `System.out.println`은 그 특정한 목적지로 정보를 보내는 서브루틴이다. PrintStream 자료형의 다른 객체는 파일이나 네트워크를 통해 다른 컴퓨터로 정보를 전송할 수 있다. 이것이 객체 지향 프로그래밍이다: 공통점이 있는 많은 다른 것들 — 이들은 모두 출력(output)의 목적지로 사용될 수 있음 — 은 `println` 서브루틴을 통해 모두 같은 방식으로 사용될 수 있다. PrintStream 클래스는 이러한 모든 객체 간의 공통점(commonalities)을 표현한다.

클래스의 이중적인 역할로 혼란스러울 수 있으며, 실제로 대부분의 클래스는 두 가지 가능한 역할 중 한 가지만을 주로 또는 단독으로 수행하도록 설계된다. 다행히도 제5장에서 좀 더 진지한 방법으로 객체를 다루기 시작할 때까지는 이들에 대해 너무 걱정할 필요가 없을 것이다.

그런데 클래스 이름과 변수 이름이 비슷한 방식으로 사용되기 때문에 어느 것이 어떤 것인지 구별하기가 어려울 수도 있다. 자바에서 미리 정의된, 모든 내장된 이름은 클래스 이름은 대문자로 시작하는 반면 변수 이름은 소문자로 시작한다는 규칙을 따른다는 점을 기억하자. 이는 공식적인 구문 규칙은 아니지만, 필자는 당신의 프로그래밍에서 이를 따를 것을 강력히 권한다. 서브루틴 이름 또한 소문자로 시작해야 한다. 프로그램의 서브루틴 이름은 항상 왼쪽 괄호가 따르기 때문에 변수를 서브루틴과 혼동할 가능성은 없다.

마지막으로 일반적인 지적 사항으로서, 자바의 서브루틴을 종종 **메서드(method)** 라고 언급한다는 것을 알아야 한다. 일반적으로 "메서드"란 클래스나 객체에 포함된 서브루틴을 말한다. 이는 자바의 모든 서브루틴에 해당되기 때문에, 자바의 모든 서브루틴은 메서드이다. 이는 다른 프로그래밍 언어에 있어서 똑같이 적용되지는 않기에, 당분간은 좀 더 일반적인 용어인 "서브루틴"을 사용하는 것을 필자는 선호할 것이다. 그러나, 몇몇 사람들은 처음부터 "메서드"이라는 용어 사용을 선호한다는 것을 알아야 한다.

## 3. 문자열 작업
String 은 클래스이고 String 자료형의 값은 객체이다. 그 객체에는 데이터, 즉 문자열을 만드는 문자들의 나열(the sequence of characters)이 들어 있다. 또한 서브루틴도 가지고 있다. 이 모든 서브루틴은 사실상 함수이다. 예를 들어, 모든 문자열 객체에는 해당 문자열의 문자 수를 계산하는 `length`라는 함수가 포함되어 있다. `advice` 변수는 String 을 참조한다고 가정하자. 예를 들어 다음과 같이 `advice`가 선언되고 값이 할당되었을 것이다:

```java
String advice;
advice = "Seize the day!";
```

그러면 `advice.length()`는 문자열 "Seize the day!"의 문자들의 수를 반환하는 함수 호출이다. 이 경우, 반환값은 14가 될 것이다. 일반적으로 String 자료형인 어떤 변수 `str`에 대해 `str.length()` 값은 그 문자열의 문자 수와 동일한 int 자료형이다. 이 함수에는 매개변수가 없다는 점에 유의하라; 길이가 계산되는 특정 문자열은 `str`의 값이다. `length` 서브루틴은 String 클래스에 의해 정의되며 String 자료형이라면 어떤 값이든 이를 함께 사용할 수 있다. 심지어 String 리터럴과 함께 사용될 수도 있는데, 이는 결국 String 자료형의 상수 값일 뿐이다. 예를 들어, 다음과 같이 말하여 "Hello World"에 나오는 문자들을 세는 프로그램을 가질 수 있다:

```java
System.out.print("The number of characters in ");
System.out.print("the string \"Hello World\" is ");
System.out.println( "Hello World".length() );
```

String 클래스는 많은 함수를 정의한다. 유용하다고 생각할 수 있는 몇 가지 것들이 여기에 있다. s1 및 s2가 String 자료형의 변수라고 가정한다:

- `s1.equals(s2)`는 부울 값을 반환하는 함수다. `s1`이 `s2`와 정확히 같은 문자들의 나열로 구성되면 `true`를 반환하고, 그렇지 않으면 `false`를 반환한다.
- `s1.equalsIgnoreCase(s2)`는 `s1`이 `s2`와 동일한 문자열인지 확인하는 또 다른 부울 값 함수지만, 이 함수는 대문자와 소문자를 동등하게 간주한다. 따라서 `s1`이 "cat"이면 `s1.equalsIgnoreCase("Cat")`는 `true`인 반면, `s1.equals("Cat")`은 `false`이다.
- `s1.length()`는 위에서 언급한 바와 같이 s1의 문자 수를 부여하는 정수 값 함수다.
- `s1.charAt(N)`은 N이 정수인 경우 char 자료형의 값을 반환한다. 문자열에서 N번째 문자를 반환한다. 위치는 0부터 번호가 매겨지기 때문에 `s1.charAt(0)`는 실제로 첫 번째 문자, `s1.charAt(1)`는 두 번째 문자, 이런 식이다. 마지막 위치는 `s1.length() - 1`이다. 예를 들어 `"cat".charAt(1)`의 값은 'a'이다. 매개변수 값이 0보다 작거나 `s1.length()`보다 크거나 같으면 오류가 발생한다.
- `s1.substring(N,M)`은 N과 M이 정수인 경우 String 자료형의 값을 반환한다. N, N+1, ..., M-1에 위치한 `s1`의 문자들로 구성된 값이 반환된다. M 위치의 문자는 포함되지 않는다는 점에 유의하라. 반환된 값은 `s1`의 하위 문자열(substring)이라고 한다. 서브루틴 `s1.substring(N)`은 N 위치에서 시작하여 문자열 끝까지의 문자들로 이루어진 s1의 하위 문자열을 반환한다.
- `s1.indexOf(s2)`는 정수를 반환한다. `s2`가 `s1`의 하위 문자열로 나타나면 반환된 값은 해당 하위 문자열의 시작 위치가 된다. 그렇지 않으면 반환되는 값은 -1이다. `s1.indexOf(ch)`를 사용하여 `s1`에서 char 자료형 `ch`를 검색할 수도 있다. 위치 N 또는 그 이후 x가 처음 나타나는 것을 찾으려면 `s1.indexOf(x,N)`를 사용하라. `s1`에서 `x`가 마지막으로 나타나는 것을 찾으려면 `s1.lastIndexOf(x)`를 사용하라.
- `s1.compareTo(s2)`는 두 문자열을 비교하는 정수 값 함수다. 문자열이 같으면 반환되는 값은 0이다. `s1`이 `s2`보다 작으면 반환되는 값은 0보다 작은 숫자, `s1`이 `s2`보다 크면 반환되는 값은 0보다 큰 숫자이다. 또한 `s1.compareToIgnoreCase(s2)` 기능도 있다(두 문자열 모두 소문자로 완전히 구성되거나 대문자로 완전히 구성된 경우, "보다 작음"과 "보다 큼"은 알파벳 순서를 가리킨다. 그렇지 않으면 순서가 더 복잡해지기 때문이다.)
- `s1.toUpperCase()`는 `s1`의 소문자가 대문자로 변환되었다는 점을 제외하면 s1과 동일한 새 문자열을 반환하는 String 값 함수다. 예를 들어 `"Cat".toUpperCase()`는 "CAT" 문자열이다. `s1.toLowerCase()` 함수도 있다.
- `s1.trim()`은 공백이나 탭과 같은 비인쇄 문자가 문자열의 처음과 끝에서 잘린(trimmed) 점을 제외하면 `s1`과 동일한 새 문자열을 반환하는 String 값 함수다. 따라서 `s1`의 값이 "fred "인 경우 `s1.trim()은` 끝에 있는 공백이 제거된 "fred" 문자열이다.

`s1.toUpperCase()`, `s1.to.LowerCase()` 및 `s1.trim()` 함수의 경우 `s1`의 값은 변경되지 않는다는 점에 유의하라. 대신 새로운 문자열이 생성되어 함수의 값으로 반환된다. 반환된 값은 예를 들어 `"SmallLetters = s1.toLowerCase();"`와 같은 할당문에서 사용할 수 있다. `s1` 값을 변경하려면 `"s1 = s1.toLowerCase()"`처럼 할당문을 사용할 수 있다.

<hr>

여기에 문자열에 대한 또 다른 매우 유용한 점이 있다: 덧셈 연산자 `+`를 사용하여 두 문자열을 **연결(concatenate)** 할 수 있다. 두 문자열의 연결은 첫 번째 문자열의 모든 문자와 두 번째 문자열의 모든 문자로 구성된 새로운 문자열이다. 예를 들어, "Hello" + "World"는 "HelloWorld"로 판단한다. (당연하지만 공백에 대해 주의하라 — 연결 문자열의 공백을 원한다면 "Hello " + "World"처럼 입력 데이터의 어딘가에 공백이 있어야 한다.)

`name`이 String 자료형의 변수이며 프로그램을 사용하는 사람의 이름을 이미 참조하고 있다고 가정하자. 그러면 프로그램은 다음 문구를 실행하여 사용자를 맞이할 수 있다:

```java
System.out.println("Hello, " + name + ". Pleased to meet you!");
```

더욱 놀라운 것은 `+` 연산자를 사용하여 **어느** 자료형의 값도 실제로 String 에 연결할 수 있다는 것이다. 표준 출력(standard output)으로 이를 출력하는 것과 마찬가지로, 해당 값은 문자열로 변환되고 해당 문자열은 다시 다른 문자열과 연결된다. 예를 들어 "Number" + 42라는 표현식은 "Number42" 문자열로 판단된다. 그리고 다음 문장들

```java
System.out.print("After ");
System.out.print(years);
System.out.print(" years, the value is ");
System.out.print(principal);
```

은 다음과 같은 단일 문장으로 대체될 수 있다:

```java
System.out.print("After " + years +
" years, the value is " + principal);
```

명백하게도, 매우 편리하다. 이는 이 장의 앞 부분에서 제시된 예시들 중 일부를 짧아지게 할 것이다.

<hr>

## 4. 열거형 소개
자바에는 8개의 원시 자료형이 내장되어 있으며 String 과 같은 클래스로 정의되는 방대한 자료형의 모음이 있다. 그러나 이렇게 많은 자료형의 집합도 프로그래머가 처리해야 할법한 모든 가능한 상황을 다루기에는 충분하지 않다. 그래서 거의 다른 프로그래밍 언어와 마찬가지로, 자바의 본질적인 부분은 **새로운** 자료형을 창조하는 능력이다. 대부분의 경우, 이것은 새로운 클래스를 정의함으로써 이루어진다; 제5장에서 그것을 하는 방법을 배울 것이다. 그러나 여기서 한 가지 특별한 경우를 살펴보겠다: 즉 **열거형(enums)** (**열거된 자료형**의 줄임말)을 정의할 수 있는 능력이다.

엄밀히 말하면 열거형은 특별한 종류의 클래스로 간주되지만, 현재로서는 그것이 중요하지 않다. 이 절에서 우리는 단순화된 형태로 열거형을 살펴볼 것이다. 실제로 대부분의 열거형 사용은 여기에서 제시된 단순화된 형식만을 필요로 할 것이다.

enum은 가능한 값의 고정된 목록을 가진 자료형으로, 열거형이 생성될 때 명시된다. 어떤 면에서 enum은 유일하게 가능한 값으로 `true`와 `false`을 갖는 boolean 데이터 자료형과 유사하다. 그러나 boolean은 원시적인 자료형인 반면 enum은 그렇지 않다.

enum 자료형의 정의는 다음과 같은 (단순화된) 형식을 갖는다:

`enum enum 타입 이름 { enum 값의 리스트 }`

이 정의는 서브루틴 내부에 있을 수 없다. 프로그램의 `main()` 루틴 **외부에** 이를 배치할 수 있다(또는 별도의 파일로 가능하다). {enum 타입 이름}은 어떤 간단한 식별자도 가능하다. 이 식별자는 "boolean"이 boolean 자료형의 이름이고 "String"이 String 자료형의 이름인 것과 마찬가지로 enum 자료형의 이름이 된다. {enum 값의 리스트}의 각각의 값은 반드시 간단한 식별자여야 하며, 목록의 식별자들은 쉼표(comma)로 구분된다. 예를 들어, 다음은 Season 이라는 이름의 enum 자료형의 정의로, 이 자료형의 값들은 한 해 4계절의 이름이다:

```java
enum Season { SPRING, SUMMER, FALL, WINTER }
```

관례에 따라 열거형 값들에는 대문자로 구성된 이름이 주어지지만, 그것은 구문 규칙이 아닌 스타일 지침이다. 열거형 값은 **상수(constant)**, 즉 변경할 수 없는 고정값을 나타낸다. enum 자료형의 가능한 값은 통상 **열거형 상수(enum constants)** 라고 언급된다.

Season 자료형의 열거형 상수들은 Season 에 "포함되어 있다(contained in)"고 여겨진다는 점을 지적하고자 한다. 말하자면 — 다른 항목에 포함된 항목에는 복합 식별자(compound identifier)를 사용한다는 관례에 따라 — 프로그램에서 이들을 참조하기 위해 실제로 사용할 이름은 `Season.SPRING`, `Season.SUMMER`, `Season.FALL`, 그리고 `Season.WINTER`이란 것이다.

enum 자료형이 생성되면 다른 자료형이 사용되는 것과 정확히 동일한 방식으로 변수를 선언하는 데 이를 사용할 수 있다. 예를 들어 다음과 같은 문장으로 `Season` 자료형의 `vacatio`n이란 이름의 변수를 선언할 수 있다.

```java
Season vacation;
```

변수를 선언한 후에는 할당문을 사용하여 값을 할당할 수 있다. 할당시 오른쪽의 값은 `Season` 자료형의 열거형 상수 중 하나가 될 수 있다. "`Season`"을 포함하여 상수의 전체적인 이름을 사용함을 기억하라! 예를 들자면:

```java
vacation = Season.SUMMER;
```

`System.out.print(vacation)`와 같은 출력문으로 열거형 값을 출력할 수 있다. 출력값은 열거형 상수의 이름이 된다("`Season`"이 제외된). 이 경우 출력은 "SUMMER"가 된다.

열거형은 기술적으로 클래스이기 때문에 열거형 값들은 기술적으로 객체다. 객체로서 그들은 서브루틴을 포함할 수 있다. 모든 열거형 값의 서브루틴 중 하나는 `ordinal()`란 이름이다. 열거형 값과 함께 이를 사용할 경우, 열거형 값들의 목록에 있는 해당 값의 **순서 번호(ordinal number)** 를 반환한다. 순서 번호란 단순히 리스트에서 값의 위치를 알려주는 것이다. 즉, `Season.SPRING.ordinal()`은 int 자료형 0이며, `Season.SUMMER.ordinal()`은 1, `Season.FALL.ordinal()`은 2, `Season.WINTER.ordinal()`은 3이다. (컴퓨터 과학자들이 0부터 세기를 좋아하는 것을 반복해서 보게 될 것이다!) 물론 `vacation.ordinal()`와 같이 Season 자료형의 변수를 사용하여 `ordinal()` 메서드를 사용할 수도 있다.

열거형을 사용하면 의미 있는 이름을 값에 사용할 수 있기 때문에 프로그램을 보다 쉽게 읽히도록 할 수 있다. 그리고 컴파일러는 열거형 변수에 할당된 값이 실제로 해당 변수에 대한 허용되는 값인지 확인할 수 있기 때문에, 특정 유형의 오류를 방지할 수 있다. 현재로서는, 중요한 개념의 첫 번째 예로서 이들을 높이 평가해야 한다: 새로운 자료형의 창조. 전체 프로그램에서 열거형이 사용되고 있다는 것을 보여주는 짧은 예제가 여기에 있다:

```java
public class EnumDemo {

       // Define two enum types -- remember that the definitions
       // go OUTSIDE the main() routine!

    enum Day { SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY }

    enum Month { JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC }

    public static void main(String[] args) {

         Day tgif;     // Declare a variable of type Day.
         Month libra;  // Declare a variable of type Month.

         tgif = Day.FRIDAY;    // Assign a value of type Day to tgif.
         libra = Month.OCT;    // Assign a value of type Month to libra.

         System.out.print("My sign is libra, since I was born in ");
         System.out.println(libra);   // Output value will be:  OCT
         System.out.print("That's the ");
         System.out.print( libra.ordinal() );
         System.out.println("-th month of the year.");
         System.out.println("   (Counting from 0, of course!)");

         System.out.print("Isn't it nice to get to ");
         System.out.println(tgif);   // Output value will be:  FRIDAY

         System.out.println( tgif + " is the " + tgif.ordinal() 
                                            + "-th day of the week.");
    }

}
```


(내가 언급했듯이 열거형은 실제로 별도의 파일에 정의될 수 있다. 샘플 프로그램 SeparateEnumDemo.java는 EnumDemo.java와 동일하지만, 사용하는 열거형 자료형은 Month.java 및 Day.java라는 파일에 정의되어 있다는 점이 다르다.

<hr>

## 5. 텍스트 블록: 다행 문자열

자바 15는 다행(multiline) 문자열을 나타내기 위해 새로운 종류의 문자열 리터럴을 도입했다. (리터럴은 프로그램에 입력하여 상수 값을 나타내는 것임을 떠올려라.) 새로운 리터럴은 **텍스트 블록(text block)** 이라고 불린다. 텍스트 블록은 세 개의 쌍따옴표 문자로 된 문자열로 시작하고, 그 다음 선택적으로 공백과 새로운 행이 이어진다. 공백과 새로운 행은 텍스트 블록으로 표현되는 문자열 상수의 일부가 아니다. 텍스트 블록은 세 개의 쌍따옴표 문자로 구성된 또 다른 문자열로 종료된다. 텍스트 블록은 일반적인 문자열 리터럴을 사용할 수 있는 모든 곳에서 사용할 수 있다. 예를 들어,

```java
String poem = """
As I was walking down the stair,
I met a man who wasn't there.
He wasn't there again today.
I wish, I wish he'd go away!""";
```

이는 연결(concatenation)을 사용하여 다행 문자열을 작성하는 다음과 같은 동등한 의미의 코드보다 더욱 쓰고 읽기 쉽다.

```java
String poem = "As I was walking down the stair,\n"
+ "   I met a man who wasn't there.\n"
+ "He wasn't there again today.\n"
+ "   I wish, I wish he'd go away!\n";
```

텍스트 블록 내의 각각의 행이 시작되는 부분에 있는 여분의 공백은 리터럴에 의해 표현되는 문자열에서 제거되지만, 새로운 행은 보존된다는 점에 유의하라.

텍스트 블록에는 `\t` 또는 `\\`와 같은 이스케이프 문자가 포함될 수 있지만 백슬래시 문자 '`\ `'를 제외하고 어떤 것도 텍스트 블록 내에서 특별한 의미를 갖지 않는다. 예를 들어, 텍스트 블록에서 자바의 주석처럼 보이는 것은 실제로 주석이 아니다; 그것은 문자열의 일부인 평범한 문자일 뿐이다.

