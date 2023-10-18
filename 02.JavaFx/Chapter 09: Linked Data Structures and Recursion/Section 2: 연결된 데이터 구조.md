# 연결된 데이터 구조(Linked Data Structures)

매우 유용한 객체 에는 인스턴스 변수가 포함되어 있다. 인스턴스 변수의 유형이 클래스 또는 인터페이스 이름으로 제공되면 변수는 다른 객체에 대한 참조를 보유할 수 있다. 이러한 참조를 포인터라고도 하며 변수가 객체를 가리킨다 고 말한다(물론 객체에 대한 참조를 포함할 수 있는 모든 변수에는 아무 곳도 가리키는 특수 값인 `null` 도 포함될 수 있다.) 한 객체에 다른 객체를 가리키는 인스턴스 변수가 포함되어 있으면 객체가 "연결되어 있는" 것으로 생각한다. 객체를 서로 연결하면 매우 복잡한 데이터 구조를 구성할 수 있다.

## 1. 재귀 연결

객체에 동일한 유형의 다른 객체를 참조할 수 있는 인스턴스 변수가 포함되어 있으면 흥미로운 일이 발생한다. 이 경우 객체 클래스의 정의는 재귀적이다. 이러한 재귀는 많은 경우에 자연스럽게 발생한다. 예를 들어 회사의 직원을 나타내기 위해 설계된 클래스를 생각해 보자. 상사를 제외한 모든 직원에게 회사의 또 다른 직원인 감독자가 있다고 가정한다. 그러면 Employee 클래스에는 자연스럽게 직원의 감독자를 가리키는 Employee 유형의 인스턴스 변수가 포함된다.

```java
/**
 * Employee 유형의 객체는 한 명의 직원에 대한 데이터를 보유합니다.
 */
public class Employee {
       
   String name;          
   
   Employee supervisor;  

      .
      .  // (Other instance variables and methods.)
      .
      
} 
```

`emp` 가 `Employee` 유형의 변수인 경우 `emp.supervisor`는 `Employee` 유형의 또 다른 변수이다. `emp`가 상사를 참조하는 경우 `emp.supervisor`의 값은 상사에게 감독자가 없다는 사실을 나타내기 위해 `null` 이어야 한다. 예를 들어 직원의 감독자의 이름을 인쇄하려면 다음 Java 문을 사용할 수 있다.

```java
if ( emp.supervisor == null) {
    System.out.println( emp.name + " is the boss and has no supervisor!" );
}
else {
    System.out.print( "The supervisor of " + emp.name + " is " );
    System.out.println( emp.supervisor.name );
}
```

이제 특정 직원과 상사 사이에 몇 단계 감독관이 있는지 알고 싶다고 가정한다. 일련의 감독자 링크를 통해 명령 체계를 따르고 상사에게 도달하는 데 몇 단계가 필요한지 계산하면 된다.

```java
if ( emp.supervisor == null ) {
    System.out.println( emp.name + " is the boss!" );
}
else {
    Employee runner;  
    runner = emp.supervisor;
    if ( runner.supervisor == null) {
        System.out.println( emp.name  + " reports directly to the boss." );
    }
    else {
        int count = 0;
    while ( runner.supervisor != null ) {
        count++;  // Count the supervisor on this level.
        runner = runner.supervisor; // Move up to the next level.
    }
    System.out.println( "There are " + count
                             + " supervisors between " + emp.name
                             + " and the boss." );
   }
```

`while` 루프가 실행됨에 따라 `runner`는 차례로 원래 직원(emp), emp의 감독관, emp의 감독관 등을 가리킨다. `runner`가 새로운 직원을 "방문"할 때마다 카운트 변수가 증가합니다. `runner.supervisor`가 `null`일 때 루프가 종료되며, 이는 `runner`가 사장에 도달했음을 나타낸다. 그 시점에서 `count`는 `emp`와 사장 사이의 단계 수를 카운트한다. 


