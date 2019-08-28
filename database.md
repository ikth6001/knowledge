<h3>데이터베이스 내용 정리</h3>


<h4>정규화와 비정규화</h4>
정규화는 데이터의 중복을 최소화 하기 위해 테이블을 나누는 설계를 의미한다. 필요한 데이터 조회를 위해 join을 해야 하므로 조회 시 성능이 떨어지게 된다.
반면 비정규화는 조회 성능을 올릴 수 있도록 설계하는 방식을 의미한다.

<br><br>

<h4>Join</h4>

* 내부조인 : 조건에 부합되는 데이터만 결과 집합에 포함되는 조인
* 외부조인
  + 좌측 외부 조인 : 왼쪽 테이블은 모든 데이터가 포함되고 오른쪽 테이블 중 조건에 안맞는 데이터는 null로 채워진다.
  + 우측 외부 조인 : 오른쪽 테이블에 대하여 좌측 외부 조인과 같은 특성 적용
  + 완전 외부 조인 : 왼쪽 오른쪽 모두 모든 데이터가 집합에 포함된다.

<br><br>

<h4>ACID</h4>  

* 원자성(Atomicity)
트랜잭션과 관련된 작업들이 부분적으로 실행되다가 중단되지 않는 것을 보장하는 능력이다.
예를 들어, 자금 이체는 성공할 수도 실패할 수도 있지만 보내는 쪽에서 돈을 빼 오는 작업만 성공하고 받는 쪽에 돈을 넣는 작업을 실패해서는 안된다.
원자성은 이와 같이 중간 단계까지 실행되고 실패하는 일이 없도록 하는 것이다.

* 일관성(Consistency)
트랜잭션이 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것을 의미한다.
무결성 제약이 모든 계좌는 잔고가 있어야 한다면 이를 위반하는 트랜잭션은 중단된다.

* 고립성(Isolation) 
트랜잭션을 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 것을 의미한다.
이것은 트랜잭션 밖에 있는 어떤 연산도 중간 단계의 데이터를 볼 수 없음을 의미한다.
은행 관리자는 이체 작업을 하는 도중에 쿼리를 실행하더라도 특정 계좌간 이체하는 양 쪽을 볼 수 없다.
공식적으로 고립성은 트랜잭션 실행내역은 연속적이어야 함을 의미한다. 성능관련 이유로 인해 이 특성은 가장 유연성 있는 제약 조건이다.

* 지속성(Durability)
성공적으로 수행된 트랜잭션은 영원히 반영되어야 함을 의미한다. 시스템 문제, DB 일관성 체크 등을 하더라도 유지되어야 함을 의미한다.
전형적으로 모든 트랜잭션은 로그로 남고 시스템 장애 발생 전 상태로 되돌릴 수 있다.
트랜잭션은 로그에 모든 것이 저장된 후에만 commit 상태로 간주될 수 있다.
<br><br>

<h4>Isolation level</h4>
트랜잭션 실행 중 중간 연산 결과가 다른 트랜잭션으로 접근 불가하도록 하는 고립성을 유지하기 위한 데이터를 허용하는 수준  

1. 병행제어 미처리 시 발생하는 문제 유형
      * 부정판독(dirty read) : commit 되지 않은 데이터를 읽을 수 있음. 데이터를 읽었는데 만약 해당 데이터가 rollback 되면 없는 데이터를 처리하는 결과가 된다.
      * 비반복판독(Nonrepeatable read) : commit된 데이터만 읽음. 한 트랜잭션에서 같은 조건의 row를 두번 읽는데 그 사이 그 row가 수정/삭제되면 쿼리 결과가 달라짐.
      * 가상판독(phantom read) : 특정 조건의 복수개 row를 두번 조회하는데 그 사이 다른 트랜잭션이 insert를 하여 값이 다르게 조회되는 현상
2. isolation level  
      * Read uncommited : commit이 안된 데이터도 읽을 수 있음. shared lock이 없는 level. diry read, Nonrepeatable read, phantom read 모두 발생할 수 있음.
      * Read commited : commit된 데이터만 읽을 수 있음. DBMS별로 shared lock이 걸릴수도 아닐수도 있다. Nonrepeatable read, phantom read가 발생할 수 있음.
      * Repeatable read : 선행 트랜잭션이 읽은 row를 후행 트랜잭션이 수정/삭제 못하도록 shared lock을 걸음. Phantom read가 발생할 수 있음.
      * Serializable : 선행 트랜잭션이 읽은 데이터를 수정/삭제도 못하고 중간에 새로운 레코드를 추가할 수도 없도록 함. Repeatable read 수준에서 범위에 대한 shared lock을 걸음. 직렬성 위반 발생 불가.
3. 주의점  
isolation level이 높다고 무조건 좋은것이 아님. isolation level이 높으면 그만큼 성능이 떨어짐.

<br><br>

<h4>오라클 튜닝 교육 1일차</h4>  

* 오라클 딕셔너리 테이블  
1. DBA_ : 모든 Admin 권한을 가진 계정만 접근 가능
2. ALL_ : 사용자가 접근 가능한 범위 내의 뷰
3. USER_ : 사용자 계정에 존재하는 뷰


* SQL 작성 시 대/소문자, 공백을 통일하는 이유는 오라클이 동일 SQL인지 판단할 수 있도록 하기 위해서이다. 동일성을 판단 함으로써 SQL 하드/소프트 파싱 사용 여부에 영향을 끼친다.  
1. 하드파싱 : SQL과 실행계획을 캐시에서 찾지 못해 최적화 과정을 거치고 나서 실행단계로 넘어가는 경우
2. 소프트파싱 : SQL과 실행계획을 캐시에서 찾아 곧바로 실행단계로 넘어가는 경우  


