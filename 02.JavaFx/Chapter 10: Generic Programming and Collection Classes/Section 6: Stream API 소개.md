# Stream API 소개

새로운 기능 중 하나인 Java 8에서는 데이터 컬렉션에 대한 작업을 표현하는 새로운 방법을 나타내는 스트림 API를 도입했다. 새로운 접근 방식의 주요 동기는 Java 컴파일러가 계산을 "병렬화"할 수 있도록 하는 것, 즉 계산을 여러 프로세서에서 동시에 실행할 수 있는 조각으로 나누는 것이다. 이렇게 하면 계산 속도가 크게 향상될 수 있다. 12장에서는 스레드를 사용한 Java 병렬 프로그래밍에 대해 설명한다. 스레드를 직접 사용하는 것은 어렵고 오류가 발생하기 쉽다. 스트림 API는 일부 종류의 계산을 자동으로 안전하게 병렬화할 수 있는 가능성을 제공하며 이러한 오류로 많은 관심을 불러일으킨 것은 놀라운 일이 아니다.

스트림 API를 정의하는 클래스와 인터페이스는 `java.util.stream` 패키지에 정의되어 있다. Stream은 스트림을 나타내고 스트림에 대한 기본 작업을 정의하는 해당 패키지의 인터페이스이다.

스트림 은 단순히 데이터 값의 시퀀스이다. 스트림은 Collection, 배열 또는 기타 다양한 데이터 소스에서 생성될 수 있다. 스트림 API는 스트림에서 작동하는 연산자 세트를 제공한다. (API는 일반 프로그래밍과 매개변수화된 유형을 광범위하게 사용하므로 이 장에서 다룬다.) 스트림 API를 사용하여 일부 계산을 수행한다는 것은 일부 소스에서 데이터를 가져오기 위한 스트림을 생성한 다음 다음과 같은 일련의 스트림 작업을 적용하는 것을 의미한다. 원하는 결과가 나올 거다. 스트림이 이런 방식으로 사용되면 다시 사용할 수 없다. 물론 다른 계산에 사용하려는 경우 일반적으로 동일한 데이터 소스에서 다른 스트림을 만들 수 있다.

계산을 일련의 스트림 작업으로 표현하려면 새로운 종류의 사고가 필요하며 익숙해지는 데 시간이 걸린다. 예부터 시작해 본다. `stringList` 가 `null` 요소가 아닌 큰 ArrayList<String> 이고 목록에 있는 문자열의 평균 길이를 알고 싶다고 가정해 본다 . 이는 for-each 루프를 사용하여 쉽게 수행할 수 있다.

```java
int lengthSum = 0;
for ( String str : stringList ) {
    lengthSum = lengthSum + str.length();
}
double average = (double)lengthSum / stringList.size();
```

스트림 API로 동일한 작업을 수행한다.

```java
int lengthSum = stringList.parallelStream()
                          .mapToInt( str -> str.length() )
                          .sum();
double average = (double)lengthSum / stringList.size();
```

이 버전에서 `stringList.parallelStream()`이 목록의 모든 요소로 구성된 스트림을 생성한다. `parallelStream`이라는 사실은 계산을 병렬화하는 것을 가능하게 한다. `mapToInt()` 메서드는 문자열 스트림에 맵 작업을 적용한다. 즉, 스트림에서 각 문자열을 가져와서 여기에 함수를 적용한다. 이 경우 함수는 문자열의 길이를 계산하여 int 유형의 값을 제공한다. 맵 작업으 결과는 새로운 스트림을 생성하는 것이다. 이번에는 함수의 모든 출력으로 구성된 정수 스트림이다. 마지막 연산인 `sum()`은 정수 스트림의 모든 숫자를 더하고 결과를 반환한다.

