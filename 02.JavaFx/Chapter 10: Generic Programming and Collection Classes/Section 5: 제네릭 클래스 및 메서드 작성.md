# 제네릭 클래스 및 메서드 작성

지금까지 이 장에서는 Java 컬렉션 프레임워크의 일부인 제네릭 클래스와 메서드를 사용하는 방법을 배웠다. 이제 새로운 제네릭 클래스와 메서드를 처음으로 작성하는 방법을 배울 차례이다. 제네릭 프로그래밍은 매우
일반적이고 재사용 가능한 코드를 생성한다. 제네릭 프로그래밍을 수행하는 방법을 아는 거슨 코드를 작성할 수 있기 때문읻. 모든 프로그래머가 재사용 가능한 소프트웨어 라이브러리를 작성할 필요는 없지만 모든 프로그래머는
이를 수행하는 방법에 대해 최소한 어느 정도 알고 있어야 한다. 실제로 Java의 표준 제네릭 클래스에 대한 Javadoc 문서를 읽으려면 이 섹션에 소개된 구문 중 일부를 알아야 한다.

이 섹션에서는 Java 제네릭 프로그래밍에 대한 모든 세부 사항을 다루지는 않지만 여기에 제시된 자료는 가장 일반적인 경우를 다루기에 충분하다.

## 1. 간단한 제네릭 클래스

제네릭 프로그래밍의 동기를 보여주는 예부터 시작한다. 섹션 10.2.1에서 나는 LinkedList를 사용하여 대기열을 구현하는 것이 쉬울 것이라고 언급했다. 목록에서 수행되는 유일한
작업이 `enqueue`, `dequeue` 및 `isEmpty`이 있고 연결된 목록을 개인 인스턴스 변수로 포함하는 새 클래스를 만들 수 있다. 예시는 다음과 같다.

```java
class QueueOfStrings {
    private LinkedList<String> items = new LinkedList<>();

    public void enqueue(String item) {
        items.addLast(item);
    }

    public String dequeue() {
        return items.removeFirst();
    }

    public boolean isEmpty() {
        return (items.size() == 0);
    }
}
```

훌륭하고 유용한 클래스이다. 그러나 이것이 큐 클래스를 작성하는 방법이고 Integers, Doubles, Colors 또는 기타 유형의 큐를 원한다면 각 유형에 대해 다른 클래스를 작성해야 한다. 이러한 모든
클래스의 코드는 거의 동일하므로 프로그래밍이 중복되는 것처럼 보인다. 중복을 피하기 위해 모든 유형의 객체 대기열을 정의하는 데 사용할 수 있는 제네릭 큐 클래스를 작성할 수 있다.

제네릭 클래스를 작성하는 구문은 간단하다. 특정 유형의 String을 T와 같은 유형 매개 변수로 바꾸고 유형 매개 변수를 클래스 이름에 추가한다.

```java
class Queue<T> {
    private LinkedList<T> items = new LinkedList<>();

    public void enqueue(T item) {
        items.addLast(item);
    }

    public T dequeue() {
        return items.removeFirst();
    }

    public boolean isEmpty() {
        return (items.size() == 0);
    }
}
```

클래스내에서 유형 매개변수 T는 일반 유형 이름과 마찬가지로 사용된다. 이는 `dequeue`에서 형식 매개 변수 항목의 유형으로, LinkedList<T>에서 실제 유형 매개 변수로 `enqueue`에 대한 반환
유형을 선언하는 데 사용된다. 이 클래스 정의가 주어지면 Queue<String>, Queue<Integer>, Queue<Color>와 같은 매개 변수화된 유형을 사용할 수 있다. 즉, Queue 클래스는
LinkedList 및 HashSet과 같은 내장 일반 클래스와 정확히 동일한 방식으로 사용된다.

제네릭 클래스 정의에서 유형 매개변수의 이름으로 "T"를 사용할 필요는 없다. 유형 매개 변수는 서브 루틴의 형식 매개 변수와 같다. 클래스 정의에서 원하는 이름을 만들 수 있다. 클래스를 사용하여 선언하거나 객체를
생성하면 정의의 이름이 실제 유형 이름으로 대체된다. 유형 매개 변수에 대해 보다 의미 있는 이름을 사용하려는 경우 Queue 클래스를 다음과 같이 정의할 수 있다.

