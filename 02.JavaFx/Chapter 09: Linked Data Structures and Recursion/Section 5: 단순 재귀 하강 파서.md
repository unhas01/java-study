# 단순 재귀 하강 파서(A Simple Recursive Descent Parser)

나는 항상 영어와 같은 자연어와 컴퓨터가 사용하는 인공 언어 모두 언어애 매료되어있다. 언어가 어떻게 정보를 전달 할 수 있는지, 어떻게 구조화되어 있는지, 어떻게 처리될 수 있는지에 대한 어려운 질문이 많이 있다. 자연어와 인공 언어는 매우 유사하므로 프로그래밍 언어를 연구하면 훨씬 더 복잡하고 어려운 자연어에 대한 통찰력을 얻을 수 있다. 그리고 프로그래밍 언어는 그 자체로 공부할 가치가 있을 만큼 흥미로운 문제를 많이 제기한다. 결국 프로그램을 작성하는 데 사용되는 상대적으로 단순한 언어까지 컴퓨터가 "이해"하도록 만들 수 있는 방법은 무엇인가? 컴퓨터는 매우 간단한 기계어로 표현된 명령어만을 직접적으로 사용할 수 있다. 고급언어는 기계어로 번역해야 한다. 그러나 번역은 단지 프로그램인 컴파일러에 의해 수행된다. 그러나 번역 프로그램은 어떻게 작성될 수 있나??

## 1. Backus-Naur 형식

자연어와 인공 언어는 문법이나 구문이라는 구조를 가지고 있다는 점에서 유사하다. 구문은 법적 문장이나 프로그램의 의미를 설명하는 일련의 규칙으로 표현될 수 있다. 프로그래밍 언어의 경우 구문 규칙은 종종 **BNF(Backus-Naru Form)** 로 표현 된다. 1950년대 후반에 컴퓨터 과학자인 John Backus와 Peter Naru가 개발한 시스템이다. 흥미롭게도 자연어의 문법을 기술하기 위해 언어학자 Noam Chomsky가 거의 같은 시기에 독립적으로 등가 시스템을 개발했다. BNF는 가능한 모든 구문 규칙을 표현할 수 없다. 예를 들어, 변수를 사용하기 전에 정의해야 한다는 사실을 표현할 수 없다. 게다가 언어의 의미나 의미에 대해서는 아무 말도 하지 않는다. 인공 프로그래밍 언어라도 언어의 의미를 지정하는 문제는 아직 완전히 해결되지 않은 문제이다. 그러나 BNF는 언어의 기본 구조를 표현하여 컴파일러 설계에서 중심 역할을 한다.

BNF에는 다양한 표기법이 사용된다. 여기서 사용하라 것은 매우 일반적인 것이다. 다른 표기법이 사용되더라고 동일한 개념을 표현한다.

영어에서 "명사", "타동사", "전치사구"와 같은 용어는 문장의 구성 요소를 설명하는 **구문 범주(syntactic categories)** 이다. 마찬가지로 "문", "숫자", "while 루프"는 Java 프로그래밍의 구성 요소를 설명하는 구문 범주이다. BNF에서는 구문 법주를 "<"와 ">" 사이에 단어로 묶어 쓴다. 예. <명사>, <동사구>, <while-loop>. BNF의 규칙은 다른 구문 범주 및 언어의 기본 기호 측면에서 특정 구문 범주의 항목 구조를 지정한다.

```
<sentecne> ::= <noun-phrase> <verb-phrase>
<문장> ::= <명사구> <동사구>
```

` ::= ` 기호는 "될 수 있음"으로 읽혀지므로 이 규칙은 <문장>이 <명사구>뒤에 <동사구>가 올 수 있음을 의미한다. (문장에 대해 다른 가능한 형태를 지정하는 다른 규칙이 있을 수 있기 때문에 용어는 "is"가 아닌 "can be"이다.) 이 규칙은 문장의 레시피로 생각할 수 있다. 명사구를 만들고 그 뒤에 동사구가 온다. 명사구와 동사구는 차례로 다른 BNF 규치에 의해 정의되어야 한다.

BNF에서 대안 간의 선택은 `"|"` 기호로 표시되며 "or"로 읽힌다.

