# 파일

컴퓨터 메인 메모리에 있는 데이터와 프로그램은 전원이 켜져 있는 동안에만 살아남는다. 보다 영구적인 저장을 위해 컴퓨터는 하드 디스크, USB 메모리, CD-ROM 또는 기타 유형의 저장 장치에 저장된 데이터 모음인 파일을 사용한다. 파일은 디렉터리 (폴더라고도 불린다.)로 구성된다. 디렉터리에는 파일 뿐만 아니라 다른 디렉터리도 포함될 수 있다. 디렉터리와 파일에는 모두 식별하는 데 사용되는 이름이 있다.

프로그램은 기존 파일에서 데이터를 읽을 수 있다. 새 파일을 생성하고 파일에 데이터를 쓸 수 있다. Java에ㅛㅓ는 I/O 스트림을 사용하여 이러한 입력 및 출력을 수행할 수 있다. 사람이 읽을 수 있는 문자 데이터는 Reader의 하위 클래스인 FileReader 클래스에 속하는 객체를 사용하여 파일에서 읽을 수 있다. 마찬가지로 Writer 하위 클래스인 FileWriter 유형의 객체를 통해 사람이 읽을 수 있는 형식으로 파일데 데이터를 쓸 수 있다. 데이터를 머신 형식으로 저장하는 파일의 경우 적절한 I/O 클래스는 FileInputStream 및 FileOutputStream이다. 이 섹션에서는 FileReader 및 FileWriter를 사용한 문자 중심 파일 I/O에 대해서만 설명한다. 그러나 FileInputStream과 FileOutputStream은 정확히 병렬 방식으로 사용된다. 이러한 모든 클래스는 `java.io` 패키지에 정의되어 있다.

## 1. 파일 읽기 및 쓰기

FileReader 클래스에는 파일 이름을 매개 변수로 사용하고 해당 파일에서 읽는 데 사용할 수 있는 입력 스트림을 생성하는 생성자가 있다. 파일이 존재하지 않는 경우 이 생성자는 FileNotFoundException 유형의 예외를 발생시킨다. 예를 들어 `data.txt`라는 파일이 있고 프로그램이 해당 파일에서 데이터를 읽도록 하려고 한다고 가정한다. 파일에 대한 입력 스트림을 생성하려면 다음을 수행할 수 있다.

```java
FileReader data;   //(변수 앞에 변수를 선언합니다.
                    // try 문을 사용하거나 변수를 사용합니다.
                    // try 블록에 로컬이므로 그렇지 않습니다.
                    // 나중에 프로그램에서 사용할 수 있습니다.)
try {
   data = new FileReader("data.txt");  
}
catch (FileNotFoundException e) {
   ... // do something to handle the error—maybe, end the program
}
```

FileNotFoundException 클래스는 IOException의 하위 클래스이므로 위의 `try..catch`문에서 IOException을 포착하는 것이 허용된다. 더 일반적으로 말하면 입력/출력 작업 중에 발생할 수 있는 거의 모든 오류는 IOException을 처리하는 `catch` 절에 포착될 수 있다.

FileReader를 성공적으로 생성하면 여기에서 데이터 읽기를 시작할 수 있다. 그러나 FileReader에는 기본 Reader 클래스에서 상속된 기본 입력 방법만 있으므로 FileReader를 Scanner, BufferedReader 또는 다른 래퍼 클래스로 래핑하고 싶을 수 있다. `data.dat` 라는 파일에서 읽기 위해 BufferedReader를 만들려면 다음과 같이 말할 수 있다.

```java
BufferedReader data;

try {
    data = new BufferedReader( new FileReader("data.dat") );
}
catch (FileNotFoundException e) {
    ... // handle the exception
}
```

BufferedReader에 Reader를 래핑하면 파일에서 텍스트 줄을 쉽게 읽을 수 있으며 버퍼링을 통해 입력을 더 효율적으로 만들 수 있다.