```java
class Queue<ItemType> {
    private LinkedList<ItemType> items = new LinkedList<>();

    public void enqueue(ItemType item) {
        items.addLast(item);
    }

    public ItemType dequeue() {
        return items.removeFirst();
    }

    public boolean isEmpty() {
        return (items.size() == 0);
    }
}
```

이름을 "T"에서 "ItemType"으로 변경해도 클래스 정의의 의미나 Queue가 사용되는 방식에는 전혀 영향이 없다.

일반 인터페이스도 비슷한 방식으로 정의할 수 있다. 표준 인터페이스 Map<K, V>를 사용하여 수행되는 것처럼 두 개이상의 유형 매개 변수가 있는 일반 클래스 및 인터페이스를 정의하는 것도 쉽다. 일반적인 예는 서로 다은 유형의 두 객체를 포함하는 "쌍"의 정의이다. 이러한 클래스의 간단한 버전은 다음과 같이 정의할 수 있다.

```java
public class Pair<T,S> {
    public T first;
    public S second;
    public Pair( T a, S b ) {
        first = a;
        second = b;
    }
}
```

이 클래스는 다음과 같은 변수를 선언하고 객체를 생성하는 데 사용할 수 있다.

```java
Pair<String,Color> colorName = new Pair<>("Red", Color.RED);
Pair<Double,Double> coordinates = new Pair<>(17.3,42.8);
```

이 클래스의 생성자 정의에서 "Pair"라는 이름에는 유형 매개 변수가 없다. "Pair<T, S>"를 예상했을 수도 있다. 그러나 클래스 이름은 "Pair<T, S>"가 아닌 "Pair"이며, 클래스 정의 내에서 "T", "S"는 마치 특정 실제 타입의 이름인 것처럼 사용된다. 어떠한 경우에도 유형 매개 변수는 메서드나 생성자의 이름에는 추가되지 않고 클래스 및 인터페이스의 이름에만 추가된다는 점에 유의하자.

레코드 클래스는 일반 클래스일 수도 있다. 
```java
public record Pair(T a, S b) {
    
}
```

## 2. 간단한 제네릭 메서드

Java에는 제네릭 클래스 외에도 제네릭 메서드가 있다. 예를 들어, 모든 유형의 객체 컬렉션을 정렬할 수 있는 `Collection.sort()` 메서드가 있다. 제네릭 메서드를 작성하는 방법을 보려면 문자열 배열에서 특정 문자열이 나타나는 횟수를 계산하는 제네릭이 아닌 메서드 부터 시작해 본다.

```java
/**
 * 목록에서 itemToCount가 발생한 횟수를 반환합니다. 항목
 * 목록은 itemToCount.equals()를 사용하여 동일한지 테스트됩니다.
 * itemToCount가 null인 특수한 경우입니다.
 */

public static int countOccurrences(String[] list, String itemToCount) {
    int count = 0;
    if (itemToCount == null) {
        for ( String listItem : list )
            if (listItem == null)
                count++;
    }
    else {
        for ( String listItem : list )
            if (itemToCount.equals(listItem))
                count++;
    }
    return count;
}
```

다시 한번, String 유형에서 작동하는 일부 코드가 있으며, 다른 유형의 객체에서도 작동하도록 거의 동일한 코드를 작성하는 것을 상상할 수 있다. 제네릭 메서드을 작성함으로써 우리는 기본이 아닌 모든 유형의 객체에 대해 작동하는 단일 메서드 정의를 작성하게 된다. 메서드 정의의 특정 유형 String을 T와 같은 유형 매개 변수의 이름으로 바꿔야 한다. 그러나 그것이 우리가 변경한 유일한 것이라면 컴파일러는 "T"가 실제 유형의 이름이라고 생각하고 이를 선언되지 않은 식별자가 표시된다. "T"가 유형 매개변수임을 컴파일러에 알리는 방법이 필요하다. 제네릭 메서드의 경우 <T>는 메서드의 반환 유형 이름 앞에 온다.