```
<verb-phrase>  ::=  <intransitive-verb>  |
                    ( <transitive-verb> <noun-phrase> )
                    
<동사구> ::= <자동사> |
                    ( <타동사> <명사구> )
```

`<verb-phrase>`는 `<intrasxitive-verb>`이거나 `<transitive-verb>` 뒤에 `<none-phrase>`가 올 수 있다고 말한다. 그룹화에는 괄호를 사용할 수도 있다. 항목이 선택사항이라는 사실을 표현하기 위해 `"["`, `"]"` 사이에 묶을 수 있다. 여러번 반복할 수 있는 옵션 항목은 `"["`, `"]"` 사이에 표시된다. 그리고 설명되는 언어의 실제 부분인 기호는 따옴표로 묶여있다.

```
<noun-phrase>  ::=  <common-noun> [ "that" <verb-phrase> ]  |
                    <common-noun> [ <prepositional-phrase> ]...

<명사구> ::= <공통명사> [ "그" <동사구> ] |
                    <공통명사> [ <전치사구> ]...
```

`<none-phrase>`는 `<common-noun>` 일 수 있고, 선택적으로 리터럴 단어 "that"과 `<verb-phrase>`가 뒤에 올 수 있고 `<common-noun>` 뒤에 0개 이상의 `<prepositional-phrase>`가 올 수도 있다. 분명히 우리는 매우 복잡한 구조를 이런 식으로 설명할 수 있다. 지정한 힘은 BNF 규칙이 재귀적일 수 있다는 사실에서 비롯된다. 실제로 앞의 두 규칙을 함께 사용하면 재귀적이다. `<noun-phrase>`는 `<verb-phrase>`로 부분적으로 정의되는 반면, `<verb-phrase>`는 `<noun-phrase>`로 부분적으로 정의된다. <명사구>는 "치즈를 먹은 쥐"가 될 수 있다. "치즈를 먹었다"는 <동사구>이기 때문이다. 언어의 재귀적 구조는 언어의 가장 기본적인 속성 중 하나이며, 이러한 재귀적 구조를 표현하는 BNF의 능력은 이를 매우 유용하게 만든다.

BNF는 Java와 같은 프로그래밍 언어의 구문을 형식적이고 정확한 방식으로 설명하는 데 사용될 수 있다 

```
<while-loop> ::= "while" "(" <condition> ")" <statement>
```

이는 while-loop가 단어 "while", 왼쪽 괄호, condition, 오른쪽 괄호, statement로 구성되어 있음을 의미한다. 물론 조건과 진술이 무엇을 의미하는지 정의하는 것은 여전히 남아 있다. 명령문은 무엇보다도 while 루프일 수 있으므로 이미 Java 언어의 재귀 구조를 볼 수 있다. 말로 명확하게 표현하기 어려운 `if`문의 정확한 사양은 다음과 같이 주어질 수 있다.

```
<if-statement>  ::=  
             "if" "(" <condition> ")" <statement>
             [ "else" "if" "(" <condition> ")" <statement> ]...
             [ "else" <statement> ]
```

이 규칙의 "else" 부분이 선택 사항이며 선택적으로 하나 이상의 "else if" 부분이 있을 수 있음을 분명히 한다.

## 2. 재귀 하강 구문 분석(Recursive Descent Parsing)

이 섹션의 나머지 부분에서는 언어에 대한 BNF 문법이 파서를 구성하기 위한 가이드로 어떻게 사용될 수 있는지 보여줄 것이다. 파서는 언어 구문의 문법 구조를 결정하는 프로그램이다. 이는 프로그래밍 언어의 경우 해당 문구를 기계어로 번역하는 것을 의미하는 문구의 의미를 결정하는 첫 번째 단계이다. 비록 간단한 예만 살펴보겠지만, 이것이 컴파일러가 실제로 인간에 의해 작성되고 이해될 수 있다는 점을 확신시키고 그것이 어떻게 이루어질 수 있는지에 대한 아이디어를 제공하기에 충분할 것이다.

