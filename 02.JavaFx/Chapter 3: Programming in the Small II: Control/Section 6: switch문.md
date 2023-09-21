# Section 6: switch문

자바의 두 번째 분기문 은 `switch` 문으로, 이 절에서 소개된다. `switch` 문은 `if` 문보다 훨씬 덜하게 사용되지만, 때때로 특정 유형의 다방향 분기를 표현하는 데 유용하다.

<hr>

## 1. 기본 switch 문
`switch` 문은 표현식의 값을 테스트할 수 있고, 그 값에 따라 `switch` 문 내의 어떤 위치로 직접 이동할 수 있다. 특정 자료형의 표현식만 사용 가능하다. 표현식의 값은 int, short 또는 byte의 원시적 정수 자료형 중 하나가 될 수 있다. 원시적 char 자료형도 가능하다. String 도 될 수 있다. 또는 열거형일 수 있다(열거형에 대한 소개는 제2장 제3절 제4관 참조). 특히, 표현식이 double 또는 float 값이 **될 수 없다**는 점에 유의하라.

`switch` 문 안에서 이동할 수 있는 위치에는 다음의 형식을 취하는 **케이스 레이블(case label)** 이 표시된다: "case {constant}:". 여기서 {constant}는 `switch`에서의 표현식과 같은 유형의 리터럴이다. 케이스 레이블은 표현식이 지정된 {constant} 값으로 평가될 때 컴퓨터가 이동하는 위치를 표시한다. `switch` 문에 "default:" 레이블을 사용할 수도 있다; 이는 표현식의 값이 케이스 레이블에 나열되지 않을 때 사용되는 기본(default) 이동 지점을 제공한다.

가장 자주 사용되는 `switch` 문은 다음과 같은 형식을 가진다:

```java
switch (expression) {
    case constant-1:
        statements-1
        break;
    case constant-2:
        statements-2
        break;
    .
    . // (더 많은 case들)
    .
    case constant-N:
        statements-N
        break;
    default:    // 선택적인 default case
        statements-(N+1)
}   // switch 문 끝
```

이는 다음과 같은 다방향(multiway) `if` 문과 정확히 같은 효과를 내지만, `switch` 문이 더 효율적일 수 있다. 왜냐하면 컴퓨터가 하나의 표현식을 평가하여 올바른 케이스(correct case)로 바로 이동할 수 있기 때문이며, 반면에 `if` 문에서는 컴퓨터가 어떤 문장들의 집합을 실행해야 하는지 알기 전에 반드시 N개까지의 표현식들을 평가해야 한다:

```java
if (expression == constant-1) { // String의 경우는 .equals를 사용하라!!
    statements-1
}
else if (expression == constant-2) {
    statements-2
}
.
.
.
else if (expression == constant-N) {
    statements-N
}
else {
    statements-(N+1)
}
```

`switch` 안의 `break` 문은 `switch` 문의 구문상 실제로 필요하지는 않다. `break`의 효과는 컴퓨터가 나머지 모든 케이스들을 건너뛰면서 `switch` 문의 끝을 뛰어넘도록 하는 것이다. 만약 `break` 문을 빼면, 컴퓨터는 하나의 케이스를 완료한 후에 그저 전진할 것이며 다음 케이스 레이블과 관련된 문장을 실행할 것이다. 이는 거의 원하는 상황은 아니겠지만, 허용되는 것이다. (여기에서 — 다음 장에 도달할 때까지 이해하지 못하겠지만 — 서브루틴 내부에서는 때때로 `switch`와 마찬가지로 서브루틴을 종료하는 `return` 문으로 `break` 문이 대체된다는 점을 지적하고자 한다.)

문장들의 그룹 중 하나를 완전히 제외할 수 있다는 점에 유의하라(`break`를 포함하여). 그렇게 하면 두 개의 서로 다른 상수를 포함하는 두 개의 케이스 레이블을 연속하여 갖게 된다. 이는 단지 컴퓨터가 같은 장소로 이동하여 두 상수 각각에 대해 동일한 동작을 수행한다는 것을 의미한다.