```java
public static <T> int countOccurrences(T[] list, T itemToCount) {
    int count = 0;
    if (itemToCount == null) {
        for ( T listItem : list )
            if (listItem == null)
                count++;
    }
    else {
        for ( T listItem : list )
            if (itemToCount.equals(listItem))
                count++;
    }
    return count;
}
```

"<T>"는 메서드를 제네릭 메서드를로 표시하고 정의에 사용될 유형 매개변수의 이름을 지정한다. 물론, 유형 매개변수의 이름이 "T"일 필요는 없다. 그것은 무엇이든 될 수 있다. 

제네릭 메서드 정의가 주어지면 이를 모든 유형의 객체에 적용할 수 있다. `wordList`가 String[] 유형의 변수이고 `word`가 String 유형의 변수 인 경우

```java
int ct = countOccurrences(wordList, word);
```

`wordList`에서 해당 `word`가 나타나는 횟수를 계산한다. `palette`가 Color[] 유형의 변수 이고, `color`가 Color 유형의 변수인 경우

```java
int ct = countOccurrences(palette, color);
```

`numbers`가 Integer[] 유형이 ㄴ경우

```java
int ct = countOccurrences(numbers, 17);
```

`numbers`에 17이 나타나는 횟수를 계산한다. 이 마지막 예에서는 오토박싱을 사용한다. 17은 자동으로 Integer 유형의 값으로 변환된다. Java의 제네릭 프로그래밍은 객체에만 적용되므로 int[] 유형의 배열에서는 계산할 수 없다.

제네릭 메서드에서 `countOccurrences`의 "T"와 같은 하나 이상의 유형 매개 변수가 있을 수 있다. 함수 호울 `countOccurrences(wordList, word)`에서와 같이 제네릭 메서드가 사용되는 경우 유형 매개 변수를 대체하는 유형에 대한 명시적인 언급이 없다. 컴파일러는 메서드 호출의 실제 매개 변수 유형에서 유형을 추론한다. `wordList`는 String[] 유형이므로 컴파일러는 `countOccurrences(wordList, word)`에서 "T"를 대체하는 유형이 String 임을 알 수 있다. 이는 `Queue<String>`에서와 같이 제네릭 클래스를 사용하는 것과 대조된다.

`countOccurrences` 메서드는 배열에서 작동한다. 컬렉션에서 객체의 발생 횧수를 계산하는 유사한 메서드를 작성할 수도 있다. 

```java
public static <T> int countOccurrences(Collection<T> collection, T itemToCount) {
    int count = 0;
    if (itemToCount == null) {
        for ( T item : collection )
            if (item == null)
                count++;
    }
    else {
        for ( T item : collection )
            if (itemToCount.equals(item))
                count++;
    }
    return count;
}
```

Collection<T> 자체는 제네릭 유형이므로 이 메서드는 매우 제네릭하다. 

## 3. 와일드카드 유형

지금까지 살펴본 제네릭 클래스와 메서드의 종류에는 제한이 있다. 일반적으로 T라는 예제의 유형 매개변수는 어떤 유형이든 될 수 있다. 대부분의 경우 괜ㅊ낳다. 하지만 이는 T로 할 수 있느느 유일한 작업은 모든 유형에서 수행할 수 있는 작업이고, T 유형의 모든 객체로 수행할 수 있는 유일한 작업은 T로 수행할 수 있는 작업 뿐이라는 의미이다.예를 들어, 지금까지 다룬 기술로는 객체를 비교하는 제네릭 메서드를 작성할 수 없다. `compareTo()` 메서드는 해당 메서드가 모든 객체에 대해 정의되어 있지 않기 때문이다. `compareTo()` 메서드는 Comparable 인터페이스에 정의된다. 우리에게 필요한 것은 제네릭 클래스나 메서드가 임의의 객체가 아닌 Comparable 유형의 객체에만 적용되도록 지정하는 방법이다. 이러한 제한으로 인해 제네릭 클래스나 메서드 정의에서 `compareTo()`를 자유롭게 사용할 수 있다.

