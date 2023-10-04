# 파일을 사용한 프로그래밍

이 섹션은 섹션 11.1과 11.2에 소개된 기술을 사용하여 파일로 작업하는 여러 프로그래밍 예제를 살펴본다.


## 1. 파일 복사

첫 번쨰 예로, 파일 복사본을 만들 수 있는 간단한 명령줄 프로그램을 살펴본다. 파일 복사는 매우 일반적인 작업이며 모든 운영 체제에는 이미 이를 수행하기 위한 명령이 있다. 그러나 동일한 작업을 수행하는 Java 프로그램을 살펴보는 것은 여전히 유익하다. 많은 파일 작업은 입력 파일의 데이터가 출력 파일에 기록되기 전에 어떤 방식으로 처리된다는 점을 제외하면 파일 복사와 유사하다. 이러한 모든 작업은 동일한 일반 형식을 가진 프로그램으 수행할 수 있다. 섹션 4.3.6에는 TextIO를 사용하여 텍스트 파일로 복사하는 프로그램이 포함되어 있다. 

프로그램은 모든 파일을 복사할 수 있어야 하므로 파일의 데이터가 사람이 읽을 수 있는 형식이라고 가정할 수 없다. 따라서 파일에 대한 작업을 수행하려면 바이트 스트림인 InputStream 및 OutputStream으르 사용해야 한다. 이 프로그램은 모든 데이터를 한 번에 한 바이트씩 InputStream에서 OutputStream으로 복사한다. `source`가 InputStream을 참조하는 변수인 경우 `source.read()` 함수를 사용하여 1바이트를 읽을 수 있다. 이 함수는 입력 파일의 모든 바이트를 읽었을 때 -1값을 반환한다. 마찬가지고, `copy`가 OutputStream을 참조하는 경우 `copy.write(b)` 출력 파일에 1바이트를 쓴다. 따라서 프로그램의 핵심은 간단한 `while` 루프이다. 평소와 같이 I/O 작업에서 예외가 발생할 수 있으므로 이 작업은 `try..catch`문에서 수행된다.

```java
while (true) {
    int data = source.read();
    if (data < 0) 
        break;
    copy.write(data);
}
```

UNIX와 같은 운영 체제의 파일 복사 명령은 명령줄 인수를 사용하여 파일 이름을 지정한다. 예를 들어, 사용자는 기존 파일 `original.dat`를 `backup.dat`라는 파일로 복사하기 위해 `copy original.dat backup.dat`라고 말할 수 있다. 명령줄 인수는 Java 프로그램에서도 사용할 수 있다. 명령줄 인수는 `main()`루틴에 대한 매개 변수인 문자열 배열 `args`에 저장된다. 프로그램은 이 배열에서 명령줄 인수를 검색할 수 있다. 예를 들어 프로그램이 `CopyFile`이고 사용자가 다음 명령을 사용하여 프로그램을 실행하는 경우

```console
$ java CopyFile work.dat oldwork.dat
```

그러면 프로그램에서 `args[0]`는 "work.dat" 문자열이 되고 `args[1]`는 "oldwork.dat" 문자열이 된다. `args.length` 값은 사용자가 지정한 명령줄 인수 수를 프로그램에 알려준다.