최종 결과는 목록에 있는 모든 문자열 길이를 합산한 것이다. 병렬화 가능성으로 인해 스트림 버전은 for 루프 버전보다 훨씬 더 빠를 수 있다. 실제로 스트림을 설정하고 조작하는 데 상당한 오버헤드가 발생하므로 속도 향상을 보려면 목록이 상당히 커야한다. 실제로 작은 목록의 경우 스트림 버전이 for 루프보바 시간이 더 오래 걸릴 것이 거의 확실하다.

스트림 API는 복잡하므로 여기서는 기본적인 소개만 할 수 있다. 하지만 그 정신의 일부 전달하는 데는 충분할 것이다.

## 1. 제네릭 Functional 인터페이스

많은 스트림 연산자는 종종 람다 표현식으로 제공되는 매개변수를 사용한다. 위 예제의 `mapToInt()` 연산자는 String에서 int까지 함수를 나타내는 매개변수를 사용한다. 해당 매개 변수의 유형은 `java.util.function` 패키지의 매개변수화된 함수형 인터페이스인 `ToIntFunction<T>`에 의해 제공된다. 이 인터페이스는 T 유형의 입력을 받아 int를 출력하는 함수의 일반적인 아이디어를 나타낸다. 해당 인터페이스의 정의를 살펴보면 다음과 같다.

```java
public interface ToIntFunction<T> { 
    public int applyAsInt( T x );
}
```

Stream<T>도 매개변수화된 인터페이스이다. 위의 예에서 `stringList`는 ArrayList<String> 유형이고 `stringList.parallelStream()`에 의해 생성된 스트림은 Stream<String> 유형이다. `mapToInt()` 연산자는 해당 스트림에 적용될 때 `ToIntFunction<String>` 유형의 매개변수가 필요하다. 람다식 `str -> str.length()`는 String을 int에 매핑하므로 올바른 유형의 값을 나타낸다. 다행히 스트림 API를 사용하기 위해 이 모든 것을 생략할 필요는 없다. `mapToInt`를 사용하려면 문자열을 int로 매핑하는 함수를 제공해야 한다. 그러나 API 문서를 읽으려면 ToIntFunction과 유사한 매개 변수 유형을 처리해야 한다.

`java.util.function` 패키지에는 다수의 제네릭 함수형 인터페이스가 포함되어 있다. ToIntFunction과 같은 이들 중 다수는 매개변수화된 유형이며, 설정된 의미가 없는 매우 일반적인 함수를 나타낸다는 점에서 모두 일반적이다. 예를 들어 함수형 인터페이스 DoubleUnaryOperator는 double에서 double까지의 함수에 대한 일반적인 아이디어를 나타낸다. 

`java.util.function`의 인터페이스는 많은 스트림 연산자와 Java API의 기타 내장 함수에 대한 매개 변수 유형을 지정하는 데 사용되며, 이를 사용하여 자신의 서브 루틴에 대한 매개 변수 유형을 지정할 수도 있다. 여기서는 그 중 일부를 논의한다. 대부분의 다른 것들은 내가 다루는 것의 변형이다. 

일반 용어 predicate는 반환 유형이 boolean인 함수를 나타낸다. 함수형 인터페이스 Predicate<T>는 T 유형의 매개변수를 사용하여 boolean 값 또는 `test(t)`를 정의한다. 예를 들어 이 인터페이서는 Collection에 대해 정의된 `removeIf(p)` 메서드의 매개변수 유형으로 사용된다. 예를 들어 `strList`가 LinkedList<String> 유형인 경우 다음과 같이 간단히 목록에서 모든 `null` 값을 제거할 수 있다.

```java
strList.removeIf(s -> (s == null) );
```

매개 변수는 입력 `s`가 `null`인지 여부를 테스트하는 Predicate<String>이다. `removeIf()` 메서드는 목록에서 조건자 값이 `true`인 모든 요소를 제거한다. 

