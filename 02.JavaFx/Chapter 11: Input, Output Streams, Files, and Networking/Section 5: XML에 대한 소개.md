# XML에 대한 간략한 소개

데이터가 파일에 저장되거나 네트워크를 통해 전송되는 경우 나중에 파일을 읽거나 전송을 수신할 때 동일한 데이터를 다시 작성할 수 있는 방식으로 표시되어야 한다. 우리는 많은 경우에 텍스트 기반의 표현을 선호하는 타당한 이유가 있다는 것을 확인했지만, 주어진 데이터 모음을 텍스트로 표현하는 방법도 많다. 이 섹션에서는 점점 일반화되고 있는 문자 기반 데이터 표현의 한 유형을 간략하게 살펴본다.

XML(eXtensible Markup Language)은 데이터 표현 언어를 생성하기 위한 구문이다. XML에는 두 가지 측면 또는 수준이 있다. 첫 번째 수준은 XML은 엄격하지만 비교적 간단한 구문을 지정한다. 해당 구문을 따르는 모든 문자 시퀀스는 **올바른 형식(well-formed)** 의 XML 문서이다. 두 번째 수준에서 XML의 문서에 나타날 수 있는 내용에 추가 제한을 두는 방법을 제공한다. 이는 **DTD(Document Type Definition)** 를 XML 문서와 연결하여 수행된다. DTD는 기본적으로 XML 문서에 표시되도록 혀용된 항목의 목록이다. 관련 DTD가 있고 DTD 규칙을 따르는 잘 구성된 XML 문서를 유요한 XML 문서라고 한다. XML은 데이터 표현을 위한 일반적인 형식이고 DTD는 XML을 사용하여 특정 종류의 데이터를 표현하는 방법을 지정한다는 개념이다. (유효한 XML 문서를 정의하기 위해 XML 스키마와 같은 DTD에 대한 대안도 있지만 여기선 무시한다.)

XML에는 마법 같은 것이 없다. 확실히 완벽하지는 않다. 그것은 매우 장황한 언어이고 어떤 사람들은 그것이 추악하다고 생각하다. 반면에 매우 유연하다. 거의 모든 유형의 데이터를 나타나는 데 사용할 수 있다. 처음부터 모든 언어와 알파벳을 지원하도록 제작되었다. 가장 중요한 점이 이것이 표준으로 받아들여졌다는 점이다. XML 문서 처리를 위한 거의 모든 프로그래밍 언어가 지원된다. 다양항 종류의 데이터를 설명하기 위한 표준 DTD가 있다. 데이터 표현 언어를 설계하는 방법에는 여러 가지가 있지만 XML은 우연히 널리 사용되는 언어 중 하나이다. 실제로 이는 정보 기술의 거의 모든 분야에 적용되었다. 예를 들면 다음과 같다. 수학 표현(MathXML), 음악 표기(MusicXML), 분자 및 화학 반응(CML), 벡터 그래픽(SVG) 및 기타 다양한 종류의 정보를 제공한다. XML은 OpenOffice와 최신 버전의 Microsoft Office에서 워드, 스프레드 시트, 프리젠테이션과 같은 Office 응용 프로그램용 문서 형식으로 사용된다. XML 사이트 신디케이션 언어(RSS, ATOM)를 사용하면 웹 사이트, 신문 및 블로그에서 다른 웹 사이트 및 웹 브라우저에서 사용할 수 있는 표준 형식으로 최신 헤드라인 목록을 만들 수 있다. 팟캐스트를 게시하는 데에도 동일한 형식이 사용된다. 그리고 XML은 비즈니스 정보의 전자 교환을 위한 일반적인 형식이다.

여기서 목적은 XML에 대해 알아야 할 모든 것을 알려주는 것이 아니다. 단지 그것이 여러분의 프로그램에서 사용될 수 있는 몇 가지 방법을 설명할 것이다. DTD와 유효한 XML에 대해서는 더 언급하지 않는다.

## 1. 기본 XML 구문

웹 페이지 작성 언어인 HTML을 알고 있다면 XML이 친숙해 보일 것이다. XML 문서는 HTML 문서와 매우 유사하다. HTML은 엄격한 XML 구문 규칙을 모두 따르지 않기 때문에 그 자체로는 XML 언어가 아니지만 기본 아이디어는 유사하다.

