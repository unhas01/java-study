# Section 3: ArrayList

섹션 7.2.4에서 본 것처럼 동적 배열 패턴을 클래스로 쉽게 인코딩할 수 있지만 각 데이터 유형마다 다른 클래스가 필요한 것 처럼 보인다. Java에는 클래스로 정의된 모든 데이터 유형에 대해 동적 배열 패턴을 구현하는 `ArrayList` 단일 클래스가 있다.


## 1. ArrayList와 매개 변수화된 유형

Java에는 `Strings`의 동적 배열을 나타내는 다소 이상한 이름의 `ArrayList<String>`을 가진 표쥰 유형이 있다. 마찬가지로 `Colors`의 동적 배열을 나타내는 데 사용할 수 있는 `ArrayList<Color>`의 동적 배열을 나타낼 수 있다. 

여기에는 여전히 많은 클래스가 있는 것처럼 보일 수 있지만 실제로는 `java.util` 패키지에 정의된 `ArrayList` 클래스라는 단 하나의 클래스만 있다. `ArrayList`는 매개변수화 된(parameterized type) 유형이다. 매개변수화된 유형은 유형 매개변수를 취할 수 있으므로 단일 클래스 `ArrayList`에서 `ArrayList<String>, ArrayList<Color>` 및 실제로 모든 객체 유형 `T`에 대한 `ArrayList<T>`를 포함한 다양한 유형을 얻는다. 유형 매개 변수 `T` 클래스 이름이나 인터페이스 이름과 같은 객체 유형이어야 한다. primitive 유형일 수 없다. 이는 불행이도 `int`, `char`의 ArrayList를 가질 수 없음을 의미한다.

`ArrayList<String>` 유형을 고려하자.

```java
ArrayList<String> nameList;
```

또한 서브루틴 정의에서 형식 매개변수의 유형으로 사용되거나 서브루틴의 반환 유형으로 사용될 수도 있다. 객체를 생성하기 위해 `new` 연산자와 함께 사용할 수 있다.

```java
nameList = new ArrayList<String>();
```

이러한 방식으로 생성된 객체는 `ArrayList<String>` 유형이며 동적 문자열 리스트를 나타낸다. 여기에는 리스트에 문자열을 추가하기 위한 `nameList.add(str)`, 문자열을 가져오기 위한 `nameList.get(i)` 및 현재 리스트에 있는 항목 수를 가져오기 위한 `nameList.size()`와 같은 인스턴스 메서드가 있다.

그러나 `ArrayList`를 다른 유형과 함께 사용할 수도 있다.

```java
ArrayList<Player> playerList = new ArrayList<Player>();
```

그런 다음 플레이어 `plr`을 추가하면 `playerList.add(plr)`라고 하면 된다.

`ArrayList<T>`와 같은 유형을 사용하면 컴파일러는 `T` 유형의 객체만 리스트에 추가될 수 있도록 보장한다. `T` 유형이 아닌 객체를 추가하려는 시도는 구문 오류(syntax error)가 되며 프로그램이 컴파일 되지 않는다. 그러나 `T`의 서브 클래스에 속하는 객체는 여전히 `T` 유형으로 간주되므로 `T`의 서브 클래스에 속하는 객체를 리스트에 추가할 수 있다. 따라서 `Shape` 클래스에 `Rectangle`, `Oval`, `RoneRect` 하위 클래스가 있는 경우 `ArrayList<Shape>`는 위 타입 모두 수용할 수 있다. (물론, 이는 배열 `T[]`와 작동하는 방식은 동일하다.) `T`가 인터페이스인 경우 인터페이스 `T`를 구현하는 모든 객체를 인터페이스에 추가할 수 있다.

`ArrayList<T>` 유형의 객체에는 동적 배열 구현에서 기대할 수 있는 모든 인스턴스 메서드가 있다. 다음은 유용한 것들이다.

- `list.size()`
  - 리스트의 현재 크기 반환
- `list.add(obj)`
  - 리스트 끝에 객체를 추가하여 크기를 1씩 늘린다.
- `list.get(N)`
  - 리스트의 N위치에 저장된 값을 반환
- `list.set(N, obj)`
  - N위치에 객체를 대체
- `list.clear()`
  - 리스트에 모든 항목을 제거, 크기를 0으로 설정
- `list.remove(N)`
  - 리스트의 N번째 항목을 제거
- `list.remove(obj)`
  - 리스트에 있으면 리스트에서 제거
- `list.indexOf(obj)`
  - 리스트에서 객체를 검색