int 값을 테스트하기 위한 조건자는 Predicate<Integer> 유형으로 표시될 수 있지만 이는 Integer 유형 래퍼의 모든 int를 오토박싱하는 오버헤드를 발생시킨다. 이러한 오버헤드를 피하기 위해 `java.util.function` 패키지에는 boolean 값 함수 `test(n)`을 정의하는 함수형 인터페이스 IntPredicate가 있다. 여기서 n은 int 유형이다. 마찬가지로 DoublePredicate 및 LongPredicate를 정의한다. 이는 스트림 API가 기본 유형을 처리하는 일반적인 방법이다. 예를 들어 IntStream을 정의한다. Stream<Integer>에 대한 보다 효율적인 대안으로 int 스트림을 나타낸다.

함수형 인터페이스 Supplier<T>는 매개 변수가 없고 T의 반환 유형이 없는 함수, `get()`을 정의합니다. 이는 T 유형의 값의 소스를 나타낸다. T유형의 매개변수를 사용하여 `voie` 함수 `accept(t)`를 정의하는 동반 인터페이스 Consumer<T>가 있다. IntSupplier, IntConsumer, DoubleSupplier를 포함하여 기본 유형에 대한 특수 번도 있다. 아래는 Suppliers와 Consumers를 사용하는 예를 제시한다.

Function<T, R>은 T 유형의 값에서 R 유형의 값까지의 함수를 나타낸다. 이 함수형 인터페이스는 `apply(t)` 함수를 정의한다. 여기서 t는 T 유형이고 반환 유형은 R이다. UnaryOperator<T> 인터페이스는 기본적으로 Function<T, T>이다. 즉, 입력 유형과 출력 유형이 동일한 함수를 나타낸다. DoubleUnaryOperator는 UnaryOperator<Double>의 특수 버전이다.

마지막으로 BinaryOperator<T>와 IntBinaryOperator와 같은 전문화에 대해 언급한다. BinaryOperator<T> 인터페이스는 `t1`과 `t2`가 모두 T 유형이고 반환 유형인 T인 `apply(t1, t2)` 함수를 정의한다.

## 2. 스트림 만들기

스트림 API를 사용하려면 먼저 스트림을 생성해야 한다. 스트림을 만드는 방법에는 여러 가지가 있다.

스트림에는 순자 스트림(sequential streams)와 병렬 스트림(paraller streams) 두 가지 기본 유형이 있다. 차이점은 병렬 스트림의 작업은 잠재적으로 병렬화될 수 있지만 스트림의 값은 for 루프에서와 마찬가지로 단일 프로세스에서 항상 순차적으로 처리된다는 것이다. (순차 스트림이 존재해야 하는 이유가 명확하지 않을 수 있지만 일부 작업은 안전하게 병렬화할 수 없다.) 스트림을 한 유형에서 다른 유형으로 변환하는 것이 가능하다. `stream`이 Stream이 `stream.parallel()`은 동일한 값 스트림을 나타내지만 병렬 스트림으로 변환된다. 마찬가지로 `stream.sequential()`과 동일한 값을 가진 순차 스트림이다.

우리는 `c`가 Collection 이라면 `c.parallelStream()`은 그 값이 컬렉션의 값인 스트림이라는 것을 이미 보았다. 짐작할 수 있듯이 이는 병렬 스트림이다. `c.stream()` 메서드는 동일한 값의 순차적 스틀미을 생성한다. 이는 목록 및 세트를 포함한 모든 컬렉션에 적용된다. `c.stream().paraller()`을 호출하여 병렬 스트림을 얻을 수도 있다.

배열에는 `stream()` 메서드가 없지만 `java.util` 패키지의 Arrays 클래스에 있는 `static` 메서드를 사용하여 배열에서 스트림을 생성할 수 있다. 

```java
Arrays.stream(A)
```

