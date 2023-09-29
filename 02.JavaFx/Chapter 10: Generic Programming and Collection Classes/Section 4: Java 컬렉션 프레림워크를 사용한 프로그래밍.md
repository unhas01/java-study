# Java 컬렉션 프레임워크를 사용한 프로그래밍

이 섹션에서는 Java Collection Framework의 클래스를 사용하는 몇 가지 프로그래밍 예제를 살펴본다. 컬렉션 프레임워크는 특히 처음부터 새로운 데이터 구조를 프로그래밍하는 어려움에 비해 사용하기 쉽다.

## 1. Symbol Tables(기호표)

우리는 간단하지만 중요한 맵 적용부터 시작한다. 컴파일러가 프로그램의 소스 코드를 읽을 때 변수, 서브 루틴 및 클래스의 정의를 만나게 된다. 이러한 항목은 이름은 나중에 프로그램에서 사용할 수 있다. 컴파일러는 이름을 인식하고 나중에 프로그램에서 해당 이름을 발견할 때 정의를 적용할 수 있도록 각 이름의 정의를 기억해야 한다. 이는 Map에 대한 자연스러운 적용이다. 이름은 맵에서 키로 사용될 수 있다. 키와 연관된 값은 이름의 정의이며, 어떻게는 객체로 인코딩된다. 이런식으로 사용되는 맵을 심볼 테이블이라고 한다. 

컴파일러에서 심볼 테이블의 값은 상당히 복잡할 수 있다. 왜냐하면 컴파일러는 다양한 종류의 사물에 대한 이름을 처리해야 하고 각 유형의 이름에 대해 서로 다른 유형의 정보가 필요하기 때문이다. 우리는 다른 맥락에서 기호 테이블을 살펴봄으로써 일을 단순하게 유지할 것이다. 사용자는 입력한 표현식을 평가할 수 있는 프로그램을 원하고 표현식에 연산자, 숫자, 괄호 외에 변수가 포함될 수 있다고 가정해 본다. 이를 이해하려면 변수에 값을 할당하는 방법이 필요하다. 변수가 표현식에 사용되면 변수의 값을 검색해야 한다. 심볼 테이블을 사용하여 필요한 데이터를 저장할 수 있다. 심볼 테이블의 키는 변수 이름이다. 키와 연관된 값은 해당 변수의 값이다. 심볼 테이블은 Map<String, Double> 유형의 객체가 된다. (double과 같은 기본 유형은 유형 매개 변수루 사용될 수 없음을 기억하자.)

아이디를 보여주기 위해 사용자가 다음과 같은 명령을 입력하는 다소 단순한 프로그램을 사용한다.

```
let x = 3 + 12
print 2 + 2
print 10*x +17
let rate = 0.06
print 1000*(1+rate)
```

이 프로그램은 매우 간단한 언어에 대해 통역사(interpreter)이다. 프로그램이 이해하는 명령어는 "print", "let" 두 가지 뿐이다. "print" 명령이 실행되면 컴퓨터는 표현식을 평가하고 값을 표시한다. 표현식에 변수가 포함되어 있으면 컴퓨터는 심볼 테이블에서 해당 변수의 값을 찾아야 한다. "let" 명령은 변수에 값을 부여하는 데 사용된다. 컴퓨터는 변수의 값을 심볼 테이블에 저장해야 한다. (참고: 여기서 말하는 "변수"는 Java 프로그래의 변수가 아니다. Java 프로그램은 사용자가 입력한 일종의 프로그램을 실행한다.)