* DELETE, TRUNCATE, DROP 차이
DELETE는 수행하더라도 실제 데이터를 테이블 스페이스에서 바로 삭제 안하므로 복구가 가능하지만 TRUNCATE는 복구가 불가능하다. 때문에 실제로 DELETE를 오랜시간 한 테이블은 SELECT COUNT를 할 때, 시간이 오래 걸릴 수 있다. 이 문제를 해결하기 위해선 테이블스페이스를 정리해주어야 함(alter tablespace ${name} coalesce) DROP은 데이터뿐 아니라 테이블까지 삭제하며, 복구가 불가능하다.


* 오라클 인스턴스가 하나 생성되면 오라클은 데이터베이스(디스크)와 이를 엑세스하는 프로세스가 생성된다. 그리고 각 인스턴스들이 공통으로 사용하는 
공유 메모리 캐시 영역(SGA)가 있다. SGA 영역이 튜닝 시점에 관심을 가져야 하는 부분임. 인스턴스는 SGA + 백그라운드 프로세스로 구성됨. 오라클의 전체적인 구조는 아래와 같다.  

![oracleStructure](images/database/oracleStructure.PNG?raw=true "oracleStructure")


* SGA는 다음과 같이 구성 된다.
1. Shared Pool
	- Library Cache : SQL에 대한 실행 계획을 만들고 저장한다.
	- Data dictionary : 오브젝트 권한 정보를 디스크를 통해 가져오면 오래 걸리므로, 메모리에 캐쉬 해놓는다.
2. Database buffer cache : 기본 조회성 쿼리 데이터를 캐쉬하고 있는다. 없으면 디스크로부터 가져온다.(LRU 캐쉬)
	- DB_CACHE_SIZE : Database buffer cache의 크기 정보를 가진 파라미터(변수)
	- DB_BLOCK_SIZE : 오라클에서 다루는 가장 작은 크기의 데이터 단위인 블락의 크기 정보를 가진 파라미터(변수)
	...
3. Redo log buffer : 실제 명령어를 실행하기 전 데이터 변경에 대한 로그 기록. 명령어 수행 중 장애로 인해 DB가 죽으면 해당 데이터 변경에 대하여 Commit을 수행 했는지 확인하고, Commit을 안하면 다시 Rollback 처리를 한다. Redo log buffer의 데이터는 disk의 Redo log file에 작성 되는데, 설정에 의해 Redo log file의 갯수는 정해져 있는데, Redo log file이 전부 꽉 차면, Redo log file 갯수가 늘어나는게 아니라 덮어쓰게 되는데, 이 때 Archive log를 사용한다.


* 오라클 ASM이란 오라클에서 파일들이 저장되는 Storage를 관리하는 방식을 의미한다. 오라클에선 OS가 아닌 이 ASM에게 데이터 저장/불러오기 요청을 하고, 성능 및 편의성이 향상 되었다.


* 오라클 Connection 첫 접속 시 리스너에 연결 요청을 하면 인스턴스를 띄우고 인스턴스가 사용할 PGA 메모리를 할당한다(부하가 큰 작업). 이 후 요청은(연결이 끊기지 않은 상태) 바로 인스턴스에 요청을 통해 SQL 작업을 하게 된다. 때문에 접속을 끊지 않고 Connection Pool을 만들어 사용한다.


* 고 가용성을 위한 오라클 기능  
1. HA : 두 대의 DBMS 서버를 구성하고 하나는 Active 하나는 Standby로 설정하여 Active 서버가 다운되면 Standby를 이어서 사용할 수 있도록 구성하는 방법이다. Standby 서버는 사용안되는데 비해 Active 서버는 바쁜 상태이기 때문에 효율이 좋지 못하다.  
2. OPS : HA의 단점을 보완한 방법으로 하나의 Storage 공간과 두 개의 인스턴스가 있고 두 인스턴스 모두 Active 상태로써 효율을 높이는 방법이다. 하지만 RAC Ping이란 문제가 발생할 수 있다. RAC Ping이란 인스턴스 A가 데이터를 변경하고 완료했을 때 인스턴스 B가 그 내용을 조회한다면 인스턴스 A가 디스크에 반영을 완전히 한 후에 인스턴스 B가 가져올 수 있다는 의미로 성능저하의 원인이 된다.  
3. RAC : OPS의 RAC Ping 문제를 해결한 방법으로, 인스턴스 A와 인스턴스 B를 연결하는 interconnect라는 구성을 추가하여 데이터 접근을 위해 디스크에 접근 안하고 각 인스턴스의 메모리의 데이터를 interconnect를 통해 교환하는 구조이다.


* 버퍼 캐시 구조
1. 버퍼 블록을 변경하면, 해당 블록은 Dirty 블럭으로 변경되고 버퍼를 지우면 Free 블럭으로 변경된다. 처음 SGA를 구성하여 깨끗한 블럭을 Free 블럭이라고 한다.


* Redo는 Redo로그의 엔트리를 의미한다. Redo는 Database/Cache Recovery, Fast Commit을 위해 사용한다. Fast Commit 이란 데이터를 실제 디스크에 반영은 안하고 반영한다고 마킹만 해놓고, 백그라운드로 디스크 반영 작업을 하는 것을 말한다.


