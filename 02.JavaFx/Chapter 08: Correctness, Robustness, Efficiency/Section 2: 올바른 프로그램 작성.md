# 올바른 프로그램 작성

올바른(Correct) 프로그램은 그냥 만들어지는 것이 아니다. 프로그램의 오류를 방지하려면 세부적인 계획과 주의가 필요하다. 프로그래머가 자신의 프로그램이 정확할 가능성을 높이기 위해 사용할 수 있는 몇 가지 기술이 있다.

## 1. 입증 가능한 올바른 프로그램

어떤 경우에는 프로그램이 정확하다는 것을 **증명(prove)** 하는 것이 가능하다. 즉, 프로그램이 나타내는 일련의 계산이 항상 올바른 결과를 생성한다는 것을 수학적으로 입증하는 것이 가능하다. 엄격한 증명은 실제로 매우 작은 프로그램에만 적용할 수 있을 만큼 어렵다. 또한 "올바른 결과"가 올바르고 완전하게 지정되었는지 여부에 따라 달라진다. 이미 지적했듯이 사양을 올바르게 충족하는 프로그램이라도 사양이 잘못되면 유용하지 않다. 그럼에도 불구하고 일상적인 프로그래밍에서도 프로그램이 정확하다는 것을 증명하는 데 사용되는 아이디어와 기술 중 일부를 적용할 수 있다.

기본 적인 아이디어는 **프로세스(process)** 와 **상태(state)**이다. 상태는 프로그램 실행 중 특정 순간의 프로그램 실행과 관련된 모든 정보로 구성된다. 상태에는 예를 들어 프로그램의 모든 변수 값, 생성된 출력, 읽기를 기다리는 모든 입력, 컴퓨터가 작동 중인 프로그램의 위치 기록이 포함된다. 프로세스는 컴퓨터가 프로그래밍을 실행할 때 거치는 일련의 상태이다. 이러한 관점에서 볼 때, 프로그램에서 명령문의 의미는 해당 명령문의 실행이 컴퓨터 상태에 미치는 영향으로 표현될 수 있다. 간단 한 예로, `x = 7;` 대입문의 의미는 변수 x에 값이 7이 된다는 것이다 우리는 이 사실을 절대적으로 확신할 수 있으므로 수학적 증명의 일부를 구축할 수 있는 것이다. 

실제로, 프로그램을 보고 프로그램 실행 중 특정 지점에서 어떤 사실이 true가 틀림없다고 추론하는 것이 종종 가능하다. 예를 들어 `do` 루프를 고려해보자

```java
do {
    System.out.print("Enter a positive integer: ");
    N = TextIO.getlnInt();
} while (N <= 0);
```

이 루프가 끝나면 변수 N의 값이 0보다 크다는 것을 절대적으로 확신할 수 있다. 이 조건이 충족될 때까지 루프는 종료될 수 없다. 이 사실을 `while` 루프의 의미 중 일부이다. 보다 일반적으로 `while` 루프가 `while(condition)` 테스트를 사용하고 루프에 `break` 문이 없으면 루프가 끝난 후 조건이 false라고 확신할 수 있다. 그런 다음 이 사실을 사용하여 프로그램 실행이 계속될 때 어떤 일이 발생하는지에 대한 추가 추론을 이끌어낼 수 있다. (그런데 루프의 경우 루프가 끝날지 여부에 대한 질문도 걱정해야 한다.)

## 2. 전제 조건(Preconditions)과 사후 조건(Postconditions)

주어진 프로그램 세그먼트가 실행된 후에 true라고 입증될 수 있는 사실을 해당 프로그램 세그먼트의 사후 조건이라고 한다. 사후 조건은 프로그램의 동작에 대해 추가 추론을 할 수 있는 알려진 사실이다. 프로그램 전체의 사후 조건은 프로그램 실행이 완료된 후 true라고 입증될 수 있는 단순한 사실이다. 프로그램의 사후 조건이 프로그램의 사양을 충족한다는 것을 보여줌으로써 프로그램이 올바른 것으로 입증될 수 있다.

모든 변수가 `double` 유형인 다음 프로그램 세그먼트를 고려하자.

```java
disc = B*B - 4*A*C;
x = (-B + Math.sqrt(disc)) / (2*A);
```