여기 `switch` 문의 예제가 있다. 유용한 예제는 아니지만, 따라 하기엔 쉬울 것이다. 그건 그렇고, 케이스 레이블의 상수는 특정한 순서로 있을 필요는 없지만, 반드시 모두 다른 값이어야 한다는 점에 유의하라:

```java
switch ( N ) {   // (N은 정수인 변수로 가정한다.)
    case 1:
        System.out.println("숫자는 1.");
        break;
    case 2:
    case 4:
    case 8:
        System.out.println("숫자는 2, 4, 또는 8.");
        System.out.println("(이는 2의 승수(power)임!)");
        break;
    case 3:
    case 6:
    case 9:
        System.out.println("숫자는 3, 6, 또는 9.");
        System.out.println("(이는 3의 배수(multiple)임!)");
        break;
    case 5:
        System.out.println("숫자는 5.");
        break;
    default:
        System.out.println("숫자는 7이거나 1부터 9 사이의 범위를 벗어남.");
}
```

`switch` 문장은 제어 구조가 진행될수록 상당히 원시적이며, 사용할 때 실수하기 쉽다. 자바는 구 프로그래밍 언어 C와 C++에서의 모든 제어 구조를 직접적으로 취한다. `switch` 문은 분명 자바 설계자들이 몇몇 개선사항을 도입했어야 했을 지점 중 하나이다.

<hr>

## 3. 메뉴 및 switch 문
`switch` 문 중 하나의 응용례는 메뉴의 처리에 있다. 메뉴(menu)는 선택지들의 목록이다. 사용자는 선택지 중 하나를 선택한다. 컴퓨터는 각각의 가능한 선택에 대하여 각기 다른 방식으로 반응해야 한다. 선택지 번호가 1, 2, ...인 경우, 적절한 응답을 고르기 위해 선택된 선택지의 번호를 `switch` 문에서 사용할 수 있다.

명령줄 프로그램에서 메뉴는 번호가 매겨진 선택지 목록으로 제시될 수 있으며, 사용자는 해당 번호를 입력하여 선택지를 선택할 수 있다. 다음은 이전 절의 `LengthConverter` 예제를 변형하여 사용한 것이다:

```java
int optionNumber;   // 사용자에 의해 선택되는, 메뉴의 선택지 번호.
double measurement; // 사용자에 의해 입력되는, 숫자 측정값.
//    측정 단위는 사용자가 선택한 선택지에 달려 있음.
double inches;      // 동일한 측정값을 인치로 변환한 것.

/* 메뉴를 표시하고 사용자가 선택한 선택지 번호를 얻음. */

System.out.println("어떤 측정 단위를 입력값에 사용합니까?");
System.out.println();
System.out.println("         1.  인치(inches)");
System.out.println("         2.  피트(feet)");
System.out.println("         3.  야드(yards)");
System.out.println("         4.  마일(miles)");
System.out.println();
System.out.println("선택할 번호를 입력하세요: ");
optionNumber = TextIO.getlnInt();

/* 사용자의 측정값을 읽고 인치로 이를 변환함. */

switch ( optionNumber ) {
    case 1:
        System.out.println("인치 수를 입력: ");
        measurement = TextIO.getlnDouble();
        inches = measurement;
        break;          
    case 2:
        System.out.println("피트 수를 입력: ");
        measurement = TextIO.getlnDouble();
        inches = measurement * 12;
        break;          
    case 3:
        System.out.println("야드 수를 입력: ");
        measurement = TextIO.getlnDouble();
        inches = measurement * 36;
        break;          
    case 4:
        System.out.println("마일 수를 입력: ");
        measurement = TextIO.getlnDouble();
        inches = measurement * 12 * 5280;
        break;
    default:
        System.out.println("오류! 허용되지 않은 선택지 번호! 종료합니다!");
        System.exit(1);          
} // switch 종료

/* 이제 인치를 피트, 야드, 그리고 마일로 변환하는 일을 계속함... */
```

이 예제는 그 대신으로 `switch` 문 안에 String 을 사용하여 작성될 수 있다:

```java
String units;       // 사용자에 의해 입력되는, 측정 단위.
double measurement; // 사용자에 의해 입력되는, 숫자 측정값.
double inches;      // 동일한 측정값을 인치로 변환한 것.

/* 사용자의 측정 단위를 읽음. */

System.out.println("어떤 측정 단위를 입력값에 사용합니까?");
System.out.print("허용되는 응답: inches, feet, yards, 또는 miles : ");
units = TextIO.getln().toLowerCase();

/* 사용자의 측정값을 읽고 인치로 변환. */

System.out.print("단위 " + units + " 의 수를 입력:  ");
measurement = TextIO.getlnDouble();

switch ( units ) {
    case "inches":
        inches = measurement;
        break;          
    case "feet":
        inches = measurement * 12;
        break;          
    case "yards":
        inches = measurement * 36;
        break;          
    case "miles":
        inches = measurement * 12 * 5280;
        break;
    default:
        System.out.println("잠시만! 허용되지 않는 측정 단위네요! 종료합니다!");
        System.exit(1);          
} // switch 종료
```

<hr>

## 3. switch 문 안의 열거형
`switch`의 표현식 자료형이 열거형일 수 있다. 이 경우, 케이스 레이블의 상수는 반드시 열거형의 값이어야 한다. 예를 들어, 표현식 자료형이 다음에 의해 정의된 열거형 Season 이라 가정하자:

```java
enum Season { SPRING, SUMMER, FALL, WINTER }
```

그리고 `switch` 문 안의 표현식이 Season 자료형의 표현식이라 가정한다. 케이스 레이블의 상수는 Season.SPRING, Season.SUMMER, Season.FALL, 또는 Season.WINTER의 값들 중에서 선택되어야 한다. 그러나 해당 구문에는 기이한 점(quirk)이 있다: 케이스 레이블에 열거형 상수가 사용될 때, "`SPRING`"과 같은 단순한 이름만 사용되며 "`Season.SPRING`"과 같은 전체 이름이 사용되지 않는다는 것이다. 물론 컴퓨터는 이미 케이스 레이블의 값이 열거형에 반드시 속하고 있음을 알고 있는데, 이는 사용된 표현식의 자료형으로부터 알 수 있기에 실제로 상수에 자료형 이름을 지정할 필요가 없기 때문이다. 예를 들어, `currentSeason`이 Season 자료형의 변수라고 가정하면 다음과 같은 `switch` 문을 사용할 수 있다:

```java
switch ( currentSeason ) {
    case WINTER:    // ( Season.WINTER가 아니다! )
        System.out.println("December, January, February");
        break;
    case SPRING:
        System.out.println("March, April, May");
        break;
    case SUMMER:
        System.out.println("June, July, August");
        break;
    case FALL:
        System.out.println("September, October, November");
        break;
}
```

<hr>

## 4. 확정적 할당과 switch 문
다소 현실적인 예로서, 다음의 `switch` 문은 가능한 세 가지 선택지 중 하나를 무작위로 선택한다. 표현식 `(int)(3*Math.random())`의 값이 동일한 확률로 무작위 선택된 정수 0, 1 또는 2 중 하나임을 떠올려라. 따라서 아래의 `switch` 문에서는 "`Rock`", "`Paper`", "`Scissors`" 중 하나의 값을 computerMove에 할당하고, 각각의 경우 그 확률은 1/3이다:

```java
switch ( (int)(3*Math.random()) ) {
    case 0:
        computerMove = "Rock";
        break;
    case 1:
        computerMove = "Paper";
        break;
    case 2:
        computerMove = "Scissors";
        break;
}
```

자, 이 `switch` 문장은 완벽하게 OK이나, 다음 코드 조각에서 사용한다고 가정하자:

```java
String computerMove;
switch ( (int)(3*Math.random()) ) {
    case 0:
        computerMove = "Rock";
        break;
    case 1:
        computerMove = "Paper";
        break;
    case 2:
        computerMove = "Scissors";
        break;
}
System.out.println("The computer's move is " + computerMove);  // ERROR!
```

