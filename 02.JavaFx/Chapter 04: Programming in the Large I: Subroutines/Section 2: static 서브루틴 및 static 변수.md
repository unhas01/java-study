# Section 2: static 서브루틴 및 static 변수

자바의 모든 서브루틴은 어떤 클래스 안에서 정의되어야 한다. 대부분의 언어는 변동하는(free-floating) 독립적인 서브루틴을 허용하기에, 이는 프로그래밍 언어들 사이에서 자바를 다소 특이한 것으로 만든다. 클래스의 한 가지 목적은 관련된 서브루틴과 변수를 함께 그룹화하는 것이다. 아마도 자바의 설계자들은 모든 것이 무언가와 관련되어야 한다고 느꼈을 것이다. 덜 철학적인 동기로는, 자바의 설계자들은 사물이 명명되는 방식에 확고한 통제권을 두기를 원했는데, 자바 프로그램은 많은 다른 프로그래머들에 의해 만들어진 엄청난 수의 서브루틴에 잠재적으로 접근할 수 있었기 때문이다. 이러한 서브루틴이 명명된 클래스로 그룹화되는 것(그리고 클래스가 나중에 보게 될 것처럼 "패키지(package)"로 그룹화되는 것)은 너무 많은 다른 이름들로 인해 야기될 수 있는 혼란을 통제하는 데 도움이 된다.

자바에는 **정적(static)** 서브루틴과 **비정적(non-static)** 서브루틴의 기본적인 구별이 있다. 클래스 정의는 두 가지 유형의 서브루틴에 대한 소스 코드를 포함할 수 있지만, 프로그램이 실행될 때 이러한 소스 코드를 사용하여 수행되는 작업은 매우 다르다. 정적 서브루틴은 이해하기 쉽다: 실행 중인 프로그램에서 정적 서브루틴은 클래스 자체의 멤버(member)이다. 반면에 비정적 서브루틴 정의는 객체가 생성될 때에만 사용되며, 서브루틴 자체가 객체의 멤버가 된다. 비정적 서브루틴은 객체를 다룰 때만 관련성을 가진다. 정적인 것과 비정적인 것의 구별은 또한 변수 및 클래스 정의에서 발생할 수 있는 다른 일에도 적용된다. 이 장에서는 정적 서브루틴과 정적 변수를 거의 전적으로 다룰 것이다. 다음 장에서는 비정적인 것들과 객체 지향적인 프로그래밍에 대해 살펴볼 것이다.

클래스나 객체에 있는 서브루틴을 **메서드(method)** 라고 부르는 경우가 종종 있으며, "메서드"는 대부분의 사람들이 자바에서 서브루틴을 언급하는 용어다. 필자도 가끔은 "메서드"라는 용어를 사용하기 시작하겠지만, 적어도 이 장에서 정적 서브루틴에 대해서는 보다 일반적인 용어인 "서브루틴"을 계속 선호할 것이다. 그러나, "메서드"과 "서브루틴"이라는 용어는 자바에 관한 한 본질적으로 동의어라고 생각하기 시작해야 한다. 서브루틴을 지칭하는 데 사용되는 다른 용어는 "프로시저(procedure)"와 "함수(function)"이다. (필자는 일반적으로 값을 계산하고 반환하는 서브루틴에만 "함수"라는 용어를 사용하지만, 일부 프로그래밍 언어에서 이는 서브루틴을 일반적으로 지칭하는 데 사용된다.)

<hr>

## 1. 서브루틴 정의
서브루틴은 어딘가에서 정의되어야 한다. 정의(definition)에는 서브루틴의 이름, 서브루틴을 호출할 수 있을 만큼 충분한 정보, 서브루틴이 호출될 때마다 실행될 코드를 포함해야 한다. 자바의 서브루틴 정의는 다음과 같은 형식을 취한다:

```java
modifiers return-type subroutine-name (parameter-list) {
    statements
}
```
이 모든 것이 무엇을 의미하는지 자세히 이해하려면 시간이 다소 — 이 장의 대부분 — 걸릴 것이다. 물론, 이미 제3장 제9절의 애니메이션 프로그램에 있는 `main()` 루틴과 `drawFrame()` 루틴 처럼, 서브루틴의 예제를 이전 장에서 이미 본 적이 있다. 그렇기에 이러한 일반적인 형식에 익숙할 것이다.

서브루틴 정의에서 중괄호 { 및 } 사이의 {statements}는 서브루틴의 **본체(body)** 를 구성한다. 이러한 문장들은 이전 절에서 논의한 바와 같이 "블랙박스"의 내부, 또는 구현(implementation) 부분이다. 이들은 메소드가 호출될 때 컴퓨터가 실행하는 명령어들이다. 서브루틴은 제2장과 제3장에서 논의된 어떠한 문장들도 포함할 수 있다.

서브루틴 정의의 시작 부분에서 나타날 수 있는 {modifiers}는 정적인지 아닌지와 같이 서브루틴의 특정한 특성(characteristics)을 설정하는 단어들이다. 지금까지 본 제한자(modifier)는 "`static`"과 "`public`"이다. 가능한 제한자가 모두 합쳐서 대략 반 다스(half-dozen) 밖에 없다.

서브루틴이 일부 값을 계산하는 함수인 경우, {return-type}을 사용하여 함수에 의해 반환되는 값의 자료형을 지정한다. 이는 String 또는 int와 같은 자료형 이름일 수도 있고, double[] 같은 배열 자료형일 수도 있다. 제4장 제4절에서 함수과 반환 자료형을 자세히 살펴볼 것이다. 서브루틴이 함수가 아닌 경우 {return-type}은 반환되는 값이 없음을 나타내는 특수한 값인 `void`로 대체된다. "void"라는 용어는 반환 값이 비어 있거나 존재하지 않음을 표시하기 위한 것이다.

마지막으로, 메서드의 {parameter-list}에 이르른다. 매개변수(parameter)는 서브루틴의 인터페이스 일부이다. 이들은 서브루틴의 내부 계산에서 사용되도록 외부로부터 서브루틴으로 전달되는 정보를 나타낸다. 구체적인 예를 들면, `changeChannel()`이라는 메소드를 포함하는 Television이라는 클래스를 상상해보자. 즉각적인 질문: 어떤 채널로 바꿀까? 이 질문에 답하기 위해 매개변수를 사용할 수 있다. 채널 번호가 정수일 경우 매개변수의 자료형은 int가 되며 `changeChannel()` 메서드의 선언은 다음과 같이 보일 터이다:

```java
public void changeChannel(int channelNum) { ... }
```

이 선언은 `changeChannel()`에 int 자료형의 `channelNum`라는 매개변수가 있음을 명시한다. 그러나 `channelNum`에는 아직 특별한 값이 없다. 서브루틴이 호출될 때 `channelNum`의 값이 제공된다; 예를 들면: `changeChannel(17);`

서브루틴의 매개변수 목록은 비어 있거나, 하나 또는 그 이상으로 된 {type parameter-name} 형식의 매개변수 선언으로 구성될 수 있다. 여러 선언들이 있을 경우, 이들은 쉼표(comma)로 구분된다. 각 선언은 하나의 매개변수만 지정할 수 있다는 점에 유의하라. 예를 들어, double 자료형의 두 매개변수를 원한다면, "`double x, y`"라기 보다 "`double x, double y`"라고 말해야 한다.

매개변수는 다음 절에서 자세히 다룬다.

