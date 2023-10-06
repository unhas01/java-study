# Section 7: 예외 및 try-catch 소개

자바에는 프로그램에서 정상적인 제어 흐름을 결정하는 제어 구조 외 에도, 제어 흐름을 정상 궤도에서 이탈시키는 "예외적인(exceptional)" 사례에 대처하는 방법이 있다. 프로그램 실행 중 오류가 발생하면 프로그램을 중단하고 오류 메시지를 인쇄하는 것이 기본적인 동작이다. 하지만, 자바에서는 이러한 오류를 "포착(catch)"하고 단순히 프로그램을 중단시키는 것과는 다른 대응을 프로그래밍할 수 있게 한다. 이는 **try..catch** 문으로 이루어진다. 이 절에서는 `try..catch` 문의 다소 복잡한 구문을 상당히 생략한 채로, 예비적이고 불완전하게 이를 살필 것이다. 오류 처리 문제는 제8장 제3절에서 다루게 될 복잡한 주제로서, `try..catch`문의 전체 구문을 그 때에 다룰 것이다.

<hr>

## 1. 예외
**예외(exception)** 라는 용어는 `try..catch`를 통해 처리하길 바라는 이벤트 유형을 가리키는 데 사용된다. 예외는 프로그램의 정상적인 제어 흐름의 예외다. 이 용어는 "오류(error)"에 우선하여 사용된다. 경우에 따라 예외는 전혀 오류로 간주되지 않을 수 있기 때문이다. 때때로 예외를 프로그램을 구성하는 다른 방법이라고 생각할 수 있다.

자바의 예외는 Exception 자료형의 객체로써 표시된다. 실제 예외는 일반적으로 Exception 의 하위 클래스에 의해 정의된다. 다른 하위 클래스는 다른 유형의 예외를 나타낸다. 이 절에서는 두 가지 유형의 예외만 살펴보기로 한다: NumberFormatException 및 IllegalArgumentException.

NumberFormatException 은 문자열을 숫자로 변환하려고 시도할 때 발생할 수 있다. 이러한 변환은 `Integer.parseInt` 및 `Double.parseDouble` 함수에 의해 수행된다. (제2장 제5절 제7관 참조) `str`이 String 자료형의 변수인 함수 호출 `Integer.parseInt(str)`을 생각해보자. `str`의 값이 문자열 "42"인 경우 함수 호출은 문자열을 int 42로 올바르게 변환한다. 그러나 str의 값이, 흠, "`fred`"라면, "`fred`"는 int 값에 대한 허용된 문자열 표현이 아니기에 함수 호출이 실패하게 된다. 이 경우 NumberFormatException 자료형의 예외가 발생한다. 예외를 처리하기 위해 아무 조치도 취하지 않으면 프로그램이 중단될 것이다.

IllegalArgumentException 은 잘못된 값이 매개변수로 서브루틴에 전달될 때 발생할 수 있다. 예를 들어 서브루틴에서 매개변수가 0보다 크거나 같아야 하는 경우, 서브루틴에 음수 값이 전달될 때 IllegalArgumentException 예외가 발생할 수 있다. 허용되지 않는 값에 어떻게 대응할 것인가는 서브루틴을 작성한 사람에게 달려있기 때문에, 모든 허용되지 않는 매개변수 값이 IllegalArgumentException 으로 귀결될 것이라고 간단히 말할 수는 없다. 그러나, 이는 일반적인 반응이다.

<hr>

## 2. try..catch
예외가 발생하면, 예외가 '던져졌다(thrown)'라고 한다. 예를 들어 `Integer.parseInt(str)`는 `str` 값이 허용되지 않을 경우 자료형 NumberFormatException 의 예외를 **던진다(throw)**. 예외가 던져지면 이 예외를 '포착(catch)'할 수 있고, 프로그램이 중단되는 것을 막을 수 있다. 이는 `try..catch` 문으로 이루어진다. 간략한 형식의 `try..catch` 문은 다음과 같을 수 있다:

```java
try {
    statements-1
}
catch (exception-class-name variable-name) {
    statements-2
}
```

위의 {exception-class-name}은 NumberFormatException, IllegalArgumentException, 또는 몇몇 다른 예외 클래스가 될 수 있다. 컴퓨터가 이 `try..catch` 문을 실행할 때, `try` 부분 안의 문장인 {statements-1}을 실행한다. {statements-1}의 실행 동안 예외가 발생하지 않으면, 컴퓨터는 `catch` 부분을 건너뛰고 프로그램의 나머지 부분을 진행한다. 하지만. {statements-1}의 실행 중에 {exception-class-name} 자료형의 예외가 발생하면, 컴퓨터는 즉시 예외가 발생하는 지점에서 `catch` 부분으로 뛰어나가 {statements-1}의 나머지 문장은 뭐든 건너뛰면서 {statements-2}를 실행한다. 오로지 하나의 예외 자료형만 포착된다는 점에 유의하라; {statements-1}을 실행하는 동안 몇몇의 다른 자료형의 예외가 발생할 경우, 이전처럼 이는 프로그램을 중단시킬 것이다.

