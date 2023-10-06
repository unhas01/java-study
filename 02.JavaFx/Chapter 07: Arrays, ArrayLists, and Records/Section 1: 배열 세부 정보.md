# Section 1: 배열 세부 정보

배열의 기본 사항은 이전 장에서 논의 되었지만 아직 채워야 할 Java 구문의 세부 사항이 있으며 배열 사용에 대해 더 많은 설명이 있다. 이 섹션에서는 구문 세부 사항(syntatic details) 일부를 살펴보고 이 장의 나머지 부분에서 배열 처리에 대한 추가 정보를 제공한다.


몇 가지 기본 사항을 간략하게 검토해 본다. 배열은 번호가 매겨진 요소의 시퀀스이며 각 요소는 별도의 변수처럼 작동한다. 모든 요소는 동일한 유형이며 이를 배열의 기본 유형(base type)이라 한다. 기본 유형이 `btype`이면 배열의 유형은 `btype[]`이다. 배열의 각 요소에는 인덱스가 있다. 이는 0부터 번호가 매겨진 요소 시퀀스의 숫자 위치이다. `A` 배열이 있으면 배열의 요소 번호 i는 `A[i]`이다. 배열 요소 수를 길이(length)라고 한다. `A` 배열의 길이는 `A.length`이다. 배열을 만든 후에는 배열 길이를 변경할 수 없다. `A` 배열의 요소는 `A[0], A[1], ... A[A.lenth - 1]`이다. `0 ~ A.length-1` 범위를 벗어나는 인덱스를 사용하여 배열 요소를 참조하려고 하면 `ArrayIndexOutOfBoundsException`이 발생한다.

Java의 배열은 객체이므로 배열 변수는 배열만 참조할 수 있다. 배열을 포함하지 않는다. 배열 변수의 값은 null일 수도 있다. 이 경우 어떤 배열도 참조하지 않으며 `A[i]`와 같은 배열 요소를 참조하려고 하면 `NullPointerException`이 발생한다. 배열은 `new` 연산자의 특별한 형태를 사용하여 생성된다. 

```java
int[] A = new int[10];
```

기본 유형이 `int`이고 길이가 10인 배열을 생성하고 새로 생성된 배열을 참조하도록 변수 `A`를 설정한다.

## 1. For-each 루프

배열은 종종 `for`루프를 사용하여 처리된다. `for`루프를 사용하면 배열의 각 요소를 처음부터 끝까지 쉽게 처리할 수 있다. 예를 들어 `nameList`가 `String` 배열의 경우 목록의 모든 값은 다음을 사용하여 print할 수 있다.
```java
for (int i = 0; i < nameList.length; i++) {
    System.out.println(nameList[i]);    
}
```

이러한 유형의 처리는 매우 일반적이므로 작성을 더 쉽게 해주는 대체 형식의 `for`루프가 있다. **for-each loop** 라고 한다. 

```java
for (String name : nameList) {
    System.out.pringln(name);    
}
```

`"for (String name : nameList)"`의 의미는 "배열 nameList의 각 문자열, name에 대해 다음을 수행한다"이다. 결과적으로 변수 `name`은 `nameList`의 각 값을 차례로 취하고 각 값에 대해 루프 본문이 실행된다. 루프에는 배열 인덱스가 없다. 루프 제어 변수 `name`은 값 중 하나의 인덱스가 아니라 배열에 있는 값 중 하나를 나타낸다. 


for-each 루프는 데이터 구조의 모든 값을 처리하기 위해 특별히 고안되었으며 섹션 7.3과 섹션 10장에서 배열 이외의 다른 데이터 구조에 적용되는 것을 볼 수 있다. for-each 루프를 사용 하면 데이터 구조에 대한 세부 정보를 알지 못해도 데이터 구조의 값을 처리할 수 있다. 배열의 경우 배열 인덱스 사용으로 인한 복잡성을 피할 수 있다.