우리가 사용할 구문 분석 방법은 재귀 하강 구문 분석이라고 한다. 이것은 유일한 구문 분석 방법이거나 가장 효율적인 방법은 아니지만 손으로 컴파일러를 작성하는 데 가장 적합한 방법이다. 재귀 하강 파서에서 BNF 문법의 모든 규칙은 서브 루틴의 모델이다. 모든 BNF 문법이 재귀 하강 구문 분석에 적합한 것은 아니다. 문법은 특정 속성을 만족해야 한다. 기본적으로 문구를 구문 분석하는 동안 입력의 다음 항목을 보는 것만으로도 다음에 어떤 구문 범주가 나올지 알 수 있어야 한다. 많은 문법은 이 속성을 염두에 두고 설계되었다.

---

구문 오류가 포함된 구문을 구문 분석하려고 구문 분석하려고 하면 오류에 응답할 수 있는 방법이 필요하다. 이를 수행하는 편리한 방법은 예외를 발생시키는 것이다. 다음과 같이 정의된 예외 클래스를 사용한다.

```java
private static class ParseError extends Exception {
    ParseError (String message) {
        super(message);
    }
}
```

또 다른 일반적인 요점은 BNF 규칙이 항목 사이의 공백에 대해 아무 것도 말하지 않지만 실제로는 항목 사이에 마음대로 공백을 삽입할 수 있기를 원한다는 것이다. 이를 허용하기 위해 다음에 입력할 내용을 미리 확인하기 전에 항상 `TextIO.skipBlanks()` 루틴을 호출한다. `TextIO.skipBlanks()`는 입력에서 공백 및 탭과 같은 공백을 건너뛰고 입력의 다음 문자가 공백이 아닌 문자이거나 줄 끝 문자인 경우 중지된다.

아주 간단한 예부터 시작한다. "완전히 괄호로 묶인 표현식"은 규칙에 따라 BNF에 지정될 수 있다.

```
<expression>  ::=  <number>  |
                   "(" <expression> <operator> <expression> ")"
                   
<operator>  ::=  "+" | "-" | "*" | "/"

<식> ::= <번호> |
                   "(" <식> <연산자> <식> ")"
                   
<연산자> ::= "+" | "-" | "*" | "/"
```

여기서 `<number>`는 음수가 아닌 실수를 나타낸다. 완전히 괄호로 묶인 표현식의 예는 "(((34-17)*8)+(2*7))" 이다. 모든 연산자는 한 쌍의 괄호에 해당하므로 연산자가 적용되는 순서에 대한 모호성은 없다. 그러한 표현식을 읽고 평가하는 프로그램이 필요하다고 가정해 본다. `TextIO`를 사용하여 표준 입력에서 표현식을 읽는다. 재귀 하강 구문 분석을 적용하려면 문법의 각 규칙에 대한 서브 루틴이 필요하다. `<operator>` 규칙에 따라 연산자를 읽는 서브 루틴을 얻는다. 연산자는 네 가지 중 하나를 선택할 수 있다.

```java
/**
 * 입력의 다음 문자가 적법한 연산자 중 하나인 경우
 *읽고 돌려주세요. 그렇지 않으면 ParseError를 발생시킵니다.
 */
static char getOperator() throws ParseError {
    TextIO.skipBlanks();
    char op = TextIO.peek(); // look ahead at the next char, without reading it
    if ( op == '+' || op == '-' || op == '*' || op == '/' ) {
        TextIO.getAnyChar();  // read the operator, to remove it from the input
        return op;
    }
    else if (op == '\n')
        throw new ParseError("Missing operator at end of line.");
    else
        throw new ParseError("Missing operator.  Found \"" +
            op + "\" instead of +, -, *, or /.");
}
```

나는 다음 문자가 줄 끝인지 아니면 다른 문자인지에 따라 합리적인 오류 메시지를 제공하려고 노력했다. 나는 `TextIO.peek()`를 사용하여 다음 문자를 읽기 전에 미리 보고, 항목을 구분하는 공백을 무시하기 위해 `TextIO.peek()`를 테스트하기 전에 `TextIO.skipBlanks()`를 호출한다. 나는 모든 경우에 이와 동일한 패턴을 따를 것이다. 

`<expression>`에 대한 서브루틴을 살펴보면 상황이 좀 더 흥미로워진다. 규칙에 따르면 표현식은 숫자이거나 괄호로 묶인 표현식일 수 있다. 우리는 다음 문자를 미리 보면 그것이 무엇인지 알 수 있다. 문자가 숫자이면 숫자를 읽어야 한다. 문자가 "("이면 "("를 읽어야 하고 표현식, 연산자, 다른 표현식, ")"를 읽어야 한다. 다음 문자가 다른 문자인 경우 오류다. 중첩된 표현식을 읽으려면 재귀가 필요하다. 루틴은 표현식을 읽는 것 뿐만 아니라 해당 값을 계산하고 반환한다. 이를 위해서는 BNF 규칙에 지정되지 않은 의미 정보가 필요하다.