스캐너를 사용하여 파일을 읽으려면 비슷한 방식으로 스캐너를 구성할 수 있다. 그러나 File 유형의 객체에서 더 직접적으로 구성하는 것이 더 일반적이다.

```java
Scanner in;

try {
    in = new Scanner( new File("data.dat") );
}
catch (FileNotFoundException e) {
   ... // handle the exception
}
```

출력 파일 작업은 이보다 더 어렵지 않다. FileWriter 클래스에 속하는 객체를 생성하기만 하면 된다. 아마도 출력 스트림을 PrintWriter 유형의 객체로 래핑하고 싶을 것이다. 예를 들어, `result.dat`라는 파일에 데이터를 쓰려고 한다고 가정한다. FileWriter의 생성자는 IOException 유형의 예외를 발생시킬 수 있으므로 `try..catch` 문을 사용해야 한다.

```java
PrintWriter result;

try {
   result = new PrintWriter(new FileWriter("result.dat"));
}
catch (IOException e) {
   ... // handle the exception
}
```

그러나 스캐너와 마찬가지로 File을 매개 변수로 사용하는 생성자를 사용하는 것이 더 일반적이다. 그러면 PrintWriter를 생성하기 전에 FileWriter에 파일이 자동으로 래핑된다.

```java
PrintWriter result;

try {
   result = new PrintWriter(new File("result.dat"));
}
catch (IOException e) {
   ... // handle the exception
}
```

생성자에 대한 매개변수로 문자열만 사용할 수도 있으며 이는 파일 이름으로 해석된다. (그러나 스캐너 생성자의 문자열은 파일 이름을 지정하지 않으며 대신 스캐너가 문자열에서 문자를 읽는다.)

`result.dat` 라는 파일이 없으면 새 파일이 생성된다. 파일이 이미 존재하는 경우 파일의 현재 내용은 지워지고 프로그램이 파일에 쓰는 데이터로 대체된다. 이 작업은 아무런 경고 없이 수행된다. 이미 존재하는 파일을 덮어쓰는 것을 방지하려면 이 섹션의 뒷 부분에서 설명하는 것 처럼 스트림을 생성하기 전에 동일한 이름의 파일이 이미 존재하는지 확인할 수 있다. 예를 들어 "쓰기 금지"된 디스크에 파일을 생성하려고 하면 PrintWriter 생성자에서 IOException이 발생할 수 있다. 즉, 수정할 수 없다.

PrintWriter 사용이 끝나면 `result.flush()`와 같은 `flush()` 메서드를 호출하여 모든 출력이 대상으로 전송되었는지 확인해야 한다. 이 작업을 잊어버리면 파일 출력 스트림에 작성한 일부 데이터가 실제로 파일에 표시되지 않을 수 있다.

파일 사용을 마친 후에는 파일을 닫고 운영 체제에 파일 사용이 끝났음을 알리는 것이 좋다. 연결된 PrintWriter, BufferedReader, Scanner의 `close()` 메서드를 호출하여 파일을 닫을 수 있다. 파일이 닫히면 새 I/O 스트림으로 다시 열지 않는 한 더 이상 파일에서 데이터를 읽거나 쓸 수 없다. (BufferedReader를 포함한 대부분의 I/O 스트림 클래스의 경우 `close()` 메서드는 checker exception인 IOException을 발생시킬 수 있다. 그러나 PrintWriter 및 Scanner는 예외가 발생하지 않도록 이 메서드를 재정의한다.) 파일을 닫는 것을 잊어버리는 경우 일반적으로 프로그램이 종료되거나 파일 객체가 가비지 수집될 때 파일이 자동으로 닫히지만 이에 의존하지 않는 것이 좋다. `close()`를 호출하면 파일이 닫히기 전에 자동으로 `flush()`가 호출되어야 한다.