여기에 나열된 마지막 두 메서드의 경우 `obj`가 `null`이 아닌 한 `obj.equals(item)`을 호출하여 리스트의 항목과 비교한다. 예를 들어, 이는 메모리에서 위치가 아닌 문자열의 내용을 확인하여 문자열이 동일한지 테스트한다는 것을 의미한다.

Java에는 다양한 데이터 구조를 나타내는 여러 매개변수화된 클래스가 제공된다. 이러한 클래스는 **Java Collection Framework** 을 구성한다.

그런데 `ArrayList`는 매개 변수화 되지 않은 유형으로도 사용할 수 있다. 이는 변수를 선언하고 다음과 같은 `ArrayList` 유형의 객체를 생성할 수 있음을 의미한다. 

```java
ArrayList list = new ArrayList();
```

`list`는 `Object`의 하위 클래스에 속하는 모든 객체를 보유할 수 있다. 모든 클래스는 `Object`의 하위 클래스이므로 이는 모든 객체가 `list`에 저장될 수 있음을 의미한다. 


## 2. Wrapper 클래스

이미 언급했듯이 매개변수화된 유형은 primitive type과 작동하지 않는다. `ArrayList<int>`와 같은 것은 없다. 그러나 이 제한은 `Integer`, `Character`와 같은 소위 래퍼 클래스로 인해 결국 제한되지 않은 것으로 나타났다.

섹션 2.5에서 `Double`, `Integer` 클래스를 간략하게 살펴보았다. 이러한 클래스에는 문자열을 숫자 값으로 변환하는 데 사용되는 정적 메서드 `Double.parseDouble()`, `Integer.parseInteger()`가 포함되 어 있으며 `Integer.MAX_VALUE` 같은 상수도 포함되어 있다. 또한 `char` 유형의 주어진 값이 문자인지 테스트하는 데 사용할 수 있는 `Character.isLetter()`도 있다. 다른 기본 유형 각각에 대해 `Long`, `Short`, `Byte`, `Float`, `Boolean` 래퍼 클래스가 있다. 유용한 정적 멤버가 포함되어 있지만 다른 용도 있다. 기본 유형 값을 객체로 나타내는 데 사용된다.


기본 유형은 클래스가 아니며 기본 유형의 값은 객체가 아니란 점을 기억하자. 그러나 때로는 기본 값을 객체인 것처럼 처리하는 것이 유용할 때도 있다. 래퍼 클래스 중 하나에 속하는 객체의 기본 유형 값을 래핑할 수 있다.

```java
Integer n = Integer.valueOf(42);
```

`n` 값은 `int` 유형의 값과 동일한 정보를 갖지만 객체이다. 객체에 래핑된 `int` 값을 검색하려면 `n.intValue()` 함수를 호출하면 된다. 각 래퍼 클래스에는 객체의 기본 유형 값을 래핑하기 위한 정적 메서드 `valueOf()`가 있다.


`Integer.valueOf()` 메서드는 Integer 유형의 객체를 반환하는 정적 팩토리 메서드이다. Integer 클래스에는 객체 생성을 위한 생성자도 있지만 **더 이상 사용되지 않는다(Deprecated)**. 즉, 새 코드에서 사용해선 안되며 향후에 제거될 수 잇다. 정적 팩토리 메서드에는 동일한 매개 변수 값으로 `Integer.valueOf()`가 두 번 이상 호출되는 경우 매번 동일한 객체를 반환할 수 있는 옵션이 있다는 장점이 있다. Integer 유형의 객체는 **불변(immutable)** 이기 때문에 괜찮다. 즉, 객체가 생성된 후에는 객체의 내용을 수정할 수 없다. Integer를 손에 넣은 사람은 그것이 나타내는 기본 int값을 변경할 수 없다. 불변 객체를 생성하기 위한 정적 팩토리 메서드도 포함하는 섹션 6.2.1의 Color 클래스에서 비슷한 것을 보았다.

---

래퍼 클래스를 더욱 쉽게 사용할 수 있도록 기본 유형과 해당 래퍼 클래스 간의 자동 변환이 있다. 

```java
Integer answer = 42;
```

컴퓨터는 이 내용을 마치 사실인 것처럼 조용이 읽는다.

```java
Integer answer = Integer.valueOf(42);
```

이것을 오토박싱이라고 한다. 다른 방향으로도 작동한다. 예를 들어 `d`가 `Double` 유형의 객체를 참조하는 경우 숫자 표현식에 d를 사용할 수 있다. `d` 내부의 `double` 값은 자동으로 언박싱된다. 오토박싱과 언박싱은 서브 루틴 호출에도 적용된다. 실제로 오토박싱과 언박싱을 사용하면 많은 상황에서 기본 유형과 객체 간의 차이를 무시할 수 있다.