* 실행 계획(AUTO TRACE)
1. 예상 실행 계획 : 예상되는 실행 계획( EXPLAIN PLAN FOR > SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY) )
2. 실제 실행 계획 : 실행하여 구한 실행 계획( SET AUTOTRACE ON )


* ROWID : 데이터가 저장된 주소(데이터에 접근하기 위한), ROWNUM : 쿼리 결과 데이터 Row에 채번한 것.


* 인덱스를 통한 검색 시엔, 인덱스를 통해 하나의 블럭 주소를 파악 및 접근(Single Block IO)한 후, 데이터를 가져오는 방식을 사용하고, 인덱스를 사용 안할 경우, 한 번에 복수 개의 블럭을 한 번에 접근(Multi Block IO)하여 데이터를 가져온다. 때문에, 전체 데이터의 극히 일부, 적은 데이터를 가져오는 경우엔 인덱스 검색이 효율적이지만 대부분의 데이터를 가져올 땐, 인덱스 없이 한 번에 많은 블럭에 접근하여 검색하는 것이 유리하다(I/O횟수가 줄어들기 때문).


* ORDER/GROUP BY 사용시 PGA 메모리를 기본적으로 사용하고 PGA가 모자라면 TEMP 영역을 사용한다. 때문에 정렬되어있는 인덱스를 사용하면 빨라질 수 있다.


* 병렬 프로세싱
```
(/*+ full(${테이블}) parallel(${테이블} ${프로세스 수} */
```
내부적으로 DBMS가 프로세스를 띄워 작업을 병렬적으로 수행하는 것을 의미한다. 또는 다음과 같이 사용할 수 있다. 
```
/*+ index_ffs(${테이블}) parallel_index(...) */
```
여기서 index_ffs 힌트는 index를 디스크로부터 가져올 때, single block io를 사용 안하고 multi block io를 사용한다는 의미이다. 대신 인덱스를 통한 순서를 보장할 수 없다.


* Clustering Factor : 디스크 블럭으로부터 데이터를 가져올 때, 인덱스 A, 인덱스 B가 같은 블럭일 경우 두 번 disk에 접근할 필요 없이 한 번만
접근하면 된다. 이런 경우 Clustering Factor = 1이라고 한다. 하지만 A, C는 같은 블럭, B는 다른 블럭인 경우엔 CF=3이 된다.
```
SQL>	create table t
	as
	select * from all_objects
	order by object_id;

SQL>	create index t_object_id_idx on t(object_id);
```
CF가 좋을 확률이 높다. 왜냐하면 object_id로 정렬하여 테이블에 insert한 후, 해당 컬럼으로 인덱스를 만드므로, 인덱스 순서대로 disk의 블록에 저장될 확률이 높기 때문이다.


* 오라클 옵티마이저는 규칙/비용 기반 두가지 방식으로 실행계획을 생성 하는데, 비용 기반 방식을 사용 하려면 여러가지 통계 정보가 필요하다.
이 통계 정보를 생성하려면, EXEC DBMS_STATS.GATHER_TABLE_STATS('${유저}', '${테이블}'); 명령을 수행한다.


<br><br>

<h4>오라클 튜닝 교육 2일차</h4>  

* 실행계획을 세우는 옵티마이저는 두가지로 구성된다.
1. 룰 기반 옵티마이저 : 룰을 기반으로 실행계획을 세운다(@deprecated)
2. 통계(비용) 기반 옵티마이저 : 통계 정보를 기반으로 실행계획을 세운다. 통계 정보엔 테이블 인덱스 기반 row, CF 등등의 정보 포함
	- Selectivity : 조건을 만족하는 데이터가 차지하는 비율
	- Cardinality : Selectivity * 전체 데이터
	- Cost : Selectivity, Cardinality를 통해 구한 비용


* 인덱스의 종류
1. B Tree : 이진트리 형태로 구성되는 인덱스. 실제 컬럼 값을 인덱스에도 보관하고 있어야 하기에 대용량 데이터 관리시 부담이 되고 컬럼 값의 분포도가 좋아야 효율을 발휘할 수 있다.
2. BitMap : bit 값으로 만들어진 인덱스. 컬럼 값에 대하여 특정 값이 있는 시작점부터 끝나는 점까지 위치를 기록한 후 bit 데이터로 변경. (사용하는 메모리가 적고, 컬럼 값이 남자, 여자 와 같이 분포도가 적은 데이터에 사용). DML 발생시 부하가 굉장히 크다. single block IO를 사용하는건 B Tree와 같지만 데이터가 모여있어 성능이 좋음.
3. Reverse Key : 기본적으로 B Tree 인덱스와 마찬가지로 이진트리로 구성한다. 하지만 예를 들어 사원번호 컬럼이 B Tree 인덱스인 경우 사원 번호는 계속 증가하므로 이진트리의 우측 leaf에 계속 데이터가 쌓여 경합이 발생하게 된다. 이러한 한쪽으로의 데이터 경합을 방지하기 위한 방법으로 reverse key는 인덱스 컬럼의 값을 거꾸로 바꾸어 저장한다. 예를 들어 사원번호가 12345이면 54321로 만들어 저장하는 것이다.
4. Function Based : 오라클은 묵시적으로 형변환을 해주는데(예를 들어 VARCHAR2 컬럼에 조건으로 = 10과 같이 한 경우 컬럼의 문자를 숫자로 변경. 숫자가 문자보다 우선순위 높음) 이런 묵시적 형변환 때문에 인덱스 컬럼이 가공되면거나 부정(<>) 비교 조회 시, 인덱스를 사용 안하게 된다. Function Based는 어쩔 수 없이 인덱스 컬럼에 가공을 해야할 경우 해당 가공한 데이터를 인덱스로 만드는 방법이다. DML 성능 저하 및 인덱스 유연성이 저하된다.


