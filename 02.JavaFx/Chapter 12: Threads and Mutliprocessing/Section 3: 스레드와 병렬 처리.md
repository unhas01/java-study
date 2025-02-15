# 스레드와 병렬 처리

이전 섹션 12.2.4의 예에서 대규모 작업의 일부를 실행하기 위해 병렬 처리를 사용했다. 여러 프로세서가 있는 컴퓨터에서는 이를 통해 계산을 더 빠르게 완료할 수 있다. 그러나 프로그램이 계산을 하위 작업으로 나누는 방식은 최적이 아니었다. 스레드를 관리하는 방식도 마찬가지였다. 이 섹션에서는 해당 프로그램의 두 가지 버전을 더 살펴본다. 첫 번째는 문제가 하위 작업으로 분해되는 방식을 개선한다. 두 번째는 스레드가 사용되는 방식을 개선한다. 그 과정에서 병렬 처리를 지원하기 위해 Java가 제공하는 몇 가지 내장 클래스를 소개하나다. 이 섹션 뒷 부분에서 `wait()`, `inform()`을 다룰 것 이다. 병렬 프로세스를 보다 직접적으로 제어하는 데 사용할 수 있는 하위 수준 방법이다.

## 1. 문제 분해(Decomposition)

샘플 프로그램 [MultiprocessingDemo1.java](https://math.hws.edu/javanotes/source/chapter12/MultiprocessingDemo1.java)에서는 이미지 계산 작업을 여러 하위 작업으로 나누고 각 하위 작업을 스레드에 할당한다. 이 작업은 문제 없이 작동하지만 문제가 있다. 일부 하위 작업은 다른 작업보다 훨씬 오래 걸릴 수 있다. 프로그램은 이미지를 동일한 부분으로 나누지만 사실 이미지의 일부 부분에는 다른 부분보다 더 많은 계산이 필요하다. 실제로 세 개의 스레드로 프로그램을 실행하면 중간 부분이 위쪽이나 아래쪽 부분보다 계산하는 데 약간 더 오래 걸린다는 것을 알 수 있다. 일반적으로 문제를 하위 문제로 나눌 때 각 하위 문제를 해결하는 데 걸리는 시간을 예측하기는 매우 어렵다. 특정 하위 문제 하나가 다른 모든 하위 문제보다 시간이 훨씬 오래 걸린다고 가정해 본다. 해당 하위 문제를 계산하는 스레드는 다른 모든 스레드가 완료된 후에도 상대적으로 오랜 시간 동안 계속 실행된다. 그 시간 동안에만, 컴퓨터 프로세서 중 하나가 작동 중이다. 나머지는 idle 상태이다. 

간단한 예로, 컴퓨터에 두 개의 프로세서가 있다고 가정한다. 문제를 두 개의 하위 문제로 나누고 각 하위 문제를 실행하는 스레드를 생성한다. 두 프로세서를 모두 사용하면 하나의 프로세서를 사용할 때보다 절반의 시간 안에 답을 얻을 수 있기를 바란다. 그러나 하나의 하위 문제를 해결하는 데 다른 문제보다 4배 더 오랜 시간이 걸린다면 대부분의 경우 하나의 프로세서만 작동하게 된다. 이 경우 답변을 얻는데 필요한 시간은 20%만 단축된다.

문제를 동일한 양의 계산이 필요한 하위 문제로 나누더라고 여전히 해결하는 데 동일한 시간이 필요한 모든 하위 문제에 의존할 수는 없다. 예를 들어 컴퓨터의 일부 프로세서가 다른 프로그램을 실행하는 중일 수 있다. 아니면 일부 프로세서가 다른 프로세서보다 느릴 수도 있다. (단일 컴퓨터에서 계산을 실행할 때는 이런 일이 발생하지 않지만, 이 장의 뒷부분에서 설명할 것처럼 네트워크로 연결된 여러 컴퓨터에 계산을 분산할 때는 프로세서 속도의 차이가 주요 문제가 될 수 있다.)

이 모든 것을 처리하는 일반적인 기술은 문제를 상당히 많은 수의 하위 문제(프로세서보다 더 많은 하위 문제)로 나누는 것이다. 이는 각 프로세서가 여러 하위 문제를 해결해야 함을 의미한다. 처리자가 하나의 하위 작업을 완료할 때마다 모든 하위 작업이 할당될 때까지 작업할 다른 하위 작업이 할당된다. 물론, 다양한 하위 작업에 필요한 시간에는 여전히 차이가 있다. 한 프로세서는 여러 하위 문제를 완료할 수 있고 다른 프로세서는 특히 어려운 사례를 처리할 수 있다. 느리거나 사용량이 많은 프로세서는 하위 문제가 아주 작은 한, 대부분의 프로세서는 계산이 거의 끝날 때까지 바쁜 상태를 유지할 수 있다. 

**로드 밸런싱(load balancing)** : 사용 가능한 프로세서를 최대한 바쁘게 유지하기 위해 계산 로드가 사용 가능한 프로세서 간에 균형을 이룬다. 물론 일부 프로세서는 다른 프로세서보다 먼저 완료되지만 가장 긴 하위 작업을 완료하는데 걸리는 시간보다 길지는 않다.

하위 문제는 작아야 하지만 너무 작아서는 안된다. 하위 문제를 생성하고 이를 프로세서에 할당하는 데에는 약간의 계산 오버헤드가 있다. 하위 문제가 매우 작은 경우 이 오버헤드는 수행해야 하는 총 작업량에 크게 추가될 수 있다. 예제 프로그램에서 작업은 이미지의 각 픽셀에 대한 색상을 계산하는 것이다. 해당 작업을 하위 작업으로 나누기 위한 한 가지 가능성은 각 하위 작업이 단 하나의 픽셀만 계산하도록 하는 것이다. 그러나 그런 방식으로 생성된 하위 작업은 아마도 너무 작을 것이다. 따라서 프로그램의 각 하위 작업은 한 행의 픽셀에 대한 색상을 계산한다. 이미지에는 수백 행의 픽셀이 있으므로 하위 작업의 수가 상당히 많고 각 하위 작업도 상당히 크다. 그 결과 합리적인 양의 오버헤드와 함께 상당히 좋은 로드 밸런싱이 이루어졌다.

그런데 우리가 작업하고 있는 문제는 병렬 프로그래밍에 있어서는 매우 쉬운 문제라는 점에 유의하세요. 이미지 계산 문제를 하위 문제로 나누면 모든 하위 문제가 완전히 독립적이다. 동시에 여러 작업을 수행할 수 있으며 순서에 관계없이 수행할 수 있다. 일부 하위 작업이 다른 하위 작업에 필요한 결과를 생성하면 상황이 훨씬 더 복잡해진다. 이 경우 하위 작업은 독립적이지 않으며 하위 작업이 수행되는 순서가 중요하다. 또한 한 하위 작업의 결과를 다른 작업과 공유할 수 있는 방법이 있어야 한다. 하위 작업이 다른 스레드에 의해 실행되면 공유 리소스에 대한 스레드 엑세스 제어와 관련된 모든 문제가 발생한다. 따라서 일반적으로 병렬 처리를 위해 문제를 분해하는 것은 상대적으로 간단한 예에서 나타나는 것보다 훨씬 더 어렵다. 그러나 대부분의 경우 이는 입문 프로그래밍 과정이 아닌 병렬 컴퓨팅 과정의 주제이다.

## 2. 스레드 풀과 작업 큐(대기열)

작업을 하위 작업으로 분해하는 방법을 결정한 후에는 해당 하위 작업을 스레드에 할당하는 방법에 대한 질문이 있다. 일반적으로 객체 지향 접근 방식에서는 각 하위 작업이 객체로 표시된다. 작업은 일부 계산을 나타내므로 이를 나타내는 객체가 계산을 수행하는 인스턴스 메서드를 갖는 것은 당연하다. 작업을 실행하려면 해당 계산 메서드를 호출하기만 하면 된다. 프로그램에서 계산 메서드는 `run()`이라고 하며 작업 객체는 섹션 12.1.1에서 논의한 표준 Runnable 인터페이스를 구현한다. 이 인터페이스는 계산 작업을 나타내는 자연스러운 방법이다. 각 Runnable에 대해 새 스레드를 생성하는 것이 가능하다. 그러나 작업이 많을 때는 각각의 새 스레드를 생성하는 데 상당한 양의 오버헤드가 발생하므로 이는 실제로 의미가 없다. 더 나은 대안은 몇 개의 스레드만 생성하고 각 스레드가 여러 작업을 실행하도록 하는 것이다.

사용할 최적의 스레드 수는 완전히 명확하지 않으며 해결하려는 문제가 정확히 무엇인지에 따라 달라질 수 있다. 목표는 컴퓨터의 모든 프로세서를 계속 사용하도록 유지하는 것이다. 이미지 컴퓨팅 예에서는 사용 가능한 각 프로세서에 대해 하나의 스레드를 생성하는 것이 잘 작동하지만 모든 문제에 해당되는 것은 아니다. 특히 스레드가 일부 이벤트를 기다리거나 일부 리소스에 엑세스하는 동안 적지 않은 시간 동안 차단될 수 있는 경우 다른 스레드가 차단되는 동안 프로세서가 실행될 수 있도록 추가 스레드가 있어야 한다. 섹션 12.4세어 네트워킹과 함께 스레드를 사용하게 되면 바로 그러한 상황에 직면하게 될 것이다.

작업을 수행하는 데 여러 스레드를 사용할 수 있는 경우 해당 스레드를 **스레드 풀(thread pool)**이라고 한다. 스레드 풀은 각 작업을 수행하기 위해 새 스레드가 생성되는 것을 방지하는 데 사용된다. 대신 작업을 수행해야 할 경우 "풀"의 idle 스레드에 해당 작업을 할당할 수 있다.

스레드 풀의 모든 스레드가 사용 중이며 스레드 중 하나가 idle 상태가 될 때까지 추가 작업을 기다려야 한다. 이는 큐에 대한 자연스러운 응용 프로그램이다. 스레드 풀과 연결된 것은 대기 중인 작업의 큐이다. 작업을 사용할 수 있게 되면 대기열에 추가된다. 스레드가 작업을 완료할 때마다 작업할 다른 작업을 가져오기 위해 대기열로 이동한다.

스레드 풀에는 작업 대기열이 하나만 있다. 풀의 모든 스레드는 동일한 대기열을 사용하므로 대기열은 공유 리소스이다. 항상 공유 리소스와 마찬가지로 경쟁 조건이 발생할 수 있으며 동기화가 필수적이다. 예를 들어 동기화가 없으면 큐에서 항목을 동시에 가져오려는 두 스레드가 결국 동일한 항목을 검색하게 될 수 있다. 

Java에는 이 문제를 해결하기 위해 내장 클래스인 ConcurrentLinkedQueue가 있다. 이 클래스와 병렬 ㅍ르ㅗ그래밍에 유용할 수 있는 다른 클래스는 `java.util.concurrent` 패키지에 정의되어 있다. 매개 변수화된 클래스이다. Runnable 유형의 객체를 보유할 수 있는 큐를 생성하려면 다음과 같이 말할 수 있다.

```java
ConcurrentLinkedQueue<Runnable> queue = new ConcurrentLinkedQueue<>();
```

이 클래스는 큐에 대한 작업이 적절하게 동기화되는 연결 목록으로 구현된 큐를 나타낸다. ConcurrentLinkedQueue에 대한 작업은 우리가 익숙한 큐 작업과 정확히 다르다. `queue` 끝에 새 항목 `x`를 추가하는 방법은 `queue.add(x)`이다. 앞 부분에서 항목을 제거하는 방법은 `queue.poll()`이다. 비어 있으면 `null`을 반환한다. 따라서 `poll()`은 대기열이 비어 있지 않은지 테스트 하는 것은 경쟁 조건과 관련되므로 이런 방식으로 작업을 수행하는 것이 합리적이다. 동기화가 없으면 다른 스레드가 해당 시간 사이에 대기열에서 마지막 항목을 제거할 수 있다. 대기열이 비어 있지 않은지 확인하고 대기열에서 항목을 가져오라고 하는 시간이다. 아이템을 얻으려고 할 때 거기에는 아무것도 없다. 반면, `queue.poll()`은 "atomic" 작업이다.

---

이미지 컴퓨팅 예제에서 ConcurrentLinkedQueue를 사용하려면 스레드 풀과 함께 대기열을 사용할 수 있다. 이미지 계산을 시작하려면 이미지를 구성하는 모든 작업을 생성하고 이를 대기열에 추가한다. 그런 다음 작업을 실행할 작업자 스레드를 만들고 시작할 수 있다. 각 스레드는 대기열의 `poll()` 메서드를 호출하여 대기열에서 하나의 작업을 가져와 해당 작업을 수행하는 루프에서 실행된다. 작업은 Runnable 유형의 객체이므로 스레드가 작업의 `run()` 메서드를 호출하는 것만 필요하다. `poll()` 메서드가 `null`을 반환하는 경우, 대기열은 비어 있고 모든 작업이 스레드에 할당되었기 때문에 스레드가 종료될 수 있다.

샘플 프로그램 [MultiprocessingDemo2.java](https://math.hws.edu/javanotes/source/chapter12/MultiprocessingDemo2.java)는 이 아이디어를 구현한다. ConcurrentLinkedQueue<Runnable> 유형의 대기열 `tashQueue`를 사용하여 작업을 보관한다. 또한 사용자가 계산이 끝나기 전에 중단할 수 있도록 하기 위해 `volatile` boolean 변수인 `running`을 사용하여 사용자가 계산을 중단할 때 스레드에 신호를 보낸다. 대기열에 아직 작업이 남아 있더라도 이 변수가 `false`로 설정되면 스레드가 종료되어야 한다. 스레드는 WorkerThread 라는 중첩 클래스에 의해 정의된다. 

```java
private class WorkerThread extends Thread {
    public void run() {
        try {
            while (running) {
                Runnable task = taskQueue.poll(); // 대기열에서 작업을 가져옴
                if (task == null)
                    break; 
                task.run();  // 작업을 실행
            }
        }
        finally {
            threadFinished(); // 이 스레드가 종료되었다는 사실을 기록
                                // 호출되었는지 확인하기 위해 마지막으로 완료되었다.
        }
    }
}
```

이 프로그램은 `MandelbrotTask` 라는 중첩 클래스를 사용하여 이미지에서 한 행의 픽셀을 계산하는 작업을 나타낸다. 이 클래스는 Runnable 인터페이스를 구현한다. `run()` 메서드는 실제 작업, 즉 각 픽셀의 색상을 계산하고 해당 색상을 이미지에 적용하는 작업을 수행한다. 다음은 계산을 시작하기 위해 프로그램이 수행하는 작업이다.

```java
taskQueue = new ConcurrentLinkedQueue<Runnable>(); // 큐를 생성
for (int row = 0; row < height; row++) {  // 높이는 이미지의 행 수
    MandelbrotTask task;
    task = ... ;  // 이미지의 한 행을 계산하는 작업을 만든다.
    taskQueue.add(task); // 대기열에 작업을 추
}

int threadCount = ... ; // Number of threads in the pool (selected by user).
workers = new WorkerThread[threadCount];
running = true;  // Set the signal before starting the threads!
threadsRemaining = workers;  // Records how many threads are still running.
for (int i = 0; i < threadCount; i++) {
    workers[i] = new WorkerThread();
    try {
        workers[i].setPriority( Thread.currentThread().getPriority() - 1 );
    } catch (Exception e) {
    }
    workers[i].start();
}
```

스레드가 시작되기 전에 작업을 대기열에 추가하는 것이 중요하다. 스레드는 빈 대기열을 종료 신호로 간주한다. 스레드가 시작될 때 대기열이 비어 있으면 빈 대기열이 표시되어 작업을 수행하지 않고 시작된 직후 종료될 수 있다. 

MultiprocessingDemo2를 사용해 보세요. 동일한 이미지를 계산 하지만 픽셀 행은 해당 프로그램과 동일한 순서로 계산되지 않는다. 주의 깊게 살펴보면 픽셀 행이 위에서 아래로 엄격한 순서로 이미지에 추가되지 않은 것을 볼 수 있다. 이는 한 스레드가 행 번호 `i+1`을 완료하는 동안 다른 스레드가 여전히 행 `i` 또는 그 이전 행에서 작업하는 것이 가능하기 때문이다.

## 3. 생산자(Producer)/소비자(Consumer) 및 차단 대기열

`MultiprocessingDemo2`는 이미지를 그릴 때마다 완전히 새로운 스레드 풀을 생성한다. 이것은 낭비적인 것 같다. 프로그램 시작 시 하나의 스레드 세트를 생성하고 이미지를 계산해야 할 때마다 이를 사용하는 것이 가능하지 않는가? 결국 스레드 풀의 개념은 스레드가 작업이 완료될 때까지 대기하고 작업이 완료될 때 이를 실행해야 한다는 것이다. 문제는 지금까지 스레드가 작업이 완료될 때까지 기다리게할 방법이 없다는 것이다. 이를 위해 **차단 대기열(blocking queue)** 이라는 것을 사용한다.

차단 대기열은 병렬 처리의 고전적인 패턴 중 하나인 생산자/소비자를 구현한 것이다. 이 패턴은 물건을 생산하는 한 명 이상의 "생산자"와 그러한 물건을 소비하는 한 명 이상의 "소비자"가 있을 때 발생한다. 모든 생성자와 소비자가 동시에 작업할 수 있어야 한다. 처리할 준비가 된 항목이 없으면 소비자는 해당 항목이 생산될 때까지 기다려야 한다. 많은 어플리케이션에서 생산자는 때때로 기다려야 한다. 예를 들어 분당 1개의 속도로만 소비할 수 있는 경우 생산자가 분당 2개의 속도로 무기한 생산하는 것은 의미가 없다. 그러면 처리를 기다리는 일이 무제한으로 쌓이게 된다. 따라서 처리를 기다릴 수 있는 항목 수에 제한을 두는 것이 유용한 경우가 많다.

생산자로부터 소비자에게 물건을 전달할 수 있는 방법이 필요하다. 대기열은 분명한 대답이다. 생산자는 항목이 생성될 때 대기열에 항목을 넣는다. 소비자는 대기열의 다른쪽 끝에서 항목을 제거한다.

![생산자-소비자](./images/생산자,%20소비자.png)

병렬 처리에 대해 이야기하고 있으므로 동기화된 대기열이 필요하지만 그 이상이 필요하다. 대기열이 비어 있으면 대기열에 항목이 나타날때까지 소비자가 기다리도록 하는 방법이 필요하다. 대기열이 가득 차면 생산자를 대기열에 공간이 생길때까지 기다리게 하는 방법이 필요하다. 우리 어플리케이션에서 생산자와 소비자는 스레드이다. 일시 중지되어 어떤 일이 일어나기를 기다리는 스레드를 차단되었다고 하며, 필요한 대기열 유형을 차단 대기열이라고 한다. 차단 대기열에서는 대기열이 비어 있으면 대기열에서 항목을 대기열에서 빼는 작업이 차단될 수 있다. 즉, 스레드가 빈 대기열에서 항목을 대기열에서 빼내려고 하면 항목을 사용할 수 있을 때까지 스레드가 일시 중단된다. 그 때 깨어나서 항목을 검색하고 진행한다. 마찬가지로 대기열의 용량이 제한된 경우 항목을 대기열에 추가하려고 시도하는 생산자는 대기열에 공간이 없으면 차단될 수 있다.

Java에는 차단 대기열을 구현하는 두 가지 클래스인 LinkedBlockingQueue 및 ArrayBlockingQueue가 있다. 이는 대기열이 보유할 수 있는 항목 유형을 지정할 수 있도록 하는 매개변수화된 유형이다. 두 클래스 모두 `java.util.concurrent` 패키지에 정의되어 있으며 BlockingQueue라는 인터페이스를 구현한다.

- `bqueue.take()` : 대기열에서 항목을 제거하고 반환한다. 이 메서드가 호출될 때 대기열이 비어 있으면 이를 호출한 스레드는 항목을 사용할 수 있을 때까지 차단된다. 스레드가 차단된 동안 중단되면 InterruptedException을 발생시킨다.
- `bqueue.put(item)` : 대기열에 `item`을 추가한다. 대기열의 용량이 제한되어 있고 가득 찬 경우 이를 호출한 스레드는 대기열에 공간이 열릴 때까지 차단된다. 스레드가 차단된 동안 중단되면 이 메서드는 InterruptedException을 발생시킨다.
- `bqueue.add(item)` : 공간이 있으면 대기열에 `item`을 추가한다. 대기열의 용량이 제한되어 있고 가득 찬 경우 IllegalStateException이 발생한다. 
- `bqueue.clear()` : 대기열에서 모든 항목을 제거하고 버린다.

Java의 차단 대기열은 많은 추가 메서드를 정의한다. 여기에 나열된 네 개만 있어도 우리의 목적에 충분하다. 큐에 항목을 추가하는 두 가지 방법을 나열했다. `bqueue.put(item)`은 큐에 사용 가능한 공간이 없으면 차단하고 용량이 제한된 차단 큐와 함께 사용하는 데 가장 적합하다. `bqueue.add(item)`은 차단되지 않으며 용량이 무제한인 차단 대기열과 함께 사용하는 데 가장 적합하다.

ArrayBlockingQueue에는 생성시 지정되는 최대 용량이 있다.

```java
ArrayBlockingQueue<ItemType> bqueue = new ArrayBlockingQueue<>(25);
```

이 선언을 사용하면 `bqueue.put(item)`은 `bqueue`에 이미 25개 항목이 포함된 겨우 차단되고 `bqueue.add(item)`은 이 경우 예외를 발생 시킨다. 이는 아이템이 소비될 수 있는 것보다 더 빠른 속도로 무기한 생성되지 않도록 보장한다는 점을 기억하자. LinkedBlockingQueue는 용량이 무제한인 차단 대기열을 생성하기 위한 것이다.

```java
LinkedBlockingQueue<ItemType> bqueue = new LinkedBlockingQueue<>();
```

포함할 수 있는 항목 수에 대한 상한이 없는 대기열을 생성한다. 이 경우 `bqueue.put(item)`은 절대 차단되지 않으며 `bqueue.add(item)`은 IllegalStateException을 발생시키지 않는다. 생산자 차단을 방지하고 대기열이 임의의 크기로 커지지 않도록 하는 다른 방법이 있는 경우 LinkedBlockingQueue를 사용한다. 두 가지 유형의 차단 대기열 모두 대기열이 비어 있으면 `bqueue.take()`가 차단된다.

---

샘플 프로그램 [MultiprocessingDemo3.java](https://math.hws.edu/javanotes/source/chapter12/MultiprocessingDemo3.java)는 이전 버전인 MultiprocessingDemo2.java의 ConcurrentLinkedQueue 대신 LinkedBlockingQueue를 사용한다. 이 예에서는 큐는 작업(Runnable 유형의 항목)을 보유하고 있으며 큐는 `taskQueue` 라는 인스턴스 변수로 선언된다.

```java
LinkedBlockingQueue<Runnable> taskQueue;
```

사용자가 "시작" 버튼을 클릭하고 이미지를 계산할 시간이 되면 계산을 구성하는 모든 작업이 이 대기열에 들어간다. 이는 각 작업에 대해 `taskQueue.add(task)`를 호출하여 수행된다. 작업은 이벤트 처리 스레드에서 생성되고 우리는 이를 차단하고 싶지 않기 때문에 차단 없이 이 작업을 수행할 수 있다는 것이 중요하다. 이 프로그램은 한 번에 하나의 이미지에서만 작동하고 이미지당 작업 수는 수백 개에 불과하므로 이 프로그램의 대기열은 무한정 커질 수 없다.

이전 버전의 프로그램과 마찬가지고 스레드 풀에 속한 작업자 스레드는 대기열에서 작업을 제거하고 수행한다. 그러나 이 경우 스레드는 프로그램 시작 시 한 번 생성되며 동일한 스레드가 원하는 수의 이미지에 재사용된다. 실행할 작업이 없으면 작업 대기열은 비어 있고 작업을 사용할 수 있을 때까지 작업자 스레드가 차단된다. 각 작업자 스레드는 무한 루프에서 실행되어 작업을 영원히 처리하지만 작업이 대기열에 추가될 때까지 기다리면서 차단되는 데 많은 시간을 소비한다. 작업자 스레드를 정의하는 내부 클래스는 다음과 같다.

```java
/**
 * 이 클래스는 스레드 풀을 구성하는 작업자 스레드를 정의합니다.
 * WorkerThread는 루프에서 작업을 검색하는 루프에서 실행됩니다.
 * taskQueue 및 해당 작업에서 run() 메서드를 호출합니다. 만약에 참고하세요
 * 큐가 비어 있으면 작업을 사용할 수 있을 때까지 스레드가 차단됩니다.
 * 대기열에 있습니다. 생성자가 스레드를 시작하므로 스레드가 없습니다.
 * 이를 위해서는 메인 프로그램이 필요합니다. 스레드가 우선순위로 실행됩니다.
 * 이는 호출하는 스레드의 우선순위보다 1 낮습니다.
 * 생성자.
 *
 * WorkerThread는 무한 루프에서 실행되도록 설계되었습니다. 그럴 것이다
 * 자바 가상 머신이 종료될 때만 종료됩니다. (이것은 다음과 같이 가정합니다.
 * 실행되는 작업은 예외를 발생시키지 않습니다. 이는 사실입니다.
 * 이 프로그램에서는.) 생성자는 스레드가 다음과 같이 실행되도록 설정합니다.
 * 데몬 스레드; Java Virtual Machine은 다음과 같은 경우 자동으로 종료됩니다.
 * 유일한 스레드는 데몬 스레드이므로 스레드의 존재
 * 풀은 JVM 종료를 중지하지 않습니다.
 */
private class WorkerThread extends Thread {
    WorkerThread() {
        try {
            setPriority( Thread.currentThread().getPriority() - 1);
        } catch (Exception e) {
        }
        try {
            setDaemon(true);
        } catch (Exception e) {
        }
        start(); // 스레드는 생성되자마자 시작됩니다.
    }
    public void run() {
        while (true) {
            try {
                Runnable task = taskQueue.take(); // 필요한 경우 작업을 기다립니다.
                task.run();
            } catch (InterruptedException e) {
            }
        }
    }
}
```

스레드 풀이 어떻게 작동하는 지 좀 더 자세히 살펴봐야 한다. 작업자 스레드는 수행할 작업이 있기 전에 생성되고 시작된다. 각 스레드는 즉시 `taskQueue.take()`를 호출한다. 작업 대기열이 비어 있으므로 모든 작업자 스레드는 시작되자마자 차단된다. 이미지 계산을 시작하기 위해 이벤트 처리 스레드는 작업을 생성하고 이를 대기열에 추가한다. 이런 일이 발생하자마자 작업자 스레드는 깨어나 작업 처리를 시작하며 대기열이 비워질 때까지 계속 작업을 수행한다. 대기열이 비어 있으면 작업자 스레드는 다음 처리가 시작될 때까지 다시 절전 모드로 전환된다.

---

이 프로그램의 흥미로운 점은 계산이 끝나기 전에 계산을 중단할 수 있기를 원하지만 그런 일이 발생하더라고 작업자 스레드가 종료되는 것을 원하지 않는다는 것이다. 사용자가 "중단" 버튼을 클릭하면 프로그램은 `taskQueue.clear()`를 호출하여 더 이상 작업자 스레드에 할당되지 않도록 한다. 그러나 일부 작업은 작업 대기열이 지워질 때 이미 실행 중일 가능성이 높다. 이러한 작업은 하위 작업인 계산이 중단된 후에 완료된다. 해당 하위 작업이 완료되면 해당 출력이 이미지에 적용되는 것을 원하지 않는다.

내 해결책은 각 계산에 작업 번호를 할당하는 것이다. 현재 작업의 작업 번호는 `jobNum`이라는 인스턴스 변수에 저장되며, 각 작업 객체에는 해당 작업이 어떤 작업에 속해 있는지 알려주는 인스턴스 변수가 있다. 작업이 저절로 끝나거나 사용자가 작업을 중단하여 작업이 종료되면 `jobNum` 값이 증가한다. 작업이 완려되면 작업 객체에 저장된 작업 번호가 `jobNum`과 비교된다. 동일하면 작업은 현재 작업의 일부이며 해당 출력은 이미지에 적용된다. 동일하지 않으면 작업은 이전 작업의 일부였으며 해당 출력은 삭제된다. 

`jobNum`에 대한 엑세스를 적절하게 동기화하는 것이 중요하다. 그렇지 않으면 한 스레드가 다른 스레드 작업 번호를 증가시키는 것처럼 작업 번호를 확인할 수 있으며 해당 작업이 중단된 후 이전 작업에 대한 출력이 몰래 빠져나갈 수 있다. 프로그램에서는 `jobNum`에 엑세스 하거나 변경하는 모든 메서드가 동기화된다.

---

`MultiprocessingDemo3`에 대해 한 가지 더 말씀드린다. 저는 이 프로그램에서 작업자 스레드를 종료하는 방법을 제공하지 않았다. JVM이 종료될 때까지 계속 실행된다. 그 전에 스레드 종료를 허용하려면 `volatile` boolean 변수인 `running`을 사용하고 작업자 스리드가 종료되기를 원할 때 해당 값을 `false`로 설정할 수 있다. 스레드의 `run()` 메서드는 다음으로 대체된다.

```java
public void run() {
    while ( running ) {
        try {
            Runnable task = taskQueue.take();
            task.run();
        }
        catch (InterruptedException e) {
        }
    }
}
```

그러나 `taskQueue.take()`에서 스레드가 차단되면 차단이 해제될 때까지 새로운 `running` 값을 볼 수 없다. 이를 보장하려면 `runner`를 `false`로 설정한 직후에 각 작업자 스레드에 대해 `worker.interrupt()`를 호출해야 한다.

`running`이 `false`로 설정된 경우 작업자 스레드가 작업을 실행하는 경우 해당 작업이 완료될 때까지 스레드가 종료되지 않는다. 작업이 비교적 짧은 경우에는 문제가 되지 않는다. 스레드가 종료될 때까지 기다리는 것보다 작업 실행 시간이 더 오래 걸릴 수 있는 경우 각 작업은 주기적으로 실행 값을 확인하고 해당 값이 `false`가 되면 종료 해야 한다.

## 4. ExecutorService 접근 방식

스레드 풀은 병렬 프로그래밍에서 일반적이므로 Java에 스레드 풀을 생성하고 관리하기 위한 더 높은 수준의 도구가 있다는 것은 놀라운 일이 아니다. `java.util.concurrent` 패키지의 ExecutorService 인터페이스는 제출된 작업을 실행할 수 있는 서비스를 정의한다. 클래스는 실행자에서 다양한 유형의 ExecutorService를 생성하는 데 사용할 수 있는 `static` 메서드가 포함되어 있다. 특히 `Executors.newFixedThreadPool(n)`은 n개의 스레드가 있는 스레드 풀을 생성한다. 사용 가능한 프로세서당 하나의 스레드가 있는 스레드 풀을 얻으려면 다음과 같이 할 수 있다.

```java
int processors = Runtime.getRuntime().availableProcessors();
ExecutorService executor = Executors.newFixedThreadPool(processors);
```

`executor.execute(task)` 메서드는 실행을 위해 Runnable 객체 `task`를 제출하는 데 사용될 수 있다. 이 메서드는 작업을 대기 중인 작업 대기열에 넣은 후 즉시 반환한다. 스레드 풀의 스레드는 대기열에서 작업을 제거하고 실행한다. 

`executor.shutdown()` 메서드는 모든 대기 작업이 실행된 후 스레드 풀을 종료하도록 지시한다. 이 메서드는 스레드가 완료될 때까지 기다리지 않고 즉시 반환된다. 이 메서드가 호출된 후에는 새 작업을 추가하는 것이 올바르지 않다. `shutdown()`을 두 번 이상 호출하는 것은 오류가 아니다. 스레드 풀의 스레드는 데몬 스레드가 아니다. 서비스가 종료되지 않으면 스레드가 존재하므로 다른 스레드가 종료된 후에도 JVM이 종료되지 않는다. 종료하기 전에 풀의 스레드는 대기열에서 이미 제거한 모든 작업을 완료한다. 

`executor.shutdownNow()` 메서드는 `executor.shutdown()`과 유사하지만 아직 대기열에 있는 작업을 모두 삭제하고 현재 실행 중인 작업을 중지하려고 시도한다.

샘플 프로그램 [MultiprocessingDemo4.java](https://math.hws.edu/javanotes/source/chapter12/MultiprocessingDemo4.java)는 스레드와 차단 대기열을 직접 사용하는 대신 ExecutorService를 사용하는 MultiprocessingDemo3의 변형이다. 

---

ExecutorService에 대한 작업은 Callable<T> 유형의 객체로 표시될 수도 있다. 이는 매개 변수 없이 `call()` 메서드와 T 반환 유형을 정의하는 매개 변수화된 기능 인터페이스이다. Callable은 값을 출력하는 작업을 나타낸다. 

Callable `c`는 `executor.submit(c)`를 호출하여 ExecutorService에 제출할 수 있다. 그런 다음 Callable은 나중에 실행된다. 문제는 계산이 완료되었을 때 계산 결과를 어떻게 얻을 수 있냐는 것이다. 이 문제는 미래까지 사용할 수 없는 T 유형의 값을 나타내는 또 다른 인터페이스인 Future<T>를 통해 해결된다. `executor.submit(c)` 메서드는 미래 계산 결과를 나타내는 Future를 반환한다.

Future `v`는 결과가 사용 가능한지 확인하기 위해 호출할 수 있는 boolean 값 함수인 `v.isDone()`을 포함한 여러 메서드를 정의한다. 그리고 `v.get()`은 미래의 가치를 검색한다. `v.get()` 메서드는 값을 사용할 수 있을 때까지 차단된다. 또한 예외를 생성할 수 있으며 `try..catch` 문에서 호출해야 한다. 

예를 들어 [ThreadTest4.java](https://math.hws.edu/javanotes/source/chapter12/ThreadTest4.java)는 Callables, Futures 및 ExecutorService를 사용하여 특정 범위의 정수에서 소수의 수를 계산한다. 이 프로그램에서 하위 작업은 정수와 하위 범위에서 소수를 계산한다. 하위 작업은 중첩 클래스로 정의된 Callable<Integer> 유형이다.

```java
/**
 * 이 클래스에 속하는 객체는 지정된 범위의 소수를 계산합니다.
 * 정수. 범위는 최소부터 최대까지입니다. 여기서 최소와 최대는
 *는 생성자에 매개변수로 제공됩니다. 계산은 다음에서 이루어집니다.
 * 발견된 소수의 개수를 반환하는 call() 메서드.
 */
private static class CountPrimesTask implements Callable<Integer> {
    int min, max;
    public CountPrimesTask(int min, int max) {
        this.min = min;
        this.max = max;
    }
    public Integer call() {
        int count = countPrimes(min,max);  // 계산을 한다
        return count;
    }
}
```

모든 하위 작업은 ExecutorService로 구현된 스레드 풀에 제출되고 반환된 Future는 배열 목록에 저장된다.

```java
int processors = Runtime.getRuntime().availableProcessors();
ExecutorService executor = Executors.newFixedThreadPool(processors);

ArrayList<Future<Integer>> results = new ArrayList<>();

for (int i = 0; i < numberOfTasks; i++) {
    CountPrimesTask oneTask = . . . ;
    Future<Integer> oneResult = executor.submit( oneTask );
    results.add(oneResult);  // 결과를 나타내는 Future를 저장합니다.

}
```

최종 결과를 얻으려면 모든 하위 작업에서 출력되는 정수를 더해야 한다. 하위 작업의 출력은 목록에 있는 `Future`의 `get()` 메서드를 사용하여 가져온다. `get()`은 결과를 사용할 수 있을 때까지 차단되므로 모든 하위 작업이 완료된 경우에만 프로세스가 완료된다.

```java
int total = 0;
for ( Future<Integer> res : results) {
    try {
        total += res.get();  // 작업이 완료될 때까지 기다립니다!
    } catch (Exception e) { // 이 프로그램에서는 발생하지 않아야 합니다.
    }
}
```

## 5. Wait and Notify

자체적인 차단 대기열을 구현하고 싶다고 가정해 본다. 그러기 위해서는 어떤 이벤트가 발생할 때까지 스레드 블록을 만들 수 있어야 한다. 스레드는 이벤트가 발생하기를 _기다리고(waiting)_ 있다. 어떻게든 그런 일이 발생하면 _알려야(notified)_ 한다. 한 스레드를 깨우는 이벤트는 큐에 항목을 추가하는 등 다른 스레드가 수행한 작업으로 인해 발생하므로 두 개의 스레드가 관련된다.

이는 대기열 차단에만 국한되는 문제가 아니다. 한 스레드가 다른 스레드에 필요한 일종의 결과를 생성할 때마다 스레드가 계산을 수행할 수 있는 순서에 일부 제한이 적용된다. 두 번째 스레드가 첫 번째 스레드의 결과가 필요한 지점에 도달하면 중지하고 결과가 생성될 때까지 기다려야 할 수 있다. 두 번째 스레드는 계속할 수 없으므로 절전 모드로 전환될 수도 있다. 그러나 결과가 준비되면 두 번째 스레드가 깨어나서 계산을 계속할 수 있도록 이를 알리는 방법이 있어야 한다.

물론 Java에는 이러한 종류의 "대기" 및 "알림"을 수행하는 방법이 있다. Object 클래스에 인스턴스 메서드로 정의된 `wait()` 및 `inform()` 메서드가 있으므로 모든 객체와 함께 사용할 수 있다. 이러한 메서드는 차단 대기열에서 내부적으로 사용될 수 있다. 상당히 낮은 수준이고 까다로우며 오류가 발생하기 쉬우므로 가능하면 차단 대기열과 같은 더 높은 수준의 제어 전략을 사용해야 한다. 그러나 직접 사용해야 하는 경우를 대비해 `wait()` 및 `inform()`에 대해 알아 두는 것이 좋다. 

`wait()` 와 `inform()` 이 객체와 연관되어야 하는 이유는 명확하지 않으므로 이 시점에서 걱정하지 말자. 적어도 어떤 객체의 `inform()` 메서드가 호출되는지에 따라 다양한 수신자에게 다양한 알림을 보내는 것이 가능해졌다.

일반적인 개념은 스레드가 일부 객체에서 `wait()` 메서드를 호출하면 해당 스레드는 동일한 객체의 `inform()` 메서드가 호출될 때까지 절전 모드로 전환된다는 것이다. `wait()`를 호출한 스레드가 대기 중이므로 분명히 다른 스레드에 의해 호출되어야 한다. 일반적인 패턴은 스레드 A가 스레드 B의 결과가 필요할 떄 `wait()`를 호출하지만 해당 결과를 아직 사용할 수 없다는 것이다. 스레드 B가 결과를 준비하면 스레드 A가 대기 중인 경우 결과를 사용할 수 있도록 깨우는 `inform()`을 호출한다. 아무도 기다리지 않을 때 `notify()`를 호출하는 것은 오류가 아니다. 효과가 없다. 이를 구현하기 위해 스레드 A는 다음과 유사한 코드를 실행한다. 

```java
if ( resultIsAvailable() == false )
    obj.wait();  // 결과를 사용할 수 있다는 알림을 기다립니다.
useTheResult();
```

스레드 B는 다음과 같은 작업을 수행한다.

```java
generateTheResult();
obj.notify();  // 결과를 사용할 수 있다는 알림을 보냅니다.
```

이제 이 코드에는 정말 불쾌한 경쟁 조건이 있다. 두 스레드는 다음 순서로 코드를 실행할 수 있다.

```
1.  Thread A checks resultIsAvailable() and finds that the result is not ready,
        so it decides to execute the obj.wait() statement, but before it does,
2.  Thread B finishes generating the result and calls obj.notify()
3.  Thread A calls obj.wait() to wait for notification that the result is ready.

1. 스레드 A는 resultIsAvailable()을 확인하고 결과가 준비되지 않았음을 발견합니다.
        그래서 obj.wait() 문을 실행하기로 결정했지만 그 전에
2. 스레드 B는 결과 생성을 마치고 obj.notify()를 호출합니다.
3. 스레드 A는 obj.wait()를 호출하여 결과가 준비되었다는 알림을 기다립니다.
```

3단계에서 스레드 A는 결코 오지 않을 알림을 기다리고 있다. 왜냐하면 2단계에서 이미 `inform()`이 호출되었기 때문이다. 이는 스레드 A를 영원히 기다리게 할 수 있는 일종의 교착 상태이다. 분명히 일종의 동기화가 필요하다. 해결책은 스레드 A의 코드와 스레드 B의 코드를 모두 `synchronized` 문으로 묶는 것이다. `wait()`, `inform()` 호출에 사용되는 동일한 객체 `obj`에서 동기화하는 것은 매우 자연스러운일이다. 실제로 `wait()`, `inform()`을 사용할 때 거의 항상 동기화가 필요하므로 Java에서는 이를 절대적인 요구사항으로 만든다. Java에서 스레드는 합법적으로 `obj.wait()`를 호출할 수 있다. `obj.notify()`는 해당 스레드가 객체 `obj`와 관련된 동기화 잠금을 보유하는 경우에만 해당된다. 해당 잠금을 보유하지 않으면 예외가 발생한다. (예외는 필수 처리가 필요하지 않으며 일반적으로 포착되지 않는 IllegalMonitorStateException 유형이다.) 한 가지 더 복잠한 문제는 `wait()` 메서드가 InterruptedException을 발생시킬 수 있으므로 예외를 처리하는 `try` 문에서 호출해야 한다는 것이다.

상황을 더 명확하게 하기 위해 한 스레드에서 계산된 결과를 결과가 필요한 다른 스레드로 가져올 수 있는 방법을 고려해 본다. 이는 하나의 품목만 생산하고 소비되는 단순화된 생산자/소비자 문제이다. 생산자에서 소비자로 결과를 전송하는 데 사용되는 `sharedResult` 라는 공유 변수가 있다고 가정한다. 결과가 준비되면 생상자는 변수를 `null`이 아닌 값으로 설정한다. 소비자는 `sharedResult` 값이 `null`인지 테스트하여 결과가 준비되었는지 확인할 수 있다. 동기화를 위해 `lock` 이라는 변수를 사용한다. 생산자 스레드의 코드는 다음과 같은 형식을 가질 수 있다.

```java
makeResult = generateTheResult();  // 동기화되지 않았습니다!
synchronized(lock) {
    sharedResult = makeResult;
    lock.notify();
}
```

소비자는 다음과 같은 코드를 실행한다.

```java
synchronized(lock) {
    while ( sharedResult == null ) {
        try {
            lock.wait();
        } catch (InterruptedException e) {
        }
    }
    useResult = sharedResult;
}
useTheResult(useResult);  // 동기화되지 않았습니다!
```

`generateTheResult()` 및 `useTheResult()`에 대한 호출은 동기화되지 않으므로 `lock`에서 동기화할 수도 있는 다른 스레드와 병렬로 실행될 수 있다. `shardedResult`는 공유 변수 이므로 `sharedResult`에 대한 모든 참조는 동기화되어야 하므로 `sharedResult`에 대한 참조는 동기화된 문 내에 있어야 한다. 목표는 동기화된 코드 세그먼트에서 가능한 한 적은 작업을 수행하는 것이다.

드물게 경계심이 있는 경우 이상한 점을 발견할 수 있다. `lock.wait()`는 `lock.notify()`가 실행될 때까지 완료되지 않는다. 그러나 이 두 메서드는 모두 동일한 객체에서 동기화되는 `synchronized` 문에서 호출되므로 종료해서는 안된다. 두 가지 방법을 동시에 실행하는 것은 불가능한가? 실제로 `lock.wait()`는 특별한 경우이다. 스레드가 `lock.wait()`를 호출하면 동기화 객체에 대해 보유하고 있는 잠금을 포기한다. 이는 다른 스레드가 `lock.notify()`를 포함하는 `synchronized(lock)` 블록을 실행할 수 있는 기회를 제공한다. 두 번째 스레드가 이 블록에서 종료되면 계속할 수 있도록 잠금이 소비자 스레드로 반환된다.

전체 생산자/소비자 패턴에서는 하나 이상의 생산자 스레드에서 여러 결과가 생성되고 하나 이상의 소비자 스레드에서 사용된다. 하나의 `sharedResult` 객체를 갖는 대신, 생성되었지만 아직 소비되지 않은 객체 목록을 유지한다. LinkedBlockingQueue<Runnable>에 대한 세 가지 작업을 구현하는 매우 간단한 클래스에서 이것이 어떻게 작동하는지 살펴본다.

```java
import java.util.LinkedList;

public class MyLinkedBlockingQueue {

    private LinkedList<Runnable> taskList = new LinkedList<Runnable>();

    public void clear() {
        synchronized(taskList) {
            taskList.clear();
        }
    }

    public void add(Runnable task) {
        synchronized(taskList) {
            taskList.addLast(task);
            taskList.notify();
        }
    }

    public Runnable take() throws InterruptedException {
        synchronized(taskList) {
            while (taskList.isEmpty())
                taskList.wait();
            return taskList.removeFirst();
        }
    }

}
```

이 클래스에서는 `taskList` 객체를 동기화하도록 선택했지만 모든 객체를 사용할 수 있다. 실제로 간단히 `synchronized` 메서드를 사용할 수 있는 데 이는 `this`이다. 

그런데 `taskList.clear()` 호출이 `wait()` 또는 `inform()`을 호출하지 않더라고 동일한 객체에서 동기화되는 것이 중요하다. 그렇지 않으면 경쟁 조건이 발생할 수 있다. `take()` 메서드가 `takeList`가 비어 있지 않은지 확인한 직후와 목록에서 항목을 제거하기 전에 목록이 지워질 수 있다. 이 경우 `takeList.removeFirst()`가 호출될 때 목록이 다시 비어 있어 오류가 발생한다.

---

여러 스레드가 알림을 기다리는 것이 가능하다. `obj.notify()`를 호출하면 `obj`를 기다리고 있는 스레드 중 하나만 깨울 것이다. `obj`를 기다리고 있는 모든 스레드를 깨우려면 `obj.notifyAll()`를 호출하면 된다. `obj.notify()`는 소비자 스레드만 차단할 수 있으므로 위 예제에서는 제대로 작동한다. 작업이 대기열에 추가될 때 하나의 소비자 스레드만 깨우면 되며, 어떤 소비자가 작업을 가져오는지 중요하지 않다. 그러나 생산자와 소비자가 모두 차단할 수 있는 제한된 용량의 차단 대기열을 생각해 보자. 항목이 대기열에 추가되면 다른 생산자 뿐만 아니라 소비자 스레드에도 알림이 전달되도록 하려고 한다. 한 가지 해결책은 `notify()` 대신 `notifyAll()`을 호출하는 것이다. `notifyAll()`을 사용 하면 대기 중인 소비자를 포함한 모든 스레드에 알릴 수 있다.

또한 `obj.notify()` 메서드의 이름에 대한 혼돈 가능성도 언급하고 싶다. 이 방법은 `obj`에게 아무 것도 알리지 않는다. `obj.wait()`를 호출한 스레드에 알린다. 마찬가지로 `obj.wait()`에서 무언가를 기다리고 있는 것은 `obj`가 아니다. 메서드를 호출하는 스레드이다.

그리고 `wait`에 대한 마지막 참고 사항: 밀리초 단위를 매개 변수로 사용하는 또 다른 버전의 `wait()`이 있다. `obj.wati(milliseconds)`를 호출하는 스레드는 알림을 위해 지정된 밀리초 까지만 기다린다. 해당 기간 동안 알림이 발생하지 않으면 스레드가 깨어나 알림 없이 계속된다. 실제로 이 기능은 "계산을 완료를 기다리는 중"이라는 ㅁ메시지를 깜빡이게 하는 등 일부 주기적인 작업을 수행하기 위해 대기 중인 스레드가 주기적으로 깨어나도록 하는 데 가장 자주 사용된다. 

---

한 스레드가 다른 스레드를 제어할 수 있도록 `wait()` 및 `inform()`을 사용하는 예제를 살펴본다. 샘플 프로그램 [TowersOfHanoiGUI.java](https://math.hws.edu/javanotes/source/chapter12/TowersOfHanoiGUI.java)는 하노이 타워 퍼즐을 해결한다. 사용자가 알고리즘 실행을 제어할 수 있는 제어 버튼이 있다. 사용자는 "다음단계" 버튼을 클릭하여 솔루션의 한 단계만 실행할 수 있다. 이 단계는 단일 디스크를 한 더미에서 다른 더미로 이동한다. "실행"을 클릭하면 알고리즘이 자동으로 자체적으로 실행된다. 버튼의 텍스트가 "실행"에서 "일시 중지"로 변경되고, "일시 중지"를 클릭하면 자동 실행이 중지된다. 현재 솔루션을 중단하고 퍼즐을 초기 구성으로 되돌리는 "다시 시작" 버튼도 있다. 다음은 버튼을 포함하여 솔루션 중간에 있는 프로그램의 그림이다.

![하노이 타워 GUI](./images/하노이%20GUI.png)

이 프로그램에는 퍼즐을 풀기 위해 재귀 알고리즘을 실행하는 스레드와 사용자 작업에 반응하는 이벤트 처리 스레드라는 두 개의 스레드가 있다. 사용자가 버튼 중 하나를 클릭하면 이벤트 처리 스레드에서 메서드가 호출된다. 그러나 실제로는 솔루션의 한 단계를 수행하거나 다시 시작하여 응답해야 하는 재귀를 실행하는 스레드이다. 이벤트 처리 스레드는 솔루션 스레드에 일종의 신호를 보내야 한다. 이는 두 스레드가 공유하는 변수 값을 설정하여 수행된다. 변수 이름은 `status` 이며 가능한 값은 `GO`, `PAUSE`, `STEP`, `RESTART` 상수이다.

이벤트 처리 스레드가 이 변수의 값을 변경하면 솔루션 스레드는 새 값을 확인하고 응답해야 한다. `status`가 `PAUSE` 이면 솔루션 스레드가 일시 중지되어 사용자가 "실행" 또는 "다음 단계"를 클릭할 때까지 기다린다. 프로그램이 시작될 떄의 초기상태이다. 사용자가 "다음 단계"를 클릭하면 이벤트 처리 스레드가 `status`를 "STEP"으로 설정한다. 솔루션 스레드는 새 값을 확인하고 솔루션의 한 단계를 실행한 다음 `status`를 `PAUSE`로 재설정하여 응답해야 한다. 사용자가 "실행"을 클릭하면 `status`가 `GO`로 설정된다. 그러면 솔루션 스레드가 자동으로 실행된다. 솔루션이 실행되는 동안 사용자가 "일시 중지"를 클릭하면 `status`가 `PAUSE`로 재설정되고 솔루션 스레드는 ㅇ리시 중지된 상태로 돌아가야 한다. 사용자가 "다시 시작"을 클릭하면 이벤트 처리 스레드는 `RESTART`로 설정하고 솔루션 스레드는 현재 재귀 솔루션을 종료하여 응답해야 한다. 

우리에게 중요한 점은 솔루션 스레드가 일시 중지 되면 `Sleeping` 상태라는 것이다. 깨어나지 않으면 `status`에 대한 새로운 값이 표시되지 않는다. 이를 가능하게 하기 위해 프로그램 솔루션 스레드에서 `wait()`를 사용하여 해당 스레드를 절전 모드로 전환하고, 이벤트 처리 스레드에서 `inform()`을 사용하여 `status` 값이 변경될 때마다 솔루션 스레드를 꺠운다. 버튼 클릭에 응답하는 메서드는 다음과 같다. 사용자가 버튼을 클릭하면 해당 메서드 상태 값을 변경하고 `inform()`을 호출하여 솔루션 스레드를 깨운다. 

```java
synchronized private void doStopGo() {
    if (status == GO) {  // 애니메이션이 실행 중입니다. 일시중지하세요.
        status = PAUSE;
        nextStepButton.setDisable(false);
        runPauseButton.setText("Run");
    }
    else {  // 애니메이션이 일시중지됩니다. 실행을 시작하세요.
        status = GO;
        nextStepButton.setDisable(true);  // 애니메이션이 실행 중일 때는 비활성화됩니다.
        runPauseButton.setText("Pause");
    }
    notify();  // Wake up the thread so it can see the new status value!
}

synchronized private void doNextStep() {
    status = STEP;
    notify();
}

synchronized private void doRestart() {
    status = RESTART;
    notify();
}
```

이 메서드는 호출이 `notify()`를 하도록 동기화 된다. 개체의 `notify()` 메서드는 해당 개체의 동기화 잠금을 유지하는 스레드에서만 호출할 수 있다. 이 경우 동기화 개체는 이것이다. 상태 값이 솔루션 스레드에 의해 변경될 수 있기 때문에 발생하는 레이스 상황 때문에 동기화도 필요하다.

솔루션 스레드는 `checkStatus()` 라는 메서드를 호출하여 `status` 값을 확인한다. 이 메서드는 상태가 `PAUSE`인 경우 `wait()`를 호출하여 이벤트 처리 스레드가 `inform()`을 호출할 때까지 솔루션 스레드를 절전 모드로 전환한다. 상태가 `RESTART`인 경우 `checkStatus()`는 IllegalStateException을 발생시킨다.

```java
synchronized private void checkStatus() {
    while (status == PAUSE) {
        try {
            wait();
        }
        catch (InterruptedException e) {
        }
    }
    // 이때 상태는 RUN, STEP, RESTART 입니다.
    if (status == RESTART)
        throw new IllegalStateException("Restart");
        // 이때 상태는 RUN 또는 STEP이므로 솔루션을 진행해야 합니다.
}
```

솔루션 스레드의 `run()` 메서드는 퍼즐의 초기 상태를 설정한 다음 퍼즐을 풀기 위해 `solv()` 메서드를 호출한다. 퍼즐을 여러 번 풀 수 있도록 무한 루프로 실행된다. 대기/알림 제어 전략을 구현하기 위해 `run()`은 솔루션을 시작하기 전에 `checkStatus()`를 호출하고 `solv()`각 이동 후에 `checkStatus()`를 호출한다. `checkStatus()`가 IllegalStateException을 발생시키는 경우, `solv()`에 대한 호출이 조기에 종료된다. 

전체 소스 코드를 확인하여 이 모든 것이 전체 프로그램에 어떻게 적용되는지 확인할 수 있다. `wait()` 및 `inform()`을 직접 사용하는 방법을 알고 싶으면 이 예제는 좋은 출발이다.