고등학교 수학의 이차 공식은 x에 할당된 값이 방정식 `A*^2 + B*x + C = 0`의 해임을 보장한다. 단 disk 값은 0보다 크거나 같다. 그리고 A의 값은 0이 아니다. 이 조건을 보장할 수 있다면 x가 방정식의 해라는 사실이 프로그램 세그먼트의 사후 조건이 된다. 프로그램 세그먼트의 전제 조건이라고 말한다. A가 0이 아니라는 조건도 또 다른 전제 조건이다. 전제 조건은 프로그램이 올바르게 계속되기 위해 프로그램 실행 중 특정 시점에 참이어야 하는 조건으로 정의된다. 전제 조건은 당신이 참이기를 바란다. 프로그램이 정확하기를 원한다면 이는 확인하거나 사실이 되도록 강제해야 하는 것이다.

섹션 4.7.1에서 전제 조건과 사후 조건을 한 번 만났다. 해당 섹션에서는 서브 루틴 계약을 지정하는 방법으로 전제 조건과 사후 조건을 소개했다. 여기서 사용되는 용어에 따르면, 서브 루틴의 전제 조건은 서브 루틴 정의를 구성하는 코드의 전제조건일 뿐이며, 서브 루틴의 사후 조건은 동일한 코드의 사후조건이다. 이 섹션에서는 일반적인 프로그램 정확성에 대해 이야기하는 데 더 유용하도록 이러한 용어를 일반화했다.

더 긴 프로그램 세그먼트를 고려하여 이것이 어떻게 작동하는지 살펴본다.

```java
do {
   System.out.println("Enter A, B, and C.");
   System.out.println("A must be non-zero and B*B-4*A*C must be >= 0.");
   System.out.print("A = ");
   A = TextIO.getlnDouble();
   System.out.print("B = ");
   B = TextIO.getlnDouble();
   System.out.print("C = ");
   C = TextIO.getlnDouble();
   if (A == 0 || B*B - 4*A*C < 0)
      System.out.println("Your input is illegal.  Try again.");
} while (A == 0 || B*B - 4*A*C < 0);

disc = B*B - 4*A*C;
x = (-B + Math.sqrt(disc)) / (2*A);
```

루프가 끝나면 `B*B-4*A*C >= 0`이고 `A != 0`임을 확인할 수 있다. 마지막 두 줄의 전제 조건이 충족되므로 x가 방정식의 해라는 사후 조건도 유효하다. 이 프로그램 세그먼트는 방정식에 대한 해 정확하고 입증 가능하게 계산한다. (실제로 컴퓨터에서 실수를 표현하는 데 문제가 있기 때문에 이는 100% 사실이 아니다.)

다음은 if문 으로 전제 조건을 확인하는 또 다른 변형이다. 솔루션이 계산되고 인쇄되는 if문의 첫 번째 부분에서 전체 조건이 충족되었음을 알 수 있다. 다른 부분에서는 전제 조건 중 하나가 유지되지 않는다는 것을 알 수 있다. 

```java
System.out.println("Enter your values for A, B, and C.");
System.out.print("A = ");
A = TextIO.getlnDouble();
System.out.print("B = ");
B = TextIO.getlnDouble();
System.out.print("C = ");
C = TextIO.getlnDouble();

if (A != 0 && B*B - 4*A*C >= 0) {
   disc = B*B - 4*A*C;
   x = (-B + Math.sqrt(disc)) / (2*A);
   System.out.println("A solution of A*X*X + B*X + C = 0 is " + x);
}
else if (A == 0) {
   System.out.println("The value of A cannot be zero.");
}
else {
   System.out.println("Since B*B - 4*A*C is less than zero, the");
   System.out.println("equation A*X*X + B*X + C = 0 has no solution.");
}
```

프로그램을 작성할 때마다 전제 조건을 주의 깊에 살펴보고 프로그램이 이를 어떻게 처리하는지 생각해 보는 것이 좋다. 종종 전제조건은 프로그램 작성 방법에 대한 단서를 제공할 수 있다.

예를 들어 `A[i]`와 같은 모든 배열 참조에는 조건이 있다. 인덱스는 배열에 대한 유효한 인덱스 범위 내에 있어야 한다. 전제 조건은 `0 <= i < A.length`이다. 컴퓨터는 `A[i]`를 평가할 때 이 조건을 확인 하고, 조건이 만족되지 않으면 프로그램이 종료된다. 이를 방지하려면 인덱스에 합법적인 값이 있는지 확인해야 한다. 배열 A에서 숫자 x를 검색하고 i 값을 설정하는 다음 코드를 살펴본다. x를 포함하는 배열 요소의 인덱스가 된다.