* 인덱스 선정
테이블 아키텍처 선정 > 엑세스 Path 검토 > 결합/단일 인덱스 고려 > 인덱스 선정 > 검토


* 랜덤 엑세스(인덱스에 작성되어 있는 rowId를 통해 테이블 데이터에 접근하는 것)의 종류
1. 추출 랜덤 엑세스(COL3, COL4) : WHERE COL3='AAA' AND COL4='BBB'
2. 확인 랜덤 엑세스(COL3, COL5) : WHERE COL3='AAA' AND COL4='BBB' AND COL5 LIKE 'AA%'
3. 정렬 랜덤 엑세스(COL3, COL4) : WHERE COL3='AAA' AND COL4='BBB' ORDER BY COL5


* 조건절의 순서
equal 비교를 점조건, 범위 비교(between)을 선조건이라고 한다. and 절로 복 수개의 조건이 올 경우 점 조건이 선조건보다 앞에 오는 것이 더 효율이 좋다.

* 뷰
뷰는 복잡한 테이블 관계를 간단한 SQL로 확인할 수 있도록 하는 오브젝트. 뷰는 실제로 데이터를 저장하는 것이 아니라 이 뷰를 보여주기 위한 SQL 정보를 저장한다.

* 실행 계획 보기
(본 교육에선) DBMS_XPLAN 패키지에서 제공하는 DISPLAY와 DISPLAY_CURSOR 기능을 사용하여 실행계획을 확인한다.
1. DISPLAY : 예상 실행 계획을 보여주는 function
2. DISPLAY_CURSOR : 실제 실행한 실행 계획을 보여주는 function


* 실행 계획 정보
1. Predicate information - access : 실제 조회 데이터 대상을 줄이는 컬럼(인덱스에 포함)
2. Predicate information - filter : access를 통해 줄인 데이터 대상을 실제 테이블 access 하면서 실제 조건에 맞는 데이터를 찾는데 사용된 컬럼


* 실행 계획 보기 예시(1)

1. SQL
```
SELECT /* KTH */ *
FROM SCOTT.DEPT A, SCOTT.EMP B
WHERE A.DEPTNO = B.DEPTNO
AND B.EMPNO = 7566;
```

2. 실행 계획
```
SQL_ID  0typ7yyk05p9f, child number 0
-------------------------------------
SELECT /* KTH */ * FROM SCOTT.DEPT A, SCOTT.EMP B WHERE A.DEPTNO = 
B.DEPTNO AND B.EMPNO = 7566
 
Plan hash value: 2385808155
 
----------------------------------------------------------------------------------------
| Id  | Operation                    | Name    | Rows  | Bytes | Cost (%CPU)| Time     |
----------------------------------------------------------------------------------------
|   0 | SELECT STATEMENT             |         |       |       |     2 (100)|          |
|   1 |  NESTED LOOPS                |         |     1 |    59 |     2   (0)| 00:00:01 |
|   2 |   TABLE ACCESS BY INDEX ROWID| EMP     |     1 |    39 |     1   (0)| 00:00:01 |
|*  3 |    INDEX UNIQUE SCAN         | PK_EMP  |     1 |       |     0   (0)|          |
|   4 |   TABLE ACCESS BY INDEX ROWID| DEPT    |     4 |    80 |     1   (0)| 00:00:01 |
|*  5 |    INDEX UNIQUE SCAN         | PK_DEPT |     1 |       |     0   (0)|          |
----------------------------------------------------------------------------------------
 
Predicate Information (identified by operation id):
---------------------------------------------------
 
   3 - access("B"."EMPNO"=7566)
   5 - access("A"."DEPTNO"="B"."DEPTNO")
```

3. 해석할 땐 가장 깊은 depth 부터 읽고 동일 depth 끼린 가장 위부터 읽는다.

- EMP에서 PK_EMP unique index를 통해 인덱스 스캔(3 라인)
- 인덱스 스캔 으로부터 테이블에 row Id access(2 라인)
- DEPT의 PK_DEPT unique index로부터 인덱스 스캔(실제 데이터가 4개 인데 1개만 읽음 -> join 때문임 TOBE 설명)
- 인덱스 스캔 으로부터 테이블에 row Id access(4 라인)
- NESTED LOOPS : TOBE 설명(1 라인)
- 조회(0 라인)


* 실행 계획 보기 예시(2)

1. SQL
```
SELECT /* KTH(2) */ *
FROM SCOTT.EMP
WHERE DEPTNO = 30
ORDER BY EMPNO, ENAME;
```

2. 실행 계획
```
SQL_ID  31a2japg8t3gn, child number 0
-------------------------------------
SELECT /* KTH(2) */ * FROM SCOTT.EMP WHERE DEPTNO = 30 ORDER BY EMPNO, 
ENAME
 
Plan hash value: 150391907
 
-----------------------------------------------------------------------
| Id  | Operation          | Name | E-Rows |  OMem |  1Mem | Used-Mem |
-----------------------------------------------------------------------
|   0 | SELECT STATEMENT   |      |        |       |       |          |
|   1 |  SORT ORDER BY     |      |      4 |  2048 |  2048 | 2048  (0)|
|*  2 |   TABLE ACCESS FULL| EMP  |      4 |       |       |          |
-----------------------------------------------------------------------
 
Predicate Information (identified by operation id):
---------------------------------------------------
 
   2 - filter("DEPTNO"=30)
 
Note
-----
   - Warning: basic plan statistics not available. These are only collected when:
       * hint 'gather_plan_statistics' is used for the statement or
       * parameter 'statistics_level' is set to 'ALL', at session or system level
```