```java
/**
 * 현재 입력 줄에서 표현식을 읽고 그 값을 반환합니다.
 *  @throws ParseError 입력에 구문 오류가 포함된 경우
 */
private static double expressionValue() throws ParseError {

    TextIO.skipBlanks();
    if ( Character.isDigit(TextIO.peek()) ) {
        // 입력의 다음 항목은 숫자이므로 표현식은
        // 해당 숫자로만 구성되어야 합니다. 읽고 돌아가기
        // 수.
        return TextIO.getDouble();
    }
    else if ( TextIO.peek() == '(' ) {
        // 표현식은 다음 형식이어야 합니다.
        // "(" <식> <연산자> <식> ")"
        // 이 항목을 모두 읽어서 연산을 수행하고,
        // 결과를 반환합니다.
        TextIO.getAnyChar();  // Read the "("
        double leftVal = expressionValue();  // Read and evaluate first operand.
        char op = getOperator();             // Read the operator.
        double rightVal = expressionValue(); // Read and evaluate second operand.
        TextIO.skipBlanks();
        if ( TextIO.peek() != ')' ) {
            // 규칙에 따르면 여기에는 ")"가 있어야 합니다.
            // 누락되었으므로 ParseError를 발생시킵니다.
            throw new ParseError("Missing right parenthesis.");
        }
        TextIO.getAnyChar();  // Read the ")"
        switch (op) {    result. 
            case '+':  return leftVal + rightVal;
            case '-':  return leftVal - rightVal;
            case '*':  return leftVal * rightVal;
            case '/':  return leftVal / rightVal;
            default:   return 0;  // Can't occur since op is one of the above.
        }
    }
    else {  // 다른 문자는 합법적으로 표현식을 시작할 수 없습니다.
        throw new ParseError("Encountered unexpected character, \"" +
                TextIO.peek() + "\" in input.");
    }
}
```

이 루틴이 BNF 규칙과 어떻게 일치하는지 확인하기 바란다. 규칙에서 `"|"`를 사용하여 대안 간의 선택을 제공하는 경우 루틴에 어떤 선택을 할지 결정하는 if문이 있다. 규칙에 "(" <표현식> <연산자> <표현식> ")" 항목 시퀀스가 포함된 경우 서브 루틴에는 각 항목 차례로 읽는 일련의 명령문이 있다.

표현식 (((34-17)*8)+(2*7))을 평가하기 위해 표현식 값이 `expressionValue()`를 호출되면 입력 시작 부분에 "("가 표시되므로 if문의 else 부분은 다음과 같다. "("가 읽혀진다. 그런 다음 `expressionValue()`에 대한 첫 번째 재귀 호출은 하위 표현식 ((34-17)*8)을 읽고 평가하고, `getOperator()`에 대한 호출은 "+" 연산자를 읽고, 두 번째 재귀 호출은 `expressionValue()` 호출은 두 번째 하위 표현식 (2*7)을 일기고 평가한다. 마지막으로 표현식의 끝인 ")"을 읽는다. `expressionValue()` 루틴에 대한 추가 재귀 호출이 포함되지만 이에 대해 너무 깊게 생각하지 않는 것이 좋다.

---

완전히 괄호로 묶인 표현은 사람들이 사용하기에 그다지 자연스럽지 않다. 그러나 일반 표현식의 경우 연산자 우선 순위 문제에 대해 걱정해야 한다. 예를 들어 "5+3*7" 표현식의 "*"가 "+" 앞에 적용된다는 것을 알 수 있다. 복잡한 표현 "3*6+8*(7+1)/4-24"는 "3*6", "8*(7+1)/4", "24"와 "+", "-" 연산자를 사용하는 용어로 구성된 것으로 본다. 반면에 "*", "/" 연산자와 결합된 여러 요소로 구성될 수 있다. 인수 8, (7+1), 4를 포함한다. 또한 이 예에서는 요인이 숫자이거나 괄호 안의 표현식일 수 있음을 보여준다. 상황을 좀 더 복잡하게 만들기 위해 -7과 같이 마이너스 기호를 붙일 수 있다. 이 구조는 BNF 규칙으로 표현될 수 있다.