```java
i = 0;
while (A[i] != x) {
    i++;
}
```

이 프로그램 세그먼트는 x가 실제로 배열에 있다는 전제 조건을 갖는다. 이 전제 조건이 충족되면 `A[i] == x`일 때 루프가 종료된다. 즉, 루프가 끝날 때 i의 값은 배열의 x 위치가 된다. 그러나 x가 배열에 없으면 i의 값은 A.length와 같아질 때까지 계속 증가한다. 이때 `A[i]`에 대한 참조는 불법이며 프로그램이 종료된다. 이를 방지하기 위해 `A[i]`를 참조하기 위한 전제 조건이 충족되는지 확인하는 테스트를 추가한다.

```java
i = 0;
while (i < A.length && A[i] != x) {
    i++;
}
```

이제 루프는 확실히 종료된다. 그것이 끝나면 `i == A.length` 또는 `A[i] == x`를 만족하게 된다. 루프 뒤에 if문을 사용하여 루프가 종료되는 조건을 테스트 할 수 있다.
```java
i = 0;
while (i < A.length && A[i] != x) {
    i++;
}

if (i == A.length)
    System.out.println("x is not in the array");
else
    System.out.println("x is in position " + i);
```

## 3. 불변성(Invariants)

루프가 어떻게 작동하는지 더 자세히 살펴본다. 

```java
static int arraySum( int[] A ) {
    int total = 0;
    int i = 0;
    while ( i < A.length ) {
        total = total + A[i];
        i = i + 1;
    }
    return total;
}
```

(그런데 A가 `null`이 아니라는 요구 사항은 서브 루틴의 전제 조건이다. 이를 위반하는 경우 서브루틴의 코드는 NullPointerException을 발생시킨다.)

이 서브루틴이 작동하는지 어떻게 확신할 수 있나? `return`문이 실행될 때 `total`의 값이 A에 있는 모든 요소의 합이라는 것을 증명해야 한다. 이 문제를 생각하는 한 가지 방법은 **루프 불변성(loop invariants)** 의 관점에서 보는 것이다.

루프 불변은 대략적으로 루프가 실행될 때 참으로 유지되는 명령문이다. 보다 정확하게는 다음 사항이 성립하는 경우 해당 명령문이 루프에 대해 불변임을 보여줄 수 있다. 루프 내부의 코드가 실행되기 전에 명령문이 true인 한 루프 내부의 코드가 실행된 후에도 true가 된다. 즉, 루프 불변은 루프 본문의 전제 조건이자 사후 조건이다.

위 서브루틴의 루프에 대한 루프 불변은 "`total`은 A의 첫 번째 i요소의 합과 같다."이다. `while` 루프의 시작 부분에서 이것이 사실이라고 가정한다. 즉, "`total = total + A[i]`" 문이 실행 되기 전 `total`은 배열의 첫 번째 i요소 (A[0] ~ A[i-1])의 합계이다. `A[i]`을 total에 추가한 후 total은 이제 배열의 첫 번째 i+1 요소의 합계이다. 그러나 다음 명령문인 "`i = i + j`"이 실행되자 마자 i를 i+1로 바꾸면 루프 불변이 다시 참이 된다. 우리는 루프 불변이 루프 본문의 시작 부분에서 참이면 끝에서고 true임을 확인했다. 

루프 불변성은 루프 실행 중 모든 지점에서 반드시 참일 필요는 없다. 루프의 명령문 중 하나를 실행하면 루프의 이후 명령문이 다시 true가 되는 한 일시적으로 false가 될 수 있다.

그러면 서브루틴 `arraySum()`이 정확하다는 것을 증명했나? 아직이다. 아직 확인해야할 사항이 몇 가지 있다. 우선, 루프가 처음 실행되기전에 루프 불변성이 참인지 확인해야 한다. 이 시점에서 i는 0이고 total도 0과 같다. 이는 0개 요소의 올바른 합이다. 따라서 루프 불변성은 루프 이전에 참이다. 일단 이를 알게 되면 루프를 실행할 때마다 그 값이 true로 유지된다는 것을 알 수 있으며 특히 루프가 끝나나 후에도 여전히 true라는 것을 알 수 있다.