제네릭 프로그래밍에 사용되는 유형에 제한을 두기 위한 두 가지 서로 다르지만 관련된 구문이 있다. 그 중 하나는 제한된 유형 매개변수(bounded type parameters)로, 제네릭 클래스 및 메서드 정의에서 형식 유형 매개변수로 사용된다. 제한된 유형 매개변수는 `class GenericClass<T>` 또는 `public static <T> void genericMethod(..)`에서 단순 유형 매개변수 T대신 사용된다. 두 번째 구문은 와일드카드(wildcard) 유형이다. 변수 선언의 유형 매개변수와 메서드 정의의 형식 매개변수로 사용된다. 선언문 `List<String> list`에서 유형 매개변수 String 대신 와일드카드 유형을 사용할 수 있다. 또는 형식 매개변수 목록 `void concat(Collection<String> c)`. 먼저 와일드 카드 유형을 살펴보고 이 섹션 후반부에서 제한된 유형 주제로 돌아간다.

와일드 카드 유형이 유용한 간단한 예부터 싲가해 본다. Shape가 `public void draw()` 메서드를 정의하는 클래스이고 Shape에 Rect 및 Oval과 같은 하위 클래스가 있다고 가정한다. Shape 컬렉션의 모든 모양을 그릴 수 있는 메서드를 원한다고 가정해 본다. 

```java
public static void drawAll(Collection<Shape> shapes) {
    for (Shape s : shpaes) 
        s.draw();
}
```

이 메서드는 Collection<Shape> 유형, ArrayList<Shape> 유형의 변수 또는 유형 매개 변수 Shape가 있는 다른 컬렉션 클래스에 적용하면 잘 동작한다. 그러나 Collection<Rect> 유형의 `rectangles` 이라는 변수에 Rect 목록이 있다고 가정해 본다. Rects는 Shapes 이므로 `drawAll(rectangles)`을 호출할 수 있는 것으로 예상할 수 있다. 불행하게도 이는 작동하지 않는다. Rect 컬렉션은 Shape 컬렉션으로 간주되지 않는다. 변수 `rectangles`는 매개 변수 모양에는 할당할 수 없다. 해결책은 Shape 선언에 `? extends Shape`로 바꾸는 것이다.

```java
public static void drawAll(Collection<? extends Shape> shapes) {
    for (Shape s : shpaes) 
        s.draw();
}
```

와일드카드 유형 `? extends Shape`는 대략 Shape와 같거나 Shape의 하위 클래스인 모둔 유형을 의미한다. `shapes` 형식 매개변수는 Collection<? extends Shape>를 사용하면 Rect가 Shape의 하위 클래스이고 따라서 와일드카드와 일치하므로 Collection<Rect> 유형의 실제 매개 변수를 사용하여 `drawAll()` 메서드를 호출하는 것이 가능하다. ArrayList<Rect>, Set<Oval> 또는 List<Oval> 유형의 `drawAll()`에 실제 매개변수를 전달할 수도 있다. 그리고 Shape 클래스 자체가 `? extends Shape`와 일치하므로 여전히 Collection<Shape> 또는 ArrayList<Shape> 유형의 변수를 전달할 수 있다. 와일드카드 타입을 사용하여 메서드의 유용성을 대폭 높였다.

(필수적인 것은 아니지만 모든 Rect가 Shape로 간주됨에도 불구하고 Java가 Rect 컬렉션을 Shapes 컬렉셔으로 사용하는 것을 허용하지 않는 이유를 알고 싶을 수도 있다. 다소 어리석지만 합법적인 방법을 고려해보자.)

```java
static void addOval(List<Shape> shapes, Oval oval) {
    shapes.add(oval);
}
```

`rectangles`이 List<Rect> 유형이라고 가정하낟. Rect 목록이 Shape 목록이 아니라는 규칙 때문에 `addOval(rectangles, oval)`을 호출하는 것은 불법이다. 해당 규칙을 삭제하면 `addOval(rectangles, oval)`이 적법하며 Rect 목록에 Oval을 추가한다. 이는 좋지 않다. Oval은 Rect의 하위 클래스가 아니므로 Oval은 Rect가 아니며 Rect 목록에는 Oval이 포함될 수 없다. `addOval(rectangles, oval)` 메서드 호출은 의미가 없으며 불법이어야 한다.