```xml
<?xml version="1.0"?>
<simplepaint version="1.0">
   <background red='1' green='0.6' blue='0.2'/>
   <curve>
      <color red='0' green='0' blue='1'/>
      <symmetric>false</symmetric>
      <point x='83' y='96'/>
      <point x='116' y='149'/>
      <point x='159' y='215'/>
      <point x='216' y='294'/>
      <point x='264' y='359'/>
      <point x='309' y='418'/>
      <point x='371' y='499'/>
      <point x='400' y='543'/>
   </curve>
   <curve>
      <color red='1' green='1' blue='1'/>
      <symmetric>true</symmetric>
      <point x='54' y='305'/>
      <point x='79' y='289'/>
      <point x='128' y='262'/>
      <point x='190' y='236'/>
      <point x='253' y='209'/>
      <point x='341' y='158'/>
   </curve>
</simplepaint>
```

선택사항인 첫 번째 줄은 이를 XML 문서로 식별할 뿐이다. 이 줄은 문서를 이진 형식으로 인코딩 하는 데 사용된 문자 인코딩과 같은 기타 정보도 지정할 수 있다. 이 문서에 관련 DTD가 있는 경우 파일의 다음 줄에 있는 "DOCTYPE" 지시문에 지정된다.

첫 번째 줄을 제외하고 문서는 요소, 속성 및 텍스트 내용으로 구성된다. 요소는 `<curve>`와 같은 **태그(tag)** 로 시작하고 `</curve>`와 같은 일치하는 **종료 태그(end-tag)** 로 끝난다. 태그와 종료 태그 사이에는 텍스트와 중첩 요소로 구성될 수 있는 요소의 **컨텐츠(content)** 가 있다. 요소에 내용이 없으면 여는 태그와 끝 태그를 `<point x='79' y='289'/>`와 같이 마지막 ">" 앞에 "/"를 둘 수 있다. `<point x='83' y='96'></point>`의 축양이다. 태그에는 `<point x='79' y='289'/>`의 `x`, `y` 또는 `<simplepaint version="1.0">`의 `version`과 같은 속성이 포함될 수 있다. 문서에는 주석과 같은 몇 가지 다른 항목도 포함될 수 있지만 여기서는 다루지 않는다. 

잘 구성된 XML 문서의 작성자는 태그 이름과 속성 이름을 선택하게 되며, 독자에게 데이터를 설명하기 위해 의미 있는 이름을 선택할 수 있다. (DTD를 사용하는 유효한 XML 문서의 경우 태그 이름을 선택하는 사람은 DTD 작성자이다.)

모든 올바른 형식의 XML 문서는 엄격한 구문을 따른다. 다음은 가장 중요한 구문 규칙 중 일부이다. XML의 태그 이름과 속성 이름은 대소문자를 구분한다. 이름은 문자로 시작해야 하며 문자, 숫자 및 기타 특정 문자를 포함할 수 있다. 공백과 줄 끝은 텍스트 내용에서만 중요하다. 모든 태그는 빈 태그이거나 일치하는 종료 태그가 있어야 한다. 여기서 "일치"란 요소가 적절하게 중첩되어야 함을 의미한다. 태그가 일부 요소안에 있으면 일치하는 종료 태그도 해당 요소 안에 있어야 한다. 문서에는 다른 모든 요소가 포함된 **루트 요소(root element)** 가 있어야 한다. 위 예의 루트 요소는 `simplepaint`가 있다. 모든 속성에는 값이 있어야 하며 해당 값은 따옴표로 묶어야 한다. 이를 위해 작은 따옴표는 큰 따옴표를 사용할 수 있다. 특수문자 `<`, `&`는 속성 값 또는 텍스트 내용에 나타나는 경우 `&lt;` `&amp;`로 표기해야 한다. `&lt;` `&amp;`는 객체의 예이다. 객체 `&gt;`, `&quot;`, `&apos;`도 정의되어 각각 `>`, `"`, `'`를 나타낸다.

이 설명을 통해 XML 문서에서 접할 수 있는 모든 내용을 이해할 수는 없지만 Java 프로그램에서 사용되는 데이터 구조를 나타내는 올바른 형식의 XML 문서를 디자인할 수 있다.

## 2. DOM 작업