[CopyFile.java](https://math.hws.edu/javanotes/source/chapter11/CopyFile.java) 프로그램은 명령줄 인수에서 파일 이름을 가져온다. 파일 이름이 지정되지 않으면 오류 메시지를 인쇄하고 종료한다. 약간의 관심을 더하기 위해 프로그램을 사용화는 두 가지 방법이 있다. 명령줄에서는 두 개의 파일 이름을 간단히 지정할 수 있다. 이 경우 출력 파일이 이미 존재하면 프로그램은 오류 메시지를 인쇄하고 종료된다. 이는 사용자가 실수로 즁요한 파일을 덮어쓰지 않도록 하기 위한 것이다. 그러나 명령줄에 세 개의 인수가 있는 경우 첫 번째 인수는 `-f`여야 하고 두 번째 및 세 번째 인수는 파일 이름이어야 한다. `-f`는 프로그램의 동작을 수정하기 위한 **명령줄 옵션(command-line option)** 이다. 프로그램은 `-f`를 해석한다. 이는 기존 프로그램을 덮어써도 괜찮다는 의미이다. ("f"는 "force"를 의미) 소스코드에서 명령줄 인수가 어떻게 해석되는지 확인할 수 있다.

```java
import java.io.*;

/**
 * 파일의 복사본을 만듭니다. 원본 파일과 파일 이름
 * 사본은 명령줄 인수로 제공되어야 합니다. 또한,
 * 첫 번째 명령줄 인수는 "-f"일 수 있습니다. 존재하는 경우, 프로그램
 *는 기존 파일을 덮어씁니다. 그렇지 않은 경우 프로그램에서 보고합니다.
 * 출력 파일이 이미 존재하는 경우 오류가 발생하고 종료됩니다. 수
 * 복사된 바이트 수가 보고됩니다.
 */
public class CopyFile {

    public static void main(String[] args) {
        
        String sourceName;   // 소스 파일 이름,
                            // 명령줄에 지정된 대로입니다.
        String copyName;     // 복사본 이름,
                            // 명령줄에 지정된 대로입니다.
        InputStream source;  // 소스 파일에서 읽기 위한 스트림입니다.
        OutputStream copy;   // 복사본을 쓰기 위한 스트림입니다.
        boolean force;  // "-f" 옵션이 있으면 true로 설정됩니다.
                        // 명령줄에 지정됩니다.
        int byteCount;   // 소스 파일에서 복사된 바이트 수입니다.
      
      
        /* 명령줄에서 파일 이름을 가져와서 확인합니다.
         -f 옵션이 있습니다. 명령줄이 하나가 아닌 경우
         두 가지 가능한 법적 형식 중 오류 메시지를 인쇄하고
         이 프로그램을 종료해 주세요. */
        
        if (args.length == 3 && args[0].equalsIgnoreCase("-f")) {
            sourceName = args[1];
            copyName = args[2];
            force = true;
        } else if (args.length == 2) {
            sourceName = args[0];
            copyName = args[1];
            force = false;
        } else {
            System.out.println(
                    "Usage:  java CopyFile <source-file> <copy-name>");
            System.out.println(
                    "    or  java CopyFile -f <source-file> <copy-name>");
            return;
        }

        /* 입력 스트림을 생성합니다. 오류가 발생하면 프로그램을 종료하세요. */

        try {
            source = new FileInputStream(sourceName);
        } catch (FileNotFoundException e) {
            System.out.println("Can't find file \"" + sourceName + "\".");
            return;
        }
      
        /* 출력 파일이 이미 존재하고 -f 옵션이 없는 경우
         지정하면 오류 메시지를 인쇄하고 프로그램을 종료합니다. */
        File file = new File(copyName);
        if (file.exists() && force == false) {
            System.out.println(
                    "Output file exists.  Use the -f option to replace it.");
            return;
        }

        /* 출력 스트림을 생성합니다. 오류가 발생하면 프로그램을 종료하세요. */

        try {
            copy = new FileOutputStream(copyName);
        } catch (IOException e) {
            System.out.println("Can't open output file \"" + copyName + "\".");
            return;
        }
      
        /* 입력 스트림에서 출력 스트림으로 한 번에 한 바이트씩 복사합니다.
         스트림, read() 메서드가 -1을 반환할 때 종료됩니다(이는
         스트림의 끝에 도달했다는 신호). 만약에 어떠한
         오류가 발생하면 오류 메시지를 인쇄합니다. 다음과 같은 경우에도 메시지를 인쇄합니다.
         파일이 성공적으로 복사되었습니다. */
        
        byteCount = 0;

        try {
            while (true) {
                int data = source.read();
                if (data < 0)
                    break;
                copy.write(data);
                byteCount++;
            }
            source.close();
            copy.close();
            System.out.println("Successfully copied " + byteCount + " bytes.");
        } catch (Exception e) {
            System.out.println("Error occurred while copying.  "
                    + byteCount + " bytes copied.");
            System.out.println("Error: " + e);
        }

    }
    
}
```

실제로 한 번에 한 바이트씩 복사하는 것은 매우 비효율적이다. 여러 바이트를 읽고 쓰는 `read()` 및 `write()` 메서드의 대체 버전을 사용하면 효율성이 향상될 수 있다.(자세한 내용은 API 참조) 또는 입력 및 출력 스트림을 더 큼 블록의 파일에서 자동으로 데이터를 읽고 쓰는 BufferedInputStream 및 BufferedOutputStream 유형의 객체로 래핑할 수 있다. 이를 위해서는 스트림을 생성하는 프로그램에서 두 줄만 변경하면 된다. 

```java
source = new BufferedInputStream(new FileInputStream(sourceName));
```

그러면 버퍼링된 스트림은 버퍼링되지 않은 스트림과 정확히 동일한 방식으로 사용된다.


## 2. 영구 데이터

프로그램이 종료되면 프로그램의 변수와 객체에 저장된 모든 데이터가 사라진다. 대부분의 경우 프로그램을 다시 실행할 때 사용할 수 있도록 해당 데이터 중 일부를 보관해 두는 것이 유용하다. 문제는 프로그램 실행 사이에 데이터를 유지하는 방법이다. 물론 대답이 데이터를 파일에 저장하는 것이다.

사용자가 이름 목록과 관련 전화번호를 추적할 수 있는 "전화번호부" 프로그램을 고려해 보자. 프로그램이 실행될 때마다 사용자가 처음부터 전체 목록을 생성해야 한다면 프로그램은 전혀 의미가 없다. 전화번호부를 지속적인 데이터 모음으로 생각하고 프로그램을 해당 데이터 모음에 대한 인터페이스로 생각하는 것이 더 합리적이다. 이 프로그램을 통해 사용자는 전화번호부엥서 이름을 갖고 새 항목을 추가할 수 있다. 모든 변경 사항은 프로그램이 종료된 후에도 유지되어야 한다.

샘플 프로그램 [PhoneDirectoryFileDemo.java](https://math.hws.edu/javanotes/source/chapter11/PhoneDirectoryFileDemo.java)는 이 아이디어를 매우 간단하게 구현한 것이다. 이는 파일 사용의 예일 뿐이다. 그것이 구현하는 전화번호부는 심각하게 받아들일 필요가 없는 "장난감" 버전이다. 이 프로그램은 사용자의 홈 디렉터리에 `.phone_book_demo`라는 파일에 전화번호부 데이터를 저장한다. 사용자의 홈 디렉터리를 찾기 위해 섹션 11.2.2에서 언급한 `System.getProperty()` 메서드를 사용한다. 프로그램이 시작되면 파일이 이미 존재하는지 확인한다. 파일이 존재하는 경우 이전 프로그램 실행에서 저장된 사용자의 전화번호부가 포함되어 있어야 한다. 이 경우 파일의 데이터를 읽어서 프로그램이 실행되는 동안 전화번호부를 나타내는 `phoneBook`이라는 TreeMap이다. 전화번호부를 파일에 저장하려면 전화번호부의 데이터가 표시되는 방식에 대해 몇 가지 결정을 내려야 한다. 이 예에서는 파일의 각 줄에 이름과 관련 전화번호로 구성된 하나의 항목이 포함되는 간단한 표현을 선택했다. "%" 기호는 이름과 숫자를 구분한다. 프로그램 시작 부분에 있는 다음 코드는 전화번호부 데이터 파일이 존재하고 형식이 올바른 경우 해당 파일을 읽는다.


```java
File userHomeDirectory = new File( System.getProperty("user.home") );
File dataFile = new File( userHomeDirectory, ".phone_book_data" );  
            // 사용자의 홈 디렉터리에 있는 .phone_book_data라는 파일입니다.

if ( ! dataFile.exists() ) {
    System.out.println("No phone book data file found.  A new one");
    System.out.println("will be created, if you add any entries.");
    System.out.println("File name:  " + dataFile.getAbsolutePath());
}
else {
    System.out.println("Reading phone book data...");
    try( Scanner scanner = new Scanner(dataFile) ) {
        while (scanner.hasNextLine()) {     // 하나의 이름/번호 쌍을 포함하는 파일에서 한 줄을 읽습니다.
            String phoneEntry = scanner.nextLine();
            int separatorPosition = phoneEntry.indexOf('%');
            if (separatorPosition == -1)
                throw new IOException("File is not a phonebook data file.");
            name = phoneEntry.substring(0, separatorPosition);
            number = phoneEntry.substring(separatorPosition+1);
            phoneBook.put(name,number);
        }
    }
    catch (IOException e) {
        System.out.println("Error in phone book data file.");
        System.out.println("File name:  " + dataFile.getAbsolutePath());
        System.out.println("This program cannot continue.");
        System.exit(1);
    }
}
```

그런 다음 프로그램을 통해 사용자는 전화번호부 수정을 포함하여 다양한 작업을 수행할 수 있다. 모든 변경 사항은 데이터를 보유하는 TreeMap에만 적용된다. 프로그램이 종료되면 다음 코드를 사용하여 전화번호부 데이터가 파일에 기록된다.

```java
if (changed) {
    System.out.println("Saving phone directory changes to file " +
        dataFile.getAbsolutePath() + " ...");
    PrintWriter out;
    try {
        out = new PrintWriter( new FileWriter(dataFile) );
    }
    catch (IOException e) {
        System.out.println("ERROR: Can't open data file for output.");
        return;
    }
    for ( Map.Entry<String,String> entry : phoneBook.entrySet() )
        out.println(entry.getKey() + "%" + entry.getValue() );
    out.flush();
    out.close();
    if (out.checkError())
        System.out.println("ERROR: Some error occurred while writing data file.");
    else
        System.out.println("Done.");
}
```

이로 인해 다음에 프로그램이 실행될 때 변경 사항을 포함한 모든 데이터가 그대로 유지된다. 


## 3. 파일에 객체 저장

데이터가 파일에 저장될 때마다 데이터를 표현하기 위해 일정한 형식을 채택해야 한다. 데이터를 쓰는 출력 루틴과 데이터를 읽는 입력 루틴이 동일한 형식을 사용하는 한 파일을 사용할 수 있다. 그러나 늘 그렇듯이 정확성이 이야기의 끝은 아니다. 파일의 데이터에 사용되는 표현도 견고해야 한다. 이것이 무엇을 의미하는지 알아보기 위해 동일한 데이터를 표현하는 여러 가지 다른 방법을 살펴본다. 이 예제는 섹션 7.3.3의 [SimplePaint2.java](https://math.hws.edu/javanotes/source/chapter7/SimplePaint2.java)를 기반으로 한다. 해당 프로그램에서 사용자는 마우스를 사용하여 간단한 스케치를 그릴 수 있다. 이제 해당 프로그램에 파일 입력/출력 기능을 추가한다. 이를 통해 사용자는 스케치를 파일에 저장하고 나중에 파일에서 프로그램으로 스케치를 다시 읽어 사용자가 스케치 작업을 계속할 수 있다. 기본 요구 사항은 스케치에 대한 모든 관련 데이터를 파일에 저장해야 프로그램에서 파일을 읽을 때 스케치를 정확하기 복원할 수 있다는 것이다.

프로그램의 새 버전 소스 코드는 [SimplePaintWithFiles.java](https://math.hws.edu/javanotes/source/chapter11/SimplePaintWithFiles.java)에서 찾을 수 있다. 새 버전에는 "File" 메뉴가 추가되었다. 프로그램 데이터를 파일에 쓰고 저장된 데이터를 프로그램으로 다시 읽기 위한 "저장" 및 "열기" 명령을 구현한다.

스케치를 위한 데이터는 그림의 배경색과 사용자가 그린 곡선의 목록으로 구성된다. 곡선은 Point2D 목록으로 구성된다. Point2D `pt`에는 xy 평면에 있는 점의 좌표를 double 유형의 값으로 바ㅏㄴ환하는 인스턴스 메서드 `pt.getX()` 및 `pt.getY()`가 있다. 각 곡선은 서로 다른 색상을 가질 수 있다. 또한 곡선은 "대칭"일 수 있다. 즉, 곡선 자체 외에도 곡선의 수평 및 수직 반사도 그려진다. 각 곡선의 데이터는 프로그램에서 다음과 같이 정의된 CurveData 유형의 객체에 저장된다.

```java
/**
 * CurveData 유형의 객체는 곡선을 다시 그리는 데 필요한 데이터를 나타냅니다.
 * 사용자가 스케치한 곡선.
 */
private static class CurveData {
    Color color;
    boolean symmetric;
    ArrayList<Point2D> points;
}
```

그런 다음 ArrayList<CurveData> 유형의 목록을 사용하여 사용자가 그린 모든 곡선에 대한 데이터를 보유한다.

스케치 데이터를 텍스트 파일로 저장하는 방법을 생각해 보자. 기본 아이디어는 스케치를 재구성하는 데 필요한 모든 데이터를 특정 형식으로 출력 파일에 저장해야 한다는 것이다. 파일을 읽는 방법은 데이터를 읽는 것과 정확히 동일한 형식을 따라야 하며 프로그램이 실행되는 동안 데이터를 사용하여 스케치를 나타내는 데이터 구조를 다시 작성해야 한다.

문자 데이터를 작성할 때 모든 데이터는 궁극적으로 문자열, 원시형 값 등의 단순한 데이터 값으로 표현되어야 한다. 예를 들어, 색상은 색상의 빨간색, 녹색, 파란색 구성 요소를 제공하는 세 가지 숫자로 표현될 수 있다. 가장 먼저 떠오르는 아이디어는 필요한 모든 데이터를 정해진 순서에 따라 파일에 덤프하는 것일 수 있다. `out`은 이 파일에 쓰는 데 사용되는 PrintWriter라고 가정한다.

```java
out.println( backgroundColor.getRed() ); // 파일에 배경색을 쓴다.
out.println( backgroundColor.getGreen() );
out.println( backgroundColor.getBlue() );

out.println( curves.size() );       // 곡선 수를 쓴다.
   
for ( CurveData curve : curves ) {      // 각 곡선에 대해 다음을 쓴다
    out.println( curve.color.getRed() );      // 곡선의 색상
    out.println( curve.color.getGreen() );   
    out.println( curve.color.getBlue() );
    out.println( curve.symmetric ? 0 : 1 );   // 곡선의 대칭 속성
    out.println( curve.points.size() );       // 곡선의 점 수
    for ( Point2D pt : curve.points ) {       // 각 점의 좌표
        out.println( pt.getX() );
        out.println( pt.getY() );
    }
}
```

이는 파일 읽기 방법이 데이터를 읽고 데이터 구조를 재구성할 수 있다는 의미에서 작동한다. 입력 방법이 `scanner`를 사용하여 파일을 읽는다고 가정한다. 

```java
Color newBackgroundColor;                // 배경색을 읽습니다.
double red = scanner.nextDouble();
double green = scanner.nextDouble();
double blue = scanner.nextDouble();
newBackgroundColor = Color.color(red,green,blue);

ArrayList<CurveData> newCurves = new ArrayList<>();

int curveCount = scanner.nextInt();      // 읽어올 곡선의 개수.
for (int i = 0; i < curveCount; i++) {
    CurveData curve = new CurveData();
    double r = scanner.nextDouble();            // 곡선의 색상을 읽습니다.
    double g = scanner.nextDouble();
    double b = scanner.nextDouble();
    curve.color = Color.color(r,g,b);
    int symmetryCode = scanner.nextInt();   //  곡선의 대칭 속성을 읽습니다.
    curve.symmetric = (symmetryCode == 1);
    curveData.points = new ArrayList<>();
    int pointCount = scanner.nextInt();  // 이 곡선의 점 수입니다.
    for (int j = 0; j < pointCount; j++) {
        int x = scanner.nextDouble();        // 지점의 좌표를 읽습니다.
        int y = scanner.nextDouble();
        curveData.points.add(new Point2D(x,y));
    }
    newCurves.add(curve);
}

curves = newCurves;                     // 새로운 데이터 구조를 설치합니다.
backgroundColor = newBackgroundColor;
```

출력 방법으로 작성된 모든 데이터 조각이 입력 방법에서 동일한 순서로 어떻게 읽혀지는지 확인하세요. 이것이 작동하는 동안 데이터 파일은 단지 긴 숫자열일 뿐이다. 이진 형식 파일보다 독자에게는 어 의미가 없다. 더욱이 곡선에 새 속성을 추가하는 등 프로그램의 데이터 표현에 작은 변경이 발생하면 데이터 파일을 쓸모 없게 만든다는 점에서 여전히 취약하다.

그래서 프로그램ㅁ에서 생성된 텍스트 파일에 대해 더 복잡하고 의미 있는 데이터 형식을 사용하기로 결정했다. 숫자만 쓰는 대신 숫자가 의미하는 바를 설명하는 단어를 추가한다. 다음은 프로그램에 대한 짧지만 완전한 데이터 파일이다. 보기만 해도 무슨 일이 일어나고 있는지 알 수 있을 것이다.

```text
SimplePaintWithFiles 1.0
background 0.4 0.4 0.5

startcurve
  color 1 1 1
  symmetry true
  coords 10 10
  coords 200 250
  coords 300 10
endcurve

startcurve
  color 0 1 1
  symmetry false
  coords 10 400
  coords 590 400
endcurve

SimplePaintWithFiles 1.0
배경 0.4 0.4 0.5

시작곡선
  색상 1 1 1
  대칭 참
  코디 10 10
  코디 200 250
  코디 300 10
끝곡선

시작곡선
  색상 0 1 1
  대칭 거짓
  좌표 10400
  좌표 590400
끝곡선
```

파일의 첫 번째 줄은 데이터 파일을 생성한 프로그램을 식별한다. 사용자가 열려는 파일을 선택하면 프로그램은 파일 유형이 올바른지 확인하기 위한 간단한 테스트로 파일의 첫 번째 단어를 확인할 수 있다. 첫 번째 줄에는 버전 번호 1.0도 포함되어 있다. 프로그램의 최신 버전에서 파일 형식이 변경되면 더 높은 버전 번호가 사용된다. 프로그램이 파일에서 버전 번호 1.2를 확인하지만 프로그램이 버전 1.0만 인식하는 경우 프로그램은 데이터 파일을 읽으려면 최신 버전의 프로그램이 필요함을 사용자에게 설명할 수 있다.

파일의 두 번째 줄은 그림의 배경색을 지정한다. 세 숫자는 색상의 빨간색, 녹색, 파란색 구성 요소를 지정한다. 줄 시작 부분에 "배경"이라는 단어가 있어 의미가 명확해진다. 파일의 나머지 부분은 그림에 나타내는 곡선에 대한 데이터로 구성된다. 각 곡선의 데이터에는 "startcurve" 및 "endcurve"가 명확하게 표시되어 있다. 데이터는 곡선의 색상 및 대칭 속성과 곡선의 각 점의 x, y 좌표로 구성된다. 이번에도 의미는 분명하다. 이 형식의 파일은 손으로 쉽게 생성하거나 편집할 수 있다. 실제로 위에 표시된 데이터 파일은 실제로 프로그램이 아닌 텍스트 편집기에서 생성되었다. 또한 추가 옵션을 허용하도록 형식을 확장하는 것도 쉽다. 프로그램의 향후 버전에서는 곡선에 "두께" 속성을 추가하여 선 너비가 다른 곡선을 가질 수 있게 할 수 있다. 직사각형, 타원형 등의 모양을 쉽게 추가할 수 있다.

이 형식으로 데이터를 출력하는 것은 쉽다. `out`이 PrintWriter라고 가정한다.

```java
out.println("SimplePaintWithFiles 1.0"); // Version number.
out.println( "background " + backgroundColor.getRed() + " " +
        backgroundColor.getGreen() + " " + backgroundColor.getBlue() );
for ( CurveData curve : curves ) {
    out.println();
    out.println("startcurve");
    out.println("  color " + curve.color.getRed() + " " +
            curve.color.getGreen() + " " + curve.color.getBlue() );
    out.println( "  symmetry " + curve.symmetric );
    for ( Point2D pt : curve.points )
        out.println( "  coords " + pt.getX() + " " + pt.getY() );
    out.println("endcurve");
}
```

이 코드는 섹션 11.2.3에 제시된 것과 유사한 `doSave()` 메서드에서 사용된다. 

입력 루틴이 데이터의 모든 추가 단어를 처리해야 하기 때문에 데이터를 읽는 것은 다소 어렵다. 내 입력 루틴에서는 파일에서 데이터가 발생하는 순서에 약간의 변형을 허용하기로 결정했다. 예를 들어 배경색을 파일 시작 부분이 아닌 끝 부분에 지정할 수 있다. 완전히 생략할 수도 있으며, 이 경우 흰색이 기본 배경색으로 사용된다. 이는 각 데이터 항목에 해당 의미를 설명하는 단어가 표시되어 있기 때문에 가능하다. 레이블은 입력 처리를 구동하는 데 사용될 수 있다. 다음은 `doSave()` 메서드에 의해 생성된 데이터 파일을 읽는 메서드이다.

```java
private void doOpen() {
    FileChooser fileDialog = new FileChooser();
    fileDialog.setTitle("Select File to be Opened");
    fileDialog.setInitialFileName(null);  // No file is initially selected.
    if (editFile == null)
        fileDialog.setInitialDirectory(new File(System.getProperty("user.home")));
    else
        fileDialog.setInitialDirectory(editFile.getParentFile());
    File selectedFile = fileDialog.showOpenDialog(window);
    if (selectedFile == null)
        return;  // User canceled.
    Scanner scanner;
    try {
        scanner = new Scanner( selectedFile );
    }
    catch (Exception e) {
        Alert errorAlert = new Alert(Alert.AlertType.ERROR,
                "Sorry, but an error occurred\nwhile trying to open the file.");
        errorAlert.showAndWait();
        return;
    }
    try {
        String programName = scanner.next();
        if ( ! programName.equals("SimplePaintWithFiles") )
            throw new IOException("File is not a SimplePaintWithFiles data file.");
        double version = scanner.nextDouble();
        if (version > 1.0)
            throw new IOException("File requires a newer version of SimplePaintWithFiles.");
        Color newBackgroundColor = Color.WHITE;
        ArrayList<CurveData> newCurves = new ArrayList<CurveData>();
        while (scanner.hasNext()) {
            String itemName = scanner.next();
            if (itemName.equalsIgnoreCase("background")) {
                double red = scanner.nextDouble();
                double green = scanner.nextDouble();
                double blue = scanner.nextDouble();
                newBackgroundColor = Color.color(red,green,blue);
            }
            else if (itemName.equalsIgnoreCase("startcurve")) {
                CurveData curve = new CurveData();
                curve.color = Color.BLACK;
                curve.symmetric = false;
                curve.points = new ArrayList<Point2D>();
                itemName = scanner.next();
                while ( ! itemName.equalsIgnoreCase("endcurve") ) {
                    if (itemName.equalsIgnoreCase("color")) {
                        double r = scanner.nextDouble();
                        double g = scanner.nextDouble();
                        double b = scanner.nextDouble();
                        curve.color = Color.color(r,g,b);
                    }
                    else if (itemName.equalsIgnoreCase("symmetry")) {
                        curve.symmetric = scanner.nextBoolean();
                    }
                    else if (itemName.equalsIgnoreCase("coords")) {
                        double x = scanner.nextDouble();
                        double y = scanner.nextDouble();
                        curve.points.add( new Point2D(x,y) );
                    }
                    else {
                        throw new Exception("Unknown term in input.");
                    }
                    itemName = scanner.next();
                }
                newCurves.add(curve);
            }
            else {
                throw new Exception("Unknown term in input.");
            }
        }
        scanner.close();
        backgroundColor = newBackgroundColor;
        curves = newCurves;
        redraw();
        editFile = selectedFile;
        window.setTitle("SimplePaint: " + editFile.getName());
    }
    catch (Exception e) {
        Alert errorAlert = new Alert(Alert.AlertType.ERROR,
                "Sorry, but an error occurred while\ntrying to read the data:\n"
                        + e);
        errorAlert.showAndWait();
    }
}
```

파일 형식에 대해 이렇게 오랫동안 논의한 주된 이유는 파일에 데이터를 저장하는 데 적합한 형식으로 복잡한 데이터를 표현히는 문제에 대해 생각하게 하기 위한 것이다. 데이터를 네트워크를 통해 전송해야 하는 경우에도 동일한 문제가 발생한다. 문제에 대한 올바른 해결책은 없지만 일부 솔루션은 확실히 다른 솔루션보다 낫다. 

---

`SimplePaintWithFiles` 스케치 데이터를 텍스트 형식으로 저장할 수 있을 뿐만 아니라 그림 자체를 인쇄하거나 웹 페이지에 게시할 수 있는 이미지 파일로 저장할 수도 있다. 