완전한 예로, 다음은 `data.dat`라는 파일에서 숫자를 읽은 다음 동일한 숫자를 `result.dat`라는 파일에 역순으로 쓰는 프로그램이다. `data.dat`에는 실수만 포함되어 있다고 가정한다. 입력 파일은 Scanner를 사용하여 읽는다. 예외 처리는 도중에 문제를 확인하는 데 사용된다. 이 응용 프로그램은 특별히 유용한 응용 프로그램은 아니지만, 이 프로그램은 파일 작업의 기본 사항을 보여준다. 

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

/**
 * data.dat라는 파일에서 숫자를 읽고 파일에 씁니다.
 * result.dat를 역순으로 명명했습니다. 입력 파일에는 다음이 포함되어야 합니다.
 * 실수만 가능합니다.
 */
public class ReverseFileWithScanner {

    public static void main(String[] args) {

        Scanner data;        
        PrintWriter result;  

        ArrayList<Double> numbers;  

        numbers = new ArrayList<Double>();

        try {  
            data = new Scanner(new File("data.dat"));
        }
        catch (FileNotFoundException e) {
            System.out.println("Can't find file data.dat!");
            return;  // End the program by returning from main().
        }

        try {  // Create the output stream.
            result = new PrintWriter("result.dat");
        }
        catch (FileNotFoundException e) {
            System.out.println("Can't open file result.dat!");
            System.out.println("Error: " + e);
            data.close();  // Close the input file.
            return;        // End the program.
        }

        while ( data.hasNextDouble() ) {  // Read until end-of-file.
            double inputNumber = data.nextDouble();
            numbers.add( inputNumber );
        }

        // Output the numbers in reverse order.
        for (int i = numbers.size() - 1; i >= 0; i--)
            result.println(numbers.get(i));

        System.out.println("Done!");

        data.close();
        result.close();

    }  

}
```

이 프로그램은 입력에서 숫자가 아닌 다른 내용을 발견하면 파일에서 데이터 읽기를 중지한다. 이는 오류로 간주되지 않는다.

---

섹션 8.3.2의 끝 부분에서 언급했듯이 "리소스"를 생성하거나 열고 이를 사용한 다음 리소스를 닫는 패턴은 매우 일반적인 패턴이며 이 패턴은 `try..catch` 구문으로 지원된다. 이러한 의미에서 파일은 Scanner, PrintWriter 및 모든 Java I/O 스트림과 마찬가지로 리소스이다. 이 모든 것들은 `close()` 메서드를 정의하며, 사용이 끝나면 닫는 것이 좋다. 이들은 모두 AutoCloseable 인터페이스를 구현하므로 `try..catch`문은 다음과 같은 경우 리소스를 자동으로 닫는 데 사용할 수 있다. `try`문이 종료되므로 `finally` 절에서 직접 닫을 필요사 없다. 이는 리소스를 열고 동일한 `try..catch`에서 사용된다고 가정한다.

예를 들어 [ReverseFileWithResources.java](https://math.hws.edu/javanotes/source/chapter11/ReverseFileWithResources.java) 샘플 프로그램은 우리가 살펴본 예의 또 다른 버전이다. 이 경우 리소스 패턴을 사용하는 `try..catch`문을 사용하여 파일에서 데이터를 읽고 파일에 데이터를 쓴다. 내 원래 프로그램은 하나의 `try`문에서 파일을 열고 다른 `try` 문에서 사용했다. 리소스 패턴에서는 이 모든 작업이 한 번의 `try`로 완료되어야 하며, 이를 위해서는 코드으 ㅣ일부 재구성이 필요하다. 

```java
try( Scanner data = new Scanner(new File("data.dat")) ) {
        // Read numbers, adding them to the ArrayList.
    while ( data.hasNextDouble() ) {  // Read until end-of-file.
        double inputNumber = data.nextDouble();
        numbers.add( inputNumber );
    }
}
catch (FileNotFoundException e) {
        // Can be caused if file does not exist or can't be read.
    System.out.println("Can't open input file data.dat!");
    System.out.println("Error: " + e);
    return;  // Return from main(), since an error has occurred.
}
```

리소스 `data`는 첫 번째 줄에 구성된다. 구문에는 "try"라는 단어 뒤의 괄호 안에 초기 값이 있는 리소스 선언이 필요하다. 세미콜론으로 구분된 여러 리소스 선언이 있을 수 있다. 선언된 순서와 반대되는 순서로 종료된다.

## 2. 파일과 디렉터리

파일 이름의 주제는 실제로 지금까지 설명한 것보다 더 복잡하다. 파일을 완전히 지정하려면 파일 이름과 해당 파일이 있는 디렉터리 이름을 모두 제공해야 한다. `data.dat` 또는 `result.dat`와 같은 간단한 파일 이름은 현재 디렉터리 ("기본 디렉터리(default directory)" 또는 "작업 디렉터리(working directory)" 라고도 함)라는 디렉터리의 파일을 참조하는 데 사용된다. 현재 디렉터리는 영구적인 것이 아니다. 사용자나 프로그램에 의해 변경될 수 있다. 현재 디렉터리에 없는 파일은 파일 이름과 해당 파일이 있는 디렉터리에 대한 정보를 모두 포함하는 **경로 이름(path name)** 으로 참조해야 한다. 

문제를 더욱 복잡하게 만드는 것은 **절대 경로 이름(absolute path names)** 과 **상대 경로 이름(relative path names)** 이라는 두 가지 유형의 경로 이름이 있다는 것이다. 절대 경로 이름은 컴퓨터에서 사용할 수 있는 모든 파일 중에서 하나의 파일을 고유하게 식별한다. 여기에는 파일이 어느 디렉터리에 있는지, 파일 이름이 무엇인지에 대한 전체 정보가 포함되어 있다. 상대 경로 이름은 현재 디렉터리에서 시작하여 파일을 찾는 방법을 컴퓨터에 알려준다. 

불행하게도 파일 이름과 경로 이름의 구문은 컴퓨터 유형에 따라 다소 다르다.

- `data.dat` : 모든 컴퓨터에서 현재 디렉터리에 "data.dat" 라는 파일이 있다.
- `/home/eck/java/examples/data.dat` : Linux 및 MacOS X를 포함한 UNIX 운영 체제의 절대 경로 이름이다.
- `C:\eck\java\examples\data.dat` : Windows 컴퓨터의 절대 경로 이름이다.
- `example/data.dat` : UNIX에서는 상대 경로 이름이다. "examples"는 현재 디렉터리에 포함된 디렉터리 이름이고 data.dat은 해당 디렉터이레 있는 파일
- `../examples/data.dat` : UNIX의 상대 경로 이름으로, "현재 디렉터리가 포함된 디렉터리로 이동한 다음 해당 디렉터리 내의 example 이라는 디렉터리로 이동하여 거기서 data.dat 파일을 찾는다". 일반적으로 `".."`는 한 디렉터리 위로 이동을 의미한다.

명령줄에서 작업할 때 간단한 파일 이름만 사용하고 파일이 해당 파일을 사용할 프로그램과 동일한 디렉터리에 저장되어 있다면 괜찮을 것이라고 말하는 것이 안전하다. 이 섹션의 뒷부분에서는 사용자가 GUI 프로그램에서 파일을 지정하도록 하는 편리한 방법을 살펴본다. 이를 통해 경로 이름 문제를 완전히 피할 수 있다.

Java 프로그램은 두 개의 중요한 디렉토리인 현재 디렉터리와 사용자의 홈 디렉터리에 대한 절대 경로 이름을 알아내는 것이 가능하다. 이러한 디렉터리의 이름은 **시스템 속성(system properties)** 이며 함수 호출을 사용하여 읽을 수 있다.

- `System.getProperty("user.dir")` : 현재 디렉터리의 절대 경로 이름을 String으로 반환한다.
- `System.getProperty("user.home")` : 사용자 홈 디렉터리의 절대 경로 이름을 String으로 반환한다.

플랫폼 간의 경로 이름 차이로 인해 발생하는 일부 문제를 방지하기 위해 Java에는 `java.io.File` 클래스가 있다. 이 클래스에 속하는 객체는 실제로 파일을 나타내지 않는다!! 정확하게 말하면 File 유형의 객체는 파일 자체가 아니라 파일 이름을 나타낸다. 이름이 참조하는 파일은 존재할 수도 있고 존재하지 않을 수도 있다. 디렉터리는 파일과 동일한 방식으로 처리되므로 File 객체는 파일을 나타내는 것처럼 쉽게 디렉터리를 나타낼 수 있다.

File 객체에는 경로 이름에서 File 객체를 생허나느 생성자 `new File(String)`가 있다. 이름은 단순 이름, 상대 경로 또는 절대 경로일 수 있다. 예를 들어 `new File("data.dat")`은 현재 디렉터리에 "data.dat" 라는 파일을 참조하는 File 객체를 생성한다. 또 다른 생성자 `new File(File, String)`에는 두 개의 매개 변수가 있다. 첫 번째는 디렉터리를 참조하는 File 객체이다. 두 번째는 해당 디렉터리에 있는 파일 이름이거나 해당 디렉터리에서 파일까지의 상대 경로일 수 있다.

파일 객체에는 몇 가지 유용한 인스턴스 메서드가 포함되어 있다. 

- `file.exists()` : boolean 값 함수는 File 객체가 명명한 파일이 이미 존재하는 경우 true 반환. 새 출력 스트림을 생성할 때 기존 파일의 내용을 덮어쓰지 않으려면 이 방법을 사용할 수 있다. `file.canRead()` 함수는 파일이 존재하고 프로그램에 파일을 읽을 수 있는 권한이 있으면 true를 반환한다.
- `file.isDirectory` : File 객체가 디렉터리를 참조하는 경우 true 반환
- `file.delete()` : 파일이 있으면 삭제한다. 파일이 성공적으로 삭제되었는지 여부를 boolean 값으로 반환
- `file.list()` : File 객체가 디렉터리를 참조하는 경우 이 함수는 해당 디렉터리에 있는 파일 이름이 포함된 String[] 유형의 배열을 반환한다.

```java
import java.io.File;
import java.util.Scanner;