```
<expression>  ::=  [ "-" ] <term> [ ( "+" | "-" ) <term> ]...
<term>  ::=  <factor> [ ( "*" | "/" ) <factor> ]...
<factor>  ::=  <number>  |  "(" <expression> ")"

<식> ::= [ "-" ] <용어> [ ( "+" | "-" ) <용어> ]...
<용어> ::= <인수> [ ( "*" | "/" ) <인수> ]...
<인자> ::= <숫자> | "(" <표현식> ")"
```

첫 규칙은 포함된 항목이 0번, 1번, 2번 또는 그 이상을 나타낼 수 있음을 나타내는 "[ ] ..." 표기법을 사용한다. 이 규칙은 <expression>이 선택적으로 "-"로 시작할 수 있음을 의미한다. 그런 다음 선택적으로 연산자 "+" 또는 "-" 중 하나와 또 다른 <term>, 선택적으로 다른 연산자 및 <term> 중 하나가 올 수 있어야 하는 <term>이 있어야 한다. 표현식을 읽고 평가하는 서브 루틴에서 이 반복은 while 루프에 의해 처리된다. if문은 루프 시작 부분에 사용되어 마이너스 기호가 있는지 테스트한다.

```java
/**
 * 현재 입력 줄에서 표현식을 읽고 그 값을 반환합니다.
 * @throws ParseError 입력에 구문 오류가 포함된 경우 
 */
private static double expressionValue() throws ParseError {
    TextIO.skipBlanks();
    boolean negative;  // 앞에 빼기 기호가 있으면 참입니다.
    negative = false;
    if (TextIO.peek() == '-') {
        TextIO.getAnyChar(); // 빼기 기호를 읽습니다.
        negative = true;
    }
    double val;  
    val = termValue();
    if (negative)
        val = -val;  // 첫 번째 항에 선행 빼기 기호를 적용합니다.
    TextIO.skipBlanks();
    while ( TextIO.peek() == '+' || TextIO.peek() == '-' ) {
        // 다음 항을 읽고 더하거나 뺍니다.
        // 표현식에서 이전 항의 값입니다.
        char op = TextIO.getAnyChar(); 
        double nextVal = termValue();  
        if (op == '+')
            val += nextVal;
        else
            val -= nextVal;
        TextIO.skipBlanks();
    }
    return val;
}
```

## 3. 표현식 트리 작성

