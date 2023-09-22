# Section 4: return 값

값을 반환하는 서브루틴 을 **함수(function)** 라 한다. 주어진 함수는 함수의 **반환 자료형(return type)** 이라 불리는, 지정된 자료형의 값만 반환할 수 있다. 함수 호출은 일반적으로, 할당문의 오른쪽, 서브루틴 호출의 실제 매개변수, 혹은 몇몇 큰 표현식의 중간과 같이 컴퓨터가 값을 찾기를 기대하는 위치에서 나타난다. 부울 값 함수는 심지어 `if`, `while`, `for` 또는 `do..while` 문에서 테스트 조건으로 사용될 수도 있다.

(마치 일반적인 서브루틴인 것처럼 독립된 문장으로 함수 호출을 사용하는 것 또한 허용된다. 이 경우 컴퓨터는 서브루틴에 의해 계산된 값을 무시한다. 때때로 이것은 타당하다. 예를 들어, String 반환 자료형을 가지는 `TextIO.getln()` 함수는 사용자가 타이핑한 입력 행을 읽고 반환한다. 통상적으로, 반환되는 행은 "`name = TextIO.getln();`" 문장에서처럼 프로그램 후반에 사용할 변수에 할당된다. 그러나, 이 함수는 서브루틴 호출문 "`TextIO.getln();`"으로도 유용하며, 이는 여전히 다음의 캐리지 리턴을 포함하는 모든 입력을 읽는다. 반환 값은 변수에 할당되거나 표현식에 사용되지 않았기에, 이는 단순히 폐기된다. 따라서 서브루틴 호출의 효과는 몇몇 입력을 읽고 **그리고 폐기하는** 것이다. 때로는 원하지 않는 입력을 버리는 것이 정확히 해야 할 일인 것이다.)

<hr>

## 1. 반환 문
이미 `Math.sqrt()`와 `TextIO.getInt()`와 같은 함수를 어떻게 사용할 수 있는지 보았을 것이다. 아직 보지 못한 것은 자신만의 함수를 작성하는 방법이다. 함수는 서브루틴이 반환할 값을 지정해야 한다는 점을 제외하면, 일반적인 서브루틴과 동일한 형태를 취한다. 이는 다음과 같은 구문을 가진 반환 문을 사용하여 이루어진다:

```java
return expression;
```

이러한 `return` 문은 함수의 정의 안에서만 나타날 수 있으며, {expression}의 자료형은 함수에서 지정된 반환 자료형과 일치해야 한다. (더 정확히 하자면, 함수의 반환 자료형에 의해 그 자료형이 지정된 변수에 할당이 허용될 수 있는 표현식이어야 한다.) 컴퓨터가 이 `return` 문을 실행하면 표현식을 평가하고, 함수 실행을 종료하며, 표현식의 값을 함수의 반환 값으로 사용한다.

예를 들어, 다음의 함수 정의를 생각해보자:

```java
static double pythagoras(double x, double y) {
    // 각 변이 x와 y인 직각 삼각형의
    // 빗변의 길이를 계산한다.
    return  Math.sqrt( x*x + y*y );
}
```

컴퓨터가 "`totalLength = 17 + pythagoras(12,5);`"라는 문장을 실행한다고 가정하자. `pythagoras(12,5)` 항목에 컴퓨터가 도달하면, 이는 함수의 형식적 매개변수 `x`와 `y`에 실질적 매개변수 `12`와 `5`를 할당한다. 함수의 본체에서는 `13.0`으로 계산되는 `Math.sqrt(12.0*12.0 + 5.0*5.0)`를 평가한다. 이 값은 함수에 의해 "반환"되기 때문에 `13.0`은 본질적으로 할당문의 함수 호출을 대체하며, 이는 "`totalLength = 17+13.0`" 문장과 동일한 효과를 가진다. 반환 값은 `17`에 추가되며, 그 결과인 `30.0`은 변수 `totalLength`에 저장된다.

`return` 문은 함수 정의의 마지막 문장일 필요는 없다는 점에 유의하라. 반환할 값을 알고 있는 함수의 어느 지점에서라도 이를 반환할 수 있다. 값을 반환하면, 함수의 후속 문장들을 건너뛰면서 함수가 즉시 종료된다. 하지만, 함수의 실행이 코드를 통해 어떤 경로를 취하든 간에, 함수가 분명 어떠한 값을 반환하는 경우임에는 틀림없다.