for-each 루프는 배열에 저장된 각 값에 대해 동일한 작업을 수행한다. `itemArray`가 `BaseType[]` 유형의 배열인 경우 `itemArray`에 대한 for-each 루프 형식은 다음과 같다.

```java
for (BaseType item : itemArray) {
    ...
    // process the item
}
```

평소와 같이 루프 내부에 명령문이 하나만 있는 경우 중괄호는 선택 사항이다. 이 루프에서 `item`은 루프 제어 변수이다. 이는 `BaseType`은 배열의 기본 유형이다. (for-each 루프에서는 루프 제어 변수가 루프 내에서 선언되어야 하며 루프 외부에 이미 존재하는 변수일 수 없다.) 이 루프가 실행되면 배열의 각 값이 차례로 항목에 할당되고 루프의 본문은 각 값에 대해 실행된다. 따라서 위의 루프는 다음과 동작이 같다.

```java
for (int index = 0; index < itemArray.length; index++) {
    BaseType item;
    item = itemArray[index];
    
    ...
    // process the item
}
```

예를 들어 `A`가 `int[]` 유형인 배열인 경우 for-each 루프를 사용하여 `A`의 모든 값을 print 할 수 있다.
```java
for (int item : A) {
    System.out.println(item);
}
```
그리고 우리는 `A`의 모든 양의 정수를 다음과 같이 더할 수 있다.
```java
int sum = 0;
for (int item : A) {
    if (item > 0)
        sum += item;
}
```

또한 섹션 4.8.2에서 소개된 지역 변수 선언을 위한 `var` 사용은 for-each 루프의 루프 제어 변수에 적용된다. 따라서 `for(BaseType item : itemArray)` 대신 `for(var item : itemArray)`라고 쓸 수 있다. 변수의 유형은 배열의 기본 유형에서 추론된다. 이 구문은 더 복잡한 유형을 처리할 때 더욱 유용하다.

for-each 루프가 항상 적절한 것은 아니다. 예를 들어 배열의 일부 항목만 처리하거나 요소를 역순으로 처리하는 데 이를 사용하는 간단한 방법은 없습니다. 그러나 모든 값을 순서대로 처리하려는 경우 코드가 좀 더 단순해진다. 배열 인덱스를 사용할 필요가 없기 때문이다.

그러나 for-each 루프는 요소가 아닌 배열의 값을 처리한다는 점을 유의하는 것이 중요하다. 여기서 요소는 배열의 일부이고 값을 보유하는 실제 메모리 위치를 의미한다. 예를 들어, 정수 배열을 17로 채우려는 다음과 같은 잘못된 시도를 생각하자.

```java
int[] intList = new int[0];
for (int item : intList) {
    item = 17;      // ERROR    
}
```

할당문 `item = 17`은 값 17을 루프 제어 변수 `item`에 할당한다. 그러나 이는 배열과 관련 없다. 루프 본문이 실행되면 배열 요소 중 하나의 값이 `item`에 복사된다. `item = 17`문은 복사된 값을 대체하지만 복사된 원본 배열 요소에는 영향을 주지 않는다. 배열의 값은 변경되지 않는다. 

```java
int[] intList = new int[10];
for (int i = 0; i < intList.length; i++) {
    int item = intList[i];
    item = 17;
}
```

확실히 배열의 요소 값은 변경되지 않는다.


## 2. 가변 Arity(항수, 개수) Methods
> Arity <=> 항수, 개수

Java 5 이전에는 Java의 모든 메서드에 고정된 항수(개수)가 있었다. (메서드의 개수는 메서드 호출 시 매개 변수 수로 정의된다.) 고정 개수 메서드에서는 메서드를 호출할 때 마다 매개 변수 개수가 동일해야 하며, 메서드 개수와 동일해야 한다. Java 5에서는 가변 Arity Method가 도입 되었다. 가변 개수 메서드에는 메서드에 호출마다 매개 변수 수가 다를 수 있다. 예를 들어, 섹션 2.4.1에서 소개된 형식화된 출력 메서드 `System.out.printf`는 가변 개수 메서드이다. `System.out.printf`의 첫 번째 매개 변수는 문자열이어야 하지만, 유형에 관계 없이 추가 매개 변수를 얼마든지 가질 수 있다.