여기에 서브루틴 정의에 관한 몇 가지 예제가 있는데, 서브루틴이 수행하는 작업을 정의하는 문장은 제외한 것이다:
```java
public static void playGame() {
    // "public" 및 "static" 은 제한자이다; "void"는
    // 반환 자료형이다; "playGame"은 서브루틴의 이름이다;
    // 매개변수 목록은 비어 있다.
    . . .  // playGame이 무얼 하는지를 정의하는 문장들이 여기에 이어진다.
}

int getNextN(int N) {
    // 제한자가 없다; "int"는 반환 자료형이다;
    // "getNextN" 은 서브루틴의 이름이다; 매개변수 목록은
    // 하나이며 그 이름은 "N"이고, 이것의
    // 자료형은 "int"이다.
    . . .  // getNextN이 무얼 하는지를 정의하는 문장들이 여기에 이어진다.
}

static boolean lessThan(double x, double y) {
    // "static"은 제한자이다; "boolean"은
    // 반환 자료형이다; "lessThan"은 서브루틴의 이름이다;
    // 매개변수 항목은 둘이며 그 이름들은
    // "x" 및 "y"이고, 매개변수들 각각의 자료형은
    // "double"이다.
    . . .  // lessThan이 무얼 하는지를 정의하는 문장들이 여기에 이어진다.
}
```

여기서 제시된 두 번째 예에서 `getNextN`은 비정적 메서드인데, 왜냐하면 "`static`"이란 제한자가 포함되어 있지 않기 때문이며 — 따라서 이 장에서 살펴봐야 할 예가 아니다! 이 예에서 보여지는 다른 제한자는 "`public`"이다. 이 제한자는 해당 메소드가 프로그램의 어느 곳에서도, 심지어 메서드가 정의된 클래스 밖에서도 호출될 수 있음을 나타낸다. 같은 클래스 **안에서만** 메서드를 호출할 수 있음을 나타내는 또 다른 제한자인 "`private`"가 있다. `public` 및 `private` 제한자를 **액세스 지정자(access specifier)** 라고 한다. 메서드에 대하여 액세스 지정자가 제공되지 않는 경우, 이는 기본적으로(by default), 클래스를 포함하는 패키지 안의 어떤 위치에서든 해당 메서드를 호출할 수 있지만 해당 패키지 밖에서는 호출할 수 없음을 뜻한다. (이 장의 뒷부분인 제4장 제6절에서 패키지에 대해 자세히 배울 것이다.) `protected`라는 다른 액세스 제한자가 하나 있는데, 이는 제5장의 객체 지향 프로그래밍으로 넘어갈 때에만 관련성을 가질 것이다.

그건 그렇고, 프로그램의 `main()` 루틴은 서브루틴의 일반적인 구문 규칙을 따른다는 점에 주의하라. 다음에서

```java
public static void main(String[] args) { ... }
```

제한자는 `public` 및 `static`, 반환 자료형은 `void`, 서브루틴 이름은 `main`, 매개변수 목록은 "`String[] args`"이다. 이 경우 매개변수의 자료형은 배열 자료형 String[]이다.

이미 서브루틴의 구현을 채워넣은 경험이 있을 터이다. 이 장에서는 인터페이스 부분을 포함하여 자신만의 완전한 서브루틴 정의를 작성하는 법을 전부 배우게 될 것이다.

<hr>

## 2. 서브루틴 호출
서브루틴을 정의할 때, 서브루틴이 존재하며 서브루틴이 무엇을 하는지를 컴퓨터에 알려주는 것이 할 일의 전부다. 서브루틴은 호출되기 전까지는 실제로 실행되지 않는다. (이는 클래스의 `main()` 루틴에도 심지어 해당되는 것이다 — 비록 당신이 호출하지는 않지만, 이는 시스템이 프로그램을 실행할 때 시스템에 의해 호출된다.) 예를 들어, 위의 예로 제시된 `playGame()` 메서드는 다음과 같은 서브루틴 호출문을 사용하여 호출될 수 있다:

```java
playGame();
```

이 문장은 `main()` 메서드든 다른 서브루틴이든 `playGame()`의 정의가 포함된 동일한 클래스가 있는 곳이면 어디든지 나타날 수 있다. `playGame()`은 `public` 메서드이기 때문에 다른 클래스에서도 호출할 수 있지만, 그럴 경우 어느 클래스에서 이것이 오는지를 컴퓨터에게 알려야 한다. `playGame()`은 `static` 메서드이기 때문에 그 전체 이름에 이것이 정의된 클래스의 이름이 포함되어 있다. 예를 들어, `playGame()`이 `Poker`라는 클래스에서 정의된다고 하자. 그러면 `Poker` 클래스 **밖에서** `playGame()`을 호출하기 위해 이렇게 해야 할 것이다:

```java
Poker.playGame();
```

여기서 사용한 클래스 이름은 컴퓨터가 메서드를 찾기 위해 어떤 클래스를 검색해야 하는지를 알려준다. 또한 `Poker.playGame()`와, `Roulette.playGame()` 혹은 `Blackjack.playGame()`처럼 다른 클래스에서 정의된 다른 잠재적인 `playGame()` 메서드를 구별할 수 있게 한다.

보다 일반적으로는, `static` 서브루틴에 대한 **서브루틴 호출문(subroutine call statement)** 은 호출되는 서브루틴이 동일한 클래스에 있는 경우라면 다음의 형식을 취하며:

```java
subroutine-name(parameters);
```

혹은 서브루틴이 다른 클래스 어딘가에 정의된 경우라면 다음의 형식을 취한다:

```java
class-name.subroutine-name(parameters);
```

(비정적 메서드는 클래스보다는 객체에 속하며, 클래스 이름 대신 객체를 사용하여 호출된다. 이는 나중에 더 다룰 것이다.) 매개변수는 `playGame()` 예에서 보듯 비워둘 수 있지만, 괄호 사이에는 아무 것도 없더라도 반드시 괄호는 있어야 한다는 점에 유의하라. 서브루틴을 호출할 때 제공하는 매개변수의 갯수는 서브루틴 정의의 매개변수 목록에서 지정된 숫자와 일치해야 하며, 호출문에 있는 매개변수의 자료형은 서브루틴 정의의 자료형과 일치해야 한다.

<hr>

## 3. 프로그램의 서브루틴
`main()` 루틴 외에 다른 서브루틴을 포함했을 때, 완전한 프로그램이 어떤 모습인지 예를 들어볼 때가 되었다. 사용자와 함께 추측 게임(guessing game)을 하는 프로그램을 작성해보자. 컴퓨터는 1에서 100 사이의 무작위 숫자를 선택할 것이고, 사용자는 이를 추측하려고 할 것이다. 컴퓨터는 사용자에게 추측한 값이 높은지 낮은지 또는 정확한지 여부를 알려준다. 사용자가 6회 또는 그 이하의 추측을 한 후 해당 숫자를 얻으면, 게임에서 승리한다. 각 게임이 끝나면 사용자는 다른 게임을 계속할 수 있다.

하나의 게임을 하는 것은 단일의 일관된 작업으로 생각할 수 있기 때문에, 사용자와 하나의 추측 게임을 하는 서브루틴을 작성하는 것이 타당하다. `main()` 루틴은 사용자가 원하는 횟수만큼 반복해서 `playGame()` 서브루틴을 호출하기 위해, 하나의 루프를 사용한다. `main()` 루틴을 작성하는 것과 같은 방법으로 `playGame()` 서브루틴을 설계하는 문제에 접근한다: 알고리즘의 초안(outline)부터 시작하여 단계적 개량(stepwise refinement)을 적용하라. 다음은 추측 게임 루틴을 위한 짧은 유사코드 알고리즘이다:

```html
무작위 숫자를 선택한다
게임이 끝나지 않는 동안에:
    사용자의 추측을 얻는다
    사용자에게 추측이 높은지, 낮은지, 또는 정확한지를 알려준다
```

사용자가 정확하게 추측하거나 추측 횟수가 6회일 경우 게임이 끝나기 때문에, 게임 종료 여부에 관한 테스트는 복잡하다. 많은 경우와 마찬가지로, "`while (true)`" 루프를 사용하고 그러한 사유를 찾을 때마다 `break`를 사용하여 루프를 끝내는 것이 가장 쉽다. 또한, 6회의 추측 끝에 게임을 끝내려면 사용자가 몇 번을 추측했는지를 계속 추적해야 할 것이다. 알고리즘 채우기는 다음과 같이 주어진다:

```html
설정 computersNumber는 1과 100 사이의 무작위 숫자
설정 guessCount = 0
while (true):
    사용자의 추측을 얻는다
    1을 guessCount에 추가하여 추측의 수를 센다
    if 사용자의 추측이 computersNumber와 같다면:
        사용자가 이겼음을 알린다
        루프를 중단한다
    if 추측의 횟수가 6이라면:
        사용자가 졌음을 알린다
        루프를 중단한다
    if 사용자의 추측이 computersNumber보다 작다면:
        추측이 낮다는 사실을 사용자에게 알린다
    else if 사용자의 추측이 computersNumber보다 크다면:
        추측이 높다는 사실을 사용자에게 알린다
```

변수 선언이 추가되고 자바로 번역되면, `playGame()` 루틴의 정의가 된다. 1과 100 사이의 무작위 정수는 `(int)(100 * Math.random()) + 1`로 계산될 수 있다. 필자는 사용자와의 상호작용을 정리하여 흐름을 좋게 하였다.

```java
static void playGame() {
    int computersNumber; // 컴퓨터에 의해 선택된 무작위 숫자.
    int usersGuess;      // 사용자가 추측으로 입력한 숫자.
    int guessCount;      // 사용자가 한 추측의 횟수.
    computersNumber = (int)(100 * Math.random()) + 1;
                    // computersNumber에 할당된 값은
                    // 무작위로 선택된 1과 100 사이의 정수.
    guessCount = 0;
    System.out.println();
    System.out.print("첫 번째 추측은 무엇입니까? ");
    while (true) {
        usersGuess = TextIO.getInt();  // 사용자의 추측을 얻음.
        guessCount++;
        if (usersGuess == computersNumber) {
            System.out.println("다음 " + guessCount
                + " 번의 추측으로 맞추었네요! 나의 숫자는 " + computersNumber);
            break;  // 게임이 종료됨; 사용자가 승리함.
        }
        if (guessCount == 6) {
            System.out.println("6번의 추측으로 숫자를 얻지 못했군요.");
            System.out.println("게임에서 졌습니다. 나의 숫자는 " + computersNumber);
            break;  // 게임이 종료됨; 사용자가 패배함.
        }
        // 이 시점에 도달한 경우 게임은 계속됨.
        // 추측이 너무 높다거나 낮다는 점을 사용자에게 알림.
        if (usersGuess < computersNumber)
            System.out.print("이는 너무 낮습니다. 다시 시도하세요: ");
        else if (usersGuess > computersNumber)
            System.out.print("이는 너무 높습니다. 다시 시도하세요: ");
    }
    System.out.println();
} // playGame() 종료
```

자, 이를 정확히 어디에 두어야 할까? 이는 `main()` 루틴과 같은 클래스에 속해야 하지만, `main()` 루틴 내부에 있어서는 **안 된다**. 하나의 서브루틴이 다른 서브루틴 안에 물리적으로 중첩되는 것은 허용되지 않는다. `main()` 루틴은 `playGame()`을 **호출하지만**, 그 정의를 보유하지는 않으며 호출문만을 가진다. `playGame()`의 정의를 `main()` 루틴의 앞이나 뒤에 둘 수 있다. 자바는 하나의 클래스에 어떤 특정한 순서에 따라 서브루틴 정의를 두는 것에 그다지 까다롭지 않다.

`main()` 루틴을 작성하는 것은 꽤나 쉽다. 전에도 이런 일을 했었다. 전체 프로그램의 모습은 다음과 같다(중요한 프로그램은 필자가 여기에 포함시킨 것보다 더 많은 주석이 필요하다는 점을 제외한다면).

