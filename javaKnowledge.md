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
이미지
위의 그림과 같이 JVM은 메모리를 큰 카테고리로 2개, 세부 카테고리로 4개로 구성한다.  
좌측 METHOD AREA 및 HEAP 영역은 모든 쓰레드가 공유하는 메모리 영역이다. 반면 우측을 보면 각 쓰레드 별로 Stack과 PC Register 메모리가 할당되는 것을 볼 수 있다. 각 메모리의 역할을 보자.  

* METHOD AREA : 클래스 정보를 저장하는 영역. 클래스로더는 클래스 파일 바이트코드로부터 클래스를 로드한 후 JVM에게 정보를 넘긴다. JVM은 넘겨받은 정보를 통해 내부적으로 바이트코드를 다시 만든 후 METHOD AREA에 저장한다. METHOD AREA는 정적/동적으로 그 크기를 조절할 수 있고 JVM의 Garbage collector로부터 처리하는 메모리 영역이 아니다. 각 저장되는 정보는 다음과 같다.
   * Runtime Constant Pool : int/long/float/final String과 같은 상수, 클래스 타입, 속성과 같은 정보
   * Method Code : 메소드 구현 정보
   * FieldsValues : Runtime Constat Pool에 있는 클래스의 모든 필드 정보
* HEAP : 클래스 인스턴스 및 배열 인스턴스가 저장되는 영역이다. JVM이 기동되면서 같이 메모리 영역이 생성되고 마찬가지로 정적/동적으로 그 크기를 조절할 수 있다. 반면에 HEAP은 Garbage collector로부터 할당/해제 되는 메모리 영역이다. HEAP은 아래와 같이 구성된다.
이미지
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

