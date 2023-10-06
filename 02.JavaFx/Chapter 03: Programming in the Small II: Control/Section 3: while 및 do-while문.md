# Section 3: while 및 do-while문

자바의 문장 은 단순한 문장이나 복합적인 문장일 수 있다. 할당문과 서브루틴 호출문과 같은 단순한 문장은 프로그램의 기본 구성 요소들이다. while 루프나 if 문과 같은 복합적인 문장은 단순한 문장을 복잡한 구조로 구성하는데 사용되는데, 문장이 실행되는 순서를 제어하기 때문에 이를 제어 구조라고 한다. 다음의 다섯 절에서는 while 문과 do..while문을 시작으로 자바에서 사용할 수 있는 제어 구조에 대한 세부사항을 살펴본다. 동시에 각 제어 구조에 의한 프로그래밍의 예제를 살펴보고, 이전 절에서 소개된 알고리즘 설계 기법을 적용하기로 한다.

## 1. while 문

`while` 문은 이미 제3장 제1절에서 소개되었다. `while` 루프는 다음 형태를 가진다:

```java
while (boolean-expression)
    statement
```

물론 {statement}는 중괄호 한 쌍 사이에 함께 있는, 그룹화된 여러 문장으로 구성된 블록문(block statement)이 될 수도 있다. 이러한 문장을 **루프 본체(body of the loop)** 라고 한다. {boolean-expression}이 참인 한 루프 본체는 반복된다. 이런 루프의 부울 표현식을 **연속 조건(continuation condition)**, 또는 더 간단히 **테스트(test)** 라고 한다. 약간의 설명이 필요할 수 있는 몇몇 지점이 있다. 해당 조건이 애초에 거짓이라면, 한 번이라도 루프 본체가 실행되기 전에는 어떻게 되는가? 그럴 경우, 루프 본체는 전혀 실행되지 않는다. while 루프의 본체는 0을 포함하여 임의의 횟수만큼 실행될 수 있다. 해당 조건은 사실이지만, 루프 본체의 중간 어디선가 거짓이 되면 어떻게 되는가? 이런 일이 일어나자마자 루프가 끝나는가? 컴퓨터가 끝에 도달할 때까지 루프의 본체를 계속 실행하기 때문에, 그렇지는 않다. 그래야 다시 루프의 시작으로 점프하여 조건을 테스트하고, 그래야 루프가 끝날 수 있다.

`while` 루프를 사용하여 해결할 수 있는 일반적인 문제를 살펴보자: 사용자가 입력한 양의 정수 집합의 평균을 찾는 것. 평균은 정수의 합을 정수의 수로 나눈 값이다. 프로그램은 사용자에게 한 번에 하나의 정수를 입력하도록 요청할 것이다. 이는 입력된 정수의 수를 계속 세고, 지금까지 읽은 숫자의 총합를 유지하게 된다. 다음은 프로그램에 대한 유사코드 알고리즘이다:

```java
설정 sum = 0     // 사용자가 입력한 정수의 합.
설정 count = 0   // 사용자가 입력한 정수의 수.
처리할 정수가 더 있는 동안에:
    정수를 읽는다
    sum에 이를 더한다
    정수의 수를 센다
평균을 구하기 위해 sum을 count로 나눈다
평균을 출력한다
```

그러나 처리할 정수가 더 많은지 어떻게 테스트할 수 있을까? 전형적인 해결책은 모든 데이터를 입력한 후에 0을 입력하라고 사용자에게 알리는 것이다. 이는 모든 데이터가 양수이므로 0은 허용된 데이터 값이 아니라고 가정하기 때문에 효과가 있을 것이다. 0은 그 자체가 평균화될 데이터의 일부가 아니다. 단지 실제 데이터의 끝을 표시하기 위해 있을 뿐이다. 이런 방식으로 사용되는 데이터 값을 때때로 **감시 값(sentinel value)** 이라고 한다. 그래서 이제 `while` 루프에서의 테스트는 "입력 정수가 0이 아닌 동안에"가 된다. 하지만 또 다른 문제가 있다! 처음으로 테스트가 이루어 질 때, 루프 본체가 아직 실행되기 전에, 어느 정수도 읽히지 않았다. 아직 "입력 정수"가 없기 때문에 입력 정수가 0인지 테스트하는 것은 말이 되지 않는다. 그렇기에, 테스트가 맞는지 확인하기 위해 해당 `while` 루프 이전에 무언가를 해야만 한다. `while` 루프에 있는 테스트가 처음 실행될 때 이를 말이 되도록 설정하는 일을 **루프 예열하기(priming1 the loop)** 이라고 한다. 이 경우, 루프가 시작되기 전에 간단히 첫 번째 정수를 읽을 수 있다. 여기에 수정된 알고리즘이 있다:

```java
설정 sum = 0
설정 count = 0
정수를 읽는다
처리할 정수가 더 있는 동안에:
    정수를 sum에 더한다
    정수의 수를 센다
    정수를 읽는다
평균을 구하기 위해 sum을 count로 나눈다
평균을 출력한다
```

필자가 루프 본체를 재정렬했다는 점에 주의하라. 정수는 루프 전에 읽히기 때문에, 루프는 그 정수를 처리하는 것으로 시작해야 한다. 루프가 끝나면 컴퓨터는 새로운 정수를 읽는다. 그런 다음 컴퓨터는 다시 루프의 시작으로 되돌아가 방금 읽은 정수를 시험한다. 컴퓨터가 마침내 감시 값을 읽을 때, 감시 값이 처리되기 전에 루프가 종료된다는 점에 유의하라. 이는 총합에 합산되지 않고, 계수되지도 않는다. 원래 이런 식이다. 감시 값은 데이터의 일부가 아니다. 원래의 알고리즘은, 예열(priming) 없이 동작하도록 만들 수 있었더라도, 감시 값을 포함한 모든 정수를 합산하고 계수했을 것이기 때문에 부정확했다.(감시 값이 0이기 때문에, 그 합계(sum)는 여전히 정확하겠지만, 해당 계수(count)는 1이 틀릴 것이다. 이러한 소위 **1이 차이나는 오류(off-by-one error)** 는 매우 흔하다. 수를 세는 것은 보기보다 어려운 것으로 판명되었다!)

알고리즘을 완전한 프로그램으로 쉽게 바꿀 수 있다. 프로그램은 "`average = sum/count;`"라는 문장을 사용하여 평균을 계산할 수 없다는 점에 유의하라. `sum`와 `count`는 모두 int 자료형의 변수이므로 `sum/count` 값도 정수다. 평균은 실수여야 한다. 전에도 이런 문제를 본 적이 있다: 우리는 컴퓨터가 실수로 그 몫을 계산하도록 강제하기 위해 int 값 중 하나를 double로 변환해야 한다. 이 작업은 변수 중 하나를 double로 자료형 변환(type-casting)하여 될 수 있다. 자료형 변환 "`(double)sum`"은 `sum` 값을 실수로 변환하므로 프로그램에서 평균은 "`average = ((double)sum) / count;`"로 계산된다. 이 경우의 또 다른 해결책은 애초에 `sum`을 double 자료형의 변수로 선언하는 것일 터이다.

다른 한 가지 문제는 프로그램에 의해 제기된다: 사용자가 첫 번째 입력값으로 0을 입력하면 처리할 데이터가 없어진다. `while` 루프 후에도 `count`가 여전히 0과 동일한지 확인함으로써 이 경우에 대한 테스트를 할 수 있다. 이것은 사소한 점처럼 보일 수도 있지만, 신중한 프로그래머는 모든 것을 감안해야 한다.

프로그램의 전체 소스 코드(물론 주석도 추가되었다!)는 다음과 같다:

```java
import textio.TextIO;

/**
* This program reads a sequence of positive integers input
* by the user, and it will print out the average of those
* integers.  The user is prompted to enter one integer at a
* time.  The user must enter a 0 to mark the end of the
* data.  (The zero is not counted as part of the data to
* be averaged.)  The program does not check whether the
* user's input is positive, so it will actually add up
* both positive and negative input values.
*/
public class ComputeAverage {

  public static void main(String[] args) {

    int inputNumber;   // 사용자에 의해 입력된 정수들 중 하나.
    int sum;           // 양의 정수들의 총합.
    int count;         // 양의 정수들의 수.
    double average;    // 양의 정수들의 평균.

    /* 합계 및 계수 변수를 초기화. */

    sum = 0;
    count = 0;

    /* 사용자 입력을 읽고 처리 */

    System.out.print("첫 번째 양의 정수를 입력: ");
    inputNumber = TextIO.getlnInt();

    while (inputNumber != 0) {
      sum += inputNumber;   // 실행 중인 sum에 inputNumber를 더함.
      count++;              // count에 1을 더하여 입력의 수를 셈.
      System.out.print("다음 양의 정수를 입력하거나, 0을 입력하여 종료: ");
      inputNumber = TextIO.getlnInt();
    }

    /* 결과 표시. */

    if (count == 0) {
      System.out.println("어떤 데이터도 입력되지 않았습니다!");
    }
    else {
      average = ((double)sum) / count;
      System.out.println();
      System.out.println("당신은 " + count + " 개의 양수를 입력했습니다.");
      System.out.printf("그 평균은 %1.3f.\n", average);
    }

  } // main() 종료

} // class ComputeAverage 종료
```

## 2. do..while 문
때로는 `while` 루프처럼 시작보다는, 루프의 끝에서 연속 조건을 테스트하는 것이 더 편리하다. `do..while` 문은, 이를 테스트하는 조건과 함께 "`while`"이라는 단어가 끝으로 옮겨졌다는 점을 제외하면, `while` 문과 매우 유사하다. 루프의 시작을 표시하기 위해 "do"라는 단어가 추가된다. `do..while` 문은 다음의 형식을 가진다:

```java
do
  statement
while (boolean-expression);
```

또는, 늘 그렇지만, 해당 {statement}가 블록이 될 수 있기 때문에, 다음도 가능하다:

```java
do {
    statement
} while (boolean-expression);
```

맨 끝에 있는 세미콜론 ';'에 유의하라. 이 세미콜론은 할당문이나 선언문 끝에 있는 세미콜론이 문장의 일부인 것처럼 해당 문장의 일부분이다. 이를 생략하는 것은 구문 오류가 된다. (더 일반적으로, 자바의 모든 문장은 세미콜론이나 오른쪽 중괄호 '}'로 끝난다.)

`do` 루프를 실행하기 위해 컴퓨터는 먼저 루프 본체 — 즉 문장 또는 루프 내부의 문장 — 를 실행한 다음 부울 표현식을 평가한다. 표현식의 값이 `true`이면 컴퓨터는 `do` 루프의 시작으로 돌아가 과정을 반복하고, 값이 `false`이면 루프를 종료하고 프로그램의 다음 부분을 계속한다. 루프가 끝날 때까지 조건을 테스트하지 않기 때문에, `do` 루프의 본체는 항상 적어도 한 번은 실행된다.

예를 들어, 게임 플레이 프로그램에 대한 다음과 같은 유사코드를 생각해보자. 여기서는 `while` 루프 대신 `do` 루프가 타당한데, 알겠지만 `do` 루프로 적어도 한 번의 게임은 있을 것이기 때문이다. 또한, 루프 끝에서 사용되는 테스트는 심지어 시작시에는 맞지 않을 것이다:

```java
do {
    게임을 플레이 한다
    다른 게임을 사용자가 하길 원하는지 묻는다
    사용자의 응답을 읽는다
} while ( 사용자의 응답이 예 );
```

이를 적절한 자바 코드로 변환하자. 지금은 게임 플레이에 대해 이야기하고 싶지 않기에 `Checkers`라는 클래스가 있고, `Checkers` 클래스에는 유저에 대해 체커 한 게임을 하는 `playGame()`이라는 이름의 정적 멤버 서브루틴이 포함되어 있다고 하자. 그런 다음, "게임을 플레이 한다"라는 유사코드는 서브루틴 호출문 "`Checkers.playGame();`"로 표현할 수 있다. 사용자의 응답을 저장하기 위한 변수가 필요하다. TextIO 클래스는 부울 변수를 사용하여 예/아니요 질문에 대한 답변을 저장하는 데 편리하다. 입력 함수 `TextIO.getlnBoolean()`은 사용자가 "예(yes)" 또는 "아니오(no)"로 값을 입력할 수 있도록 한다(다른 허용되는 응답들 중에서). "예"는 `true`로 간주되고, "아니오"는 `false`로 간주된다. 따라서 알고리즘은 다음과 같이 코딩될 수 있다:

```java
boolean wantsToContinue;  // 사용자가 다시 플레이를 하기 원한다면 true
do {
    Checkers.playGame();
    System.out.print("다시 플레이 하시겠습니까? ");
    wantsToContinue = TextIO.getlnBoolean();
} while (wantsToContinue == true);
```

boolean 변수 값이 `false`로 설정되면 이는 루프가 종료되어야 한다는 신호다. boolean 변수를 이런 식으로 — 프로그램의 한 부분에 설정하고 다른 부분에서 테스트하는 신호로 — 사용할 경우, 이를 때때로 (신호기(signal flag)란 의미에서) **플래그(flag)** 또는 **플래그 변수(flag variable)** 라고 부르기도 한다.

그런데, 보통보다 더 현학적인(more-than-usually-pedantic) 프로그래머는 "`while (wantsToContinue == true)`" 테스트를 비웃곤 했다. 이 테스트는 "`while (wantsToContinue)`"와 정확히 동일하다. "`wantsToContinue == true`"가 참인지 여부를 테스트하는 것은 "`wantsToContinue`"가 참인지 여부를 테스트하는 것과 같다. 조금 덜 불쾌한 것은 "`flag == false`" 형식의 표현식인데 여기서 flag는 부울 변수다. "`flag == false`"의 값은 정확히 "`!flag`"의 값과 동일하며, 여기서 !는 부울 부정 연산자다. 그래서 "`while (flag == false)`" 대신 "`while (!flag)`"을 사용하고, "`if (flag == false)`" 대신 "`if (!flag)`"라고 쓸 수 있다.

`do..while` 문이 종종 `while` 문보다 더 편리함에도, 두 종류의 루프를 갖는 것이 해당 언어를 더 강하게 만들지는 않는다. `do..while`를 사용하여 해결할 수 있는 모든 문제는 `while` 문만 사용해도 해결될 수 있고, 그 반대의 경우도 마찬가지다. 사실, {doSomething}이 어떤 프로그램 코드 블록을 나타낸다고 하면,

```java
do {
    doSomething
} while (boolean-expression);
```

은 다음과 정확히 같은 효과를 가진다:

```java
doSomething
while (boolean-expression) {
    doSomething
}
```

마찬가지로,

```java
while (boolean-expression) {
    doSomething
}
```

은 어떤 식으로든 프로그램의 의미를 바꾸지 않고 다음과 같이 바꿀 수 있다:

```java
if (boolean-expression) {
    do {
    doSomething
    } while (boolean-expression);
}
```

<hr>

## 3. break와 continue
`while` 및 `do..while` 루프는 루프의 시작 또는 끝에서 연속 조건을 테스트할 수 있게 해준다. 때로는 루프 중간에서 테스트을 하거나, 같은 루프의 서로 다른 지점에서 여러 테스트를 하는 것이 더 자연스럽다. 자바에서는 어떤 루프의 중간을 벗어나기 위한 일반적인 방법을 제공한다. 이는 `break` 문이라고 하는데, 다음과 같은 형식을 취한다.

```java
break;
```

컴퓨터가 루프에서 `break` 문을 실행하면, 즉시 루프에서 튀어나올 것이다. 그리고 나선 프로그램에서 루프에 뒤따르는 것이면 뭐든 그걸 계속한다. 다음 예시를 생각해보자:

```java
while (true) {  // 영원히 실행될 것처럼 보인다!
    System.out.print("양수를 입력: ");
    N = TextIO.getlnInt();
    if (N > 0)   // 입력값은 OK, 루프를 벗어남
        break;
    System.out.println("당신의 답은 0보다 커야 함.");
}
// 중단 이후 여기부터 계속함
```

사용자가 입력한 숫자가 0보다 크면 `break` 문이 실행돼 컴퓨터가 루프에서 튀어나온다. 그렇지 않으면 컴퓨터는 "당신의 답은 0보다 커야 함."이라고 출력하고, 또 다른 입력 값을 읽기 위해 루프 시작 부분으로 다시 돌아갈 것이다.