하지만 이것이 우리에게 도움이 되려면 루프가 실제로 끝나는지 확인해야 한다. 루프를 실행할 때마다 i값은 1씩 증가한다. 이는 결국 `A.length`에 도달해야 함을 의미한다. 그 시점에서 while 루프의 조건은 false이고 루프가 종료된다.

루프가 끝난 후 우리는 i가 `A.length`와 같다는 것을 알고 루프 불변성이 참이라는 것을 알게 된다. 이 시점에서 i는 `A.length` 이므로 루프 불변은 "`total`은 A의 첫 번째 `A.length`" 요소의 합이다. 그러나 여기에는 A의 모든 요소가 포함된다. 따라서 루프 불변은 우리에게 다음을 제공한다. 정확이 우리가 보여주고 싶었던 것이다. 서브루틴에 의해 `total`이 반환될 때, 이는 배열의 모든 요소의 합과 같다.

이는 명백한 것을 증명하기 위해 많은 작업이 필요한 것처럼 보일 수 있다. 그러나 `arraySum()`이 작동하는 것이 분명한 이유를 설명하려고 하면 용어를 사용하지 않더라고 루프 불변 뒤에 있는 논리를 사용하고 있다는 것을 알게 된다.

유사한 예를 살펴보자.

```java
static int maxInArray( int[] A ) {
    int max = A[0];
    int i = 1;
    while ( i < A.length ) {
        if ( A[i] > max )
            max = A[i];
        i = i + 1;
    }
    return max;
}
```

이 경우 "`max`는 A의 첫 번째 i 요소 중 가장 큰 값이다."라고 말하는 루프 불변이 있다. 이 진술은 루프가 시작되기 전, i가 1이고 `max`가 `A[0]`일 때 true이다. if문 앞의 루프 시작 부분에서 이것이 true라고 가정한다. if문 뒤의 `max`는 `A[i]` 보다 크거나 같다. 이는 if문의 사후 조건이고 `A[0] ~ A[i-1]` 보다 크거나 같기 때문이다. 루프 불변의 진리 때문이다. 이 두 가지 사실을 합치면 `max`가 A의 첫 번째 i+1 요소 중 가장 큰 값이라는 것을 알 수 있다. 다음 명령문에서 i가 i+1로 대체되면 루프 불변성은 다시 참이 된다. 루프가 끝난 후 i는 `A.length`이고 루프 불변량은 우리가 알아야 할 사항을 정확하게 알려준다. `max`는 전체 배열에서 가장 큰 값이다.

루프 불변성은 프로그램이 정확하다는 것을 증명하는 데만 유용하는 것이 아니다. 루프 불변성 측면에서 생각하는 것은 알고리즘을 개발하려고 할 때 유용할 수 있다. 섹션 7.5.3에서 논의된 삽입 정렬 알고리즘을 살펴보자. 배열 A를 정렬한다고 가정해 보자. 즉, 알고리즘의 끝에서 우리는 다음이 참이기를 원한다.

```java
A[0] <= A[1] <= ... <= A[A.length-1]
```

문제는 이 진술을 사실로 만들기 위해 어떤 단계별 절차를 사용할 수 있느냐는 것이다. 결국 우리는 참이기를 바라는 진술이 될 루프 불변성을 생각해 낼 수 있나? 모든 요소가 마지막에 정렬되기를 원한다면 일부 요소가 정렬되었음을 나타내는 루프 불변은 어떤가? 예를 들어 첫 번째 요소 i가 정렬된다. 이는 알고리즘의 개요로 이어진다.

```java
i = 0;
while (i < A.length) {
    // Loop invariant:  A[0] <= A[1] <= ... <= A[i-1]
    .
    .  // Code that adds A[i] to the sorted portion of the array
    .
    i = i + 1;
}
```

루프 불변성은 `while` 루프 이전에 참이고, 루프가 끝나면 루프 불변성은 우리가 알고리즘 끝에서 참이기를 원하는 진술이 된다. 우리는 알고리즘을 완성하기 위해 무엇을 해야 하는지 알고 있다. 즉, 루프 불변의 진실성을 보존할 루프 내부용 코드를 개발한다. 그렇게 할 수 있다면 루프 불변성은 우리가 개발한 알고리즘이 정확하다는 것을 보장해 줄 것이다. 배열의 정렬된 부분에 A[i]를 추가하는 알고리즘에는 자체 루프 불변성을 갖는 자체 루프가 필요하다.

