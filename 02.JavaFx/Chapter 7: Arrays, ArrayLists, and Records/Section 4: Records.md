# Section 4: Recodes(기록)

일부 프로그래밍 언어에 내장 데이터 구조에는 배열과 레코드라는 두 가지 기본 유형이 있다. 배열은 일련의 항목으로 구성되며, 여기서 개별 항목은 순서의 숫자 위치로 참조된다. 반면에 레코드에서는 데이터 구조의 위치에 숫자 대신 이름이 있다. 레코드의 항목을 "필드"라고 하며 항목 이름을 "필드 이름"이라고 한다. 필드 이름을 사용하여 필드에 엑세스한다. 우리는 레코드의 필드가 객체의 인스턴스와 동일한 역할을 한다는 점에서 레코드를 객체와 유사한 것으로 인식하지만 레코드는 객체 지향 프로그래밍 이전에도 존재했다. 실제 단어 "레코드"는 Pascal 및 Cobol과 같은 프로그래밍 언어에서 사용된다. C 프로그래밍 언어에서는 동일한 개념으로 "struct"라는 용어를 사용한다.

Java에서는 클래스를 사용하여 레코드를 나타낼 수 있지만 "레코드"라는 용어는 전통적으로 사용되지 않았다. 그러나 Java 17에서는 레코드가 특별한 종류의 클래스 형태로 언어의 공식적인 부분이 되었다. Java 레코드는 실제로 다른 언어의 레코드와 동일하지 않는다. 왜냐하면 Java 레코드는 불변이기 때문이다. 즉, 레코드가 생성된 후에 해당 내용을 수정할 수 없다. 그러나 이름이 지정된 필드에 대한 매우 간단한 컨테이너라는 점은 다른 레코드 유형이다. 


## 1. 기본 자바 레코드

Java 레코드는 레코드 클래스라고 부르는 특별한 종류의 클래스에 속하는 객체이다. 가장 간단한 경우, 레코드 클래스는 레코드 필드를 나타내는 인스턴스 변수 세트만 지정한다. 

```java
public record FullName(String firstName, String lastName) { }
```

이는 두 개 문자열 유형의 인스턴스 변수가 있는 FullName이라는 레코드 클래스에 대한 클래스 정의이다. 이러한 인스턴스 변수는 레코드의 필드(fields)이다. "{ }"는 빈 클래스 본문이다. 레코드 클래스의 인스턴스 변수는 클래스 이름 뒤의 괄호 안에 나열된다. 구문은 메서드 정의의 형식 매개 변수 목록에 대한 구문과 동일하지만 의미는 매우 다르다. FullName 유형의 레코드, 즉 레코드 클래스의 인스턴스는 `new` 연산자를 사용하여 일반적인 방법으로 생성된다.

```java
FullName fname = new FullName("Jane", "Doe");
```

이 명령문은 레코드의 각 필드에 대해 하나의 매개 변수가 있는 생성자를 호출하며, 그 효과는 단순히 각 필드에 대한 값을 제공하는 것이다. 이 생성자는 클래스에 명시적으로 정의되어 있지 않다. 이는 레코드 클래스의 **정식 생성자(canonical constructor)** 라고 하며 컴파일러에 의해 자동으로 제공된다. 실제로 컴파일러는 레코드 클래스 정의에 암시적으로 많은 항목을 추가한다. 위 레코드 정의는 일반 클래스 정의와 동일하다.

```java
public final class FullName {
    private final String firstName;
    private final String lastName;
    public FullName( String firstName, String lastName ) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    public firstName() {
        return firstName;
    }
    public lastName() {
        return lastName;
    }
    public String toString() {
       return "FullName[firstName=" + firstName 
                      + ", lastName=" + lastName + "]";
    }
    public boolean equals(Object obj) {
        // (definition omitted)
    }
    public int hashCode() {
        // (definition omitted)
    }
}
```

레코드 클래스는 자동으로 `final`이다. 즉, 하위 클래스로 확장할 수 없다. 또한 레코드 클래스는 다른 클래스를 확장할 수 없다.

레코드의 인스턴스 변수는 `private`, `final`이다. 인스턴스 변수에 대한 접근자 또는 "getter" 메서드는 자동으로 정의되지만 getter 메서드에 대한 일반적인 `getXXX()` 명명 규칙을 사용하는 대신 해당 이름은 인스턴스 변수의 이름과 동일하다. 예를 들어 FullName 유형의 변수인 경우 `fname.firstName()` 및 `fname.lastName()`으로 엑세스 된다. 인스턴스 변수가 `final`이기 때문에 레코드는 불변이므로 "setter" 메서드를 정의할 수 없다.