이 예에서 `supervisor` 변수는 매우 자연스럽고 유용하다. 실제로 객체를 서로 연결하여 구축한 데이터 구조는 매우 유용하여 컴퓨터 과학의 주요 연구 주제이다. 몇 가지 대표적인 예를 살펴본다. 다음 섹션에서 `linked list`를 살펴 본다. 연결됨 목록은 한 객체에서 다음 객체로 포인터로 서로 연결된 동일한 유형의 객체 체인으로 구성된다. 이는 위의 예에서 `emp`와 `boss` 사이의 감독자 체인과 매우 유사하다. 하나의 객체가 여러 다른 객체에 대한 링크를 포함할 수 있는 더 복잡한 상황이 있을 수도 있다.

![linked list](./images/linked%20list.png)

## 2. Linked Lists

이 섹션의 나머지 부분에 있는 대부분의 예에서 연결된 목록은 다음과 같이 정의된 `Node` 클래스에 속하는 객체로 구성된다.

```java
class Node {
    String item;
    Node next;
}
```

노드 라는 용어는 연결된 데이터 구조의 객체 중 하나를 나타내는 데 자주 사용된다. 위 그림의 상단 부분에 표시된 것 처럼 노드 유형의 객체를 함께 연결할 수 있다. 각 노드는 문자열과 목록의 다음 노드(있는 경우)에 대한 포인터를 보유한다. 이러한 목록의 마지막 노드는 마지막 노드의 다음 인스턴스 변수가 아닌 `null` 값을 보유한다는 사실로 항상 식별될 수 있다. 다른 노드에 대한 포인터 대신, 노드 체인의 목적은 문자열 목록을 나타내는 것이다. 목록의 첫 번째 문자열은 첫 번째 노드에 저장되고, 두 번째 문자열은 두 번째 노드에 저장되는 식이다. 포인터와 노드 객체는 구조를 만드는데 사용되지만 우리가 표현하려는 데이터는 문자열 목록이다. 물론 각 노드에 저장된 `item`의 유형을 변경하여 정수 목록 등 다른 유형의 데이터 목록을 쉽게 나타낼 수 있다.

이 예제의 노드는 매우 간단하지만 이를 사용하여 연결된 목록의 일반적인 작업을 설명할 수 있다. 일반적인 작업에는 목록에서 노드 삭제, 목록에 새 노드 삽입, 목록의 `item` 중 지정된 문자열 검색이 포함된다. 특히 이러한 모든 작업을 수행하는 서브 루틴을 살펴본다.

프로그램에서 연결된 목록을 사용하려면 해당 프로그램에는 목록의 첫 번째 노드를 참조하는 변수가 필요하다. 목록의 다른 모든 노드는 첫 번째 노드에서 시작하여 목록을 따라 한 노드에서 다음 노드로 링크를 따라가면 액세스할 수 있으므로 첫 번째 노드에 대한 포인터만 필요하다. 내 예에서 항상 연결된 목록의 첫 노드를 가리키는 `Node` 유형의 `head` 변수를 사용한다. 목록이 비어 있으면 `null` 값이다.

![linked list](./images/linked%20list%202.png)

## 3. 기본 링크트 리스트 처리

어떤 방식으로든 연결된 목록의 모든 항목을 처리하는 것은 매우 일반적이다. 일반적인 패턴은 목록의 선두에서 시작한 다음 노드의 포인터를 따라 각 노드에서 다음 노드로 이동하고 목록의 끝을 표시하는 `null`에 도달하면 중지하는 것이다. `head`가 목록의 첫 번째 노드를 가리키는 `Node` 유형의 변수인 경우 연결된 목록의 모든 항목을 처리하기 위한 코드의 일반적인 형식은 다음과 같다.

```java
Node runner;
runner = head;
while (runner != null) {
    process(runner.item);
    runner = runner.next;
}
```

목록에 대한 유일한 접근 변수 `head`를 통해서만 이루어지므로, 대입문 `runner = head`를 사용하여 `head`에 있는 값의 복사본을 얻는 것부터 시작한다. `run` 값을 변경할 것이므로 `head` 복사본이 필요하다. `head` 값을 변경할 수 없다. 그렇지 않으면 목록에 대한 유일한 엑세스 권한을 잃게 된다. `runner` 변수는 목록의 각 노드를 차례로 가리킨다. `runner`가 목록에 있는 노드 중 하나를 가리키는 경우, `runner.next`는 목록에 있는 다음 노드에 대한 포인터이므로 할당문은 `runner = runner.next`이다 포인터는 목록을 따라 각 노드에서 다음 노드로 이동한다. 러너가 `null`과 같아지면 목록 끝에 도달했다는 것을 알 수 있다. 우리는 목록 처리 코드는 빈목록에 대해서도 작동한다. 왜냐하면 빈 목록의 경우 `head` 값은 `null`이고 `while` 루프의 본문은 전혀 실행되지 않는다. 