이제 마지막 행에 미묘한 오류가 있다! 여기의 문제는 확정적 할당(definite assignment)에 기인하는데, 자바 컴파일러가 해당 값을 사용하기 전에 변수가 확실히 그 값을 할당받았음을 반드시 결정할 수 있어야 한다는 개념 때문이다. 확정적 할당은 제3장 제1절 제4관에서 소개한 바 있다. 이 예제에서 `switch`의 세 가지 케이스가 모든 가능성을 맡는 것은 사실이지만, 컴파일러는 이를 알아낼 만큼 똑똑하지 않다; 이는 그저 `switch`에 정수 값 표현식이 있지만 주어진 케이스들로 모든 가능한 정수 값들이 담보되는 것은 아니라는 점을 지켜볼 뿐이다.

간단한 해결책은 `switch` 문의 최종 `case`를 `default`로 교체하는 것이다. `default` 케이스에서, `switch` 안의 표현식의 가능한 모든 값이 확실히 다루어지며, 컴파일러는 `computerMove`에 다음 값이 확정적으로 할당된다는 것을 알게 된다:

```java
String computerMove;
switch ( (int)(3*Math.random()) ) {
    case 0:
        computerMove = "Rock";
        break;
    case 1:
        computerMove = "Paper";
        break;
    default:
        computerMove = "Scissors";
        break;
}

System.out.println("The computer's move is " + computerMove);  // OK!
```

## 5. 새로운 switch 문 구문
새로운 버전의 `switch` 문이 자바 14에서 추가되었다. 새로운 버전은 `case` 후에 콜론(colon) 대신 `->`를 사용하며, 케이스 안의 코드는 단일 문장이나 중괄호로 감싸진 여러 문장으로 구성된 블록문(block statement)도 가능하다. 케이스를 조기에 끝내는 데 사용될 수는 있지만, `break` 문이 필요한 것은 아니다. 이는 누락된 `break`로 인해 한 케이스에서 다음 케이스로 우발적으로 제어가 벗어나는 일반적인 오류를 방지한다. 게다가, 케이스 레이블당 하나의 값만 허용하는 대신에, 쉼표로 구분된 여러 값을 케이스가 취할 수 있다. 새로운 구문을 사용하면, 이 절의 첫 번째 예제는 다음과 같이 작성될 수 있다:

```java
switch ( N ) {   // (N은 정수인 변수로 가정한다.)
    case 1 -> System.out.println("숫자는 1.");
    case 2, 4, 8 -> {
        System.out.println("숫자는 2, 4, 또는 8.");
        System.out.println("(이는 2의 승수(power)임!)");
    }
    case 3, 6, 9 -> {
        System.out.println("숫자는 3, 6, 또는 9.");
        System.out.println("(이는 3의 배수(multiple)임!)");
    }
    case 5 -> System.out.println("숫자는 5.");
    default ->
        System.out.println("숫자는 7이거나 1부터 9 사이의 범위를 벗어남.");
}
```

필자는 이를 큰 발전으로 본다. 그러나 원래의 `switch` 구문도 여전히 사용할 수 있다.

개선된 `switch` 문과 함께 새로운 "`switch` 표현식"도 도입되었다. 다른 표현식과 마찬가지로 `switch` 표현식도 단일의 값을 계산하고 반환한다. 구문은 `switch` 문과 비슷하지만, 각각의 `case` 문장 대신에 표현식이 있다. 예를 들어,

```java
String computerMove = switch ( (int)(3*Math.random()) ) {
    case 1 -> "Rock";
    case 2 -> "Paper";
    default -> "Scissors";
};
```

`switch` 표현식은 항상 값을 반드시 계산해야 하므로 거의 언제나 `default` 케이스를 갖는다. `case` 안의 표현식은 여러 개의 문장을 포함하는 블록으로 대체될 수 있다: 따라서 해당 `case`의 값은 `return`이나 `break` 문 보다는 `yield` 문("`yield 42;`"와 같은)으로 지정되어야 한다.