배열의 값을 포함하는 순차적 스트림이다. (병렬 스트림을 얻으려면 `Arrays.stream(A).parallel()`을 사용한다.) 이는 객체 배열과 기본 유형 int, double, long 배열에 작동한다. `A`가 T[] 유형인 경우 스트림은 Stream<T> 유형이다. `A`가 int인 배열인 경우 결과는 IntStream이고 double, long인 경우도 마찬가지이다.

`supplier`가 Supplier<T> 유형이라고 가정한다. `supplier.get()`을 반복해서 호출하여 T 유형의 값 스트림을 생성하는 것이 가능해야 한다. 해당 스트림은 실제로 다음을 사용하여 생성될 수 있다.

```java
Stream.generate(supplier)
```

스트림은 순차적이며 사실상 무한이다. 즉, 영원히 또는 거렇게 하려고 하면 오류가 발생할 때까지 계속해서 값을 생성한다. 마찬가지로 `IntSteam.generate(s)`는 IntSupplier에서 int 값의 스트림을 생성하고 DoubleSupplier는 double 스트림을 생성한다.

```java
DoubleStream.generate(() -> Math.random());
```

무한한 난수 스트림을 생성한다. 실제로 Random 유형의 변수 `rand`에서 유사한 난수 값을 스트림을 얻을 수 있다. `rand.double()`는 0에서 1 범위에 있는 난수의 무한 스트림이다. 유한한 수의 난수만 원하면 `rand.doubles(count)`를 사용하세요. Random 클래스에는 임의의 double 및 int 스트림을 생성하는 다른 메서드가 있다. 다양한 표준 클래스에서 스트림을 생성하는 다른 방법을 찾을 수 있다.

IntStream 인터페이스는 주어진 범위의 정수 값을 포함하는 스트림을 생성하는 방법을 정의한다.

```java
IntStream.range(start, end)
```

`start`, `start+1`, ..., `end-1` 값을 포함하는 순차 스트림이다.

최신 버전의 Java에는 스트림을 만들기 위한 몇 가지 추가 방법이 도입되었다. 예를 들어 Java 11에서 Scanner `input`의 경우 `input.tokens()` 메서드는 `input.next()`를 반복하여 호출하여 반환되는 모든 문자열로 구성된 스트림을 생성한다. 그리고 여러 줄의 텍스트가 포함된 String `str`의 경우 Java 11에서는 스트림을 생성하는 `str.lines()`을 추가했다.

## 3. 스트림에서 작업

스트림의 일부 작업은 다른 스트림을 생성한다. 최종 결과를 생성하려면 결과 스트림에 대해 여전히 작업을 수행해야 하기 때문에 이를 "중간 작업(intermediate operations)"이라 한다. 반면에 "터미널 작업(terminal operations)"은 스트림에 적용되어 스트림이 아닌 결과를 생성한다. 스트림 작업의 일반적인 패턴은 스트림을 생성하고, 여기세 일련의 중간 작업을 적용한 다음 터미널 작업을 적용하여 원하는 최종 결과를 생성하는 것이다. 이 섹션의 시작 부분에 있는 예제에서 `mapToInt()`는 문자열 스트림에 int 스트림으로 변환하는 중간 작업이고 `sum()`은 int 스트림에서 숫자의 합을 구하는 터미널 작업이다.

가장 기본적인 두 가지 중간 작업은 필터와 맵이다. 필터는 Predicate(조건자)를 스트림에 적용하고 조건가가 참인 원래 스트림의 값으로 구성된 새 스트림을 생성한다. 예를 들어, 정수 `n`이 소수인지 테스트하는 boolean 값 함수 `isPrime(n)`이 있다면

```java
IntStream.range(2, 1000).filter(n -> isPrime(n))
```

2에서 1000사이의 모든 소수르르 포함하는 스트림을 생성한다.

맵은 스트림의 각 값에 함수를 적용하고 출력 값으로 구성된 스트림을 생성한다. 예를 들어 `strList`가 ArrayList<String>이고 목록에 있는 `null`이 아닌 모든 문자열로 구성된 스트림을 소문자로 변환하려고 가정한다.