선언된 반환 자료형 "`void`"를 가진, 일반적인 서브루틴 내에서 반환 문을 사용할 수 있다. `void` 서브루틴은 값을 반환하지 않기 때문에, `return` 문에는 표현식이 포함되지 않는다; 단순히 "`return;`"이란 형식을 가진다. 이 문장의 효과는 서브루틴의 실행을 중단하고 서브루틴이 호출된 프로그램의 지점으로 되돌아가는 것이다. 서브루틴의 중간 어딘가에서 실행을 중단하려면 이 방법이 편리할 수 있지만, 함수가 아닌 서브루틴에서는 `return` 문이 선택적이다. 반면에 함수에서는 표현식과 함께 항상 반환 문이 필요하다.

루프 내부의 `return`은 이를 포함하는 서브루틴뿐만 아니라 해당 루프도 종료한다는 점에 유의하라. 마찬가지로 `switch` 문의 `return`은 서브루틴뿐만 아니라 `switch` 문도 벗어난다. 그렇기에, `break`을 보는 것이 익숙한 문맥상의 지점에서 때때로 `return`을 사용하게 될 것이다.

<hr>

## 2. 함수 예제
여기 3N+1 순서를 계산하는 프로그램에서 사용될 수 있는 매우 간단한 함수가 있다. (3N+1 순서 문제는 이전 절을 포함하여 이미 여러 번 살펴왔던 문제다.) 3N+1 순서에서 하나의 항목이 주어지면, 이 함수는 순서의 다음 항을 계산한다:

```java
static int nextN(int currentN) {
    if (currentN % 2 == 1)     // 현재의 N이 홀수인지를 테스트
        return 3*currentN + 1;  // 그러하다면 이 값을 반환
    else
        return currentN / 2;    // 그렇지 않다면 이 값을 대신 반환
}
```

이 함수에는 두 개의 `return` 문이 있다. 함수의 값을 부여하기 위해 두 개의 `return` 문들 중 정확히 하나가 실행된다. 몇몇 사람들은 가능한 한 함수 맨 끝에 단일의 `return` 문을 사용하는 것을 선호한다. 이는 독자가 쉽게 `return` 문을 찾을 수 있도록 한다. 예를 들면, 다음과 같이 `nextN()`을 작성하길 선택할 수도 있다:

```java
static int nextN(int currentN) {
    int answer;  // answer는 반환될 값이 된다
    if (currentN % 2 == 1)    // 현재의 N이 홀수인지를 테스트
        answer = 3*currentN+1; // 그러하다면, 이것이 answer가 된다
    else
        answer = currentN / 2; // 그렇지 않다면, 이것이 answer가 된다
    return answer;   // (answer를 반환하는 일을 잊지 마라!)
}
```

이 `nextN` 함수를 사용하는 서브루틴이 여기에 있다. 이 경우, 제4장 제3절 서브루틴 버전으로부터 개선은 크지 않지만, `nextN()`이 만약 복잡한 연산을 수행하는 긴 함수였다면, 함수 내부에 그러한 복잡성을 감추는 것에는 많은 의미가 있을 것이다:

```java
static void print3NSequence(int startingValue) {

    int N;       // 순서에서의 항목들 중 하나.
    int count;   // 발견된 항목들의 갯수.
    
    N = startingValue;   // 순서를 startingValue로 시작.
    count = 1;
    
    System.out.println("3N+1 순서를 다음 숫자부터 시작: " + N);
    System.out.println();
    System.out.println(N);  // 순서의 초기 항목을 출력
    
    while (N > 1) {
        N = nextN( N );   // 함수 nextN을 사용하여 다음 항목을 계산.
        count++;          // 이 항목의 수를 셈.
        System.out.println(N);  // 이 항목을 출력.
    }
    
    System.out.println();
    System.out.println("순서에는 " + count + " 개의 항목들이 있습니다.");

}
```

<hr>

여기 함수의 몇 가지 예제들이 더 있다. 첫 번째는 전형적인 등급 범위에서, 주어진 숫자 등급에 대응되는 문자 등급을 계산하는 것이다:

```java
/**
* Returns the letter grade corresponding to the numerical
* grade that is passed to this function as a parameter.
*/
static char letterGrade(int numGrade) {
    if (numGrade >= 90)
        return 'A';   // 90 or above gets an A
    else if (numGrade >= 80)
        return 'B';   // 80 to 89 gets a B
    else if (numGrade >= 65)
        return 'C';   // 65 to 79 gets a C
    else if (numGrade >= 50)
        return 'D';   // 50 to 64 gets a D
    else
        return 'F';   // anything else gets an F

}  // end of function letterGrade
```

`letterGrade()`의 반환 값 자료형은 char이다. 함수는 모든 자료형의 값을 반환할 수 있다. 여기에는 반환 값이 boolean 자료형인 함수가 있다. 이는 몇 가지 흥미로운 프로그래밍 요소들을 보여주므로, 주석을 읽어야 할 것이다:

(역주: 아래의 코드에서 왜 sqrt(N)까지 테스트를 하면 소수를 찾을 수 있는지 이해가 되지 않는다면 아래 링크1의 '제곱근'을 참고하세요.)

```java
/**
* 이 함수는 만약 N이 소수라면 true를 반환한다. 소수란
* 1보다 큰 정수로 그 자신과 1을 제외한 어떤 양의 정수로도
* 나누어지지 않는 수를 말한다. 만약 N이 어떤 약수 D를
* 1 < D < N 범위에서 가진다면, 이는 2부터 Math.sqrt(N)까지의 범위,
* 다시 말해 D 자신 또는 N/D를 약수로 가진다.  따라서 2부터 Math.sqrt(N)까지의
* 가능한 약수만을 테스트한다.
*/
static boolean isPrime(int N) {

    int divisor;  // N을 균등하게 나누는지를 테스트 할 숫자.
    
    if (N <= 1)
        return false;  // <= 1인 숫자는 소수가 아님.
    
    int maxToTry;  // 테스트가 필요한 가장 큰 약수.
    
    maxToTry = (int)Math.sqrt(N);
        // N을 2와 maxToTry 사이의 숫자들로 나눌 것이다.
        // 만약 N이 이들 숫자들로 균등하게 나누어지지 않으면,
        // N은 소수다. (Math.sqrt(N)는 double 자료형의 값을
        // 반환하도록 정의되었기 때문에, maxToTry에 할당되기 전에
        // 해당 값은 int 자료형으로 반드시 형변환을 해야 한다.)
    
    for (divisor = 2; divisor <= maxToTry; divisor++) {
        if ( N % divisor == 0 )  // 약수가 N을 균등히 나누는지 테스트.
            return false;         // 그렇다면 N이 소수가 아님을 알게 된다.
                                // 계속 테스트를 할 필요가 없다!
        }
    
    // 이 시점에 도달하면 N은 분명 소수다. 그렇지 않으면
    // 이전 루프에서 반환 문에 의해 함수가 이미
    // 중단되었을 것이기 때문이다.
    
    return true;  // 그래, N은 소수다.

}  // 함수 isPrime 종료
```

마지막으로, 여기에 반환 자료형이 String 인 함수가 있다. 이 함수는 매개변수로 String 을 가진다. 반환된 값은 매개변수의 역순 복사본(reversed copy)이다. 예를 들어, "Hello World"의 역순은 "dlroW olleH"이다. 문자열의 역순인 `str`을 계산하는 알고리즘은, 빈 문자열로 시작한 다음 str의 마지막 문자부터 시작하여 첫 번째 문자까지 역방향으로 각각의 문자를 추가하는 것이다:

```java
static String reverse(String str) {
    String copy;  // 역순 복사본.
    int i;        // str의 위치 중 하나로,
                // str.length() - 1부터 0까지 감소.
    copy = "";    // 빈 문자열로 시작.
    for ( i = str.length() - 1;  i >= 0;  i-- ) {
        // str의 i번째 char를 copy에 추가.
        copy = copy + str.charAt(i);  
    }
    return copy;
}
```

**회문(palindrome)** 은 "레이더(radar)"와 같이 앞으로 읽으나 뒤로 읽으나 같은 문자열이다. 문자열 `word`가 회문인지 아닌지는 "`if (word.equals(reverse(word)))`"를 테스트하여 확인되는데, 여기에 `reverse()` 함수가 사용될 수 있다.