이 루프의 첫 줄인 "`while (true)`"은 좀 이상하게 보일지 모르지만, 완벽하게 허용되는 것이다. `while` 루프의 조건은 어떤 부울 값 표현식도 될 수 있다. 컴퓨터는 이 표현식을 평가하여 값이 `true`인지 `false`인지 확인한다. 부울 리터럴 "`true`"는 항상 참으로 평가되는 부울 표현식일 뿐이다. 그렇기에 "`while (true)`"을 사용하여 무한 루프 또는 `break` 문으로 종료되는 루프를 작성할 수 있다.

`break` 문은 `break` 문을 직접적으로 감싸는 루프를 종료시킨다. 하나의 루프 문이 다른 루프 안에 들어 있는 **중첩 루프(nested loop)** 를 가질 수 있다. 중첩 루프 내에서 `break` 문을 사용하면, 해당 루프를 벗어날 뿐 중첩 루프를 포함하고 있는 바깥 루프를 벗어날 수 없다. 어떤 루프를 중단할지 명시할 수 있는 **레이블된(labeled)** `break` 문이라는 것이 있다. 이는 그리 흔한 것이 아니니 빠르게 검토한다. 레이블은 다음과 같이 작동한다: 어떤 루프든 그 앞에 **레이블(label)** 을 붙일 수 있다. 레이블은 콜론(colon)이 뒤따르는 간단한 식별자로 구성된다. 예를 들어, 레이블이 붙어 있는 `while`은 "`mainloop: while...`"과 같이 보일 수 있다. 이 루프 안에서 레이블된 루프를 중단하기 위해 "`break mainloop;`"라는 레이블된 `break` 문을 사용할 수 있다. 예를 들어, 여기에 `s1`과 `s2`라는 두 문자열에서 공통 문자가 있는지 여부를 확인하는 코드 조각이 있다. 공통 문자가 발견되면 `nothingInCommon` 플래그 변수의 값이 `false`로 설정되며, 해당 지점에서 처리를 중단하기 위해 레이블된 `break`가 사용된다:

```java
boolean nothingInCommon;
nothingInCommon = true;  // s1과 s2가 공통된 문자가 없다고 가정.
int i,j;  // s1과 s2의 문자들을 반복하는 변수들.
        
i = 0;
bigloop: while (i < s1.length()) {
    j = 0;
    while (j < s2.length()) {
        if (s1.charAt(i) == s2.charAt(j)) { // s1 및 s2가 공통된 문자가 있다면...
        nothingInCommon = false;  // 즉 nothingInCommon은 실제로 거짓.
        break bigloop;  // 양 루프를 *모두* 중단
        }
        j++;  // s2의 다음 문자로 이동.
    }
    i++;  // s1의 다음 문자로 이동.
}
```

<hr>

`continue` 문은 `break`과 관련이 있지만 덜 일반적으로 사용된다. `continue` 문은 현재 반복되는 루프의 나머지 부분을 건너뛰라고 컴퓨터에 말한다. 그러나, 루프에서 완전히 튀어나오는 대신 루프의 시작으로 다시 되돌아가 다음 반복을 계속한다(추가 반복이 필요한지 여부를 확인하기 위해 루프의 연속 조건을 평가하는 것을 포함하여). `break`와 마찬가지로, `continue`가 중첩 루프에 있을 때, 직접적으로 이를 포함하는 루프를 계속한다; 포함하는 루프를 계속하기 위해 "레이블된 `continue`"를 대신 사용할 수도 있다.

`break`와 `continue`는 `while` 루프와 `do..while` 루프 안에서 사용될 수 있다. 이들은 또한 다음 절에서 다루는 `for` 루프에 사용될 수 있다. 제3장 제6절에서, 우리는 `break`가 `switch` 문을 벗어나는 데에도 사용될 수 있다는 점을 살필 것이다. `break`는 `if` 문 안에 나타날 수 있지만, 오직 `if` 문이 루프 또는 `switch` 문 안에서 중첩된 경우에만 사용된다. 이런 경우에는 `if`를 중단하겠다는 뜻이 아니다. 대신, `if` 문을 포함하고 있는 루프 또는 `switch` 문이 중단된다. `if` 안에 있는 `continue` 문에도 동일한 개념이 적용된다.