```java
import textio.TextIO;

public class GuessingGame {

    public static void main(String[] args) {
        System.out.println("게임을 합시다. 1과 100 사이의 숫자를");
        System.out.println("내가 고르고, 당신이 이를 추측해보세요.");
        boolean playAgain;
        do {
            playGame();  // 하나의 게임을 하는 서브루틴을 호출
            System.out.print("다시 게임을 하겠습니까?");
            playAgain = TextIO.getlnBoolean();
        } while (playAgain);
        System.out.println("게임을 해주셔서 감사합니다. 안녕히.");
    } // main() 종료

    static void playGame() {
        int computersNumber; // 컴퓨터에 의해 선택된 무작위 숫자.
        int usersGuess;      // 사용자가 추측으로 입력한 숫자.
        int guessCount;      // 사용자가 한 추측의 횟수.
        computersNumber = (int)(100 * Math.random()) + 1;
        // computersNumber에 할당된 값은
        // 무작위로 선택된 1과 100 사이의 정수.
        guessCount = 0;
        System.out.println();
        System.out.print("첫 번째 추측은 무엇입니까? ");
        while (true) {
        usersGuess = TextIO.getInt();  // 사용자의 추측을 얻음.
        guessCount++;
        if (usersGuess == computersNumber) {
            System.out.println("다음 " + guessCount
                + " 번의 추측으로 맞추었네요! 나의 숫자는 " + computersNumber);
            break;  // 게임이 종료됨; 사용자가 승리함.
        }
        if (guessCount == 6) {
            System.out.println("6번의 추측으로 숫자를 얻지 못했군요.");
            System.out.println("게임에서 졌습니다. 나의 숫자는 " + computersNumber);
            break;  // 게임이 종료됨; 사용자가 패배함.
        }
        // 이 시점에 도달한 경우 게임은 계속됨.
        // 추측이 너무 높다거나 낮다는 점을 사용자에게 알림.
        if (usersGuess < computersNumber)
            System.out.print("이는 너무 낮습니다. 다시 시도하세요: ");
        else if (usersGuess > computersNumber)
            System.out.print("이는 너무 높습니다. 다시 시도하세요: ");
        }
        System.out.println();
    } // playGame() 종료

} // 클래스 GuessingGame 종료
```

시간을 내어 프로그램을 주의 깊게 읽고 어떻게 작동하는지 이해하라. 그리고 이처럼 비교적 간단한 경우에도, 프로그램을 두 가지 메서드로 세분화하면 프로그램을 이해하기 쉽고 아마도 각각의 조각을 작성하기 쉽게 만든다는 점을 스스로 납득하도록 노력하라.

<hr>

## 4. 멤버 변수
클래스는 서브루틴 외에 다른 것들을 포함할 수 있다. 특히 변수 선언도 포함할 수 있다. 물론 서브루틴 **내부에** 변수를 선언할 수도 있다. 이들을 **지역 변수(local variable)** 라고 한다. 그러나 서브루틴의 일부분이 아닌 변수를 가질 수도 있다. 이러한 변수를 지역 변수와 구별하기 위해 **멤버 변수(member variable)** 라고 부를 수 있는데, 이들은 한 클래스의 멤버이기 때문이다. 이들을 위한 다른 용어는 **전역 변수(global variable)** 다.

서브루틴과 마찬가지로, 멤버 변수는 정적이거나 정적이 아닐 수 있다. 이 장에서는 정적 변수(static variable)를 고수할 것이다. 정적 멤버 변수는 클래스 전체에 속하며 클래스가 존재하는 한 존재한다. 자바 인터프리터가 클래스를 처음 로드할 때 변수에 메모리가 할당된다. 변수에 값을 할당하는 할당문은 프로그램에서 해당 할당문이 어디에 있든 간에 해당 메모리의 내용을 변경한다. 변수가 표현식에서 사용될 때마다 프로그램의 표현식이 어디에 있든 간에 해당 값은 동일한 메모리에서 가져오게 된다. 이는 정적 멤버 변수의 값이 하나의 서브루틴에서 설정되고 다른 서브루틴에서 이것이 사용될 수 있음을 의미한다. 정적 멤버 변수는 클래스의 모든 정적 서브루틴에 의해 "공유"된다. 반면에 서브루틴의 지역 변수는 서브루틴이 실행되는 동안에만 존재하며, 해당 서브루틴의 외부에서는 완전히 접근할 수 없다.

