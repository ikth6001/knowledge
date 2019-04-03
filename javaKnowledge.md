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