섹션 9.5.2에서 우리는 변수를 포함하지 않는 표현식을 평가할 수 있는 [SimpleParser2.java](https://math.hws.edu/javanotes/source/chapter9/SimpleParser2.java) 프로그램을 작성하는 방법을 살펴보았다. 여기서는 이진 프로그램을 기반으로 하는 또 다른 예제 프로그램인 [SimpleInterpreter.java](https://math.hws.edu/javanotes/source/chapter10/SimpleInterpreter.java)에 대해 설명한다. 심볼 테이블과 관련된 부분에 대해서만 이야기한다.

프로그램은 HashMap을 심볼 테이블로 사용한다. TreeMap을 사용할 수 도 있지만 프로그램이 알파벳 순서로 변수에 엑세스할 필요가 없으므로 키를 정렬된 순서로 저장할 필요가 없다. 프로그램의 심볼 테이블은 HashMap<String, Double> 유형의 `symbolTable`이라는 변수에 표시된다. 프로그램 시작 시 다음 명령을 사용하여 심볼 테이블 객체가 생성된다.

```java
symbolTable = new HashMap<>();
```

그러면 처음에는 키/값 연결이 포함되지 않은 맵이 생성된다. "let" 명령을 실행하기 위해 프로그램은 심볼 테이블의 `put()` 메서드를 사용하여 값을 변수 이름과 연결한다. 변수 이름이 String 유형 `varName`으로 제공되고 변수 값이 double 유형인 `val`에 저장되어 있다고 가정한다. 그런 다음 명령은 심볼 테이블의 변수와 관련된 값을 설정한다.

```java
symbolTable.put(varName, val);
```

[SimpleInterpreter.java](https://math.hws.edu/javanotes/source/chapter10/SimpleInterpreter.java) 프로그램의 `doLetCommand()`라는 메서드에서 이를 찾을 수 있다. 심볼 테이블에 저장되는 실제 값은 Double 유형의 객체이다. Java는 필요할 때 double 유형을 자동으로 Double로 변환하기 때문에 `put`에서 double 값 `val`을 사용할 수 있다.

단지 재미를 위해 값이 일반적인 수학 상수 π 및 e인 "pi", "e"라는 두 변수를 미리 정의하기로 결정했다. Java에서 이러한 상수의 값은 Math.PI및 Math.E에 의해 제공된다. 프로그램은 사용자가 이러한 변수를 사용할 수 있도록 하기 위해 다음 명령을 사용하여 심볼 테이블에 추가한다.

```java
symbolTabel.put("pi", Math.PI);
symbolTabel.put("e", Math.E);
```

프로그램이 표현식을 평가하는 동안 변수르르 발견하면 심볼 테이블의 `get()` 메서드를 사용하여 해당 값을 검색한다. `symbolTable.get(varName)` 함수는 Double 유형의 값을 반환한다. 반환 값이 `null` 일 수도 있다. 이는 심볼 테이블의 `varName`에 값이 할당되지 않은 경우 발생한다. 이 가능성을 확인하는 것이 중요하다. 이는 사용자가 정의하지 않은 변수를 사용하려 함을 나타낸다. 프로그램은 이를 오류로 간주하므로 처리 과정은 다음과 같다.

```java
Double val = symbolTable.get(varName);
if (val == null) {
   ... // Throw an exception:  Undefined variable.
}
```

## 2. Map 내부의 세트(Sets)

컬렉션이나 맵의 객체는 모든 유형이 될 수 있다. 컬렉션이 될 수도 있다. 다음은 세트를 맵의 값 객체로 저장하는 것이 자연스로운 예이다. 

책의 책인을 만드는 문제를 생각해 보자. 색인은 책에 나오는 용어 목록으로 구성된다. 각 용어 옆에는 해당 용어가 나타나는 페이지 목록이 있다. 프로그램에서 색인을 나타내려면 각 용어에 대한 페이지 목록과 함께 용어 목록을 보유할 수 있는 데이터 구조가 필요하다. 새로운 데이터를 추가하는 것은 쉽고 효율적이어야 한다. 색인을 인쇄할 때는 알파벳 순서로 용어에 쉽게 접근할 수 있어야 한다. 이를 수행할 수 있는 방법은 여러 가지가 있지만 저는 Java의 일반 데이터 구조를 사용하고 가능한 한 많은 작업을 수행하도록 하고 싶다.

인덱스는 각 용어에 대한 페이지 참조 목록을 연결하는 맵으로 생각할 수 있다. 용오는 키이며 연관된 값은 해당 용어에 대한 페이지 참조 목록이다. Map은 TreeMap과 HashMap 일 수 있지만 TreeMap만 가능하다. 정렬된 순서로 용어에 쉽게 엑세스할 수 있다. 용어와 연관된 값은 페이지 참조 목록이다. 그러한 가치를 어떻게 표현할 수 있는가?? 생각해 보면 실제로는 Java의 일반 클래스라는 의미에서 목록이 아니라는 것을 알 수 있다. 색인을 살펴보면 페이지 참조 목록에 중복된 항목이 없다는 것을 알 수 있으므로 실제로는 목록이 아니라 집합이다. 게다가, 주어진 용어에 대한 페이지 참조는 항상 오름차순으로 인쇄되므로 정렬된 세트가 필요하다. 이는 각 페이지 참조 목록을 나타내기 위해 TreeSet을 사용해야 함을 의미한다. 우리가 실제로 이 세트에 넣고 싶은 값은 int 유형이지만 일반 데이터 구조는 객체만 보유할 수 있다는 사실을 다시 한 번 처리해야 하므로 래퍼 클래스를 사용해야 한다.

요약하면 인덱스는 TreeMap으로 표시된다. 맵의 키는 String 유형의 용어이다. 맵의 값은 페이지 번호를 나타내는 정수를 보함하는 TreeSet이다. 세트에 사용해야 하는 매개 변수 유형은 TreeSet<Integer>이다. 인덱스를 전체적으로 표현하는 TreeMap의 경우 키 유형은 String, 값 유형은 TreeSet<Integer>이다. 이는 인덱스에 유형이 있음을 의미한다.

```java
TreeMap<String, TreeSet<Integer>>
```

이것은 `K=String` 및 `V=TreeSet<Integer>`인 일반적인 TreeMap<K, V>이다. 이처럼 복잡한 유형 이름은 어렵게 보일 수 있지만 우리가 표현하려는 데이터 구조를 생각하면 의미가 있다.

색인을 만들려면 빈 TreeMap으로 시작해야 한다. 그런 다음 책 전체를 살펴보고 색인에 포함하려는 모든 참조를 맵에 삽입한다. 그런 다음 맵에서 데이터를 인쇄해야 한다. 인덱스에 넣을 참조를 어떻게 찾는지는 제쳐두고 TreeMap이 어떻게 사용되는지 살펴본다.

```java
TreeMap<String, TreeSet<Integer>> index;
index = new TreeMap<>();
```

(이 복합 유형의 경우에도 유형 매개 변수는 생성자에서 생략될 수 있다.)

이제 일부 `pageNum` (int 유형)에서 일부 `term` (String 유형)에 대한 참조를 찾았다고 가정한다. 이 정보를 색인에 삽입해야 한다. 이렇게 하려면 `index.get(term)`을 사용하여 색인에서 용어를 찾아야 한다. 반환 값이 `null` 이거나 이전에 해당 용어에 대해 찾은 페이지 참조 집합이다. 반환 값이 `null`이면 이것이 용어에 대한 첫 번째 페이지 참조이므로 방금 찾은 페이지 참조가 포함된 새 세트와 함께 용어를 인덱스에 추가해야 한다. 반환 값이 `null`이 아닌 경우, 우리는 이미 페이지 참조 세트를 가지고 있으며, 세트에 새 페이지 참조를 추가하기만 하면 된다. 다음은 이를 수행하는 서브루틴이다.

```java
/**
 * 색인에 페이지 참조를 추가합니다.
 */
void addReference(String term, int pageNum) {
    TreeSet<Integer> references; // 우리가 참조하는 페이지 세트
                                // 지금까지 용어를 사용했습니다.
    references = index.get(term);
    if (references == null){
        // 이것은 우리가 가지고 있는 첫 번째 참조입니다.
        // 해당 용어를 찾았습니다. 다음을 포함하는 새 세트를 만듭니다.
        // 페이지 번호를 인덱스에 추가합니다.
        // 용어를 키로 사용합니다.
        TreeSet<Integer> firstRef = new TreeSet<>();
        firstRef.add( pageNum );  // pageNum is "autoboxed" to give an Integer!
        index.put(term,firstRef);
    }
    else {
        // 참조는 페이지 참조 집합입니다.
        // 이전에 해당 용어에 대해 찾은 것입니다.
        // 해당 세트에 새 페이지 번호를 추가합니다. 이것
        // 세트는 이미 인덱스의 용어와 연관되어 있습니다.
        references.add( pageNum );
    }
}
```

색인으로 수행해야 할 유일한 작업은 색인을 인쇄하는 것이다. 우리는 색인을 반복하고 해당 용어에 대해 페이지 참조세트와 함께 각 용어를 인쇄하려고 한다. Iterator를 사용하여 인덱스를 반복할 수 있지만 for-each 루프를 사용하는 것이 훨씬 쉽다. 루프는 맵의 항목 세트를 통해 반복된다. 각 "항목"은 맵의 키/값 쌍이다. 키는 용어이고 값은 연관된 페이지 참조 세트이다. for-each 루프 내에서 정수 세트를 인쇄해야 한다. 이는 for-each 루프로 수행할 수 있다. 여기에 중첩된 for-each 루프 예가 있다.

```java
/**
 * 색인의 각 항목을 인쇄합니다.
 */
void printIndex() {

    for ( Map.Entry<String,TreeSet<Integer>>  entry :  index.entrySet() ) {

        String term = entry.getKey();
        TreeSet<Integer> pageSet = entry.getValue();

        System.out.print( term + ": " );
        for ( int page : pageSet ) {
            System.out.print( page + " " );
        }
        System.out.println();

    }

}
```

여기서 가장 어려운 점은 `Map.Entry<String, TreeSet<Integer>>` 유형의 이름이다. Map<K, V> 유형의 맵 항목은 Map.Entry<K, V> 유형을 가지므로 `Map.Entry<String, TreeSet<Integer>>`의 유형 매개 변수는 단순히 인덱스 선언에서 복사된다. 주목해야할 또 다른 점은 TreeSet<Integer> 유형인 `pageSet` 요소를 반복하기 위해 int 유형의 루프 제어 변수 `page`를 사용했다는 것이다. `page`가 int가 아닌 Integer 유형이고 실제로는 Integer 유형일 것으로 예상했을 수 있다. 여기서도 잘 작동했을 것이다. 그러나 int는 자동 유형 변환으로 인해 작동하낟. Integer 유형의 값을 int 유형의 변수에 할당하는 것이 합법적이다.

작업의 복잡성을 고려하면 이는 많은 코드가 아니다. 나는 완전한 인덱싱 프로그램을 작성하지 않았다면 문제 10.6은 인덱싱 문제와 거의 동일한 문제를 제시한다.

(그런데, `printIndex()` 메서드는 `var`를 사용하여 모든 지역 변수를 선언할 수 있었다. 이렇게 하면 복잡한 유형 이름을 피할 수 있지만 어떤 메서드를 알아야 하므로 여전히 유형을 알아야 한다. )

---

그런데 이 예에서는 각 페이지 참조 목록을 쉼표로 구분된 정수로 인쇄하는 것을 선호한다. 위에 제공된 `printIndex()` 메서드에서는 공백으로 구분된다. 목록의 마지막 페이지 참조 뒤에는 추가 공간이 있지만 인쇄물에서는 보이지 않으므로 아무런 해가 되지 않는다. 목록 끝에 추가 쉼표가 있으면 성가실 것이다. 목록은 "17, 42, 105,"가 아닌 "17, 42, 105"와 같은 형식이어야 한다. 문제는 마지막 쉼표를 어떻게 남겨두느냐 하는 것이다. 불행하게도 for-each 루프로는 이 작업과 수행하기가 쉽지 않다. 이 문제를 해결하는 몇 가지 방법을 살펴보는 것도 재미있다. 한 가지 대안은 iterator을 사용하는 것이다.

```java
Iterator<Integer>  iter = pageSet.iterator();
int firstPage = iter.next();  
System.out.print(firstPage);
while ( iter.hasNext() ) {
    int nextPage = iter.next();
    System.out.print("," + nextPage);
}
```

또 다른 가능성은 TreeSet 클래스가 집합의 첫 번째 항목, 즉 집합의 항목을 비교하는 데 사용되는 순서 측면에서 가장 작은 항목을 반환하는 `first()` 메서드를 정의한다는 사실을 이용하는 것이다. (또한 `last()` 메서드도 정의한다.) 이 메서드와 for-each 루프를 사용하여 문제를 해결할 수 있다.

```java
int firstPage = pageSet.first(); 
for ( int page : pageSet ) {
    if ( page != firstPage )
        System.out.print(","); 
    System.out.print(page);
}
```

마지막으로 트리의 하위 집합 뷰를 사용하는 우아한 솔루션이 있다. 실제로 이 솔루션은 약간 극단적일 수 있다.

```java
int firstPage = pageSet.first();  
System.out.print(firstPage);      
for ( int page : pageSet.tailSet( firstPage+1 ) )
    System.out.print( "," + page );
```

## 3. Comparator 사용

색인 문제에 대한 솔루션에는 잠재적인 문제가 있다. 색인의 용어에 대문자와 소문자를 모두 포함할 수 있는 경우 용어는 알파벳 순서로 되어 있지 않다. 문자열의 순서는 알파벳순이 아니다. 이는 문자열에 있는 문자의 유니코드를 기반으로 한다. 모든 대문자에 대한 코드는 소문자에 대한 코드보다 작다. 예를 들어 "Z"로 시작하는 용어는 "a"로 시작하는 용어 앞에 온다. 용어가 소문자만(또는 대문자만) 사용하도록 제한되는 경우 순서는 알파벳순이다. 그러나 대문자와 소문자를 모두 허용하고 알파벳 순서를 고집한다고 가정해 본다. 이 경우 인덱스 문자열에 대한 일반적인 순서를 사용할 수 없다. 다행히도 맵의 키를 비교하는 데 사용할 다른 방법을 지정할 수 있다. 이는 Comparator의 일반적인 용도이다.

Comparator<T> 인터페이스를 구현하는 객체는 T 유형의 두 객체를 비교하는 메서드를 정의한다는 점을 상기하자.

```java
public int compare(T obj1, T obj2)
```

이 메서드는 `obj1`이 `obj2`보다 작거나 같거나 큰지에 따라 음수, 0, 또는 양수인 정수를 반환해야 한다. 대소문자를 무시하고 두 문자열을 비교하고 싶다. String 클래스에는 이를 수행하는 메서드인 `compareToIgnoreCase()`가 이미 포함되어 있다. 그러나 TreeMap에서 기본이 아닌 비교를 사용하려면 Comparator<String> 유형의 객체가 필요하다. Comparator는 기능적 인터페이스(functional interface)이므로 comparator를 지정하는 쉬운 방법은 람다 식을 사용하는 것이다.

```java
(a, b) -> a.compareToIgnoreCase(b)
```

인덱싱 문제를 해결하려면 이 comparator를 TreeMap 생성자에 매개 변수로 전달하면 된다.

```java
index = new TreeMap<>((a, b) -> a.compareToIgnoreCase(b));
```

실제로 람다 표현식은 String 클래스에 이미 존재하는 메서드를 호출하기 때문에 메서드 참조로 제공될 수 있다. (섹션 4.5.4 참고)

```java
index = new TreeMap<>(String::compareToIgnoreCase);
```

이것은 작동한다. 그러나 한 가지 기술적 측면이 있다. 예를 들어, 인덱싱 프로그램이 `addReference("aardvark", 56)`를 호출 하고 나중에 `addReference("Aardvark", 102)`를 호출한다고 가정해 본다. "aardvark"와 "Aardvark"라는 단어는 그 중 하나가 대문자로 시작한다는 점만 다르다. 소문자로 변환하면 동일하다. 색인에 삽입할 때 두 개의 다른 용어로 계산되는가, 아니면 하나의 용어로 계산되는가? 대답은 TreeMap이 객체의 동등성을 테스트하는 방식에 따라 달라진다. 실제로 TreeMaps 및 TreeSets은 항상 Comparator 객체 또는 `compareTo` 메서드를 사용하여 동등성을 테스트한다. 그들은 이를 위해 `equals()` 메서드를 사용한다. 이 예에서는 TreeMap에 사용되는 Comparator는 "aardvark"와 "Aardvark"를 비교하는 데 사용될 때 값 0을 반환하므로 TreeMap은 두 항목을 동일한 것으로 간주한다. "aardvark" 및 "Aardvark"에 대한 페이지 참조는 단일 목록으로 결합되며, 색인이 인쇄되면 프로그램에서 발견한 단어의 첫 번째 버전만 포함된다. 이 예에서는 이는 아마도 허영 가능한 동작이다. 그렇지 않은 경우 다른 기술을 사용하여 용어를 알파벳 순서로 정렬해야 한다.

## 4. 단어 counting

이 섹션의 마지막 예제에서는 단어에 대한 정보를 저장하는 방법도 다루고 있다. 여기서 문제는 각 단어가 나타나는 횟수와 함께 파일에 나타나는 모든 단어의 목록을 만드는 것이다. 파일은 사용자가 선택한다. 프로그램의 출력은 두개의 목록으로 구성된다. 각 목ㄹ고에는 해당 단어가 발생하는 횟수에와 함께 파일의 모든 단어가 포함된다. 한 목록은 알파벳순으로 정렬되고, 다른 목록은 발생 횟수에 따라 정렬되어 가장 일반적인 단어가 맨 위에, 가장 덜 일반적인 단어가 맨 아래에 정렬된다. 여기서 문제는 발생 횟수를 세지 않고 파일에 있는 모든 단어의 알파벳순 목록을 만들도록 요청한 문제 7.6의 일반화이다.

계산 프로그램은 [WordCount.java](https://math.hws.edu/javanotes/source/chapter10/WordCount.java) 파일에서 찾을 수 있다. 프로그램은 입력 파일을 읽을 때 각 단어를 몇 번이나 만나는지 추적해야 한다. 중복된 단어가 포함된 모든 단어를 목록에 넣고 나중에 계산할 수 있다. 하지만 그렇게 하려면 추가 저장 공간이 많이 필요하고 그다지 효율적이지 않다. 더 좋은 방법은 각 단어에 대한 카운터를 유지하는 것이다. 단어가 처음 발견되면 카운터는 1로 초기화된다. 이후에 발견되면 카운터가 증가한다. 한 단어에 대한 데이터를 추적하기 위해 프로그램은 단어와 해당 단어에 대한 카운터를 보유하는 간단한 클래스를 사용한다. 클래스는 `static` 중첩 클래스이다.

```java
/**
 * 단어에 대해 필요한 데이터를 나타냅니다.
 * 발생한 횟수입니다.
 */
private static class WordData { 
    String word;
    int count;
    WordData(String w) {
        word = w;
        count = 1;
    }
}
```

프로그램은 모든 WordData 객체를 일종의 데이터 구조로 저장해야 한다. 우리는 새로운 단어를 효율적으로 추가할 수 있기를 원한다. 단어가 주어지면 해당 단어에 대한 WordData 객체가 이미 존재하는지 확인해야 하며, 존재하는 경우 카운터를 증가시킬 수 있도록 해당 객체를 찾아야 한다. Map을 사용하여 이러한 작업을 구현할 수 있다. 단어가 주어지면 Map에서 WordData 객체를 검색하려고 한다. 이는 단어가 키이고 WordData 객체가 값임을 의미한다. (키가 값 객체의 인스턴스 변수 중 하나라는 것이 이상하게 보일 수도 있지만 실제로는 매우 일반적인 상황이다. 값 객체에는 일부 엔티티에 대한 모든 정보가 포함되어 있고 키는 해당 인스턴스 중 하나이다.) 파일을 읽은 후 단어를 알파벳 순서로 출력하려고 하므로 HashMap 대신 TreeMap을 사용해야 한다. 이 프로그램은 문자열의 기본 순서가 알파벳 순서로 단어를 배치하도록 모든 단어를 소문자로 변환해야 한다. 데이터는 TreeMap<String, WordData> 유형의 `words`라는 변수에 저장된다. 변수가 선언되고 다음 명령문을 사용하여 맵 객체가 생성된다.

```java
TreeMap<String, WordData> words = new TreeMap<>();
```

프로그램이 파일에서 단어를 읽을 때 `words.get(word)`를 호울하여 해당 단어가 이미 맵에 있는지 확인한다. 반환 값이 `null` 이면 해당 단어가 처음 발견된 것이므로 새로운 WordData 객체가 생성되고 `words.put(word, new WordData(word))` 명령을 사용하여 맵에 삽입된다. `words.get(word)`가 `null`이 아닌 경우 해당 값은 이 단어에 대한 WordData 객체이고 프로그램은 해당 객체의 카운터만 증가시키면 딘다. 연습분제 7.6에서 제공한 `readNextWord()` 메서드를 사용하여 파일에서 한 단어를 읽는다. 이 메서드는 반환 파일의 끝을 만나면 `null`이다. 다음은 파일을 읽고 데이터를 수집하는 전체 코드이다. 

```java
String word = readNextWord();
while (word != null) {
    word = word.toLowerCase(); 
    WordData data = words.get(word);
    if (data == null)
        words.put( word, new WordData(word) );
    else
        data.count++;
    word = readNextWord();
}
```

단어를 읽고 알파벳순으로 인쇄한 후, 프로그램은 빈도별로 단어를 정렬하고 다시 인쇄해야 한다. 일반 알고리즘을 사용하여 정렬을 수행하려면 WordData 객체를 목록에 복사한 다음 comparator를 두 번째 매개 변수로 지정하는 일반 메서드 `Collections.sort(list, comparator)`를 사용할 수 있다. 데이터를 개수별로 내림차순으로 정렬하려고 하므로 a.count > b.count인 경우 b앞에 a를 두는 두 WordData 값 a와 b에 대한 comparator가 필요하다. 다음 람다 식이 여기서 작동할 정의가 있는지 확인한다.

```java
(a, b) -> b.count - a.count
```

우리에게 필요한 WordData 객체는 맵의 값, `word`이다. `words.values()`는 맵에 모든 값을 포함하는 컬렉션을 반환한다는 점을 기억하자. ArrayList 클래스의 생성자를 사용하면 목록이 생성될 때 목록에 복사할 컬렉션을 지정할 수 있다. 따라서 다음 명령을 사용하여 data라는 단어가 포함된 ArrayList<WordData> 유형의 목록을 만든 다음 해당 목록을 빈도에 따라 정렬할 수 있다.

```java
ArrayList<WordData> wordsByFrequency = new ArrayList<>( words.values() );
Collections.sort( wordsByFrequency, (a,b) -> b.count - a.count );
```

이 두 줄이 많은 코드를 대체한다는 것을 알 수 있다! 일반적인 데이터 구조와 알고리즘 측면에서 생각하려면 약간의 연습이 필요하지만 시간과 노력이 절약된다는 점에서 그 보상은 상당하다.

남은 유일한 문제는 데이터를 인쇄하는 것이다. 모든 WordData 객체의 데이터를 두 번 인쇄해야 한다. 먼저 알파벳 순으로 인쇄한 다음 빈도수에 따라 정렬해야 한다. 데이터는 TreeMap에서 알파벳 순서로, 더 정확하게는 TreeMap의 값으로 정렬된다. for-each 루프를 사용하여 `words.values()` 컬렉션의 데이터를 인쇄할 수 있으며 단어는 알파벳 순서로 표시된다. 또 다른 for-each 루프를 사용하여 `wordByFrequency` 목록의 데이터를 인쇄할 수 있으며, 단어는 빈도가 감소하는 순서대로 인쇄된다. 

```java
TextIO.putln("List of words in alphabetical order" 
      + " (with counts in parentheses):\n");
for ( WordData data : words.values() )
    TextIO.putln("   " + data.word + " (" + data.count + ")");
TextIO.putln("\n\nList of words by frequency of occurrence:\n");
for ( WordData data : wordsByFrequency )
    TextIO.putln("   " + data.word + " (" + data.count + ")");
```

WordCount.java 파일에서 완전한 단어 계산 프로그램을 찾을 수 있다. 파일을 읽고 쓰기 위해서는 섹션 2.4.4에서 논의된 파일 I/O 기능을 사용한다.

그런데 상당히 큰 파일에서 WordCount 프로그램을 실행하고 출력을 살펴보면 `Collections.sort()` 메서드에 대한 내용을 설명할 수 있다. 출력의 두 번째 단어 목록은 빈도순으로 정렬되지만, 빈도가 모두 동일한 단어 그룹을 보면 해당 그룹의 단어가 알파벳순으로 정렬되어 있음을 알 수 있다. 빈도별로 단어를 정렬하기 위해 `Collections.sort()` 메서드가 적용되었지만 적용되기 전에응 단어가 이미 알파벳 순으로 정렬되어 있다. `Collections.sort()`가 단어를 재배열할 때 동일한 빈도를 갖는 단어의 순서를 변경하지 않았으므로 해당 빈도를 가진 단어 그룹 내에서 여전히 알파벳 순서로 유지되었다. 그 이유는 알고리즘이 사용되기 때문이다. `Collections.sort()`는 안정적인(stable) 정렬 알고리즘이다. 다음 조건을 만족하는 경우 정렬은 동일한 항목의 상대적 순서를 변경하지 않는다. 그 재산의 가치, 즉 항목 B가 정렬 전 목록에서 항목 A 뒤에 오고 두 항목 모두 정렬 기준으로 사용되는 속성에 대해 동일한 값을 갖는 경우 항목 B는 정렬 후에도 여전히 항목 A 뒤에 온다. 완료되었다. 선택 정렬이나 퀵 정렬 모두 안정적인 알고리즘이 안디ㅏ. 삽입 정렬은 안정적이지만 속도가 그리 빠르지는 않다. `Collections.sort()`에서 사용하는 정렬 알고리즘인 병합 정렬은 안정적이고 빠르다.

이 섹션의 프로그래밍 예제를 통해 Java Collection Framework의 유용성을 확신하셨기를 바란다.






