멤버 변수의 선언은 두 가지를 제외하면 지역 변수의 선언과 마찬가지다: 멤버 변수는 서브루틴 외부에 선언되며(아직 클래스 안에 있어야 하지만), 해당 선언에는 `static`, `public`, 그리고 `private`와 같은 제한자가 표시될 수 있다. 현재는 정적 멤버 변수만을 다루고 있기 때문에, 이 장에 있는 멤버 변수의 모든 선언에는 `static`이라는 제한자가 포함될 것이다. 이들은 또한 `public` 또는 `private`로 표시될 수도 있다. 예를 들면:

```java
static String usersName;
public static int numberOfPlayers;
private static double velocity, time;
```

`private`라고 선언되지 않은 정적 멤버 변수는 이것이 정의된 클래스 내부뿐만 아니라 외부에서도 접근할 수 있다. 다른 클래스에서 이들이 사용될 경우, {class-name.variable-name} 형식의 복합 식별자와 함께 참조되어야 한다. 예를 들어 System 클래스에는 `out`이란 이름의 공개(public) 정적 멤버 변수가 포함되어 있으며, `System.out`을 참조하여 자신의 클래스에서 이 변수를 사용하게 된다. 마찬가지로, `Math.PI`는 Math 클래스의 공개 정적 멤버 변수다. `numberOfPlayers`가 `Poker`라는 이름의 클래스에서 공개 정적 멤버 변수인 경우, `Poker` 클래스의 코드는 단순히 `numberOfPlayers`라고 참조되는 반면, 다른 클래스에서의 코드는 `Poker.numberOfPlayers`라고 참조된다.

예를 들어, 앞서 이 절에서 작성한 `GuessingGame` 클래스에 정적 멤버 변수를 두어 개 추가해보자. 사용자가 몇 번의 게임을 실행했는지 추적하기 위해 `gamesPlayed`라는 이름의 변수를 추가하고, 사용자가 승리한 게임의 수를 추적하기 위해 `gamesWon`이라는 다른 변수를 추가한다. 해당 변수들은 정적 멤버 변수로 선언된다:

```java
static int gamesPlayed;
static int gamesWon;
```

`playGame()` 루틴에서는 항상 `gamesPlayed`에 1을 추가하고, 사용자가 게임에서 이긴 경우 `gamesWon`에 1을 추가한다. `main()` 루틴의 마지막에는 두 변수의 값을 출력한다. 동일한 일을 지역 변수로는 할 수 없을 것인데, 왜냐하면 두 서브루틴 모두 변수에 접근할 필요가 있고, 지역 변수는 하나의 서브루틴에만 존재하기 때문이다. 게다가 전역 변수는 하나의 서브루틴 호출과 그 다음의 서브루틴 호출 사이에 값을 유지한다. 지역 변수는 그렇지 않다; 지역 변수는 이를 포함하는 서브루틴이 호출될 때마다 새로운 값을 얻는다.

서브루틴에서 지역 변수를 선언할 때, 해당 변수에 값을 할당해야 작업을 수행할 수 있다. 반면에 멤버 변수는 자동적으로 기본값으로 초기화된다. 기본값은 배열의 요소를 초기화할 때 사용되는 값과 동일하다: 숫자 변수의 경우 기본값은 0이다; boolean 변수의 경우 기본값은 `false`이다; char 변수의 경우 유니코드 코드 번호가 0인 문자이다; String 과 같은 객체의 경우 기본 초기값은 특수한 값인 `null`이다.

`gamesPlayed` 및 `gamesWon` 정적 멤버 변수들은 int 자료형이기 때문에, 이들은 자동적으로 초기값 0을 얻는다. 이는 카운터(counter)로 사용 중인 변수에 대한 정확한 초기값이다. 물론 기본적인 초기값에 만족하지 못하거나 초기값을 보다 명시적으로 지정하려는 경우, `main()` 루틴 시작시에 변수에 값을 할당할 수 있다.

다음은 `GuessingGame.java`의 개정된 버전이다. 위 버전의 변경 사항은 빨간색으로 표시된다:

(역주: 기술적인 문제 때문에 색상 표시가 아닌 ** 표시로 대체합니다.)