---

프로그램에 대해 생각하는 데 유용한 또 다른 종류의 불변성이 있다: **클래스의 불변성(class invariants)**, 클래스 불변성은 대략적으로 클래스의 상태 또는 해당 클래스에서 생성된 객체에 대해 참인 설명이다. 예를 들어, 주사위에 표시된 값이 인스턴스 변수 `die1` 및 `die2`에 저장되는 `pairOfDice` 클래스가 있다고 가정한다. 값은 1에서 6까지의 범위에 있다. 라고 말하는 클래스 불변성을 갖고 싶을 수도 있다. 결국 이것도 모든 주사위 쌍에 대해 항상 참이어야 하는 진술이다.

그러나 클래스 불변이 되려면 해당 명령문이 항상 참임을 보장해야 한다. `die1`, `die2`가 `public` 인스턴스 변수 인 경우 클래스를 사용하는 프로그램이 이들에 할당할 수 있는 값을 제어할 수 있는 방법이 없기 때문에 그러한 보장은 불가능하다. 그래서 우리는 `public`으로 설정하게 된다. 그런 다음 클래스 정의의 모든 코드가 클래스 불변성을 준수하는지 확인하면 된다. 즉, 우선 `pairOfDice` 객체가 생성되면 변수 `die1`, `die2`가 생성된다. 1~6 범위로 초기화 되어야 한다. 또한 클래스의 모든 메서드는 클래스 불변의 진실성을 유지해야 한다. 이 경우 `die1` 또는 `die2`에 값을 할당하는 모든 메서드에서 값이 1~6 범위에 있는지 확인해야 함을 의미한다. 예를 들어 setter 메서드는 합법적인 값이 할당되고 있는지 확인해야 한다.

일반적으로 클래스 불변은 모든 생성자의 사후 조건이며 클래스에 있는 모든 메서드의 사전 조건이자 사후 조건이라고 말할 수 있다. 클래스를 작성할 때 클래스 불변성은 항상 참이기를 바라는 것이다. 메서드를 작성할 때 해당 메서드의 코드가 불변성을 준수하는지 확인해야 한다. 메서드가 호출될 때 클래스 불변성이 true라고 가정하면 메서드의 코드 이후에도 여전히 true인지 확인해야 한다. 이런 종류의 사고는 클래스 설계에 매우 유용한 도구가 될 수 있다.

```java
private int[] items = new int[8];
private int itemCount = 0;
```

클래스 불변에는 "`itemCount`"는 항목 수 이고, "`0 <= itemCount < items.length`" 항목은 "`items[0] ~ items[length-1]`" 배열 요소에 있다라는 사실에 포함된다. 이러한 불변성을 염두에 두는 것은 클래스를 작성할 때 도움이 될 수 있다. 항목을 추가하기 위한 메서드를 작성할 때 첫 번째 불변은 불변이 true로 유지되도록 하기 위해 `itemCount`를 증가시키도록 알려준다. 두 번째 불변은 새 항목을 어디에 저장해야 하는지 알려준다. 그리고 세 번째 불변은 `itemCount`를 증가시켜 `items.length`와 동일하게 만드는 경우 불변을 위반하지 않도록 조치를 취해야 함을 알려준다. (`itemCount`는 증가해야 하며, 불변성은 배열을 더 크게 만들어야 함을 의미한다.)

다음 장에서는 때떄로 전제 조건, 사후 조건, 불변량의 관점에서 생각하는 것이 어떻게 유용한지 지적한다. 

## 4. 견고한 입력 처리

정확성과 견고성이 중요하고 특히 어려운 곳 중 하나는 사용자가 데이터를 입력하든, 파일에서 읽든, 네트워크를 통해 수신하든 상관없이 입력 데이터를 처리하는 것이다. 

이 예에서는 사용자 입력을 읽기 위해 `TextIO` 클래스를 사용한다. 이 클래스에는 오류 처리 기능이 내장되어 있다. 예를 들어, `TextIO.getDouble()` 함수는 `double` 유형의 유효한 값을 반환하는 것이 보장된다. 사용자가 잘못된 값을 입력하면 사용자에게 응답을 다시 입력하도록 요청한다. 불법적인 값을 결코 보지 못한다. 그러나 이 접근 방식은 특히 사용자가 복잡한 데이터를 입력할 때 서투르고 만족스럽지 않을 수 있다.