위에 표시된 샘플 XML 파일은 사용자가 만든 간한단 그림에 대한 정보를 저장하도록 설계되었다. 섹션 7.3.3의 샘플 프로그램 [SimplePaint2.java](https://math.hws.edu/javanotes/source/chapter7/SimplePaint2.java)를 사용하여 만들 수 있는 그림이다. 데이터 파일의 XML 형식을 사용하여 사용자의 그림을 저장할 수 있는 해당 프로그램의 다른 버전을 살펴본다. 새 버전은 [SimplePaintWithXML.java](https://math.hws.edu/javanotes/source/chapter11/SimplePaintWithXML.java)이다. 이 섹션 앞에 표시된 샘플 XML 문서를 해당 프로그램과 함께 사용할 수 있다. SimplePaint2에서 그림을 재구성하는 데 필요한 모든 데이터를 나타내기 위해 해당 문서의 형식을 디자인했다. 문서는 그림의 배경색과 곡선 목록을 인코딩한다. 각 `<curve>` 요소에는 CurveData 유형의 한 객체에서 가져온 데이터가 포함되어 있다. 

모든 구문 규칙을 따라야 하기는 하지만 사용자 정의된 XML 형식으로 데이터를 작성하는 것은 충분히 쉽다. 데이터를 PrintWriter `out`에 쓰는 방법은 다음과 같다.

```java
out.println("<?xml version=\"1.0\"?>");
out.println("<simplepaint version=\"1.0\">");
out.println("   <background red='" + backgroundColor.getRed() + "' green='" +
        backgroundColor.getGreen() + "' blue='" + backgroundColor.getBlue() + "'/>");
for (CurveData c : curves) {
    out.println("   <curve>");
    out.println("      <color red='" + c.color.getRed() + "' green='" +
            c.color.getGreen() + "' blue='" + c.color.getBlue() + "'/>");
    out.println("      <symmetric>" + c.symmetric + "</symmetric>");
    for (Point2D pt : c.points)
        out.println("      <point x='" + pt.getX() + "' y='" + pt.getY() + "'/>");
    out.println("   </curve>");
}
out.println("</simplepaint>");
```

데이터를 프로그램으로 다시 읽는 것은 또 다른 문제이다. XML 문서가 나타내는 데이터 구조를 재구성하려면 문서를 구문 분석하고 문서에서 데이터를 추출해야 한다. 이 작업은 손으로 수행하기 어려울 수 있다. 다행스럽게도 Java에는 XML문서를 구분 분석하고 처리하기 위한 표준 API가 있다. (실제로는 2개가 있지만 그 중 하나만 살펴본다.)

올바른 형식의 XML 문서는 속성, 중첩 요소 및 텍스트 콘텐츠를 포함하는 요소로 구성된 특정 구조를 갖는다. 문서의 구조와 내용에 해당하는 데이터 구조를 컴퓨터 메모리에 구축하는 것이 가능하다. 물론 이를 수행하는 여러 방법은 여러 가지가 있지만 **문서 객체 모델(Document Object Model)** 이라는 공통 표준 표현이 하나 있다. DOM은 XML 문서를 표현하기 위해 데이터 구조를 구축하는 방법을 지정하고 해당 구조의 데이터에 엑세스하기 위한 몇 가지 표준 방법을 지정한다. 데이터 구조는 문서의 구조를 반영하는 일종의 트리이다. 트리는 노드로 구성된다. 다양한 유형의 요소는 있다. 속성 및 텍스트를 나타내는 노드가 있다. 속성과 텍스트는 해당 노드를 직접 조작하지 않고도 처리할 수 있으므로 거의 전적으로 요소 노드에 관심을 갖는다. 

(샘플 프로그램 [XMLDemo.java](https://math.hws.edu/javanotes/source/chapter11/XMLDemo.java)를 사용하면 XML과 DOM을 실험할 수 있다.)

Java에서든 단 두개의 문으로 XML 문서 파일의 DOM 표현을 만들 수 있다. `selectedFile`은 XML 파일을 나타내는 File 유형의 변수이다.

```java
DocumentBuilder docReader 
                 = DocumentBuilderFactory.newInstance().newDocumentBuilder();
xmldoc = docReader.parse(selectedFile);
```

파일을 열고, 내용을 읽고, DOM 표현을 빌드한다. DocumentBuilder 및 DocumentBuilderFactory 클래스는 모두 `javax.xml.parsers` 패키지에 정의되어 있다. `docReader.parse()` 메서드가 실제 작업을 수행한다. 파일을 읽을 수 없거나 파일에 합법적인 XML 문서가 포함되어 있지 않으면 예외가 발생한다. 성공하면 `docReader.parse()`가 반환하는 값은 전체 XML 문서를 나타내는 객체가 된다. (이것은 매우 복잡한 작업이다. 모든 Java 프로그램에서 매우 쉽게 사용할 수 있는 메서드로 단번에 코딩되었다.)

DOM 데이터 구조의 구조는 `org.w3c.dom` 패키지에 정의되어 있다. 이 패키지에는 XML 문서 전체와 문서의 개별 노드를 나타내는 여러 데이터 유형이 포함되어 있다. 이름의 "org.w3c"는 웹 표준 조작인 World Wide Web Consortium, [W3C](https://www.w3.org/)를 나타낸다. DOM은 XML과 마찬가지로 단순히 Java 표준이 아닌 일반 표준이다. 여기에 필요한 데이터 유형은 Document, Node, Element 및 NodeList이다. 이러한 데이터 유형에 정의된 메서드를 사용하여 XML 문서의 DOM 표현에 있는 데이터에 엑세스할 수 있다.

Document 유형의 객체는 전체 XML 문서를 나타낸다. `docReader.parse()`의 반환 값은 Document 유형이다. 이 클래스에서는 단 하나의 메서드만 필요하다.

```java
xmldoc.getDocumentElement();
```

문서의 루트 요소를 나타내는 Element 유형의 값을 반환한다. (이것은 최상위 요소이다.) 이 섹션 앞부분의 샘플 XML 문서에서 루트 요소는 `<simplepaint version="1.0">` 태그, 종료 태그 `</simplepaint>` 및 그 사이의 모든 것이다. 루트 요소 내에 중첨된 요소는 루트 노드의 하위 노드라고 하는 자체 노드로 표시된다. Element 유형의 객체에는 몇 가지 유용한 메서드가 포함되어 있다. 

- `element.getTagName()` : 요소의 태그에 사용되는 이름이 포함된 문자열 반환
- `element.getAttribute(attrName)` : `attrName`이 요소의 속송 이름인 경우 이 메서드는 해당 속성의 값을 반환
- `element.getTextContent()` : 요소에 포함된 모든 텍스트 컨텐츠가 포함된 문자열을 반환
- `element.getChildNodes()` : 요소의 하위의 모든 노드를 포함하는 NodeList 유형의 값을 반환한다. 목록에는 다른 요소를 나타내는 노드와 해당 요소에 직접 중첩된 텍스트 컨텐츠가 포함된다.
- `element.getElementByTagName(tagName)` : `element` 내부에 중첩되어 있고 지정된 태그 이름을 갖는 모든 요소를 나타내는 모든 노드가 포함된 NodeList를 반환한다.


NodeList 유형의 객체는 Nodes 목록을 나타낸다. 불행하게도 Java Collection Framework의 목록에 대해 정의된 API를 사용하지 않는다. 대신 NodeList 유형의 `nodeList` 값에는 두 가지 메서드가 있다. `nodeList.getLength()`는 목록의 노드 수를 반환하고 `nodeList.item(i)`는 위치 번호가 0, 1, ..., `nodeList.getLength() - 1` 위치에 있는 노드를 반환한다. 반환 값은 Node 유형이다. 사용하기 전에 보다 구체적인 노드 유형으로 유형 캐스팅해야 할 수도 있다.

이 정도만 알면 DOM 표현의 가장 일반적인 처리 유형을 수행할 수 있다.

```xml
<background red='1' green='0.6' blue='0.2'/>
```

이 요소는 `getChildNodes()`를 사용하여 문서를 탐색하는 동안 또는 `getElementByTagName("background")` 호출의 결과에ㅔ서 발생할 수 있다. 우리의 목표는 문서가 나타내는 데이터 구조를 재구성하는 것이며, 이 요소는 해당 데이터의 일부를 나타낸다. 이 경우 요소는 색상을 나타내며, 요소의 속성에 따라 빨간색, 녹색, 파란색 구성 요소가 부여된다. 

```java
double r = Double.parseDouble( element.getAttribute("red") );
double g = Double.parseDouble( element.getAttribute("green") );
double b = Double.parseDouble( element.getAttribute("blue") );
Color bgColor = Color.color(r,g,b);
```

이제 `element`가 노드를 참조한다고 가정한다.

```xml
<symmetric>true</symmetric>
```

이 경우 boolean 변수의 값을 나타내며 값은 요소의 텍스트 컨텐츠로 인코딩된다.

```java
String bool = element.getTextContent();
booelan symmetric;
if (bool.equals("true"))
    symmetric = true;
else
    symmetric = false;
```

다음으로 NodeList를 사용하는 예시이다.

```xml
<pointlist>
    <point x='17' y='42'/>
    <point x='23' y='8'/>
    <point x='109' y='342'/>
    <point x='18' y='270'/>
</pointlist>
```

`element`가 `<pointlist>` 노드를 참조한다고 가정한다. 

```java
ArrayList<Point2D> points = new ArrayList<>();
NodeList children = element.getChildNodes();
for (int i = 0; i < children.getLength(); i++) {
    Node child = children.item(i);   // One of the child nodes of element.
    if ( child instanceof Element ) {
        Element pointElement = (Element)child;  // One of the <point> elements.
        double x = Double.parseDouble( pointElement.getAttribute("x") );
        double y = Double.parseDouble( pointElement.getAttribute("y") );
        Point2D pt = new Point2D(x,y); // Create the Point represented by pointElement.
        points.add(pt);  // Add the point to the list of points.
    }
}
```

중첨된 `<point>` 요소는 `<pointlist>`의 하위 요소이다. 요소에 중첩된 요소 외에 다른 자식이 있을 수 있기 때문에 이 코드 조각의 if 문이 필요하다. 이 예제에서는 요소인 자식만 처리하려고 한다.

```java
Color newBackground = Color.WHITE;
ArrayList<CurveData> newCurves = new ArrayList<>();
Element rootElement = xmldoc.getDocumentElement();
if ( ! rootElement.getNodeName().equals("simplepaint") )
    throw new Exception("File is not a SimplePaint file.");
String version = rootElement.getAttribute("version");
try {
    double versionNumber = Double.parseDouble(version);
    if (versionNumber > 1.0)
        throw new Exception("File requires a newer version of SimplePaint.");
}
catch (NumberFormatException e) {
}
NodeList nodes = rootElement.getChildNodes();
for (int i = 0; i < nodes.getLength(); i++) {
    if (nodes.item(i) instanceof Element) {
        Element element = (Element)nodes.item(i);
        if (element.getTagName().equals("background")) {
            double r = Double.parseDouble(element.getAttribute("red"));
            double g = Double.parseDouble(element.getAttribute("green"));
            double b = Double.parseDouble(element.getAttribute("blue"));
            newBackground = Color.color(r,g,b);
        }
        else if (element.getTagName().equals("curve")) {
            CurveData curve = new CurveData();
            curve.color = Color.BLACK;
            curve.points = new ArrayList<>();
            newCurves.add(curve);
            NodeList curveNodes = element.getChildNodes();
            for (int j = 0; j < curveNodes.getLength(); j++) {
                if (curveNodes.item(j) instanceof Element) {
                    Element curveElement = (Element)curveNodes.item(j);
                    if (curveElement.getTagName().equals("color")) {
                        double r = Double.parseDouble(curveElement.getAttribute("red"));
                        double g = Double.parseDouble(curveElement.getAttribute("green"));
                        double b = Double.parseDouble(curveElement.getAttribute("blue"));
                        curve.color = Color.color(r,g,b);
                    }
                    else if (curveElement.getTagName().equals("point")) {
                        double x = Double.parseDouble(curveElement.getAttribute("x"));
                        double y = Double.parseDouble(curveElement.getAttribute("y"));
                        curve.points.add(new Point2D(x,y));
                    }
                    else if (curveElement.getTagName().equals("symmetric")) {
                        String content = curveElement.getTextContent();
                        if (content.equals("true"))
                            curve.symmetric = true;
                    }
                }
            }
        }
    }
}
backgroundColor = newBackground;
curves = newCurves;
```

전체 코드는 [SimplePaintWithXML.java](https://math.hws.edu/javanotes/source/chapter11/SimplePaintWithXML.java)이다. 

---

XML은 매우 중요한 기술로 발전했으며 일부 응용 프로그램은 매우 복잡하다. 하지만 Java에는 쉽게 적용할 수 있는 간단한 아이디어의 핵심이 있다. 기본 사항만 알면 자신의 Java 프로그램에서 XML을 효울적으로 활용할 수 있다.