```java
Node runner = head;
while ( runner != null ) {
    System.out.println( runner.item );
    runner = runner.next;
}
```

그런데 `while` 루프는 `for` 루프로 다시 작성할 수 있다. for 루프의 제어 변수가 숫자인 경우가 많지만 반드시 필요한 것은 아니다.

```java
for (Node runner = head; runner != null; runner = runner.next) {
    System.out.println( runner.item );
}
```

마찬가지로 정수 목록을 탐색하여 목록에 있는 모든 숫자를 더할 수 있다. 정수의 연결 리스트는 클래스를 사용하여 구성할 수 있다.

```java
public class IntNode {
    int item;
    IntNode next;
}
```

`head`가 정수의 연결된 목록을 가리키는 `IntNode` 유형의 변수인 경우 다음을 사용하여 목록에 있는 정수의 합계를 찾을 수 있다.

```java
int sum = 0;
IntNode runner = head;
while ( runner != null ) {
    sum = sum + runner.item;  
    runner = runner.next;
}
System.out.println("The sum of the list of items is " + sum);
```

재귀를 사용하여 연결된 목록을 처리하는 것도 가능하다. 반복을 사용하여 목록을 순회하는 것은 매우 쉽기 때문에 제귀는 목록을 처리하는 자연스러운 방법이 아니다. 그러나 목록에 재귀를 적용하는 방법을 이해햐면 더 복잡한 데이터 구조의 재귀 처리를 이해하는 데 도움이 될 수 있다. 비어 있지 않은 연결 목록은 두 부분, 즉, 목록의 첫 노드인 목록의 **헤드**와 **꼬리**로 구성되는 것으로 생각할 수 있다. 헤드 뒤의 나머지 목록으로 구성된 목록이다. 꼬리 자체는 연결된 목록이며 원래 목록보다 짧다. 이는 목록 처리 문제를 헤더 처리와 재귀적으로 꼬리 처리로 나눌 수 있는 재귀의 자연스러운 설정이다. 기본 사례는 빈 목록의 경우에 발생한다. 예를 들어, 다음과 같다.

```
if the list is empty then
    return 0 (since there are no numbers to be added up)
otherwise
    let listsum = the number in the head node 
    let tailsum be the sum of the numbers in the tail list (recursively)
    add tailsum to listsum
    return listsum
```

남은 질문 중 하나는 비어 있지 않은 연결 목록의 꼬리를 어ㄸ덯게 얻느냐는 것이다. `head`의 목록의 헤드 노드를 가리키는 변수인 경우 `head.next`는 목록의 두 번째 노드를 가리키는 변수이며 해당 노드는 실제로 꼬리의 첫 번째 노드이다. 따라서 `head.next`를 목록의 꼬리에 대한 포인터로 볼 수 있다. 한 가지 특별한 경우는 원래 목록이 단일 노드로 구성된 경우이다. 이 경우 목록의 꼬리는 비어 있고 `head.next`는 `null`이다. 빈 목록은 널 포인터로 표시되므로 이 특별한 경우에도 `head.next`는 목록의 끝을 나타낸다. 

```java
/**
 * 연결된 정수 목록에 있는 모든 정수의 합을 계산합니다.
 * @param head는 연결 리스트의 첫 번째 노드에 대한 포인터입니다.
 */
public static int addItemsInList( IntNode head ) {
    if ( head == null ) {
        // 기본 사례: 목록이 비어 있으므로 합계는 0입니다.
        return 0;
    }
    else {
        // 재귀 사례: 목록이 비어 있지 않습니다. 다음의 합계를 구하세요.
        // tail 목록을 만들고 이를 헤드 노드의 항목에 추가합니다.
        // (이 경우는 다음과 같이 간단하게 작성할 수 있습니다.
        // head.item + addItemsInList( head.next );) 반환
        int listsum = head.item;
        int tailsum = addItemsInList( head.next );
        listsum = listsum + tailsum;
        return listsum;
    }
}
```