해당 {statements-2}를 실행하는 동안 {variable-name}은 예외 객체를 나타내며, 따라서, 예를 들어 이를 인쇄할 수 있다. 예외 객체에는 예외의 원인에 대한 정보가 포함되어 있다. 이에는 예외 객체를 출력할 경우 표시되는 오류 메시지가 포함된다.

`catch` 부분이 끝난 후 컴퓨터는 나머지 프로그램을 계속 진행한다; 해당 예외는 포착되어 처리되었고 프로그램을 중단시키지는 않는다.

그런데, 중괄호인 { 및 }는 `try..catch` 문에서 구문의 일부라는 점에 유의하라. 중괄호 사이에 오직 하나의 문장이 있더라도 이들이 요구된다. 이는 지금까지 보았던, 단일 문장의 주변에 있는 중괄호는 선택적이었던 다른 문장들과는 다른 점이다.

예를 들어, `str`의 값이 허용되는 실수를 나타내거나 그렇지 않을 수 있는 String 자료형의 변수라고 가정하자. 그러면 다음과 같이 할 수 있다:

```java
double x;
try {
    x = Double.parseDouble(str);
    System.out.println( "숫자는 " + x );
}
catch ( NumberFormatException e ) {
    System.out.println( "허용되는 숫자가 아님." );
    x = Double.NaN;
}
```

`Double.parseDouble(str)` 호출로 인해 오류가 던져지면, `try` 부분의 출력문을 건너뛰고, `catch` 부분의 문장이 실행된다. (이 예제에서 필자는 예외가 발생할 때 `x`가 `Double.NaN` 값이 되도록 설정했다. `Double.NaN`은 double 자료형에서 "숫자가 아님(not-a-number)"이란 특수한 값이다.)

예외를 포착하고 프로그램을 계속 진행하는 것이 언제나 좋은 생각은 아니다. 이는 종종 나중에 훨씬 더 큰 혼란으로 이어질 수 있고, 예외가 발생하는 지점에서 프로그램을 중단시키는 것이 더 나을 수도 있다. 그러나, 때로는 오류로부터 회복될 수도 있다.

예를 들어, 사용자가 입력한 나열된 실수들에 관한 평균을 찾을 수 있는 프로그램과, 사용자가 빈 행(blank line)을 입력하여 해당 나열이 끝이라는 신호를 보낸다 가정하자. (이는 제3장 제3절의 샘플 프로그램 ComputeAverage.java와 유사하나, 해당 프로그램에서 사용자는 입력의 끝이란 신호로 0을 입력하였다.) `TextIO.getlnInt()`를 사용하여 사용자의 입력을 읽으면 빈 행을 탐지할 방법이 없다. 해당 함수는 빈 행을 그저 건너뛰기 때문이다. 해결책은 `TextIO.getln()`을 사용하여 사용자의 입력을 읽는 것이다. 이렇게 하면 빈 입력 행을 탐지할 수 있고, 비어 있지 않은 입력은 `Double.parseDouble`을 사용하여 숫자로 변환할 수 있다. 그리고 사용자의 입력이 허용되지 않은 숫자일 때 프로그램이 중단되는 걸 피하기 위해 `try..catch`를 사용할 수 있다. 다음은 해당 프로그램이다:

```java
import textio.TextIO;

public class ComputeAverage2 {

    public static void main(String[] args) {
        String str;     // 사용자 입력.
        double number;  // 숫자로 변환되는 입력.
        double total;   // 입력된 모든 숫자들의 총합.
        double avg;     // 숫자들의 평균.
        int count;      // 입력된 숫자들의 수.
        total = 0;
        count = 0;
        System.out.println("숫자를 입력하고, 리턴을 누르면 종료합니다.");
        while (true) {
                System.out.print("? ");
                str = TextIO.getln();
            if (str.equals("")) {
                break; // 루프 종료. 입력 행이 비었기 때문.
            }
            try {
                number = Double.parseDouble(str);
                // 오류가 발생하면, 다음 두 행은 건너뛰어진다!
                total = total + number;
                count = count + 1;
            }
            catch (NumberFormatException e) {
                System.out.println("허용된 숫자가 아님!  다시 시도하세요.");
            }
        }
        avg = total/count;
        System.out.printf("%d 개의 숫자들의 평균은 %1.6g%n", count, avg);
    }
}
```