이는 특히 매개 변수화된 유형에 해당된다. `ArrayList<int>`는 없지만 `ArrayList<Integer>`는 있다. Integer 유형의 객체를 보유하지만 Integer 유형의 모든 객체는 실제로 다소 얇은 래퍼에서 int값을 나타낸다. 

```java
ArrayList<Integer> integerList;
integerList = new ArrayList<Integer>();
```

그런 다음 숫자 42를 리스트에 객체를 추가할 수 있다.

```java
integerList.add(Integer.valueOf(42));
```

하지만 오토박싱 때문에 다음과 같이 할수 있다.

```java
integerList.add(42);
```

컴파일러는 목록에 추가하기전에 Integer 유형의 객체에 자동으로 42를 래핑한다. 마찬가지로 우리는 다음과 같이 말할 수 있다. 

```java
int num = integerList.get(3);
```

Integer 유형이지만 언박싱으로 인해 컴파일러는 자동으로 반환 값을 int로 변환한다. 


따라서 실제로 Integer의 동적 배열이 아닌 int의 동적 배열인 것처럼 사용할 수 있다.

때때로 문제를 일으키는 한 가지 문제가 있다. 리스트는 `null`값을 보유할 수 있으며 `null`은 기본 유형 값과 일치하지 않는다. `null`을 반환하는 경우 null pointer exception가 생성할 수 있음을 의미한다. 리스트의 모든 값이 `null`이 아닌지 확신하지 않는 한 이 가능성을 고려해야 한다.


## 3. ArrayList를 이용한 프로그래밍

간단한 첫 예로, 사용자 정의 동적 배열 클래스 대신 ArrayList를 사용하여 다시 할 수 있다. 이 경우 리스트에 점수를 저장한다.

```java
import textio.TextIO;
import java.util.ArrayList;

public class ReverseWithArrayList {
    
    public static void main(String[] args) {
        ArrayList<Integer> list;
        list = new ArrayList<Integer>();
        System.out.println("Enter some non-zero integers.  Enter 0 to end.");
        while (true) {
            System.out.print("? ");
            int number = TextIO.getlnInt();
            if (number == 0)
                break;
            list.add(number);
        }
        System.out.println();
        System.out.println("Your numbers in reverse are:");
        for (int i = list.size() - 1; i >= 0; i--) {
            System.out.printf("%10d%n", list.get(i));
        }
    }
}
```

이 예제에서 설명된 것 처럼 ArrayList는 일반적으로 배열이 처리되는 것과 동일한 방식으로 for 루프를 사용하여 처리된다. 

```java
for ( int i = 0; i < namelist.size(); i++ ) {
    String item = namelist.get(i);
    System.out.println(item);
}

// same
for ( String item : namelist ) {
    System.out.println(item);
}
```

래퍼 클래스로 작업할 때 for-each 루프의 루프 제어 변수는 기본 유형일 수 있다. 이것은 언박싱으로 인해 작동한다. 

```java
double sum = 0;
for ( double num : numbers ) {
   sum = sum + num;
}

// same
double sum;
for ( Double num : numbers ) {
    if ( num != null ) {
        sum = sum + num;  // Here, num is SAFELY unboxed to get a double.
      }
}
```

리스트에 `null`이 없는 한 작동한다. null값이 발생할 가능성이 있는 경우 아래와 같이 변경한다. 

---

섹션 6.3.3의 예를 본다. 새 프로그램에서 사용자는 마우스로 클릭하고 드래그하며 도면 영역에서 곡선을 스케치 할 수 있다. 사용자는 메뉴를 이용하여 그리기 색상을 선택할 수 있다. 그리기 영역의 배경색은 메뉴를 사용하여 선택할 수도 있다. 그리고 여러 명령이 포함된 "제어" 메뉴가 있다. 가장 최근에 그린 곡선을 화면에서 제거하는 "실행 취소" 명령, 모든 곡선을 제거하는 "지우기" 명령, 회전하는 "대칭 사용" 확인란이 있다. 대칭 기능을 켜고 끈다. 대칭 옵션이 켜져 있을 때 사용자가 그린 곡선이 수평 및 수직으로 반사되어 대칭 패턴을 생성한다. (대칭은 단지 예쁘게 보이기 위해 존재하는 것이다.) 