재귀를 사용하면 해결하기 쉽지만 재귀 없이는 해결하기가 매우 까다로운 목록 처리 문제를 제시하면서 마무리한다. 문제는 연결된 목록의 모든 문자열을 목록에 나타나는 순서와 반대로 인쇄하는 것이다. 이렇게 하면 목록의 머리 부분에 있는 항목이 목록의 끝 부분에 있는 모든 항목 다음에 인쇄된다는 점에 유의하세요. 이는 다음과 같이 재귀 루틴으로 이어진다. 이것이 작동한다는 것을 스스로 확신해야 하며, 재귀를 사용하지 않고 동일한 작업을 수행하는 것에 대해 생각해야 한다.

```java
public static void printReversed( Node head ) {
    if ( head == null ) {
        // 기본 사례: 목록이 비어 있고 인쇄할 항목이 없습니다.
        return;
    }
    else {
        // 재귀 사례: 목록이 비어 있지 않습니다.
        printReversed( head.next );  // 역순 인쇄
        System.out.println( head.item );  
    }
}
```

---

이 섹션의 나머지 부분에서는 연결된 문자열 목록에 대해 몇 가지 고급 작업을 살펴 본다. 우리가 고려하는 서브 루틴은 `StringList`라는 클래스의 인스턴스 메서드이다. `StringList` 유형의 객체는 문자열의 연결된 목록을 나타낸다. 클래스에는 목록의 첫 번째 노드를 가리키는 `Node` 유형의 `head`라는 개인 인스턴스 변수가 있거나 목록이 비어 있는 경우 `null`이다. StringList 클래스의 인스턴스 메서드는 전역 변수로 `head`에 엑세스 한다. [StringList.java](https://math.hws.edu/javanotes/source/chapter9/StringList.java)에서 볼 수 있다.

StringList 클래스의 메서드 중 하나는 목록을 검색하여 지정된 문자열을 찾는다. 찾고 있는 문자열이 `searchItem`이면 각 `item`과 비교해야 한다. 이는 기본 목록 순회 및 처리의 예이다.

```java
/**
 * 목록에서 특정 항목을 검색합니다.
 * @param searchItem 검색할 항목
 * @return searchItem이 목록의 항목 중 하나이면 true이고, 있으면 false입니다.
 * searchItem이 목록에 나타나지 않습니다.
 */
public boolean find(String searchItem) {

    Node runner;    // 목록을 탐색하기 위한 포인터입니다.
    
    runner = head;  // 목록의 선두부터 살펴보세요.
                    // (head는 인스턴스 변수입니다!)
    
    while ( runner != null ) {
        // 각 항목의 문자열을 보면서 목록을 살펴봅니다.
        // 노드. 문자열이 우리가 찾고 있는 문자열이라면,
        // 목록에서 문자열을 찾았으므로 true를 반환합니다.
        if ( runner.item.equals(searchItem) )
            return true;
        runner = runner.next;  // 이동
    }

    // 이 시점에서 우리는 목록의 모든 항목을 살펴보았습니다.
    // searchItem을 찾지 않고. 이를 나타내기 위해 false를 반환합니다.
    // 해당 항목이 목록에 존재하지 않습니다.
    
    return false;

} 
```

목록이 비어 있을 수 있다. 즉, `head`값이 `null` 일 수 있다. 우리는 이 사건이 제대로 처리되도록 주의해야 한다. 위 코드에서 `head`가 `null`이면 `while` 루프의 본문은 전혀 실행되지 않으므로 노드가 처리되지 않고 반환 값은 false이다.

## 4. 연결 목록에 삽입

연결된 목록에 새 항목을 삽입한는 문제는 적어도 항목이 목록 중간에 삽입되는 경우에는 더 어렵다. StringList 클래스에서는 연결된 목록의 노으데 있는 `item`이 오름차순으로 유지된다. 새 항목이 목록에 삽입되면 이 순서에 따라 올바른 위치에 삽입되어야 한다. 이는 일반적으로 목록 중간, 기존 두 노드 사이에 새 항목을 삽입해야 함을 의미한다. 이렇게 하려면 `Node` 유형의 변수를 두 개 사용하는 것이 편리하다. 이는 새 노드의 양쪽에 놓일 기존 노드를 나타낸다. 다음 그림에서 이러한 변수는 `previous`와 `runner`이다. 또 다른 변수는 `newNode`는 새 노드를 참조한다. 삽입을 수행하려면 `previous`에서 `runner`로 링크가 "깨져야(broken)" 하며 새 링크로 추가해야 한다.


![linked list insert](./images/linked%20list%20추가.png)

`previous` 노드와 `runner`가 올바른 노드를 가리키면 `previous.next = newNode` 명령을 사용하여 `previous.nest`가 새 노드를 가리키도록 할 수 있다. 그리고 `newNode.next = runner` 명령은 `newNode.next`가 올바른 위치를 가리키도록 설정한다. 그러나 이러한 명령을 사용하기 전에 그림과 같이 `runner` 및 `previous` 설정을 해야 한다. 아이디어는 목록의 첫 번째 노드에서 시작한 다음 새 항목보다 작은 모든 항목을 지나 목록을 따라 이동하는 것이다. 이렇게 하는 동안 우리는 "목록에서 탈락"하는 위험을 인식해야 한다. 그건 목록 끝에 도달하면 `null`이 된다. `insertItem`이 삽입될 항목이고 실제로 목록 중간 어딘가에 속한다고 가정하면 다음 코드는 `previous` 및 `runner`를 올바르게 배치한다. 

```java
Node runner, previous;
previous = head;       
runner = head.next;
while ( runner != null && runner.item.compareTo(insertItem) < 0 ) {
    previous = runner;  
    runner = runner.next;
}
```

새 노드가 목록 중간에 삽입된다는 가정이 항상 유효한 것은 아니라는 점을 제외하면 괜찮다. `insertItem`이 목록의 첫 번째 항목보다 작을 수 있다. 이 경우 새 노드를 목록의 선두에 삽입해야 한다. 이 작업은 지침에 따라 수행할 수 있다.

```java
newNode.next = head;   
head = newNode;        
```

목록이 비어 있을 수도 있다. 이 경우 `newNode`는 목록의 첫 번째이자 유일한 노드가 된다. 이는 `head = newNode`를 설정하여 간단하게 수행할 수 있다. StringList 클래스의 다음 `insert()` 메소드는 이러한 모든 가능성을 다룬다.

```java
/**
 * 목록을 순서대로 유지하면서 지정된 항목을 목록에 삽입합니다.
 * @param insertItem 삽입할 항목입니다.
 */

public void insert(String insertItem) {
    Node newNode;          // 새 항목을 포함하는 노드입니다.
    newNode = new Node();
    newNode.item = insertItem;  // (N.B. newNode.next는 null입니다.)

    if ( head == null ) {
        // 새 항목은 목록의 첫 번째(그리고 유일한) 항목입니다.
        // 헤드가 그것을 가리키도록 설정합니다.
        head = newNode;
    }
    else if ( head.item.compareTo(insertItem) >= 0 ) {
        // 새 항목이 목록의 첫 번째 항목보다 작습니다.
        // 따라서 목록의 선두에 삽입되어야 합니다.
        newNode.next = head;
        head = newNode;
    }
    else {
        // 새 항목은 첫 번째 항목 다음 어딘가에 속합니다.
        // 목록에 있습니다. 적절한 위치를 찾아 삽입하세요.
        Node runner;     // 목록을 순회하는 노드입니다.
        Node previous;   // 항상 러너 앞의 노드를 가리킵니다.
        runner = head.next;   // 두 번째 위치부터 살펴보세요.
        previous = head;
        while ( runner != null && runner.item.compareTo(insertItem) < 0 ) {
            // 러너까지 목록을 따라 이전 및 러너를 이동합니다.
            // 끝에서 떨어지거나 다음과 같은 목록 요소에 부딪힙니다.
            // insertItem보다 크거나 같습니다. 이 때
            // 루프가 종료됩니다. 이전은 루프가 종료되는 위치를 나타냅니다.
            // insertItem을 삽입해야 합니다.
            previous = runner;
            runner = runner.next;
        }
        newNode.next = runner;     // Insert newNode after previous.
        previous.next = newNode;
   }
}
```

위의 논의에 세심한 주의를 기울였다면 언급되지 않은 특별한 경우가 하나 있다는 것을 알아차릴 것이다. 새 노드를 목록 끝에 삽입해야 하면 어떻게 되나? 목록의 모든 항목이 새 항목보다 작은 경우 이런 일이 발생한다. 실제로 이 경우는 `if`문의 마지막 부분에서 서브 루틴에 의해 이미 올바르게 처리된다. `insertItem`이 목록의 모든 항목보다 크면 실행기가 전체 목록을 순회하여 `null`이 될 때 루프가 종료된다. 그런데 그런일이 발생하면 이전 노드는 목록의 마지막 노드를 가리킨다. `previous.next = newNode`를 설정하면 `newNode`가 목록의 끝에 추가된다. `runner`가 `null`이므로 명령 `newNode.next = runner`는 `newNode.next`를 `null`로 설정한다. 이는 목록의 끝을 표시하는 데 필요한 정확한 내용이다.

## 5. 연결 목록에서 삭제

삭제 작업은 삽입과 비슷하지만 조금 더 간단하다. 고려해야 할 특별한 경우가 여전히 있다. 목록의 첫 번째 노드를 삭제하려면 이전에 목록의 두 번째 노드였던 항목을 가리키도록 `head` 값을 변경해야 한다. `head.next`는 목록의 두 번째 노드를 참조 하므로 `head = head.next`를 설정하여 이를 수행할 수 있다.

삭제되는 노드가 목록 중간에 있는 경우 삭제될 노드를 가리키는 `runner`와 목록에서 해당 노드 앞에 있는 노드를 가리킨다. 이전 노드로 `previous` 및 `runner`를 설정할 수 있다. 이 작업이 완료되면 `previous.next = runner.next` 명령이 실행되고 노드를 삭제한다. 삭제된 노드는 가비지 수집된다. 이 작업을 설명하기 위해 직접 그림을 그리는 것이 좋다.

```java
/**
 * 해당 항목이 있는 경우 목록에서 지정된 항목을 삭제합니다.
 * 목록에 항목의 사본이 여러 개 있는 경우에만
 * 목록의 첫 번째 항목이 삭제됩니다.
 * @param deleteItem 삭제할 항목
 * @return 항목을 찾아서 삭제한 경우 true, 항목이 삭제된 경우 false
 * 목록에 없었습니다.
 */

public boolean delete(String deleteItem) {
    if ( head == null ) {
        // 목록이 비어 있으므로 확실히 deleteString이 포함되어 있지 않습니다.
        return false;
    }
    else if ( head.item.equals(deleteItem) ) {
        // 문자열은 목록의 첫 번째 항목입니다. 제거하세요.
        head = head.next;
        return true;
    }
    else {
        // 문자열이 발생한다면 문자열은 다음 어딘가에 있는 것입니다.
        // 목록의 첫 번째 요소입니다. 목록을 검색하세요.
        Node runner;     // 목록을 순회하는 노드입니다.
        Node previous;   // 항상 러너 앞의 노드를 가리킵니다.
        runner = head.next;   // 두 번째 목록 노드를 살펴보는 것부터 시작합니다.
        previous = head;
        while ( runner != null && runner.item.compareTo(deleteItem) < 0 ) {
            // 러너까지 목록을 따라 이전 및 러너를 이동합니다.
            // 끝에서 떨어지거나 다음과 같은 목록 요소에 부딪힙니다.
            // deleteItem보다 크거나 같습니다. 이 때
            // 루프가 종료되고 러너는 루프가 종료되는 위치를 나타냅니다.
            // deleteItem이 목록에 있으면 있어야 합니다.
            previous = runner;
            runner = runner.next;
        }
        if ( runner != null && runner.item.equals(deleteItem) ) {
            // Runner는 삭제할 노드를 가리킨다.
            // 이전 노드의 포인터를 변경하여 제거합니다.
            previous.next = runner.next;
            return true;
        }
        else {
            // 해당 항목이 목록에 존재하지 않습니다.
            return false;
        }
    }

}

```



