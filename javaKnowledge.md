<h3>Java 내용 정리</h3>



<h4>상속 관점에서 생성자를 private로 선언하면 어떤 효과가 있는가?</h4>
private 메소드에 접근이 가능하려면 해당 객체이거나 해당 객체의 내부 클래스 또는 해당 객체를 내부 클래스로 가지고 있는 외부 클래스만 가능하다. 때문에 상속 관점에서 private 생성자를 가진 클래스는 상속이 가능하지만 오직 내부 클래스 관계를 가진 클래스만이 상속할 수 있다. 
<br>
<br>
<h4>try-catch-finally구문의 try body에서 return문을 수행하면 finally가 수행될까?</h4>
수행된다. finally가 수행 안되는 경우는 오직 JVM이 죽거나 수행 중이던 스레드가 죽는 경우이다.
<br>
<br>
<h4>final, finally, finalize의 차이는?</h4>
final은 문맥에 따라 다음과 같이 사용된다.
primitive 변수 : 해당 변수의 값 변경이 불가능하다.
object : 참조 변수가 힙 내의 다른 객체를 가리킬 수 없다.
메서드 : 해당 메서드를 오버라이드 할 수 없다.
클래스 : 해당 클래스를 상속할 수 없다.
finally는 try-catch-finally에 사용되는, try 구문 또는 catch 구분이 끝난 후 마지막으로 수행되는 구문이다.
finalize는 메서드로써, garbage collector가 해당 객체를 메모리에서 삭제하겠다고 결정되는 순간 호출되는 메서드이다.
<br>
<br>
<h4>TreeMap, HashMap, LinkedHashMap의 차이</h4>
HashMap은 검색 및 삽입에 O(1)이 소요되고, 연결리스트로 이루어진 배열로 구성되어 있다. Hash키는 무작위로 섞여있어 순서가 보장 안된다. TreeMap은 검색 및 삽입에 O(logN)이 소요되고 키는 정렬(Comparable 구현 필수)되어 있으므로(레드-블랙 트리로 구현) 정렬된 순서로 키 순회가 가능하다. LinkedHashMap 검색 및 삽입에 O(1)이 소요되고, 양방향 연결 버킷으로 구현되어 있어 키는 삽입한 순서대로 정렬되어 있다.
<br>
<br>
<h4>Lambda expression</h4>
람다식은 식별자 없이 실행 가능한 함수 표현식을 의미한다. 기존의 불필요한 코드를 줄이고 가독성을 향상시키는 것에 목적을 두고 있다. Java 8 람다식의 대표적인 feature로 Functional interface, Stream API가 있다.
<br>
<br>
<h4>FunctionalInterface</h4>
추상 메소드가 단 하나인 인터페이스를 의미한다. 람다 표현식에서 자주 사용되는 인터페이스로 복수 개의 추상 메소드 선언 시 에러 발생을 위해 어노테이션을 사용할 수 있다. 사용 예는 다음과 같다.

```
interface Func {
   int calc(int a, int b);
}

Func add= (int a, int b) -> a + b;
Func sub= (int a, int b) -> a - b;
int num= add.calc(5,3) + sub.calc(10,1);
```
<br>
<br>
<h4>Stream API</h4>
첫 번째로 각 Stream의 요소를 순회하는 경우 사용하는 forEach 사용 예는 다음과 같다.

```
Arrays.asList(1,2,3).stream()
                    .forEach(System.out::println);  // 1, 2, 3
```

두 번째로 Stream의 개별 요소마다 연산을 해 데이터를 적용할 수 있는 map 사용 예는 다음과 같다.

```
Arrays.asList(1,2,3).stream()
                    .map(i -> i*i)
                    .forEach(System.out::println);  // 1, 4, 9
```

세 번째로 Stream의 인덱스를 축소해 새로운 Stream을 만드는 limit

```
Arrays.asList(1,2,3).stream()
                    .limit(1)
                    .forEach(System.out::println);  // 1
```

네 번째로 Stream의 각 요소에 조건 비교를 통해 조건에 충족하는 요소만 모아 새로운 Stream을 만드는 filter

```
Arrays.asList(1,2,3).stream()
                    .filter(i -> 2 >= i)
                    .forEach(System.out::println);  // 1, 2
```

다섯 번째로 Stream을 단일 요소로 반환하는 reduce