또 다른 예로, Collection<T> 인터페이스의 `addAll()` 메서드를 고려해 보자. 섹션 10.1.4에서 이 메서드에 대한 설명에서 Collection<T> 유형의 `col` 컬렉션에 대해 `col1.addAll(col2)`는 `col2`의 모든 객체를 `col1`에 추가한다. 메개변수 `col2`는 다음을 수행할 수 있다. Collection<T> 유형의 컬렉션일 수 있다. 그러나 더 일반적일 수도 있다. 예를 들어 T 가 클래스이고 S 가 T 의 하위 클래스인 경우 `coll2` 는 Collection<S> 유형일 수 있다. S 유형의 모든 개체는 자동으로 T 유형 이므로 `coll` 에 합법적으로 추가될 수 있기 때문에 이는 의미가 있다." 잠시 생각해보면 내가 여기서 설명하는 내용이 조금 어색하게도 유용하다는 것을 알 수 있을 것이다. 와일드카드 유형: 우리는 `coll2`가 T 유형의 객체 컬렉션이 되도록 요구하지 않고 T 의 모든 하위 클래스 컬렉션을 허용하고 싶다. 더 구체적으로 말하자면 유사한 `addAll()` 메서드가 어떻게 될 수 있는지 살펴본다. 이 섹션의 앞부분에서 정의한 일반 Queue 클래스에 추가되었다.

```java
class Queue<T> {
    private LinkedList<T> items = new LinkedList<T>();
    public void enqueue(T item) {
        items.addLast(item);
    }
    public T dequeue() {
        return items.removeFirst();
    }
    public boolean isEmpty() {
        return (items.size() == 0);
    }
    public void addAll(Collection<? extends T> collection) {
        for ( T item : collection )
            enqueue(item);
    }
}
```

여기서 T는 제네릭 클래스 정의의 유형 매개 변수이다. 우리는 와일드카드 유형을 제네릭 클래스와 결합하고 있다. 제네릭 클래스 정의 내에서 "T"는 알 수 없지만 특정 유형인 것처럼 사용된다 와일드카드 유형 `? extends T`는 특정 유형과 동일하거나 확장하는 유형을 의미한다. Queue<Shape> 유형의 큐를 생성할 때 "T"는 "Shape"를 참조하고, 클래스 정의에서 와일드카드 유형 `? extends T`는 `? extends Shape`를 의미한다.

`addAll` 정의의 for-each 루프는 T 유형의 변수 `item`을 사용하여 `collection`을 반복한다. 이제 `collection`은 Collection<S> 유형이 될 수 있다. 여기서 S는 T의 하위 클래스이다. `item`이 S가 아닌 T 유형이므로 여기에 문제가 있나? 아니요, 문제가 없다. S가 T의 하위 클래스인 한 S 유형의 값은 T유형의 변수에 할당될 수 있다. 와일드카드 유형에 대한 제한으로 인해 모든 것이 잘 작동한다.

`addAll` 메서드는 컬렉션의 모든 항목을 대기열에 추가한다. 반대 작업을 수행하고 싶다고 가정한다. 현재 대기열에 있는 모든 항목을 지정된 컬렉션에 추가한다. 

```java
public void addAll(Collection<T> collection)
```

기본 유형이 T와 정확히 동일한 컬렉션만 작동한다. 이는 너무 제한적이다. 일종의 와일드카드가 필요하다. 그러나 `? extends T`는 작동하지 않는다.

```java
public void addAllTo(Collection<? extends T> collection) {
    while ( ! isEmpty() ) {
        T item = dequeue();  // Remove an item from the queue.
        collection.add( item );  // Add it to the collection.  ILLEGAL!!
    }
}
```