```java
strList.stream().filter(s -> (s != null)).map(s -> s.toLowerCase())
```

전문화 `mapToInt()`, `mapToDouble()`, `mapToLong()`은 매핑하기 위해 존재한다.

다음은 유용할 수 있는 스트림 `S`에 대한 몇 가지 추가 중간 작업이다. `S.limit(n)`은 `S`의 처음 n 값만 포함하는 스트림을 생성한다. `S.distinct()`는 중복된 값을 생략하여 `S`의 값에서 스트림을 생성하므로 모든 값은 `S.distinct()`와 다르다. 그리고 `S.sorted()`는 S와 동일한 값을 포함하는 스트림을 생성한다. 그러나 정렬된 순서로 되어 있다. 자연 순서가 없는 항목을 정렬하려면 `sorted()`에 매개 변수로 Comparator를 제공할 수 있다. `S.limit(n)`은 Supplier로 부터 생성된 스트림과 같이 무한 스트림이 될 것을 자르는 데 특히 유용할 수 있다.

---

실제로 스트림으로 작업을 수행하려면 마지막에 터미널 작업을 적용해야 한다. `forEach(c)` 연산자는 각 요소에 Consumer `c`를 적용한다. 소비자는 가치를 생산하지 않으므로 결과는 스트림이 아니다. 스트림 `S`에 대한 `S.forEach(c)`의 효과는 단순히 스트림의 각 값에 대해 작업을 수행하는 것이다. 예를 들어 문자열 목록의 모든 문자열을 인쇄하는 완전히 새로운 방식이 있다.

```java
strList.stream().forEach(s -> System.out.println(s));
```

병렬 스트림의 경우 소비자 함수가 스트림에서 발생하는 것과 동일한 순서로 스트림의 값에 적용된다는 보장이 없다. 이를 보장하려면 `forEach(c)` 대신 `forEachOrdered(c)`를 사용할 수 있다.

문자열 중 일부만 인쇄하려면 (예. 길이가 5이상인 문자열) 정렬된 순서로 원하는 경우 몇 가지 필터를 적용할 수 있다.

```java
stringList.stream()
        .filter(s -> (s.length() >= 5))
        .sorted()
        .forEachOrdered(s -> System.out.println(s))
```

일부 터미널 작업은 단일 값을 출력한다. 예를 들어, `S.count()`는 스트림 S의 값 수를 반환한다. IntStream, LongStream, DoubleStream에는 스트림의 모든 값의 합계를 계산하는 터미널 작업 `sum()`이 있다. 예를 들어 10000개의 난수를 생성하고 그 중 0.5보다 작은 수를 계산하여 난수 생성기를 테스트한다고 가정해 본다.

```java
long half = DoubleStream.generate(Math::random)
        .limit(10000)
        .filter(x -> (x < 0.5))
        .count();
```

`count()`는 int가 아닌 long을 반환한다는 점에 유의하세요. 또한 여기서 동등한 람다 표현식 `() -> Math.random()` 대신 `Math::randmo` 메서드 참조를 사용했다. 이와 같은 내용을 읽는 데 어려움이 있는 경우 패턴은 스트림 생성, 일부 중간 작업 적용, 터미널 작업 적용이라는 점을 명심하자. 여기서는 `Math.random()`을 계속해서 호출하여 무한한 난수 스트림이 생성된다. 작업 `limit(10000)`은 해당 스트림을 10000개 값으로 자르므로 실제로는 10000개 값만 생성된다. `filter()` 연산은 `x < 0.5`가 참이 되도록하는 숫자만 통과한다. 마지막으로 `count()`는 결과 스트림의 항목 수를 반환한다.