```
Arrays.asList(1,2,3).stream()
                    .reduce((a,b) -> a-b)
                    .get();  // -4
```
<br>
<br>
<h4>JVM Memory Architecture</h4>

![runtimeDataArea](images/java/runtimeDataArea.PNG?raw=true "runtimeDataArea")
위의 그림과 같이 JVM은 메모리를 큰 카테고리로 2개, 세부 카테고리로 4개로 구성한다.  
좌측 METHOD AREA 및 HEAP 영역은 모든 쓰레드가 공유하는 메모리 영역이다. 반면 우측을 보면 각 쓰레드 별로 Stack과 PC Register 메모리가 할당되는 것을 볼 수 있다. 각 메모리의 역할을 보자.  

* METHOD AREA : 클래스 정보를 저장하는 영역. 클래스로더는 클래스 파일 바이트코드로부터 클래스를 로드한 후 JVM에게 정보를 넘긴다. JVM은 넘겨받은 정보를 통해 내부적으로 바이트코드를 다시 만든 후 METHOD AREA에 저장한다. METHOD AREA는 정적/동적으로 그 크기를 조절할 수 있고 JVM의 Garbage collector로부터 처리하는 메모리 영역이 아니다. 각 저장되는 정보는 다음과 같다.
   * Runtime Constant Pool : int/long/float/final String과 같은 상수, 클래스 타입, 속성과 같은 정보
   * Method Code : 메소드 구현 정보
   * FieldsValues : Runtime Constat Pool에 있는 클래스의 모든 필드 정보
* HEAP : 클래스 인스턴스 및 배열 인스턴스가 저장되는 영역이다. JVM이 기동되면서 같이 메모리 영역이 생성되고 마찬가지로 정적/동적으로 그 크기를 조절할 수 있다. 반면에 HEAP은 Garbage collector로부터 할당/해제 되는 메모리 영역이다. HEAP은 아래와 같이 구성된다.
![heapMemoryStructure](images/java/heapMemoryStructure.PNG?raw=true "heapMemoryStructure")
위의 그림을 보면 HEAP은 크게 Young Generation과 Old Generation으로 나뉜다.
   * Young : 새로운 클래스 인스턴스가 할당되는 영역이다. Young이 꽉차면 Garbage Collector는 Young에서 오래된 객체 인스턴스를 Old로 옮기게된다. 이를 Minor GC라고 한다. Young 영역은 3개의 영역으로 또 다시 나뉘게 된다.
      * Eden : 안전 최신 클래스 인스턴스는 Eden 영역에 할단된다.
      * Survivor1/2 : Eden 영역이 꽉차면 Minor GC가 발생하고 Old로 이동 안한 객체는 Survivor로 이동하게 된다. 또한 Garbage Collector는 2개의 Survivor중 하나의 Survivor은 항상 비어있도록 메모리 영역을 조절한다. 오랜시간 Survivor에 남아있는 객체는 마침내 Old 영역으로 이동하게 된다.
   * Old : 이 영역은 Minor GC 때 Young으로부터 넘어온 객체들이 할당되는 영역이다. Old가 꽉 차면 마찬가지로 Garbage Collector가 동작하여 Old 영역에서 메모리를 해제하게 된다. 이를 Major GC라고 하고 이 때는 시간이 굉장히 오래걸리고 프로세스가 일시적으로 동작을 중지하게 된다.
   * Perm : 클래스와 메소드에 의해 사용되는 어플리케이션 메타 데이터를 가지고 있는 영역. Perm은 클래스에 의해 런타임에 할당되고 Java SE 라이브러리 클래스와 메소드들드 Perm 영역에 포함된다. Perm의 Object는 Major GC때 해제된다. **주의)** Java8부터 Perm 영역은 사라지고 Metaspace라는 영역이 생기게 되었다. Heap에 할당되는 Perm과 달리 Metaspace는 Heap 영역이 아닌 Native memory에 위치하게 된다. 위에서 설명한 METHOD AREA는 Perm영역의 일부이다.
* PC Register : 현재 JVM이 실행하는 Context(어느 쓰레드의 어느 메소드)를 가리키는 포인터이다.
* JVM Stack : 현재 쓰레드가 실행하는 Context정보(called Frame)를 저장하는 Stack 메모리이다. Frame에는 메소드 지역 변수, 상수 Reference와 같은 정보를 가지고 있다.
 

<br>
<br>
<h4>쓰레드 덤프 분석하기</h4>