문제는 T의 일부 하위 클래스 S에 속하는 항목만 보유할 수 있는 컬렉션에 T 유형의 `item`을 추가할 수 없다는 것이다. 포함이 잘못된 방향으로 진행되고 있다. T유형의 `item`이 반드시 S 유형일 필요는 없다. 예를 들어 Queue<Shape> 유형의 큐가 있는 경우 모든 Shape가 Rect가 아니기 때문에 큐의 항목을 Collection<Rect> 유형의 컬렉션에 추가하는 것은 의미가 없다. 반변에 Queue<Rect>가 있는 경우 해당 대기열의 항목을 Collection<Shape> 또는 실제로 S가 Rect의 슈퍼 클래스인 임의의 컬렉션 Collection<S>에 적용된다.

이러한 유형의 관계를 표현하려면 새로운 종류의 와일드카드 유형인 `? super T`가 필요하다. 이 와일드카드는 대략 T자체 또는 T의 슈퍼 클래스인 모든 클래스를 의미한다. 예를 들어 Collection<? super Rect>는 Collection<Shape>, ArrayList<Object> 및 Set<Rect> 유형과 일치한다. 이것이 `addAllTo` 메서드에 필요한 것이다. 

```java
class Queue<T> {
    private LinkedList<T> items = new LinkedList<T>();
    public void enqueue(T item) {
        items.addLast(item);
    }
    public T dequeue() {
        return items.removeFirst();
    }
    public boolean isEmpty() {
        return (items.size() == 0);
    }
    public void addAll(Collection<? extends T> collection) {
        // Add all the items from the collection to the end of the queue
        for ( T item : collection )
            enqueue(item);
    }
    public void addAllTo(Collection<? super T> collection) {
        // Remove all items currently on the queue and add them to collection
        while ( ! isEmpty() ) {
            T item = dequeue();  // Remove an item from the queue.
            collection.add( item );  // Add it to the collection.
        }
    }
}
```

`? extends T`와 같은 와일드카드 유형에는 T는 클래스 대신 `interface`가 될 수 있다. T가 인터페이스인 경우에도 `extends`(`implements` 아님)라는 용어가 와일드카드 유형에 사용된다. 예를 들어 Runnable은 `public void run()` 메서드를 정의하는 `interface`라는 것을 확인했다. 다음은 각 실행 객체에서 `run()` 메서드를 실행하여 Runnable 컬렉션의 모든 객체를 실행하는 메서드이다.

```java
public static runAll(Collection<? extends Runnable> runnables) {
    for (Runnable runnable : runnables) 
        runnable.run();
}
```

와일드카드 유형은 Collection<? extends Runnable>과 같은 매개변수화된 유형의 유형 매개변수로만 사용된다. 지금까지 와일드카드 유형이 발생할 가능성이 가장 높은 곳은 형식 매개 변수 목록에서 와일드카드 유형이 형식 매개 변수 유형 선언에 사용된다. 그러나 몇 가지 다른 장소에서도 사용할 수 있다. 예를 들어, 변수 선언문의 유형지정에 사용될 수 있다. 

마지막 설명 : 와일드카드 유형 "<?>"는 `<? extends Object>`와 동일하다. 즉, 가능한 모든 유형과 일치한다. 예를 들어 일반 인터페이스 Collection<T>의 `removeAll()` 메서드는 다음과 같이 선언된다.

```java
public boolean removeAll(Collection<?> c) {
    
}
```

이는 단지 `removeAll` 메서드가 모든 유형의 객체 컬력센에 적용될 수 있음을 의미한다.

## 4. 제한된(Bounded) 유형

와일드카드 유형은 모든 문제를 해결하지 못한다. 이를 통해 메서드 정의를 일반화하여 단일 유형이 아닌 다양한 유형의 객체 컬렉션으로 작업할 수 있다. 그러나 제네릭 클래스나 메서드 정의에서 형식적 매개변수로 허용되는 유형을 제한하는 것은 허용되지 않는다. 이 목적을 위해 제한된 유형이 존재한다.

