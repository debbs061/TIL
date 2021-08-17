# JVM의 Runtime Data Area

* Runtime Data Area는 JVM이 프로그램을 수행하기 위해 운영체제로부터 할당 받는 메모리 영역이다. Runtime Data Areas는 각각의 목적에 따라 5개 영역으로 나뉜다.
* Method Area 와 Heap 은 모든 스레드가 공유한다.
* PC Registsers, JVM Stacks, Native Method Stacks 는 스레드마다 존재한다.

## Method Area

* 클래스 로더가 클래스 파일을 읽어오면 클래스 정보를 파싱해서 Method Area 에 저장한다.
  * 인스턴스 변수, 메소드 코드, 정적 변수는 어떤 게 있는 가..

## Heap

* 프로그램을 실행하면서 생성한 모든 객체를 Heap 에 저장
* Heap 영역은 GC의 대상이 되는 영역이다.

## PC Registers

* 각 스레드는 무조건 메서드를 실행하고 있다. pc는 그 메서드 안에서 몇 번째 줄을 실행해야 하는 지를 나타내는 역할이다.
* Thread가 생성될 때마다 생기는 공간으로 Thread가 어떠한 명령을 실행하게 될지에 대한 부분을 기록한다. 

## JVM Stacks

* 자바 스택은 스레드 별로 1개만 존재하고, 스택 프레임은 메서드가 호출될 때마다 생성된다. 메서드 실행이 끝나면 스택 프레임은 pop 되어 스택에서 제거된다.

## Native Method Stacks

* 성능 향상을 목적으로 Java Bytecode가 아닌 다른 언어(c,c++)로 작성된 코드를 컴파일 해서 사용하는 경우가 있음. 그때 사용되는 메서드