```java
import textio.TextIO;

public class GuessingGame2 {

    static int gamesPlayed;   // ** 플레이 된 게임의 횟수.
    static int gamesWon;      // ** 승리한 게임의 횟수.

    public static void main(String[] args) {
       gamesPlayed = 0;     // **
       gamesWon = 0;  // ** 이는 실제로 중복인데, 
                      // 0은 기본적인 초기값이기 때문임.
       System.out.println("게임을 합시다. 1과 100 사이의 숫자를");
       System.out.println("내가 고르고, 당신이 이를 추측해보세요.");
       boolean playAgain;
       do {
          playGame();  // 하나의 게임을 하는 서브루틴을 호출
          System.out.print("다시 게임을 하겠습니까?");
          playAgain = TextIO.getlnBoolean();
       } while (playAgain);
       System.out.println();    // **
       System.out.println("당신은 " + gamesPlayed + " 번의 게임을 하였고,");     // **
       System.out.println("당신은 " + gamesWon + " 번의 게임을 이겼습니다.");   // **
       System.out.println("게임을 해주셔서 감사합니다. 안녕히.");
    } // main() 종료          

    static void playGame() {
        int computersNumber; // 컴퓨터에 의해 선택된 무작위 숫자.
        int usersGuess;      // 사용자가 추측으로 입력한 숫자.
        int guessCount;      // 사용자가 한 추측의 횟수.
        gamesPlayed++;  // ** 해당 게임의 수를 셈.
        computersNumber = (int)(100 * Math.random()) + 1;
                 // computersNumber에 할당된 값은
                 // 무작위로 선택된 1과 100 사이의 정수.
        guessCount = 0;
        System.out.println();
        System.out.print("첫 번째 추측은 무엇입니까? ");
        while (true) {
           usersGuess = TextIO.getInt();  // 사용자의 추측을 얻음.
           guessCount++;
           if (usersGuess == computersNumber) {
              System.out.println("다음 " + guessCount
                      + " 번의 추측으로 맞추었네요! 나의 숫자는 " + computersNumber);
              gamesWon++;  // 해당 승리의 수를 셈.
              break;       // 게임이 종료됨; 사용자가 승리함.
           }
           if (guessCount == 6) {
              System.out.println("6번의 추측으로 숫자를 얻지 못했군요.");
              System.out.println("게임에서 졌습니다. 나의 숫자는 " + computersNumber);
              break;  // 게임이 종료됨; 사용자가 패배함.
           }
           // 이 시점에 도달한 경우 게임은 계속됨.
           // 추측이 너무 높다거나 낮다는 점을 사용자에게 알림.
           if (usersGuess < computersNumber)
              System.out.print("이는 너무 낮습니다. 다시 시도하세요: ");
           else if (usersGuess > computersNumber)
              System.out.print("이는 너무 높습니다. 다시 시도하세요: ");
        }
        System.out.println();
    } // playGame() 종료

} // 클래스 GuessingGame2 종료
```

(그런데, 필자의 예제 프로그램에서는 정적 서브루틴이나 변수를 `public` 또는 `private`로 표시하지 않았다는 점에 주목하라. 두 제한자를 모두 빼놓으면 무슨 의미인지 궁금할 것이다. 액세스 제한자가 없는 전역 변수와 서브루틴은 이들이 정의된 클래스가 있는 동일한 패키지의 어디에서나 사용할 수 있지만, 다른 패키지에서는 아니라는 점을 떠올려라. 패키지를 선언하지 않는 클래스는 기본 패키지 안에 있다. 따라서 기본 패키지의 클래스는 `gamesPlayed`, `gamesWon`, 그리고 `playGame()`에 접근할 수 있을 것이다 — 또한 이는 이 교재의 대부분의 클래스에도 해당된다. 사실, 달리 그렇게 할 이유가 없는 한 멤버 변수와 서브루틴을 `private`로 하는 것이 좋은 관행으로 여겨진다. (그러나 한편으로는 기본 패키지를 사용하지 않는 것도 좋은 관행으로 여겨진다.))