Stream<T>에는 스트림에서 가장 작은 값과 가장 큰 값을 반환하는 터미널 작업 `min(c)` 및 `max(c)`도 있다. 매개 변수 `c`는 Comparator<T> 유형이다. 값을 비교하는 데 사용된다. 그러나 `min()`, `max()`의 반환 유형은 약간 독특하다. 반환 유형은 Optional<T> 이며, 존재하거나 존재하지 않을 수 있는 T 유형의 값을 나타낸다. 문제는 빈 스트림에서 최대값과 최소값이 없기 때문에 빈 스트림의 최소값과 최대값이 존재하지 않는다는 점이다. Optional에는 `get()`이 있다. Optional 값이 있는 경우 해당 값을 반환하는 메서드이다. Optional이 비어 있으면 예외가 발생한다. 예를 들어 `words`가 Collection<String>인 경우 다음을 사용하여 컬렉션에서 가장 긴 문자열을 얻을 수 있다.

```java
String longest = words.parallelStream()
        .max((s1, s2) -> s1.length() - s2.length() )
        .get();
```

그러나 컬렉션이 비어 있으면 예외가 발생한다. (Optional의 boolean 값 메서드 `isPresent()`를 사용하여 값이 존재하는지 테스트할 수 있다.)

마찬가지로 IntStream, LongStream은 OptionalInt, OptionalLong 유형의 값을 반환하는 터미널 작업 `min()`, `max()`를 제공한다. 이러한 각 클래스에는 OptionalDouble을 반환하는 `average()` 메서드도 있다.

터미널 연산자 `allMatch(p)` 및 `anyMatch(p)`는 조건자를 매개변수로 사용하고 boolean 값을 계산한다. 조건자 `p`가 적용되는 스트림의 모든 값에 대해 true인 경우 `allMatch(p)`의 값은 true이다. 스트림에 p가 true인 값이 하나 이상 있으면 `anyMatch(p)`의 값이 true이다. `anyMatch()`는 조건을 충족하는 값을 찾으면 처리를 중지하고 출력으로 `true`를 반환한다. 그리고 `allMatch()`는 조건자와 일치하지 않는 값을 찾으면 처리를 중지한다.

단일 값을 계산하는 많은 터니멀 작업 보다 일반적인 작업인 **축소(reduce)** 로 표현될 수 있다. 축소 작업은 BinaryOperator를 사용하여 스트림의 값을 결합한다. 예를 들어 합계는 이진 연산이 덧셈인 축소 연산으로 계산된다. 이항 연산자는 결합적이어야 한다. 즉, 연산자가 적용되는 순서는 중요하지 않다. 스트림에 있는 값의 곱을 계산하는 기본 제공 터미널 연산자는 없지만, `reduce`를 사용하여 직접 계산할 수 있다. 예를 들어, `A`가 double 배열이고 0이 아닌 모든 요소의 곱을 원한다고 가정한다.

```java
double multiply = Arrays.stream(A).filter( x -> (x != 0) )
                                  .reduce( 1, (x,y) -> x*y );
```

여기서 이항 연산자는 숫자 쌍 (x, y)를 곱 `x*y`에 매핑한다. `reduce()`의 첫 번째 매개 변수는 이진 연산의 "identity"이다. 즉, 임의의 `x`에 대해 `1*x=x`가 되는 값이다. double 스트림의 최대값은 `reduce(Double.NEGATIVE_INFINITY, Math::max)`를 사용하여 계산할 수 있다.

마지막 주요 터미널 작업은 스트림의 모든 값을 데이터 구조 또는 일부 유형의 단일 요약 결과로 수집하는 매우 일반적인 작업인 `collect(c)`이다. 매개 변수 `c`는 수집기라고 불리는 것이다. 컬렉터는 일반적으로 Collectors 클래스의 `static` 함수 중 하나에 의해 제공된다. 이는 매우 복잡해 질 수 있으므로 몇 가지 예만 든다. `Collectors.toList()` 함수는 스트림의 모든 값을 List에 넣기 위해 `collect()`와 함께 사용할 수 있는 Collector를 반환한다. 예를 들어, `A`가 `null`이 아닌 배열이라고 가정한다. 