우리는 작고 현실적이지 않은 예부터 시작한다. ControlGroup이라는 일반 클래스를 사용하여 GUI 구성 요소 그룹을 생성한다고 가정한다. 예를 들어 매개변수화된 유형 ControlGroup<Button>은 Button 그룹을 나타내고 ControlGroup<Slider>는 Slider 그룹을 나타낸다. 클래스에는 그룹의 모든 구성 요소에 특정 작업을 한 번에 적용하기 위해 호출할 수 있는 메서드가 포함된다. 예를 들어, 다음 형식의 인스턴스 메서드가 있다.

```java
public void disableAll() {
    // 그룹의 모든 컨트롤 c에 대해 c.setDisable(true) 호출    
}
```

문제는 `setDisable()` 메서드가 Control 객체에 정의되어 있지만 임의 유형의 객체에는 정의되지 않는다는 것이다. String 및 Integer에는 `setDisable()` 메서드가 없기 때문에 ControlGroup<String> 또는 ControlGroup<Integer>과 같은 유형을 허용하는 것은 의미가 없다. ControlGrooup<T>에서 유형 매개변수 T를 제한하여 Control과 하위 클래스만 실제 유형 매개변수로 허용되도록 하는 방법이 필요하다.

```java
public class ControlGroup<T extends Control> {
    private ArrayList<T> components; // For storing the components in this group.
    public void disableAll( ) {
        for ( Control c : components ) {
            if (c != null)
                c.setDisable(true);
        }
    }
    public void enableAll( ) {
        for ( Control c : components ) {
            if (c != null)
                c.setDisable(false);
        }
    }
    public void add( T c ) {  // Add a value c, of type T, to the group.
        components.add(c);
    }
    
}
```

T에 대한 `extends Control` 제한으로 인해 매개변수화된 유형인 ControlGroup<String> 및 ControlGroup<Integer>를 생성하는 것이 불법이 된다. "T"를 대체하는 실제 유형 매개 변수는 Control 자체이거나 하위 클래스여야 하기 때문이다. 이러한 제한을 통해 우리는 그룹의 객체가 Control 유형이므로 그룹의 모든 `c`에 대해 `c.setDisable()` 작업이 정의 된다는 것을 알고 있으며 더 중요한 것은 컴파일러가 알고 있어야 한다.

일반적으로 제한된 유형 매개 변수 `T extends SomeType`는 대략 SomeType과 같거나 SomeType의 하위 클래스인 T 유형을 의미한다. 결과적으로 T 유형의 객체는 SomeType 유형이기도 하며 모든 유형은 T 유형이다. SomeType 유형의 객체에 대해 정의된 작업은 T 유형의 객체에 대해 정의된다. SomeType 유형은 클래스의 이름일 필요는 없으며 실제 객체 유형을 나타내는 모든 이름이 될 수 있다. 예를 들어 다음과 같다. `interface` 혹은 매개변수화된 유형이다.

제한된 유형과 와일드 카드 유형은 명확하게 관련되어 있다. 그러나 그들은 매우 다른 방식으로 사용된다. 제한된 유형은 제네릭 메서드, 클래스 또는 인터페이스 정의에서 형식 유형 매개 변수로만 사용할 수 있다. 와일드카드 유형은 메소드에서 형식 매개변수의 유형을 선언하는 데 가장 자주 사용되며 형식 유형 매개변수로 사용할 수 없다. 그런데 또 다른 차이점은 와일드카드 유형과 달리 제한된 유형 매개변수는 `extends`만 사용할 수 있고 `super`는 사용할 수 없다는 것이다.

제네릭 메소드를 선언할 때 제한된 유형 매개변수를 사용할 수 있다. 예를 들어 일반 ControlGroup 클래스 대신 다음과 같이 Control 컬렉션을 비활성화할 수 있는 독립형 일반 정적 메서드를 작성할 수 있다.

```java
public static <T extends Control> void disableAll(Collection<T> comps) {
    for ( Control c : comps )
        if (c != null)
            c.setDisable(true);
}
```

형식 유형 매개변수로 `<T extends Control>`을 사용하는 것은 기본 유형이 Control 또는 Button 또는 Slider 와 같은 Control 의 일부 하위 클래스인 컬렉션에 대해서만 메서드를 호출할 수 있음을 의미한다 .