또한 Object 클래스에서 상속된 세 가지 메서드인 `toString()`, `equals()`, `hasCode()`에 대해 합리적인 정의가 자동으로 제공된다. `toString()` 메서드는 클래스 이름과 해당 필드의 이름 및 값을 포함하는 문자열을 반환한다. 해당 매개 변수가 해당 필드에 대해 동일한 값을 갖는 동일한 유형인 경우 `equals()` 메서드는 true를 반환한다.

레코드 클래스 정의가 더 복잡할 수 지만 빈 클래스 본문이 있는 기본 레코드 클래스는 관련 데이터 항목 집합을 함께 그룹화하는 간단한 방법을 제공하는 매우 일반적일 것으로 예상해야 한다. 

```java
private static class CurveData {
    Color color;
    boolean symmetric;
    ArrayList<Point2D> points;
}
```

이 중첩 클래스는 중첩 레코드 클래스로 대체될 수 있다.

```java
private record CurveData (
    Color color,
    boolean symmetric,
    ArrayList<Point2D> points
) { }
```

중첩된 CurveData 레코드 클래스는 자동으로 static이기 때문에 static으로 선언할 필요가 없다. 

이렇게 변경한 후에는 CurveData 객체가 생성될 때 해당 필드에 값을 제공해야 한다. 

```java
currentCurve = new CurveData(currentColor, useSymmetry, new ArrayList<Point2D>());
```

또 다른 변경 사항은 CurveData 객체가 이제 변경 불가능한 불변이다. 예를 들어 Point2D 클래스는 x,y 좌표를 변경하지 못하기 때문에 레코드 클래스가 될 수 없다.


## 2. 추가 레코드 구문

클래스 이름 뒤의 리스트에 정의된 것 외에 추가 인스턴스 변수를 레코드 클래스에 추가하는 것은 불가능하다. 그러나 거의 모든 것이 클래스 본문에 추가될 수 있다. 예를 들어 레코드 클래스에는 모든 종류의 항목이 `static` 을 포함할 수 있다. 기본 `toString()`, `equals()`, `hasCode()` 메서드에 대한 대체 메서드를 포함하여 인스턴스 메서드를 정의할 수 있다. 그리고 몇 가지 제한 사항만 적용하여 생성자를 정의할 수 있다. 우선 생성자의 매개 변수 리스트가 생략된 구문을 이용하면 자동으로 정의되는 표준 생성자에 대한 정의를 확장할 수 있다. 예를 들어, `FullName`의 정식 생성자느 `firstName`이 `null`인 경우 예외가 발생할 수 있다.

```java
public FullName {
    if (firstName == null) {
        throw new IllegalArgumentException();    
    }    
}
```

이는 정식 생성자를 확장한다. 매개 변수 리스트가 정의에서 생략되었지만 이 생성자를 호출하며려면 여전히 두 개의 매개변수가 필요하며 생성자 정의 코드가 실행되기 전에 여전히 해당 매개 변수를 사용하여 `firstName`, `lastName`을 초기화한다.

추가 생성자를 정의할 수 있지만 비표준 생성자는 섹션 5.6.3에서 설명한 대로 특수 변수 `this`를 사용하여 동일한 클래스의 생성자에 대한 호출로 시작해야 한다. 즉, 정식 생성자는 다른 생성자에 의해 직간접적으로 호출된다. 

예를 들어, 단일 이름만 사용하는 사람들이 있다는 점에 주목하여 해당 이름을 나타내는 매개 변수 하나만 사용하는 `FullName` 생성자를 제공할 수 있다.

```java
public FullName (String onlyName) {
    this(onlyName, null);    
}
```

이 생성자는 기본 생성자를 호출하여 `firstName` 필드를 `onlyName`으로 설정하고 `lastName`을 `null`로 설정한다. 

`FullName` 레코드 클래스에 대해 보다 자연스러운 `toString()` 버전을 정의할 수도 있다. 


**참고 사항**

레코드 클래스는 다른 클래스를 확장할 수 없지만 하나 이상의 인터페이스를 구현할 수 있다.

## 3. 복잡한 예

복소수(complex number)는 복소수의 실수부와 허수부라고 하는 두 개의 실수로 구성된다. 복소수의 수학에 대해 아무것도 모르더라도 이것이 기록에 대한 자연스러운 적용이라는 것을 알아야 한다. Java에서 복소수를 표현하려면 `double` 유형의 두 값에 대한 간단한 컨테이너가 필요하다.

```java
record Complex (double re, double im) { }
```

