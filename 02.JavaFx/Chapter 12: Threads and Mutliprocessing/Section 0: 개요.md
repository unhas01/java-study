# 개요

고전적인 프로그래밍 모델에는 메모리에서 명령을 읽고 이를 차례로 수행하는 단일 중앙 처리 장치가 있다. 프로그램의 목적은 프로세서가 실행할 명령 목록을 제공하는 것이다. 이것이 지금까지 우리가 고려한 유일한 프로그래밍 유형이다.

그러나 이 프로그래밍 모델에는 한계가 있다. 최신 컴퓨터에는 여러 개의 프로세서가 있어 동시에 여러 작업을 수행할 수 있다. 모든 프로세서의 잠재력을 최대한 활용하려면 병렬 처리를 수행할 수 있는 프로그램을 작성해야 한다. Java 프로그래머에게 이는 스레드에 대해 배우는 것을 의미한다. 단일 스레드는 지금까지 작성한 프로그램과 유사하지만 두 개 이상의 스레드가 동시에 "병렬로" 실행될 수 있다. 단일 스레드 프로그래밍보다 더 흥미롭고 어려운 점은 병렬 프로그램의 스레드가 서로 완전히 독립적인 경우가 거의 없다는 사실이다. 그들은 일반적으르 협력하고 의사소통해야 한다. 스레드 간의 합력을 관리하고 제어하는 방법을 배우는 것이 이 장에서 직면하게 될 주요 장애물이다.

병렬 프로그래밍을 사용하는 데는 여러 가지 이유가 있다. 하나는 여러 프로세서가 동시에 작업하도록 설정하여 계산을 더 빠르게 수행하는 것이다. 그러나 마찬가지로 중요한 것은 스레드를 사용하여 일부 이벤트가 발생할 때까지 프로세스를 진행할 수 없는 "차단" 작업을 처리하는 것이다. 네트워크 연결을 통해 데이터가 도착하기를 기다리는 동안 프로그램이 어떻게 차단될 수 있는지 살펴보았다. 스레드를 사용하면 프로그램의 한 부분이 다른 부분이 차단되어 일부 이벤트가 발생할 때까지 기다리는 동안 유용한 작업을 계속 수행할 수 있다. 이러한 맥락에서 스레드는 처리 장치가 하나만 있는 컴퓨터에서도 중요한 프로그래밍 도구이다.

Java가 개발되면서 스레드를 직접 사용하지 않고도 병렬 프로그래밍을 수행할 수 있는 기능이 언어에 추가되었다. 섹션 10.6의 스트림 API에서 병렬 스트림 중 하나를 살펴보았다. 이 장에서 몇 가지를 더 접하게 될 것이다. 그러나 이러한 고급 언어 기능을 안전하고 효율적으로 사용하려면 스레드의 위험과 이점에 대해 알아야 한다.