이 경우에는 일반 유형 매개변수가 실제로 필요하지 않다. 와일드카드 유형을 사용하여 동등한 메소드를 작성할 수 있다.

```java
public static void disableAll(Collection<? extends Control> comps) {
    for ( Control c : comps )
        if (c != null)
            c.setDisable(true);
}
```

이 상황에서는 구현이 더 간단하므로 와일드카드 유형을 사용하는 버전이 선호된다. 그러나 제한된 유형 매개변수가 있는 일반 메소드를 와일드카드 유형을 사용하여 다시 작성할 수 없는 상황이 있다. 일반 유형 매개변수는 T 와 같은 이름을 알 수 없는 유형에 제공하는 반면, 와일드카드 유형은 알 수 없는 유형에 이름을 제공하지 않는다. 이름을 사용하면 정의 중인 메서드 본문에서 알 수 없는 유형을 참조할 수 있다. 제네릭 메서드 정의가 제네릭 유형 이름을 두 번 이상 사용하거나 메서드의 형식 매개 변수 목록 외부에서 사용하는 경우 제네릭 유형 매개 변수를 와일드카드 유형으로 바꿀 수 없다.

제한된 유형 매개변수가 필수적인 제네릭 메서드를 살펴본다. 섹션 10.2.1에서 수정된 목록이 여전히 정렬된 순서를 유지하는 방식으로 정렬된 문자열 목록에 문자열을 삽입하기 위한 코드 세그먼트를 제시했다. 

```java
static void sortedInsert(List<String> sortedList, String newItem) {
    ListIterator<String> iter = sortedList.listIterator();
    while (iter.hasNext()) {
        String item = iter.next();
        if (newItem.compareTo(item) <= 0) {
            iter.previous();
            break;
        }
    }
    iter.add(newItem);
}
```

이 방법은 문자열 목록에 적합하지만 다른 유형의 객체 목록에 적용할 수 있는 일반적인 방법이 있으면 좋을 것이다. 물론 문제는 코드에서 `compareTo()` 메서드가 목록의 개체에 대해 정의되어 있다고 가정하므로 이 메서드는 Comparable 인터페이스를 구현하는 개체 목록에 대해서만 작동할 수 있다는 것이다. 이 제한을 적용하기 위해 단순히 와일드카드 유형을 사용할 수는 없다. List<String>을 List<? extends Comparable>로 대체한다고 가정한다.

```java
static void sortedInsert(List<? extends Comparable> sortedList, ???? newItem) {
    ListIterator<????> iter = sortedList.listIterator();
   ...
}
```

와일드카드로 표시되는 알 수 없는 유형에 대한 이름이 없기 때문에 즉시 문제가 발생한다. `newItem` 및 `iter` 의 유형이 목록에 있는 항목의 유형과 동일해야 하므로 해당 유형에 대한 이름이 필요하다. 제한된 유형 매개변수를 사용하여 제네릭 메서드를 작성하면 문제가 해결된다. 그러면 알 수 없는 유형에 대한 이름이 있으므로 유효한 제네릭 메서드를 작성할 수 있다.

```java
static <T extends Comparable> void sortedInsert(List<T> sortedList, T newItem) {
    ListIterator<T> iter = sortedList.listIterator();
    while (iter.hasNext()) {
        T item = iter.next();
        if (newItem.compareTo(item) <= 0) {
            iter.previous();
            break;
        }
    }
    iter.add(newItem);
}
```

이 예에서는 아직 다루어야 할 한 가지 기술 사항이 있다. Comparable 은 그 자체로 매개변수화된 유형이지만 여기서는 유형 매개변수 없이 사용했다. 이는 합법적이지만 컴파일러에서 "raw type" 사용에 대한 경고를 표시할 수 있다. 실제로 목록의 객체는 T 유형의 항목과 비교되므로 매개변수화된 인터페이스 Comparable<T> 를 구현해야 한다. 이는 Comparable을 유형 바인딩으로 사용하는 대신 Comparable<T>를 사용해야 함을 의미한다.

```java
static <T extends Comparable<T>> void sortedInsert(List<T> sortedList, ...
```




