가변 개수 메서드를 호출하는 것은 다른 종류의 메서드를 호출하는 것과 다르지 않지만, 이를 작성하려면 몇 가지 새로운 구문이 필요하다. 예를 들어, `double` 유형 값의 개수에 관계없이 평균을 계산할 수 있는 메서드를 생각해 본다. 
```java
public static double average(double... numbers)
```

여기서 `double` 뒤의 `...`이 가변 개수 메서드를 만드는 것이다. 이는 서브 루틴이 호출될 때 `double` 유형의 값이 얼마든지 제공될 수 있음을 나타낸다. 예를 들어 `average(1, 4, 9, 16)`, `average(3.14, 2.17)` 및 심지어 `average()`도 이 메서드에 대한 합법적인 호출이다. `int` 유형은 실제 매개 변수 `average`에 전달될 수 있다. 정수는 평소와 같이 자동으로 실수로 변환된다.

메서드가 호출 되면 가변 개수 매개 변수에 해당하는 모든 실제 매개 변수의 값이 배열에 배치되고, 이 배열이 실제로 메서드에 전달된다. 즉, 메서드 본문에 T유형의 가변 개수 매개 변수는 실제로 `T[]` 유형의 일반 매개 변수 처럼 보인다. 배열의 길이는 메서드 호출에 제공된 실제 매개 변수 수를 알려준다. `average` 예시에서, 메서드 본문에는 `double[]` 유형의 `numbers`라는 배열이 표시된다. 메서드 호출이 실제 매개 변수 수는 `numbers.length`이고 실제 매개 변수의 값은 `numbers[0], numbers[1], ...`이다. 메서드의 완전한 정의는 다음과 같다.

```java
public static double average(double... numbers) {
    double sum;
    double average;
    sum = 0;
    for (int i = 0; i < numbers.length; i++) {
        sum = sum + numbers[i];
    }
    average = sum / numbers.length;
    return average;
}
```

그런데 개별 값 목록 대신 단일 배열을 가변 개수 메서드에 전달하는 것이 가능하다. 예를 들어 `salesData`가 `double[]` 유형의 변수라고 가정한다. 그런 다음 `average(salesData)`를 호출 하는 것이 적합하며 이는 배열에 있는 모든 숫자의 평균을 계산한다. 

가변 개수 메서드를 정의의 형식 매개 변수 목록에는 둘 이상의 매개 변수가 포함될 수 있지만 `...`는 마지막 형식 매개 변수에만 적용될 수 있다.

예를 들어, 원하는 개수의 점을 통해 다각형을 그릴 수 있는 방법을 생각해 본다.

```java
public static void drawPolygon(GraphicsContext g, Point... points) {
    if (points.length > 1) {  
       for (int i = 0; i < points.length - 1; i++) {
           g.strokeLine( points[i].x, points[i].y, points[i+1].x, points[i+1].y );
       }
       
       g.strokeLine( points[points.length-1].x, points[points.length-1].y, points[0].x, points[0].y );
    }
}
```

이 메서드가 호출되면 서브 루틴 호출 문에는 `GraphicsContext` 유형의 실제 매개 변수가 하나 있어야 하며, 그 뒤에는 `Point` 유형의 실제 매개 변수가 얼마든지 올 수 있다.

마지막 예로 문자열 목록의 모든 값을 하나의 긴 문자열로 묶는 메서드를 살펴본다. 

```java
public static String concat(String... values) {
    StringBuffer sb = new StringBuffer();
    for (String str : values) {
        sb.append(str);    
    }
    
    return sb.toString();
}
```