지금까지는 표현식만 평가했다. 그것이 프로그램을 기계어로 번역하는 것과 어떤 관련이 있나?? 실제로 표현식을 평가하는 대신 표현식을 평가하는 데 필요한 기계어 명령을 생성하는 것이 거의 쉬울 것이다. "스택 머신"으로 작업하는 이러한 명령은 "숫자 푸시" 또는 "+ 작업"과 같은 스택 작업이 된다. [SimpleParser3.java](https://math.hws.edu/javanotes/source/chapter9/SimpleParser3.java) 프로그램은 표현식을 평가하고 표현식 평가를 위한 스택 머신 작업 목록을 인쇄할 수 있다.

이 프로그램에서 Java로 작성된 프로그램을 읽고 동등한 기계어 코드를 생성할 수 있는 재귀 하강 파서로의 도약은 크지만 개념적 도약은 그리 크지 않다.

SimpleParser3 프로그램은 표현식을 구문 분석할 때 실제로 스택 작업을 직접 생성하지 않는다. 섹션 9.4.3에서 설명한 대로 표현식을 표현하기 위해 표현식 트리를 구축한다. 그런 다음 표현식 트리를 사용하여 값을 찾고 스택 작업을 생성한다. 트리는 ConstNode와 BinOpNode 클래스에 속하는 노드로 구성된다. ExpNode의 또 다른 하위 클래스인 UnaryMinusNode는 단항 빼기 연산을 나타내기 위해 도입됬다. `printStackCommands()` 메서드를 추가해 스택 작업을 인쇄하는 역할을 한다.


```java
private static class BinOpNode extends ExpNode {
    char op;        // The operator.
    ExpNode left;   // The expression for its left operand.
    ExpNode right;  // The expression for its right operand.
    BinOpNode(char op, ExpNode left, ExpNode right) {
        // Construct a BinOpNode containing the specified data.
        assert op == '+' || op == '-' || op == '*' || op == '/';
        assert left != null && right != null;
        // (for assert statements, see Subsection 8.4.1)
        this.op = op;
        this.left = left;
        this.right = right;
    }
    double value() {
        // 왼쪽과 오른쪽을 평가하여 값을 얻습니다.
        // 피연산자와 값을 연산자와 결합합니다.
        double x = left.value();
        double y = right.value();
        switch (op) {
            case '+':
                return x + y;
            case '-':
                return x - y;
            case '*':
                return x * y;
            case '/':
                return x / y;
            default:
                return Double.NaN;  // Bad operator! Should not be possible.
        }
    }
    void  printStackCommands() {
        // 스택 머신에서 표현식을 평가하려면 먼저 다음을 수행하십시오.
        // 왼쪽 피연산자를 평가하는 데 필요한 것은 무엇이든 남겨두고
        // 스택에 있는 답입니다. 그런 다음 동일한 작업을 수행합니다.
        // 두 번째 피연산자. 그런 다음 연산자를 적용합니다(즉, 터지는 것을 의미함).
        // 피연산자, 연산자 적용 및 결과 푸시).
        left.printStackCommands();
        right.printStackCommands();
        System.out.println("  Operator " + op);
    }
}
```

새로운 구문 분석 서브루틴을 살펴보는 것도 흥미롭다. 값을 계산하는 대신 각 서브 루틴은 표현식 트리를 작성한다. 

```java
static ExpNode expressionTree() throws ParseError {
        // 현재 입력 줄에서 표현식을 읽고
        // 표현식을 나타내는 표현식 트리를 반환합니다.
        // (반환 값은 트리의 루트에 대한 포인터입니다.)
    TextIO.skipBlanks();
    boolean negative;  // True if there is a leading minus sign.
    negative = false;
    if (TextIO.peek() == '-') {
        TextIO.getAnyChar();
        negative = true;
    }
    ExpNode exp;   // The expression tree for the expression.
    exp = termTree();  // Start with a tree for first term.
    if (negative) {
        // 적용에 해당하는 트리를 구축합니다.
        // 우리가 입력한 용어에 대한 단항 빼기 연산자
        // 그냥 읽으세요.
        exp = new UnaryMinusNode(exp);
    }
    TextIO.skipBlanks();
    while ( TextIO.peek() == '+' || TextIO.peek() == '-' ) {
        // 다음 용어를 읽고 다음 용어와 결합합니다.
        // 이전 항을 더 큰 표현식 트리로 바꿉니다.
        char op = TextIO.getAnyChar();
        ExpNode nextTerm = termTree();
        // 이항 연산자를 적용하는 트리를 생성합니다.
        // 이전 트리와 방금 읽은 용어로 이동합니다.
        exp = new BinOpNode(op, exp, nextTerm);
        TextIO.skipBlanks();
    }
    return exp;
}
```

일부 실제 컴파일러에서는 파서가 파싱되는 프로그램을 나타낸느 트리를 생성한다. 이 트리를 **구문 분석 트리(parse tree)** 혹은 **추상 구문 트리(abstract syntax tree)** 라고 한다. 구분 분석 트리는 표현식 트리와 형태가 약간 다르지만 목적은 동일하다. 일단 나무를 갖게 되면 그것으로 할 수 있는 일이 많이 있다. 우선 기계어 코드를 생성하는 데 사용될 수 있다. 그러나 트리를 검사하고 값이 할당되기 전에 지역 변수를 참조하려는 시도와 같은 특저어 유형의 프로그래밍 오류를 감지하는 기술도 있다. 최적화를 위해 트리를 조작하는 것도 가능하다. 최적화에서는 코드가 생성되기 전에 프로그램을 보다 효율적으로 만들기 위해 트리가 변환된다. 

이제 우리는 프로그래밍 언어, 컴파일러, 기계어를 살펴모벼 1장에서 시작한 곳으로 돌아왔다. 하지만 그들을 더 많은 이해와 더 넓은 관점으로 바라볼 수 있기를 바란다.



