때로는 입력을 실제로 읽지 않고도 입력 내용을 미리 볼 수 있는 것이 유용할 수 있다. 예를 들어, 프로그램은 다음 항목이 숫자인지 단어인지 알아야 할 수 있다. 이를 위해 `TextIO.peek()` 함수가 포함되어 있다. 이 함수는 사용자 입력의 다음 문자인 `char`을 반환하지만 실제로 해당 문자를 읽지 않는다. 입력의 다음 항목이 끝 줄이면 `\n`을 반환한다.

종종 우리가 정말로 알아야 할 것은 사용자 입력에서 공백이 아닌 다음 문자이다. 이를 테스트하기 전에 공백 및 탭을 건너뛰어야 한다. 다음은 이를 수행하는 함수이다. `TextIO.peek()`를 사용하여 앞을 보고 입력의 다음 문자가 줄 끝이거나 공백이 아닌 문자가 될 때까지 문자를 읽는다. (`TextIO.getAnyChar()` 함수는 해당 문자가 공백인 경우에도 사용자 입력에서 다음 문자를 읽고 반환한다. 이와 대조적으로 보다 일반적인 `TextIO.getChar()`는 공백을 건너뛴 다음 비 문자를 읽고 반환한다.)

```java
/**
 * 입력에서 공백과 탭을 지나서 읽습니다.
 * 사후 조건: 입력의 다음 문자는
 * 줄 끝 또는 공백이 아닌 문자.
 */
static void skipBlanks() {
    char ch;
    ch = TextIO.peek();
    while (ch == ' ' || ch == '\t') {
         // Next character is a space or tab; read it
         // and look at the character that follows it.
        
        ch = TextIO.getAnyChar();
        ch = TextIO.peek();
   }
} 
```

(이 작업은 `TextIO`에 내장되어 있다.)


섹션 3.5.3의 예를 통해 사용자는 "3마일" 또는 "1피트"와 같은 길이 측정값을 입력할 수 있다. 그런 다음 측정값을 인치, 피트, 야드, 마일로 변환한다. 그러나 사람들은 일반적으로 "3피트 7인치"와 같은 결합된 차수를 사용한다. 이 형식의 입력을 허용하도록 프로그램을 개선해 본다.

보다 구체적으로 사용자는 "1피트" 또는 "3마일 20야드 2피트"와 같은 하나 이상의 측정값을 포함하는 행을 입력한다. 법적 측정 단위는 인치, 피트, 야드, 마일이다. 이 프로그램은 복수형(인치, 피트)과 약어(in, ft)를 인식한다. 이 형식의 입력 한 줄을 읽고 해당하는 인치 수를 계산하는 서브루틴을 작성해 본다. 주 프로그램은 인치 수를 사용하여 피트, 야드, 마일 등의 수를 계산한다. 입력에 오류가 있는 경우 서브 루틴은 오류 메시지를 인쇄하고 -1값을 반환한다. 서브 루틴은 입력 라인이 비어 있지 않다고 가정한다. 메인 프로그램은 서브 루틴을 호출하기 전에 이를 테스트하고 빈 줄을 프로그램 종료 신호로 사용한다.

잘못된 입력 가능성을 무시하고 서브루틴에 대한 의사코드 알고리즘은 다음과 같다.

```
inches = 0    // This will be the total number of inches
while there is more input on the line:
    read the numerical measurement
    read the units of measure
    add the measurement to inches
return inches
```

공백이 아닌 다음 문자가 줄 끝 문자인지 확인하여 해당 줄에 더 많은 입력이 있는지 테스트할 수 있다. 하지만 이 테스트에는 전제 조건이 있다. 입력의 다음 문자가 실제로 줄 끝인지 또는 공백이 아닌지 확인해야 한다. 이를 확인하려면 공백 문자를 건너뛰어야 한다. 따라서 알고리즘은 다음과 같다.

```
inches = 0
skipBlanks()
while TextIO.peek() is not '\n':
    read the numerical measurement
    read the unit of measure
    add the measurement to inches
    skipBlanks()
return inches
```