## 3. TextIO 안의 예외들
`TextIO`는 사용자로부터 숫자 값을 읽을 때, 이전 예제의 `while` 루프 및 `try..catch`와 유사한 기법을 사용하여 사용자의 응답이 허용되는 것인지를 확인한다. 그러나 `TextIO`는 사용자 이외 다른 소스의 데이터를 읽을 수 있다. (제2장 제4절 제4관 참조) `TextIO`가 파일에서 읽을 때, 입력에서 허용되지 않는 값을 되찾기 위한 합리적인 방법은 없으므로, 예외를 던져서 대응한다. `TextIO`는 단순함을 유지하기 위해, 어떤 유형의 오류를 만나든 간에 오로지 IllegalArgumentException 자료형의 예외만을 던진다. 예를 들어, 파일의 모든 데이터를 이미 읽은 후에 해당 파일에서 읽으려고 하면 예외가 발생한다. `TextIO`에서 예외는 IllegalArgumentException 자료형이다. 프로그램이 중단되도록 하는 것보다 파일 오류에 더 잘 대처하려는 경우에, IllegalArgumentException 자료형의 예외를 포착하기 위해 `try..catch`를 사용할 수 있다.

예를 들어, 또 다른 숫자 평균화 프로그램을 살펴볼 것이다. 이 사례에서는, 파일에서 숫자를 읽을 것이다. 파일에 실수만 들어 있다고 가정하고, 해당 숫자들을 읽고 그 총액과 평균을 알아낼 프로그램을 원한다고 하자. 파일에 몇 개의 숫자가 들어 있는지 알 수 없기 때문에, 언제 읽기를 중단해야 하는지에 대한 문제가 있다. 한 가지 접근법은 그저 무한정 읽으려고 시도하는 것이다. 파일의 끝에 도달하면 예외가 발생한다. 이 예외는 사실 오류가 아니다 — 단지 데이터의 끝을 감지하는 방법일 뿐이므로, 예외를 포착하고 프로그램을 마칠 수 있다. 데이터를 `while (true)` 루프에서 읽을 수 있고 예외가 발생했을 때 루프에서 벗어날 수 있는 것이다. 이는 프로그램에서 예상되는 제어 흐름의 일부로 예외를 사용하는 다소 특이한 기법의 예이다.

파일에서 읽으려면, 해당 파일의 이름을 알 필요가 있다. 프로그램을 보다 일반화하기 위해, 프로그램에서 고정 파일 이름을 하드 코딩(hard-coding)하는 대신 사용자가 파일 이름을 입력하도록 할 수 있다. 그러나 사용자가 존재하지 않는 파일의 이름을 입력할 가능성도 있다. `TextIO.readfile`을 사용하여 존재하지 않는 파일을 열 때 IllegalArgumentException 자료형의 예외가 발생한다. 이 예외를 포착하고 사용자에게 다른 파일 이름을 입력하도록 요청할 수 있다. 다음은 이러한 모든 아이디어를 사용하는 전체 프로그램이다:

```java
import textio.TextIO;

/**
* This program reads numbers from a file.  It computes the sum and
* the average of the numbers that it reads.  The file should contain
* nothing but numbers of type double; if this is not the case, the
* output will be the sum and average of however many numbers were
* successfully read from the file.  The name of the file will be
* input by the user.
*/
public class AverageNumbersFromFile {

    public static void main(String[] args) {

    while (true) {
        String fileName;  // 사용자가 입력할 파일의 이름.
        System.out.print("파일의 이름을 입력하세요: ");
        fileName = TextIO.getln();
        try {
            TextIO.readFile( fileName );  // 입력을 위해 파일 열기를 시도.
            break;  // 성공한 경우 루프를 중단함.
        }
        catch ( IllegalArgumentException e ) {
            System.out.println("파일 \"" + fileName + "\"로부터 읽을 수 없습니다.");
            System.out.println("다시 시도하세요.\n");
        }
    }

    /* 이 시점에, TextIO는 파일을 읽음. */
    
    double number;  // 데이터 파일에서 읽은 숫자.
    double sum;     // 지금까지 읽은 숫자들의 합.
    int count;      // 읽은 숫자들의 수.

    sum = 0;
    count = 0;

    try {
        while (true) { // 예외 발생시 루프 종료.
            number = TextIO.getDouble();
            count++;  // 예외 발생시 이는 건너뜀
            sum += number;
        }
    }
    catch ( IllegalArgumentException e ) {// 파일 끝에 다다르면 이것이 발생한다 예상됨.// 이를 오류로 생각하지 않기에,
        // 이 catch 구문에서 할 일은 없음. 그저 프로그램 나머지를 실행함.
    }
        
    // 이 시점에서, 전체 파일을 읽음.
    System.out.println();
    System.out.println("읽은 데이터 숫자 값들: " + count);
    System.out.println("데이터 값들의 합: " + sum);
    if ( count == 0 )
        System.out.println("없는 값들의 평균을 계산할 수 없습니다.");
    else
        System.out.println("값들의 평균: " + (sum/count));
        
    }

}
```