그러나 복소수로 할 수 있는 일이 많이 있으며, 우리는 실제로 복소수를 표현한다고 할 수 있는 일 중 일부를 클래스에 포함시키고 싶다. 다음은 이러한 사항 중 몇 가지를 포함하는 레코드 클래스이다.

```java
public record Complex(double re, double im)  {
    
    public final static Complex ONE = new Complex(1,0);
    public final static Complex ZERO = new Complex(0,0);
    public final static Complex I = new Complex(0,1);
    
    public Complex(double re) {
        this(re,0);
    }
    
    public String toString() {
        if (im == 0)
            return String.valueOf(re);
        else if (re == 0) {
            if (im < 0)
                return "-I*" + (-im);
            else
                return "I*" + im;
        }
        else if (im < 0)
            return re + " - " + "I*" + (-im);
        else
            return re + " + " + "I*" + im;
    }
    
    public Complex plus(Complex c) {
        return new Complex(re + c.re, im + c.im);
    }
    
    public Complex minus(Complex c) {
        return new Complex(re - c.re, im - c.im);
    }
    
    public Complex times(Complex c) {
        return new Complex(re*c.re - im*c.im,
                re*c.im + im*c.re);
    }
    
    public Complex dividedBy(Complex c) {
        double denom = c.re*c.re + c.im*c.im;
        double real = (re*c.re + im*c.im)/denom;
        double imaginary = (im*c.re - re*c.im)/denom;
        return new Complex(real,imaginary);
    }
        
}
```

이 클래스는 일부 정적 멤버 변수, 단일 실수에서 복소수를 생성하는 생성자, 적절한 형식으로 복소수를 print하는 `toString()` 메서드 및 복소수에 대한 산술 연산을 구현하는 일부 인스턴스 변수를 추가한다. 물론 두 개의 실수로부터 복소수를 생성하는 정식 생성자도 있다. 

----

레코드 클래스가 `final`이어야 하는 이유와 레코드가 변경 불가능해야 하는 이유에 대해 생각해 볼 가치가 있다. `final`인 이유 중 하나는 컴파일러가 생성된 코드에 특정 종류의 최적화를 적용할 수 있다는 것이다. 이는 record 클래스 뿐만 아니라 final 클래스에도 적용된다.

여기에 예시가 있다. 복소수를 포함하는 복잡한 산술 표현식을 계산하는 것이 일반적이다. 이차 공식은 다음과 같이 계산할 수 있다.

```java
(A.times(x).times(x)).plus(B.times(x)).plus(C)
```

`times()` 및 `plus()` 메서드의 정의를 확인하면 메서드가 호출될 때 마다 새로운 `Complex` 객체가 생성되는 것을 볼 수 있다. 이차 공식 예에서는 5개의 새로운 객체가 생성되지만 우리는 최종 답을 나타내는 객체에만 관심이 있다. 나머지 4개 객체는 스크래치 작업에 불과하다. 생성되어 아주 잠깐 사용된 후 즉시 가비지 수집 대상이 된다. 많은 수의 객체를 생성하고 가비지 수집하는 것은 비효율적일 수 있다. 그러나 이 경우 컴파일러는 `plus()` 및 `times()` 호출을 대체하여 추가 객체 생성을 방지할 수 있다. 객체 대신 `double` 유형의 임시 지역 변수를 사용하여 동일한 작업을 직접 수행하는 코드를 사용한다. 각 메서드 호출이 수행하는 작업을 정확히 알고 있기 때문에 이를 수행할 수 있다. 그러나 이는 Complex 클래스가 최종 클래스이기 때문에 가능한 경우이다. 그렇지 않은 경우 A, B, C 또는 x는 실제로 `plus()` 및 `times()` 를 재정의한 `Complex` 의 하위 클래스에 속하는 객체를 참조할 수 있다 . 이는 컴파일 타임이 아닌 런타임에만 확인할 수 있는 것이므로 클래스가 최종 클래스가 아닌 경우 컴파일러는 프로그램이 종료될 때 `plus()` 및 `times()` 호출이 무엇을 할지 알 수 없다. 

불변성은 최적화에도 도움이 될 수 있다. 왜냐하면 컴파일러는 객체에 대한 메서드 호출이 해당 객체의 인스턴스 변수를 수정하지 않는다는 것을 확신할 수 있기 때문이다. 그러나 불변성을 통해 프로그램에 대해 추론하기가 더 쉬워진다는 것은 아마도 더 중요할 것 이다. 불변 개체의 일부 속성이 특정 시점에 true임을 증명할 수 있다면 객체가 수정되었기 때문에 나중에 false이 되지 않을 것임을 확신할 수 있다. 