3. 해석
- 정렬을 했기 때문에 메모리를 사용함(1 라인)

4. 튜닝 방안
DEPTNO, EMPNO, ENAME 컬럼을 가지는 인덱스를 생성한다.


* Index full scan 과 Index fast full scan
1. Index full scan은 single Block IO로 전체 인덱스를 스캔하여, 정렬이 유지된다.
2. Index fast full scan은 multi Block IO로 전체 인덱스를 스캔하여, 정렬이 유지되지 않는다. 단, unique, not null 이어야 한다.

* 단일 인덱스가 많은 경우 실행 계획에 AND EQUALS 라고 뜨는 경우가 있는데 기본적으로 조회 시, 복수 개의 인덱스를 사용하지 않으므로 먼저 하나의 인덱스에서 찾은 후 다음 인덱스로 이동해 찾으므로 성능이 더 느림.(따라서 복합 인덱스를 쓰자..)

* SKIP SCAN(http://www.gurubee.net/lecture/2266)
예를 들어, 다음과 같은 쿼리가 있다.
```
SELECT * FROM TABLE WHERE COLA='A'
```
하지만 인덱스가 COLA, COLB 컬럼인 경우엔 인덱스를 통한 조회를 안하게 된다. WHERE 절에서는 COLA만 사용 했으므로... 과거엔 이를 해결하기 위해 아래와 같이 쿼리를 작성하기도 했다.
```
SELECT * FROM TABLE WHERE COLA='A' AND COLB IN (전체 COLB 데이터)
```
이렇게 하면 인덱스를 사용하게 된다. 당연하겠지만 이 방식을 사용하는데엔 COLB 데이터의 분포도가 좁은 것이 유리하다(예를 들어 성별 테이블)


<br><br>

<h4>오라클 튜닝 교육 3일차</h4>  

* JOIN  
   - 조인은 항상 두 개의 테이블씩 조인한다. 세개의 테이블을 조인한다고 해도 먼저 두개를 조인한 후 나머지 한개를 결과와 조인한다.
   - 조인을 할 때 먼저 읽어야 할 테이블을 결정하는 것을 DRIVING 이라고 한다.  

* 조인 방식에 의한 분류
   - 중첩 루프 조인(Nested Loop Join) : 일반적인 방식의 조인으로 Driving 테이블을 기준으로 순차적으로 다른 테이블의 Row를 조인하여 원하는 데이터를 추출하는 방식이다. 적은 양의 데이터를 가져올 때 유용 > 성능 향상을 위해 PREFETCH, 배치 NLJ 등등 기능 제공
      a. 조인 키에 맞는 인덱스가 꼭! 존재해야 함.. 성능문제 때문..
      b. DRIVING 테이블은 대상 row가 적은 테이블이 되는 것이 좋다.
   - 소트 머지 조인 : 각 테이블을 조인 컬럼 기준으로 정렬한 후 조인을 하는 방식. 많은 양의 데이터를 조인할 때 사용. 정렬을 하므로 다소 느릴 수 있다.
      a. 해시 조인 나오기 전에 많이 사용한 조인 패턴.
   - 해시 조인 : Driving 테이블의 각 조인 컬럼 값을 해쉬함수를 통해 해쉬 테이블을 만든 후, 다음 테이블은 이 해쉬 테이블을 기준으로 Driving 테이블과 조인하는 방식이다. 많은 양의 데이터를 조인할 때 사용하고 소트 머지보다 빠르다.
      a. 해시 조인은 equal 조인이어야지만 사용이 가능하다. 아닌 경우 사용 불가능(해쉬 값을 정해진 상수를 변경하는 것이므로..)
      b. 병렬로 사용 가능 ! Inner 테이블(Probe 테이블)에 병렬을 주는 것이 좋음


* order by 절에 포함된 컬럼이 인덱스로 있는 경우 인덱스를 타는것이 맞을까? 인덱스를 타면 single block IO이기 때문에 대부분 전체 full scan 후 정렬 하는 것이 더 효율이 좋다.

* 파티셔닝 : 테이블을 적당한 단위로 나누어 각기 다른 테이블 스페이스에 저장하는 것을 의미한다. Full Scan 시 일부 파티션 세그먼트만 읽고 멈출 수 있다. 참고 : http://12bme.tistory.com/290  
   - 개선된 가용성
      - 파티션은 독립적으로 관리할 수 있다.
      - 백업 및 복구를 파티션 별로 작업할 수 있다.
      - 같은 테이블에서 문제가 생긴 파티션은 다른 파티션에 영향을 주지 않는다.
   - 관리의 용이성
      - 테이블스페이스 간에 파티션 이동이 가능하다.
      - 파티션 레벨에서 SELECT, DELETE, UPDATE가 가능하다.
   - 개선된 기능
      - 데이터를 액세스할 때 액세스 하는 범위를 줄여 퍼포먼스 향상을 가져올 수 있다.
      - RAC 환경에서 인스턴스간 Block Connection을 감소시킨다.

* 파티셔닝의 종류  
   1. Range 파티셔닝 : 범위를 기준으로 파티셔닝을 하는 것으로 가장 기초적인 파티셔닝. 주로 날짜 컬럼을 기준으로 함.
   e.g. CREATE TABLE 주문(...) PARTITION BY RANGE(${컬럼 명}) ( PARTITION ${파티션 이름} VALUES LESS THAN('${컬럼 값}') ... )
   2. Hash 파티셔닝 : 파티션 키에 해시 함수를 적용, 결과 값이 같은 레코드를 같은 파티션 세그먼트에 저장. 
   고객 ID와 같이 데이터 분포가 고른 컬럼에 효과적(해쉬 값이 고르게 나올 수 있기 때문)
   e.g. CREATE TABLE 주문(...) PARTITION BY HASH(${컬럼 명}) partitions ${파티션 갯수}
   3. List 파티셔닝 : 사용자에 의해 미리 정해진 그룹핑 기준에 따라 데이터 분할 저장
   e.g. CREATE TABLE 주문(...) PARTITION BY LIST(${컬럼 명}) partitions ${파티션 이름} values ('${컬럼 값}')


* Partition Pruning : 파티션 테이블에 쿼리/DML 수행할 때 원하는 파티션에 정확히 엑세스 하는 것을 의미한다. 같은 말로 SQL을 분석하여 엑세스 할 필요 없는 불필요한 파티션엔 접근을 안할 수 있어 성능이 향상된다고 이해할 수 있다(파티션 pruning을 하기 위해선 대상 키 컬럼이 조건절에 오면 된다(맨 앞에 올 필요는 없음)
   1. 정적 파티션 pruning : 파티션 키 컬럼을 상수 조건으로 조회하는 경우에 작동하며 쿼리 최적화 시점에 미리 결정된다.
   2. 동적 파티션 pruning : 파티션 키 컬럼을 바인드 변수로 조회하는(또는 조인으로 인해..) 경우에 작동하기에 실행 시점에 결정된다.


* Index partitioning : 인덱스를 파티셔닝 함으로써 테이블을 파티셔닝 한 것과 비슷한 효과를 가질 수 있다.
   1. 비 파티션 인덱스 : 파티셔닝 안된 인덱스
   2. 글로벌 파티션 인덱스 : 테이블 파티션 키와 인덱스 파티션 키가 서로 다른 경우. 테이블 파티션 삭제 시 기존 인덱스는 unusuable이 되어 인덱스 재생성을 해야 한다.
   3. 로컬 파티션 인덱스 : 각 인덱스 파티션 키와 테이블 파티션 키가 같은 경우. 파티션을 삭제 하더라도 인덱스를 재생성 할 필요가 없다.


<br><br>

<h4>오라클 튜닝 교육 4일차</h4>  

* Nested Loop Join
   - outer table : 먼저 처리되는 테이블. Driving 테이블이라고도 한다.
   - inner table : outer 테이블이 아닌, 나중에 처리되는 테이블. Driven 테이블이라고도 한다.
   - 고려사항(https://jojoldu.tistory.com/173)  
       - inner 테이블의 index가 존재해야 한다.  
       - outer 테이블의 결과 데이터가 적어야 한다.

* Sort Merge Join
   - 많이 사용 안하는 방식. 대용량 데이터 처리 시 사용. 기본적으로 index 사용 안한다.
   - 랜덤 엑세스 사용 안 함(기본적으로 index 사용 안하므로)
   - Join 키 컬럼이 equals 가 아닌 경우 Hash Join을 사용할 수 없으므로 사용한다.

* Hash Join
   - 첫 번째 읽히는 outer 테이블을 기준으로 Hash 테이블을 생성한다.
   - 두 개의 Hash 함수를 사용. 첫 번째 함수를 통해 비트맵 영역(파티션)을 만들고, 두 번째 함수를 통해 비트맵 영역 하위의 HASH 테이블(서브 파티션)을 생성한다.
   - 파티션은 PGA의 Hash Area에 저장된다. outer 테이블이 커 PGA 공간이 모자른 경우, PGA엔 temp disk 메모리에 있다는 표시를 하고 temp 영역에 저장한다.
   - temp를 사용 안하기 위해 outer 테이블이 작아야 하고, 해쉬함수를 사용하므로 equals 조건으로 조인해야 한다.
   - 실행계획을 통해 접근 block이 작다고 하더라도 Hash 함수를 통해 파티션 만드는 시간이 소모되기 때문에 예상보다 부하가 더 클 수 있다.
   - inner 테이블의 인덱스 순서를 기준으로 결과가 나온다! (Nested Loop Join은 outer 테이블 인덱스 기준으로 순서 보장)

* SEMI Join ( like 서브쿼리 ) : 조건문에 WHERE EMPNO IN ( SELECT ... ) 와 같이 조인과 유사하게 데이터를 연결하는 조인을 의미한다.
   1. 필터 처리 수행 방식 : Nested Loop Join과 유사한 방식이다. 다른 점은 outer 테이블에서 inner 테이블에 엑세스 하여 값을 확인하기 전에, 버퍼 라는 공간을 만들어 inner의 데이터를 넣어놓고 inner 엑세스 전에 버퍼를 확인하는 방식이다. 하지만 inner의 데이터가 중복 값이 아닌 경우 버퍼에 한 번 더 접근한 후 inner에 엑세스 하므로 오히려 성능이 저하될 수 있다.
   2. SEMI Hash 수행 방식 : Nested Loop Join 방식 대신 Hash Join 방식 사용 후 존재 여부만 확인하는 방식을 의미한다.

* ANTI Join : ( SEMI 조인 반대. not in.. )

* 카테시안 조인(Cross Join) : Join 조건이 없는 경우. 모든 가능한 데이터가 다 나오게 됨.. ( N * M )

* 인라인 뷰 ( From절 서브쿼리를 의미한다 )
   - 내부적으로 임시 뷰를 만들어 From 절의 서브쿼리 데이터를 처리하게 된다.
   - 뷰 오브젝트(쿼리를 저장한 오브젝트)는 인덱스를 만들 수 없다.
   - 뷰는 Hint 사용에도 어려움이 많다.
   - 옵티마이저에 의하여 인라인 뷰가 조인으로 변경될 수 있다 ( = 병합이 가능하다 )
   - 인라인 뷰와 테이블을 조인할 땐, 인라인 뷰를 일반적으로 outer 테이블로 해야 한다 ( 인덱스가 없으므로.. )

* 서브쿼리 팩토링 ( WITH 문 )
   - 반복되는 인라인 뷰를 WITH 문으로 정의하면 오라클이 임시로 오브젝트(뷰 또는 테이블)로 만들어 쿼리 성능을 향상 시키는 방법이다.
   - WITH에 대하여 오라클이 임시로 오브젝트를 만든다 ( 인라인 뷰는 쿼리 오브젝트 이지만 테이블이 될 수도 있음 )
   - 인라인 뷰와 마찬가지로 인덱스를 만들 수 없다.
   - 인라인 뷰와 마찬가지로 Join을 할 땐, outer 테이블이어야 한다.
   - temp 영역을 사용한다 > disk IO를 사용한다 ( 인라인 뷰로 사용 안 할 경우 )
   - 힌트를 통해 임시 테이블 또는 뷰로 만드는 것을 제어할 수 있다.

* ROWNUM
   - 가상컬럼으로, WHERE절 처리된 이후, 정렬(Sort, Aggregation)전에 처리된다.
   - 위의 특징 때문에 SELECT ROWNUM, ENAME FROM EMP ORDER BY ENAME 이라고 쿼리 작성시 ROWNUM이 뒤죽박죽이 된다.
   - ROWNUM 값은 할당된 이후에만 증가한다. 때문에 SELECT * FROM T WHERE ROWNUM > 1 이라는 쿼리는 결과 값을 반환 안한다(1보다 큰 값을 아직 할당 안 했으므로..)


<br><br>

<h4>오라클 튜닝 교육 5일차</h4>  

* 튜닝 예제

1. 문제  
```
-- 인덱스
create index tmp_1idx on tmp(col1,col2);

-- 튜닝 대상 SQL
select * from (
    select col1, col2, col3
    from scott.tmp 
    where  col1  >= 9789 and
           col2  >= 5500
    order by col1 desc, col2)
where rownum <=100;
```

2. 정답
```
select *
from(
    select /*+ ordered use_nl(v1, v2) */ v1.col1, v1.col2, v2.col3
    from(
        select *
        from(
            select /*+ INDEX_DESC(tmp tmp_1idx) */col1, col2
            from tmp
            where col1 >= 9789 and
                  col2 >= 5500
            order by col1 desc)
        where rownum <= 100) v1, tmp v2
    where v1.col1 = v2.col1 and
          v1.col2 >= 5500)
where rownum <= 100
order by col1 desc, col2;
```

3. 튜닝 기본 아이디어
   - col1이 desc이기 때문에 index를 order by 하는데 순수하게 바로는 사용 못함.
   - 정렬에 있어서 중요한 컬럼은 col1이므로 col1을 먼저 정렬하고, 100건을 가져온다.
   - 이후 상황에 맞게 조인 및 조건문을 추가한다.


* 서브쿼리(사용에 따른 구분)
   1. 스칼라 서브쿼리
      - Select 절의 컬럼 부분 서브쿼리(따라서 하나의 값만 리턴하며 NULL을 리턴할 수도 있다)
      - 무조건 Nested Loop Join 방식을 사용한다 (중요)
      - Nested Loop Join 기준 inner 테이블로 무조건 선정되므로 index가 있어야 한다.
      - 스칼라 서브쿼리의 결과 값이 대량이면 성능이 엄청 느려진다.
      - 결과 집합에 영향을 안준다는 점이 outer join과 비슷함. join으로 변경 시 outer join으로 변경한다.
      - 스칼라 서브쿼리는 캐쉬가 되기 때문에 함수를 사용하는 것보단 성능에 잇점이 있다.
   2. 서브쿼리
      - Where 절의 서브쿼리
   3. 인라인 뷰
      - From 절의 서브쿼리


* 서브쿼리(동작에 따른 구분)
   1. 후 수행 서브쿼리 : 서브쿼리 이외의 쿼리(메인쿼리)가 먼저 수행되고 서브쿼리가 나중에 수행 됨
      - 서브 쿼리가 반복 수행 되기에 서브 쿼리에 데이터가 많고, 중복된 데이터가 없으면 성능이 느리게 됨.
      - Driving 테이블은 조건에 만족하는 한 건의 데이터에 대해 서브쿼리를 한 번 수행한다. 서브쿼리를 수행하여 조건에 
        하면 결과로 추출한다.서브쿼리는 주 쿼리에서 추출하는 데이터의 건수만큼 반복수행한다.

   2. 선 수행 서브쿼리 : 서브쿼리를 먼저 수행하고 메인쿼리에 데이터를 제공함
      - Nested Loop Join과 비슷하게 동작함. 서브쿼리의 데이터를 가져온 후 존재 여부를 확인하고 Fetch(데이터 선택) 한다.
      - 조건 데이터를 제공하는 건 서브쿼리이므로 메인쿼리에 인덱스가 있어야 한다.
      - VIEW와 SORT(UNIQUE) 실행 계획이 생성된다. VIEW 실행계획은 메모리에 가상의 집합을 만드는 실행계획이다. 서브쿼리가 먼저 Access된다면 서브쿼리에서 조건에 만족하는 데이터를 한 번에 추출한다. 이와 같이 추출된 데이터를 메모리에 임시 저장하기 때문에 실행계획에는 VIEW 실행계획이 생성된다. 
        SORT(UNIQUE)는 유일한 값만 주 쿼리로 제공하여 주 쿼리의 데이블 Access 효율을 높이기 위해서이다.
        Driving 테이블인 서브쿼리의 테이블은 한 번에 모든 값을 Access하여 UNIQUE정렬을 수행한 후 메모리에 
        임시 집합으로 구성한다. Inner테이블은 Driving 테이블로부터 제공받은 조인 조건을 상수로 사용하여 처리 범위를 
        시킬 수 있다.


* 서브 쿼리 수행(세미 조인) : 조건을 만족하는 모든 데이터에 엑세스 하는 것이 아니라 조건을 만족하는 데이터 중 몇 건의 데이터만 확인하고 SQL을 종료하는 처리를 n-ROW 처리라고 한다. 데이터의 존재 유무를 확인하는 경우(EXISTS, IN) 이며 한 건의 데이터만 확인하고 종료한다. 아래와 같은 경우 세미 조인을 수행하게 된다. 메인인 EMP는 DEPTNO가 DEPT에 있는지만 확인하고 넘어가게 된다.
```
SELECT * FROM EMP WHERE DEPTNO IN (SELECT ID IN DEPT)
```


* 서브 쿼리 수행(필터) : 먼저 메인 쿼리를 수행해 데이터를 가져온 후, row 별로 서브 쿼리의 결과와 비교하여 이 데이터를 그대로 사용할지 버릴지 결정하는 방식이다. 인덱스가 없는 경우 수행되는 실행계획으로 최악의 경우 메인쿼리의 로우 단위로 서브쿼리가 수행될 수 있다.
```
SELECT * FROM EMP WHERE EXISTS (SELECT /* no_unset */ 1 IN DEPT WHERE ID='1133')
```


* 튜닝 예제

1. 데이터 생성 SQL
   
```
create table emp1 as select * from scott.emp, (select level no from dual connect by level <= 1000);
create table emp2 as select * from scott.emp;

alter table emp1 add constraint emp1_pk primary key(no, empno);
alter table emp2 add constraint emp2_pk primary key(empno);

EXEC dbms_stats.gather_table_stats(USER, 'emp1');
EXEC dbms_stats.gather_table_stats(USER, 'emp2');
```

2. 튜닝 대상 SQL
   
```
select *
from emp1 e1, emp2 e2
where e1.empno = e2.empno and
      exists ( select 'x' from scott.dept a where loc = 'NEW YORK' and a.deptno = e1.deptno)
```

3. 튜닝(NL_SJ)
      1. Nested Loop Semi Join의 첫 번째 outer 테이블은 데이터 양이 적어야 한다.
      2. 따라서 서브쿼리 절의 결과 데이터가 더 수가 적으므로 NL_SJ의 outer 테이블이 되는 것이 좋다.
      3. 서브쿼리 절을 NL_SJ를 하도록 유도하기 위해 dept의 loc, deptno 컬럼에 대한 index, emp1의 deptno 컬럼에 대한 index를 생성한다.
      4. emp1의 deptno는 emp2의 deptno와 같으므로(동일한 empno 이면) 서브쿼리 조건절의 a.deptno = e1.deptno를 e2.deptno로 변경 가능.
      5. 그럼에도 불구하고 서브쿼리 특성상 hint에 의한 핸들링이 잘 안될 수 있는데 이 경우 서브쿼리를 메인쿼리로 변경 (서브쿼리를 메인쿼리로 변경 시 deptno(조인키)의 중복되는 값을 grouping 한다)
```
select /*+ use_nl(v1,e1,e2) index(e1 emp1_idx2) */ *
from ( select deptno from scott.dept a where loc = 'NEW YORK' group by deptno ) v1, emp1 e1, emp2 e2
where e1.empno = e2.empno and v1.deptno = e2.deptno;
```

* Consistent Read
   - SQL이 수행되는 시점의 데이터를 읽는 방식을 의미한다.
   - SQL이 수행하면 SCN이라는 Id가 부여한다. SQL문이 시작된 시점에 SCN을 파악하고 해당하는 데이터 block을 읽을 때 block의 SCN이 동일한지 확인을 한다.
   - 읽으려는 블록의 SCN이 더 높은 경우 undo log를 통해 현재 SCN의 데이터를 블록에 복사하여 읽기 때문에 업데이트가 되는 데이터를 읽으면 속도가 느리다.

* Current Read
   - SQL문이 시작된 시점이 아니라( != consistent read ) 데이터를 찾아간 시점의 최종 값을 읽는다.
   - Update를 할 땐 Current Read를 해야한다.
   - 마찬가지로 Select For Update를 할 때도 Current Read를 한다.

* Update의 내부 동작
   - 처음 Select를 할 땐 consistent read를 한다.
   - 하지만 마지막 update 전에 current read를 한다(다른 트랜잭션이 해당 row에 대하여 업데이트를 했을 수 있으므로)
   - 이런 트랜잭션에 따른 처리는 DBMS마다 다르다.

* DML 수행시 고려사항
   - redo 로그, index 등등 다양한 작업을 하기 때문에 DML은 조회보다 자원을 많이 사용하는 작업이다.
   - DML이 자주 발생하는 테이블은 index를 최소화 해야한다.

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