그건 그렇고, 함수 작성에서 전형적인 초보자의 실수는 해답을 반환하는 대신 이를 출력하는 것이다. **이는 근본적인 오해를 나타낸다.** 함수의 임무는 값을 계산하여 함수가 호출된 프로그램의 지점으로 이를 되돌리는 것이다. 해당 값이 사용되는 곳은 바로 이 지점이다. 아마도 이는 인쇄될 것이다. 어쩌면 변수에 배정될지도 모른다. 혹은 표현식에서 쓰이게 될 것이다. 하지만 이는 함수가 결정할 일이 아니다.

<hr>

## 3. 3N+1 재방문
필자는 3N+1 프로그램의 완전한 새로운 버전으로 이 절을 마치고자 한다. 이를 통해 위에서 정의한 nextN() 함수를 완전한 프로그램에 사용할 수 있는 기회를 얻게 될 것이다. 또한 각 행에 5개의 항목이 있는 열로 순서의 항목들을 인쇄하도록 하여 프로그램을 개선할 수 있는 기회를 가질 것이다. 이는 그 출력을 더 잘 나타낼 수 있게 할 것이다. 아이디어는 다음과 같다: 현재 행에 얼마나 많은 항목들이 인쇄되었는지 계속 추적한다; 그 숫자가 5까지 증가하면, 새로운 행의 출력을 시작한다. 항목들을 깔끔한 열로 정렬하기 위해 형식화된 출력을 사용할 것이다.

```java
import textio.TextIO;

/**
* A program that computes and displays several 3N+1 sequences.  Starting
* values for the sequences are input by the user.  Terms in the sequence
* are printed in columns, with five terms on each line of output.
* After a sequence has been displayed, the number of terms in that
* sequence is reported to the user.
*/
public class ThreeN2 {


public static void main(String[] args) {

      System.out.println("이 프로그램은 3N+1 순서를");
      System.out.println("당신이 지정한 시작 값을 사용하여 출력합니다.");
      System.out.println();

      int K;   // 사용자가 지정한, 순서의 시작점.
      do {
         System.out.println("시작 값을 입력하세요;");
         System.out.print("프로그램을 종료하려면 0을 입력: ");
         K = TextIO.getlnInt();  // 사용자로부터 시작 값을 얻음
         if (K > 0)              // K가 0보다 큰 경우에만 순서를 출력
            print3NSequence(K);
      } while (K > 0);           // K가 0보다 큰 경우에만 순서를 출력

} // main 종료


/**
* print3NSequence prints a 3N+1 sequence to standard output, using
* startingValue as the initial value of N.  It also prints the number
* of terms in the sequence. The value of the parameter, startingValue,
* must be a positive integer.
*/
static void print3NSequence(int startingValue) {

      int N;       // 순서의 항목들 중 하나.
      int count;   // 발견된 항목들의 갯수.
      int onLine;  // 현재 행에서 지금까지
                   // 출력된 항목들의 갯수.

      N = startingValue;   // 순서를 startingValue로 시작;
      count = 1;           // 지금까지 한 항목만 있음.

      System.out.println("3N+1 순서는 다음 숫자로 시작함: " + N);
      System.out.println();
      System.out.printf("%8d", N);  // 8개의 문자를 사용하여 최초 항목을 출력.
      onLine = 1;        // 현재 출력 행에는 이제 1개의 항목이 있음.

      while (N > 1) {
          N = nextN(N);  // 다음 항목을 계산
          count++;   // 이 항목의 수를 셈
          if (onLine == 5) {  // 현재 출력 행이 가득 찬 경우
             System.out.println();  // ... 캐리지 리턴을 출력하고
             onLine = 0;      // ... 새로운 행에 관한
                              // 항목이 더는 없음을 표시한다
          }
          System.out.printf("%8d", N);  // 8칸 열로 해당 항목을 출력.
          onLine++;   // 해당 행의 항목들 갯수에 1을 더함.
      }

      System.out.println();  // 현재 출력된 행을 종료하고
      System.out.println();  // 빈 행을 추가함
      System.out.println("순서에는 " + count + " 개의 항목들이 있습니다.");

}  // print3NSequence 종료


/**
* nextN computes and returns the next term in a 3N+1 sequence,
* given that the current term is currentN.
*/
    static int nextN(int currentN) {
        if (currentN % 2 == 1)
            return 3 * currentN + 1;
        else
            return currentN / 2;
    }  // nextN() 종료


} // 클래스 ThreeN2 종료
```

이 프로그램을 주의 깊게 읽고 어떻게 작동하는지를 이해하도록 노력해야 할 것이다.