`while` 루프 끝에서 `SkipBlanks()` 호출을 확인하라. `SkipBlanks()` 호출은 테스트의 전제 조건이 다시 참이 되도록 보장한다. 보다 일반적으로 `while` 루프의 테스트에 전제 조건이 있는 경우 컴퓨터가 테스트를 재평가하기 위해 다시 점프하기 전과 시작 전에 while 루프의 끝에서 이 전제 조건이 유지되는지 확인해야 한다. 

오류 검사는 어떻게 하나? 수치 측정값을 읽기 전에 실제로 읽어야 할 숫자가 있는지 확인해야 한다. 측정 단위를 읽기 전에 읽을 내용이 있는지 테스트해야 한다. 또한 측정 단위가 유효한 단위 중 하나인지 확인해야 한다. 인치, 피트, 야드 또는 마일. 다음은 오류 검사를 포함하는 알고리즘이다.

```
inches = 0
skipBlanks()

while TextIO.peek() is not '\n':

    if the next character is not a digit:
       report an error and return -1
       
    Let measurement = TextIO.getDouble();

    skipBlanks()    // Precondition for the next test!!
    if the next character is end-of-line:
       report an error and return -1                   
    Let units = TextIO.getWord()
    
    if the units are inches:
        add measurement to inches
    else if the units are feet:
        add 12*measurement to inches
    else if the units are yards:
        add 36*measurement to inches
    else if the units are miles:
        add 12*5280*measurement to inches
    else
        report an error and return -1
 
    skipBlanks()

return inches
```

보시다시피 오류 테스트는 알고리즘의 복잡성을 크게 증가시킨다. 그러나 이는 여전히 매우 간단한 예이며 가능한 모든 오류를 처리하지 않는다. 예를 들어 사용자가 `double` 유형의 유효한 값 범위를 벗어나는 `le400`과 같은 수치 측정값을 입력하면 프로그램은 `TextIO`의 기본 오류 처리 방식으로 대체된다. 측정값이 "le380 마일" 이면 더욱 흥미로운 일이 발생한다. 숫자 `le380`은 유효하지만 해당 인치 수는 `double` 유형에 대한 유효한 값 범위를 벗어난다.

다음은 Java로 작성된 서브 루틴이다.

```java
public class A {
    /**
     * 한 줄의 입력에서 사용자의 입력 측정값을 읽습니다.
     * 전제 조건: 입력 라인이 비어 있지 않아야 합니다.
     * 사후조건 : 사용자의 입력이 정당한 경우 측정
     * 는 인치로 변환되어 반환됩니다. 만약
     * 입력이 올바르지 않으면 -1 값이 반환됩니다.
     * 이 루틴에서는 줄 끝을 읽지 않습니다.
     */

    static double readMeasurement() {
        
        double inches;  // 사용자가 측정한 총 인치 수입니다.
        double measurement;  // 한 번의 측정, "12마일"의 12와 같은
        String units;        // 측정을 위해 지정된 단위, "마일"과 같은
        char ch;  // 사용자 입력에서 다음 문자를 엿보는 데 사용됩니다.
        inches = 0;  // 아직 읽은 인치가 없습니다.

        skipBlanks();
        ch = TextIO.peek();
   

        /* 라인에 더 많은 입력이 있는 한 측정값을 읽고
         * 해당 인치 수를 변수 인치에 추가합니다. 만약
         *   루프 중에 오류가 감지되면 즉시 서브루틴을 종료합니다.
         *   -1을 반환하여. 
        */
        while (ch != '\n') {
            /* 다음 측정값과 단위를 가져옵니다. 읽기 전에
                무엇이든 읽을 수 있는 합법적인 값이 있는지 확인하세요. 
            */
            if ( ! Character.isDigit(ch) ) {
                System.out.println("Error:  Expected to find a number, but found  " + ch);
                return -1;
            }
            measurement = TextIO.getDouble();

            skipBlanks();
            if (TextIO.peek() == '\n') {
                System.out.println("Error:  Missing unit of measure at end of line.");
                return -1;
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
                System.out.println("Error: \"" + units + "\" is not a legal unit of measure.");
                return -1;
            }
     
            /* 줄의 다음 항목이 다음인지 확인하기 위해 미리 살펴보세요.
                줄 끝. */
            skipBlanks();
            ch = TextIO.peek();

        }  

        return inches;

    }
}
```