* Java 쓰레드 배경 지식
   * 쓰레드 동기화 : 쓰레드는 다른 쓰레드와 동시에 실행할 수 있다. 쓰레드가 공유자원을 사용할 땐 정합성 보장을 위하 하나의 쓰레드만 접근 가능하도록 제한해야 한다. Java는 Monitor를 통해 하나의 Java 객체에 접근은 하나의 쓰레드만 할 수 있도록 보장한다. 쓰레드는 객체 접근 시, Monitor를 획득해야 하고 Monitor가 없는 객체에 접근하기 위해선 Monitor가 해제될 때까지 Wait Queue에서 대기하게 된다.
   * 쓰레드 상태
      * NEW : 쓰레드가 생성되었지만 아직 실행되지 않은 상태
      * RUNNABLE : 현재 CPU를 점유하고 작업을 수행중인 상태. **운영체제의 자원 분배로 인해 WAITING 상태가 될 수도 있다**
      * BLOCKED : Monitor 획득을 위해 기다리는 상태
      * WAITING : wait(), join() 메서드 등을 이용해 대기하고 있는 상태
      * TIMED_WAITING : WAITING과 마찬가지로 대기하지만 sleep과 같이 특정 시간동안 대기하는 상태
   * 쓰레드 종류
      * 데몬 쓰레드 : 프로세스의 비데몬 쓰레드가 죽으면 동작을 중지하는 쓰레드로 일반적으로 어플리케이션이 동작하는데 도움을 주는 역할을 한다(e.g. Garbage Collector)
      * 비데몬 쓰레드 : 데몬 쓰레드가 아닌 쓰레드로 일반적인 쓰레드를 의미한다.
* 쓰레드 덤프 획득하기
   * jstack 활용하기  
      1. 프로세스 PID 획득하기 : jdk의 명령어를 활용한다. e.g) jps -v
      2. 단계 1에서 확인한 PID를 통해 쓰레드 덤프 획득 : jdk 명령 활용. e.g) jstack ${PID}
   * Java VisuamVM (GUI 프로그램) 사용하기
   * kill 활용하기
      1. 리눅스 환경에서만 가능하다. 먼저 ps -ef | grep java 와 같은 명령어로 PID를 획득한다.
      2. 획득한 PID를 통해 kill -3 ${PID} 또는 kill -QUIT ${PID} 명령어를 통해 쓰레드 덤프를 획득한다.
* 쓰레드 덤프의 정보
   * 쓰레드 이름 : 덤프 가장 첫줄의 pool-1-thread-13 가 쓰레드 이름이 된다.
   * 우선 순위 : 쓰레드의 우선순위로 첫 줄의 prio=6 이 우선순위를 의미한다.
   * 쓰레드 ID : 쓰레드의 ID로, 이 정보를 통해 쓰레드의 CPU 사용, 메모리 사용 등의 유용한 정보를 알 수 있다.
   * 쓰레드 상태 : 쓰레드의 상태를 의미한다. 첫 줄의 runnable [0x...] 가 쓰레드 상태이다.
   * 쓰레드 콜스택 : 쓰레드의 스택 트레이스를 의미한다.

```
"pool-1-thread-13" prio=6 tid=0x000000000729a000 nid=0x2fb4 runnable [0x0000000007f0f000]

java.lang.Thread.State: RUNNABLE

at java.net.SocketInputStream.socketRead0(Native Method)

at java.net.SocketInputStream.read(SocketInputStream.java:129)

at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:264)

at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:306)

at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:158)

- locked <0x0000000780b7e688> (a java.io.InputStreamReader)

at java.io.InputStreamReader.read(InputStreamReader.java:167)

at java.io.BufferedReader.fill(BufferedReader.java:136)

at java.io.BufferedReader.readLine(BufferedReader.java:299)

- locked <0x0000000780b7e688> (a java.io.InputStreamReader)

at java.io.BufferedReader.readLine(BufferedReader.java:362)
```
   


<br>
<br>
<h4>버블 정렬</h4>
버블 정렬 알고리즘은 최악의 경우(역순 정렬) N^2의 시간 복잡도를 가지고 최선의 경우(정렬) N의 시간복잡도를 가진 알고리즘이다. 알고리즘의 실행은 다음과 같은 방식으로 진행한다.  