```java
List<String> freds = Arrays.stream(A)
        .filter( s -> s.startsWith("Fred") )
        .collect( Collectors.toList() );
```

실제로는 꽤 쉽다. 일부 기준에 따라 스트림의 항목을 그룹화하는 수집기가 훨씬 더 유용하다. `Collectors.groupingBy(f)` 콜렉터는 함수형 인터페이스 Function<T, S>에 의해 유형이 지정되는 f 매개변수를 사용하여 T 유형 값에서 S 유형 값까지의 함수를 나타낸다. `collect()`와 함께 `Collectors.groupingBy(f)`는 Stream<T> 유형의 스트림에서 작동하며 항목에 적용될 때 함수 `f`의 값을 기반으로 스트림의 항목을 그룹으로 분리한다. 즉, 모든 항목 `x`, 주어진 그룹에서 `f(x)`에 대해 동일한 값을 갖는다. 결과는 Map<S, List<T>>이다. 이 맵에서 키는 함수 값 `f(x)` 중 하나이며 해당 키에 연결된 값은 `f`가 해당 함수 값을 할당하는 스트림의 모든 항목을 포함하는 목록이다.

예를 들어보면 상황이 명확해질 것이다. 각 사람이 이름과 성을 갖고 있는 사람들의 배열이 있다고 가정해 본다. 그리고 사람들을 특정 성을 가진 모든 살마들로 구성된 그룹으로 나누고 싶다고 가정해 본다. 사람은 `firstName` 및 `lastName`이라는 인스턴스 변수를 포함하는 Person 유형의 객체로 표현될 수 있다. `population`이 Person[] 유형의 변수라고 가정해 본다. 그런 다음 `Arrays.stream(polulation)`은 Person의 스트림이고 다음 코드를 사용하여 스트림의 사람들을 성별로 그룹화할 수 있다.

```java
Map<String, List<Person>> families;
families = Arrays.stream(population)
                 .collect(Collectors.groupingBy( person -> person.lastname ));
```

여기서 람다식 `person -> person.lastName`은 구룹화 함수를 정의한다. 이 함수는 Person을 입력으로 사용하고 해당 사람의 성을 제공하는 문자열을 출력한다. 결과는 Map, `families`에는 키는 Person의 성 중 하나이고 해당 성과 연관된 값은 해당 성을 가진 모든 Person을 포함하는 List이다. 다음과 같이 그룹을 인쇄할 수 있다.

```java
for ( String lastName : families.keySet() ) {
    System.out.println("People with last name " + lastName + ":");
    for ( Person name : families.get(lastName) ) {
        System.out.println("    " + name.firstname + " " + name.lastname);
    }
    System.out.println();
}
```

설명이 조금 길긴 하지만, 결과는 비교적 이해하기 쉬울 것이다.

## 4. 실험

지금까지 제시한 스트림 사용 예제의 대부분은 그다지 실용적이지 않다. 대부분의 경우 간단한 for 루프는 작성하기 쉽고 더 효율적일 것이다. 제가 주로 순차 스트림을 사용했고 대부분의 예제를 효율적으로 병렬화할 수 없었기 때문에 특히 그렇다. (주목할만한 예외는 축소 작업인데, 이는 병렬화가 잘 되기 때문에 중요하다.) 병렬화를 통해 실제 속도 향상을 얻을 수 있는 긴 계산에 스트림 API가 적용되는 예를 살펴본다. 문제는 리만 합(Riemann sum)을 계산하는 것이다. 이것은 미적분학의 내용이지만 그것이 의미하는 바를 전혀 이해할 필요는 없다. 원하는 합계를 계산하는 전통적인 방법은 다음과 같다.