원래 SimplePaint 프로그램과 달리 이 새 버전은 데이터 구조를 사용하여 사용자가 그린 그림에 대한 정보를 저장한다. 사용자가 새 배경색을 선택하면 캔버스가 새 배경색으로 채워지고 이전에 있던 모든 곡선이 새 배경에 다시 그려진다. 그러기 위해서는 모든 곡선을 다시 그릴 수 있을 만큼 충분한 데이터를 저장해야 한다. 마찬가지로 Undo 명령은 가장 최근에 그린 곡선의 데이터를 삭제하고 나머지 데이터를 사용하여 전체 그림을 다시 그리는 방식으로 구현된다.


필요한 데이터 구조는 ArrayList를 사용하여 구현된다. 개별 곡선의 기본 데이터는 곡선의 점 목록으로 구성된다. 이 데이터는 `ArrayList<Point2D>` 유형의 객체에 저장된다. 그러나 곡선을 다시 그리려면 곡선의 점 목록 외에도 곡선의 색상을 알아야 하며 곡선에 대칭 옵션을 적용해야 하는지 여부도 알아야 한다. 곡선을 다시 그리는 데 필요한 모든 데이터는 프로글매에서 중첩 클래스로 정의된 객체로 그룹화된다.

```java
private static class CurveData {
    Color color;
    boolean symmetric;
    ArrayList<Point2D> points;
}
```

그러나 그림에는 하나가 아닌 많은 곡선이 포함될 수 있으므로 전체 그림을 다시 그리는 데 필요한 모든 데이터를 저장하려면 CurveData 유형의 객체 리스트가 필요하다. 

```java
ArrayList<CurveData> curves = new ArrayList<CurveData>();
```

여기에는 객체 목록이 있다. 각 객체에는 데이터의 일부로 포인트 목록이 포함되어 있다. 이 데이터 구조를 처리하는 몇 가지 예를 살펴본다. 사용자가 그리기 화면에서 마우스를 클릭하면 새 곡선이 시작되고 해당 곡선을 나타내기 위해 새 객체를 만들어야 한다. 새 CurveData 객체의 인스턴스 변수도 초기화되어야 한다. 다음은 이를 수행하는 `mousePressed()` 루티의 코드이다. `currentCurve`는 전역 변수이다.

```java
currentCurve = new CurveData();
currentCurve.color = currentColor;
currentCurve.symmetric = useSymmetry;
currentCurve.points = new ArrayList<Point2D>();
```

사용자가 마우스를 드래그하면 `currentCurve`에 새 점이 추가되고 추가된 점 사이에 곡선의 선이 그려진다. 사용자가 마우스를 놓면 곡선이 완성되고 다음을 호출하여 곡선 리스트에 추가된다.

```java
curves.add(currentCurve);
```

사용자가 배경색을 변경하거나 "실행 취소" 명령을 선택하면 그림을 다시 그려야 한다. 이 프로그램에는 그림을 완전히 다시 그리는 `redraw()` 메서드가 있다. 이 메서드는 CurveData 리스트의 데이터를 사용하여 모든 곡선을 그린다. 기본 구조는 각 개별 곡선에 대한 데이터를 차례로 처리하는 for-each 루프이다. 

```java
for (CurveData curve : curves) {
    // 곡선 그리기     
}
```

이 루프의 본문에서 `curve.points`는 곡선의 점 리스트를 보유하는 `ArrayList<Point2D>` 유형의 변수 이다. 곡선의 i 번째 점은 이 리스트의 `get()`를 호출하여 얻을 수 있다. 이는 `getX()`, `getY()`라는 getter 메서드가 있는 유형의 값을 반환한다. 

```java
curve.points.get(i).getX();
```

이는 다소 복잡해 보일 수 있지만 원하는 데이터 조각에 대한 경로를 지정하는 복잡한 이름의 좋은 예이다. `curve` 개체로 이동한다. `curve` 내부에 `points`로 간다. `points` 내부에 i번째 항목을 가져온다. 그리고 해당 항목의 `getX()` 메서드를 호출하여 x좌표를 가져온다. 

```java
private void redraw() {
    g.setFill(backgroundColor);
    g.fillRect(0,0,canvas.getWidth(),canvas.getHeight());
    for ( CurveData curve : curves ) {
    g.setStroke(curve.color);
        for (int i = 1; i < curve.points.size(); i++) {
            double x1 = curve.points.get(i-1).getX();
            double y1 = curve.points.get(i-1).getY();
            double x2 = curve.points.get(i).getX();
            double y2 = curve.points.get(i).getY();
            drawSegment(curve.symmetric,x1,y1,x2,y2);
        }
    }
}
```

(x1, y1)에서 (x2, y2)까지 선을 스트로크하는 메서드이다. 첫 매개변수가 true이면 해당 세그먼트의 수평 및 수직 반사도 그린다. 