1. N 배열을 0 ~ N까지 방문 하면서 i원소가 i+1원소보다 비교 우선순위가 낮으면 서로 교체한다.
2. 단계 1에서 교체 작업이 한번이라도 일어나면 1번을 다시 반복한다.  

의사코드는 다음과 같다.
```
for i between 0 and (array length - 2):
   if(array(i+1) < array(i)):
      switch array(i) and array(i+1)

repeat until a complete iteration where no elements are switched.
```

<br>
<br>
<h4>삽입 정렬</h4>
하나의 추가 배열을 만들고, 정렬할 배열을 순회하면서 새로운 배열에 순서를 유지하며 insert 하는 알고리즘이다. 최악의 경우(정렬) N^2, 최선의 경우(역순 정렬) N의 시간 복잡도를 가진다. 의사코드는 다음과 같다.

```
Given a list l and new list nl
for each element originalItem in list l:
   for each element newItem in nl:
      if(originalItem > newItem) :
         insert originalItem before newItem
      else :
         move to next element
   if no item has been inserted:
      insert originalItem at end of nl
```

<br>
<br>
<h4>퀵 정렬</h4>
임의의 원소를 하나 정하고(이를 pivot 이라고 칭한다) 이 원소보다 우선순위가 낮은 원소를 모아놓는 배열과 높은 원소를 모아놓는 배열을 각각 만들어 원소들을 삽입한다. 이 각각의 추가된 배열들을 재귀적으로 계속 호출하고 마지막에 '작은 배열 + Pivot + 큰 배열' 방식으로 합병한다. 각 배열을 순회하는 횟수는 N이고 재귀는 전체 배열 크기의 절반만큼 호출 되므로 시간 복잡도는 NlogN이 된다 의사코드는 다음과 같다.

```
method quickSort(list l):
   if l.size < 2:
      return l
   
   var pivot= l(random number)
   for each element e in l:
      if e < pivot:
         add e to lowerList
      else:
         add e to biggerList

   var sortedLower= quickSort(lowerList)
   var sortedBigger= quickSort(biggerList)
   return sortedLower + pivot + sortedBigger
```

<br>
<br>
<h4>병합정렬</h4>
퀵정렬과 비슷한 방식으로 배열의 가운데 인덱스를 정해 인덱스가 더 작은 부분과 더 큰 부분으로 리스트를 나눠 생성하고 이를 재귀적으로 계속 호출한다. 이 후 합병할 때 각 두 개의 리스트를 차례로 방문하면서 더 작은 원소를 새로운 리스트에 앞에서부터 차례대로 삽입한다. 퀵정렬과 마찬가지로 NlogN의 시간 복잡도를 가진다. 의사코드는 다음과 같다.

```
method mergeSort(list l):
   if l.size < 2:
      return l
      
   var m= l.size / 2
   list left= l[0..m]
   list right= l[m..end]
   
   var sortedLeft= mergeSort(left);
   var sortedRight= mergeSort(right);
   
   return merge(sortedLeft, sortedRight);
   
method merge(list left, list right):
   var sortedList
   var leftIdx= 0
   var rightIdx= 0
   
   while(leftIdx < left.length and rightIdx < right.length):
      if(left[leftIdx] < right[rightIdx]):
         add left[leftIdx] to sortedList
         leftIdx++
      else:
         add right[rightIdx] to sortedList
         rightIdx++
         
   while(leftIdx < left.length):
      add left[leftIdx] to sortedList
      leftIdx++
   
   while(rightIdx < right.length):
      add right[rightIdx] to sortedList
      rightIdx++

   return sortedList
```

<br>
<br>
<h4>이진 검색</h4>
일반적인 배열 데이터에서 특정 값을 찾으려면 모든 원소를 순회해야 하지만 정렬되어있는 배열이라면 이진 검색을 통해 시간 복잡도 N의 시간에 검색을 완료할 수 있다. 배열의 가운데 값(인덱스 M)과 찾으려는 값을 비교하여 우선순위가 높으면 배열의 [0...M]을, 낮으면 [M+1... END]를 재귀적으로 다시 비교하면 된다. 의사코드는 아래와 같다.

```
method binarySearch(list l, element e):
   var m= l.length / 2
   
   if(l[m] = e):
      return true
   else(l[m] < e):
      return binarySearch(l[m+1... end])
   else:
      return binarySearch(l[0... m])
```

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>

<br>
<br>
<h4></h4>