이 메서드 정의가 주어지면, `concat("Hello", "World")`는 `"HelloWorld"` 문자열을 반환하고 `concat()`은 빈 문자열을 반환한다. 가변 개수 메서드는 배열을 실제 매개 변수로 허용할 수도 있으므로 `concat(lines)`에서 `lines`가 `String[]` 유형인 경우에도 호출할 수 있다. 이렇게 하면 배열의 모든 요소가 단일 문자열로 연결된다.


## 3. 배열 리터럴
우리는 배열 변수가 선언될 때 값 목록을 사용하여 초기화하는 것이 가능하는 것을 살펴 보았다. 
```java
int[] squares = {1, 4, 9, 16, 25, 36, 49};
```

이는 리스트의 7개 값을 포함하는 새로 생성된 배열을 참조하도록 초기화한다. 

이 형식의 리스트 초기화는 새로 선언된 배열 변수에 초기 값을 제공하는 선언문에서만 사용할 수 있다. 이미 존재하는 변수에 값을 할당하기 위해 할당문에서 사용할 수 없다. 그러나 다른 곳에서 사용할 수 있는 새 배열을 생성하기 위한 또 다른 유사한 표기법이 있다. 이 표기법은 다른 형태인 `new`연산자를 사용하여 새 배열 객체를 생성하고 값을 채운다. 
```java
cubes = new int[] {1, 8, 27, 64};
```

이는 선언이 할당문이므로 `new int[]`가 없으면 적절하지 않는다. 이 형태의 일반적인 구문은 다음과 같다.
```java
new base-type[] {list-of-values}
```

이는 실제로 값이 새로 생성된 배열 객체에 대한 참조인 표현식이다. 이런 의미에서 이것은 값을 표현하기 위해 프로그램에 입력할 수 있는 것이기 때문에 **"배열 리터럴"** 이라 한다. 이는 `base-type[]` 유형의 객체가 유효한 모든 컨텍스트에서 사용될 수 있음을 의미한다. 예를 들어 새로 생성된 배열을 실제 매개 변수로 서브 루틴에 전달할 수 있다.

```java
public static Menu createMenu( String menuName, String[] itemNames ) {
    Menu menu = new Menu(menuName);
    for ( String itemName : itemNames ) {
        if ( itemName == null ) {
            menu.getItems().add( new SeparatorMenuItem() );
        }
        else {
            MenuItem item = new MenuItem(itemName);
            menu.getItems().add(item);
        }
    }
    return menu;
}
```

`createMenu` 호출의 두 번째 매개 변수는 문자열 배열이다. 실제 매개 변수로 전달되는 배열은 `new` 연산자를 사용하여 생성될 수 있다.
```java
Menu fileMenu = createMenu("File", new String[] { "New", "Open", "Close", null, "Quit" } );
```

익명의 내부 클래스가 편리한 것과 마찬가지로 이러한 방식으로 배열을 "제자리(In place)"에서 생성하고 사용할 수 있다는 것이 매우 편리 하다는 점을 확신하게 될 것이다. 

그런데 배열 변수 선언에서 배열 초기화 구문 대신 `new BaseType[] {...}` 구문을 사용하는 것은 완전히 합법적이다. 
```java
int[] primes = { 2, 3, 5, 7, 11, 13, 17, 19 };
```
다음과 동등하다.
```java
int[] primes = new int[] { 2, 3, 5, 7, 11, 13, 17, 19 };
```

실제로 선언문의 맥락에서만 작동하는 작동하는 특별한 표기법을 사용하기 보다는 두 번째 형식을 사용하는 것을 선호할 때도 있다.

---
**마지막 참고사항**

역사적 이유로 다음과 같은 배열 선언은
```java
int[] list;
```

다음과 같이 쓸 수 있다.
```java
int list[];
```

이는 C, C++에서 사용되는 구문이다. 그러나 이 구문은 Java의 맥락에서는 그다지 의미가 없으므로 피하는 것이 가장 좋다. `type-name varible-name`을 따르는 것이 합리적이다.