```java
/**
 * 리만 합을 계산하려면 기본 for 루프를 사용하세요.
 * @param f 합산할 함수입니다.
 * @param a f가 합산되는 간격의 왼쪽 끝점입니다.
 * @param b 오른쪽 끝점입니다.
 * @param n 간격의 분할 수입니다.
 * @return 리만 합계에 대해 계산된 값입니다.
 */
private static double riemannSumWithForLoop(
        DoubleUnaryOperator f, double a, double b, int n) {
    double sum = 0;
    double dx = (b - a) / n;
    for (int i = 0; i < n; i++) {
        sum = sum + f.applyAsDouble(a + i*dx);
    }
    return sum * dx;
}
```

첫 번째 매개변수의 유형은 함수형 인터페이스이므로 이 메서드를 다음과 같이 호출할 수 있다.

```java
reimannSumWithForLoop( x -> Math.sin(x), 0, Math.PI, 10000 )
```

이 문제에 스트림 API를 어떻게 적용할 수 있나? for 루프를 모방하기 위해 `intStream.range(0, n)`을 사용하여 0부터 n까지의 정수를 스트림으로 생성하는 것으로 시작할 수 있다. 이는 순차적 스트림을 제공한다. 병렬성을 활성화하려면 `.parallel()` 작업을 적용하여 병렬 스트림으로 변환해야 한다. 합산하려는 값을 계산하기 위해 정수 i를 `f.applyAsDouble(a+i*dx)`에 매핑하여 int 스트림을 double 스트림에 매핑하는 맵 작업을 적용할 수 있다ㅏ. 마지막으로 `sum()`을 적용할 수 있다. 터미널 작업으로, 다음은 병렬 스트림을 사용하는 방법의 버전이다.

```java
private static double riemannSumWithParallelStream(
            DoubleUnaryOperator f, double a, double b, int n) {
    double dx = (b - a) / n;
    double sum = IntStream.range(0,n)
            .parallel()
            .mapToDouble( i -> f.applyAsDouble(a + i*dx) )
            .sum();
    return sum * dx;
}
```

또한 `.parallel()` 연산자를 생략한 `riemannSumWithSequentialStream()` 버전도 작성했다. 세 가지 버전 모두 샘플 프로그램 [RiemannSumStreamExperiment.java](https://math.hws.edu/javanotes/source/chapter10/RiemannSumStreamExperiment.java)에서 찾을 수 있다. 해당 프로그램의 기분 루틴은 n에 대한 다양한 값을 사용하여 세 가지 메서드를 각각 호출한다. 각 메서드가 합계를 계산하는 데 걸리는 시간을 측정하고 결과를 보고한다.

예상한 대로 순차 스트림을 사용하는 버전이 다른 버전보다 균일하게 느린 것을 발견했다. 순차 스트림 버전은 기본적으로 for 루프 버전과 동일한 작업을 수행하지만 스트림 생성 및 조작과 관련된 추가 오버헤드가 있다. 병렬 스트림의 상황은 더 흥미롭고 결과는 프로그램이 실행되는 시스템에 따라 달라진다. 4개의 프로세서가 있는 오래된 시스템에서는 for 루프 버전이 n 에 대해 더 빨랐다. 100,000이지만 항목이 1,000,000개 이상인 경우 병렬 버전이 훨씬 더 빨랐다. 다른 컴퓨터에서는 10,000개 이상의 항목에 대해 병렬 버전이 더 빨랐다. 병렬 버전의 속도에는 제한이 있다. K개의 프로세서가 있는 시스템에서 병렬 버전은 순차 버전보다 K배 이상 빠를 수 없으며 실제로는 그보다 다소 느릴 수 있다. 자신의 컴퓨터에서 샘플 프로그램을 사용해 보시기 바란다!

Java가 그래픽 카드에 포함된 많은 프로세서를 활용하여 병렬 코드를 실행할 수 있는 시스템이 있다는 것도 생각할 수 있다(또는 적어도 이것이 스트림 API의 목표이다). 그렇게 되면 속도가 매우 크게 향상될 수 있다.


