/**
 * 이 프로그램은 다음과 같이 지정된 디렉토리에 있는 파일을 나열합니다.
 * 사용자. 사용자에게 디렉토리 이름을 입력하라는 메시지가 표시됩니다.
 * 사용자가 입력한 이름이 디렉토리가 아닌 경우
 * 메시지가 출력되고 프로그램이 종료됩니다.
 */
public class DirectoryList {
    
    public static void main(String[] args) {
        
        String directoryName;  // Directory name entered by the user.
        File directory;        // File object referring to the directory.
        String[] files;        // Array of file names in the directory.
        Scanner scanner;       // For reading a line of input from the user.

        scanner = new Scanner(System.in);  // scanner reads from standard input.

        System.out.print("Enter a directory name: ");
        directoryName = scanner.nextLine().trim();
        directory = new File(directoryName);

        if (directory.isDirectory() == false) {
            if (directory.exists() == false)
                System.out.println("There is no such directory!");
            else
                System.out.println("That file is not a directory.");
        }
        else {
            files = directory.list();
            System.out.println("Files in directory \"" + directory + "\":");
            for (int i = 0; i < files.length; i++)
                System.out.println("   " + files[i]);
        }

    } // end main()

}
```

파일에서 데이터를 읽고 파일에 데이터를 쓰는 데 사용되는 모든 클래스에는 File 객체를 매개 변수로 사용하는 생성자가 있다. 예를 들어 `file`이 File 유형의 변수이고 해당 파일에서 문자 데이터를 읽으려는 경우 `new FileReader(file)`이라고 말하여 FileReader를 생성할 수 있다.




