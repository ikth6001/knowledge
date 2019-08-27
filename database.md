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

<h4></h4>

<br><br>

<h4></h4>

<br><br>

<h4></h4>

<br><br>

