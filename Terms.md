### 객체의 종류
1. 테이블 인덱스 뷰 시퀀스 시소님 프로시저 함수 패키지 트리거
  - 테이블 : 테이터 저장소

### 자료형
- 자료형은 각 COLUMN마다 한 개씩 적용된다. 해딩 자료형 이외는 사용이 불가하다.
- 스칼라 자료형 : 자료형에 맞춰 한 종류의 데이터를 저장할 수 있는 자료형


- 대표 3개 타입
1. VARCHAR2(길이) : 4000BYTE만큼의 가변 길이 문자열 데이터를 저장하기 가능.

2. NUMBER(전체자리수,소수점이하자리수) : -38~+38 의 자릿수만큼 숫자 저장가능

3. DATE : 세기,연,월,일,시,분,초 의 저장이 가능한 타입

### SQL LANGUAGE

- DESC _TABLE_ : TABLE 구성을 살펴본다.
- RETURN : COL name, data type, null ...etc

### OPERATOR
1. Arithmetic operator
  - +
  - -
  - /
  - *
  - mod(%,orcle에서 % 대체지원)

2. comparison operator
  - >
  - <
  - =<
  - =>

3. Equivalence comparison
  - IF SAME
    - =
  - NOT SAME
    - !=
    - <>
    - ^=
    - NOT : !의 부정연산자 대신 사용된다.
      - EX
        ```
        SELECT * FROM TABLE WHERE NOT AGE>10;
        ```
4. AND OR IN BETWEEN
  - IN : OR의 발전형태
    - 사용법
      ```
      WHERE COLUMN IN (COND1,COND2,COND3,...)
      -- COLUMN안에 COND1,2,3인 경우를 묻는 것임.
      ```
      - NOT 응용
      ```
      WHERE COL NOT IN (C1,C2,C3)
      -- C1 C2 C3가 아닌 경우를 의미한다.
      ```
  - BETWEEN 1_ AND 2_ : BETWEEN AND는 SET이다. OR은 불가능하다 1_과2_ 사이의 의미로 사용한다.

- 다중행 연산자 : 

1. 다중행 연산자
  1. IN () [다중행연산자중가장많이사용]
    - 사용법 : COL IN () : COL이 나타내는 값이 ()안에 하나라도 있으면 TRUE이다.


  2. ANY,SOME  : IN과 비슷한 성질. 단 사용법은 다름. 비교는 단일행 연산자로 비교. 방법은 ANY SOME 두개인데 사용법은 똑같은 둘중 하나는 자리만 차지하는 실정.
    - 사용법 
      1. any,some
        1. column 비교연산자 ANY() : ANY괄호 안에 비교연산자와 COLUMN값을 비교해서 참인것만 내봄.
        2. Column < ANY() : ()안에 아무거나 작으면 이란 의미인데.
          ```
          SELECT * FROM EMP WHERE SAL >= ANY(SELECT MAX(SAL) FROM EMP GROUP BY JOB);
          -- 조금 이상한 해석이겠지만, 서브쿼리 는 any로 묶이는데, 각 JOB 그룹별로 가장 큰 값들이 테이블이 되고 >= 연산자임으로 SAL은 안에 있는 값들보다 크거나 같은 것들만 출력하게 된다.
          ```
          ```
          SELECT * FROM EMP WHERE SAL<ANY(SELECT SAL FROM EMP WHERE DEPT=30)
          -- 해석요망
          ```
  3. ALL : 2번과 사용밥법은 동일하며, 괄호 안에 모든 값들에 대해서 피연산자가 만족을 해야만 true. 예를들어 (5000,'kbs') 라면 "이건 아직 어떻게 풀지 모르겠다. 예시를 들겠다.
    - EX 1) 어떻게 만들어 봤는데 개판이다, 매니저의 급여보다 낮은 다른 테이블 사람들을 출력하려고 했는데 두개 테이블이 만나지 못했다.
      ```
      SELECT E1.ENAME,E1.JOB FROM EMP E1,EMP E2 WHERE E1.SAL <= ALL (SELECT E2.SAL FROM E2 WHERE JOB='MANAGER');
      ```
  4. EXISTS : 자주 사용하는 편은 아니다. 서브쿼리의 존재유무로 뭔가 판단할 때 사용한다.
    - 사용예시
      - EX1) 아무것도 안 나오는 구조, 왜냐면 EXISTS에는 아무것도 없기 때문임.
        ```
        SELECT * FROM EMP WHERE EXISTS(SELECT MAX(SAL) FROM EMP WHERE DEPTNO=50)
        ```
      - EX2) 나옴, 일단 값 하나라도 있기 때문임.
        ```
        SELECT * FROM EMP WHERE EXISTS(SELECT MAX(SAL) FROM EMP WHERE DEPTNO=30)
        ```

5. LIKE
  - 사용도 : 문자열전용,파이썬의 RE에서 했던 것과 유사. 문자열과 일치하는 문자열을 찾기위해 사용하는데, 성능적인 면에서 의견이 좀 있다. 
  
  - 함께 사용되는 것
    - WHERE + LIKE + '_' OR '%'
    1. LIKE : 가장 첫번째로 나오는 키워드
    2. _ : 숫자던문자던 상관없이 딱 하나 문자
    3. % : WILD CARD, %를 기준으로 % 뒤에 문자가 올 때까지 기달리기.
  - 사용법
    1. 기본사용
      ```
      LIKE 'S%';
      -- S로시작하고 모든 문자 pass
      ```
    2. 기본사용 2
      ```
      LIKE '_K%J';
      -- 제일 앞에 아무 문자가 오고 K, 그 다음 J가 나올 때까지 기달리기
      ```
    3. 기본사용 3 : 특정 WORD포함
      ```
      WHERE COL LIKE '%WORD%';
      ```
    4. 응용, NOT 사용이 가능하다. NOT LIKE
    5. _ 나 %가 데이터 일부일 경우
      ```
      WHERE COL LIKE 'K\_\%K' ESCAPE '\';
      ```
6. IS NULL : NULL 값이면 TRUE. 매우 자주 사용되는 OPERATOR
  - 사용법
    ```
    WHERE COL IS NULL;
    ```
  - NOT 응용
    ```
    WHERE COL IS NOT NULL;
    ```

7. SET OPERATOR
  - 주의사항 : 
    1. 반드시 연산을 진행할 테이블들의 열 개수가 같아야 하며, 각 테이블의 열 간의 연산시 열의 데이터 유형이 "똑같아야 한다". 
    2. UNION과 INTERSECT는 DISTINCT의 두개 이상 열의 조합시 해당 조합을 하나의 단위로 보고 다른 똑같은 조합은 출력하지 않는 중복 제거기능을 한다.
  1. UNION : 합집합을 의미하며 중복은 자동으로 제거.
  2. UNION ALL : 1과 같으나 중복 제거 안함.
  3. MINUS : 먼저 나온 테이블에 뒤에 나온 테이블의 값을 차집합 한다.
  4. INTERSECT : 교집합. 먼저 테이블이랑 다음 테이블 중에 공통되는 것만 추출해서 결과 내보내기 + 중복 제거.



#### OPERATOR END

#### Date serach Language.[SELECT,JOIN,PROJECTION,WHERE]
1. SELECT : 행 단위로 원하는 데이터 조회. 이를 사용하여 ROW데이터 중 특정 속성값을 갖는 데이터들을 추출하기 가능.
  - EX) 1: STATE열에 END라는 속상값을 가진 행을 추출하고 싶다.
    ```
    SELECT STATE FROM INFOM;
    ```
  - EX) 2: HR DB에 있는 TABLE을 모두 꺼내오고, 그중 COUNTRIES TABLE에서 COUNTRY_ID열을 지정해서 모든 행 출력
    ```
    1. SELECT * FROM TAB;
    2. SELECT COUNTRY_ID FROM COUNTRIES;
    ```
  - EX 3): JOBS라는 테이블에서 COL의 종류를 알아내고, 그 중 아무거나 2개 출력.
    ```
    1. DESC JOBS;
    2. SELECT JOB_ID,JOB_TITLE FROM JOBS;
    ```
2. PROJECTION : 열 단위로 원하는 데이터 조회. COL의 NAME을 지정해서 COL 자체를 가져오기 가능.

3. SELECT X PROJECTION : select로 원하는 행에 projection으로 알고 싶은 속성값을 추출 가능.

4. JOIN : 두 개 이상의 테이블을 양옆에 연결해서 하나의 테이블처럼 조회하는 방식

5. WHERE : SELECT 문으로 데이터를 조회할 때 "특정 조건" 기준으로 원하는 행을 출력하는 데 사용한다.
  - 사용법
    1. WHERE 1_[LOGIC OPERATOR]2_ : 1_열에서 2_의 조건식을 만족하는 것만 출력.
    2. 조건식의 제한은 없다
      ```
      SELECT * FROM TABLE WHERE [CONDITIONALeXPRESS] AND/OR [2] AND/OR [3] ...
      ```
    3. 한 번 WHERE 사용 후엔 다른 COL을 사용하기 위해서 WHERE을 다시 사용하는 건 불가능하며 그냥 한 번만 사용해주면 된다.
      ```
      SELECT ENAME,EMPNO,SAL,DEPTNO FROM EMP WHERE (ENAME LIKE '%E%' AND DEPTNO=30 AND SAL NOT BETWEEN 1000 AND 2000) AND ***WHERE*** SAL<2000;
      -- 해당 ***WHERE***는 삭제 해야함.
      -- 괄호는 사실 없어도 된다.
      -- 프로그래밍 처리는 왼쪽부터 오른쪽으로 차근차근 진행된다.
      ```
    - EX 1) : 한 개의 조건식 사용
      ```
      SELECT * FROM TABLE WHERE AGE>25 OR/AND GENDER="MAN"
      ```
    - EX 2) : 복수개의 조건식 사용
      ```
      SELECT * FROM TABLE WHERE AGE>25 OR/AND;
      ```

#### Data serach Language END

#### 중복데이터 삭제와 DEFAULT

- ALL[DEFAULT] : DISTINCT와 반대로 데이터 중복 상관 없이 모두 출력한다.
  - USAGE
  ```
  SELECT ALL 1_ FROM _; -- ALL은 생략가능, DEFAULT임
  ```

- DISTINCT : SELECT와 함께 사용되며 해당 열의 값이 범주형이라면 중복없이 모든 범주 값을 출력한다.
  - USAGE
  ```
  SELECT DISTINCT 1_ , 1_ , ... FROM _
  ```
  - EX 1) : 한 개의 열에 적용
    ```
    SELECT DISTINCT _ FROM _;
    ```
  - EX 2) 2개 이상의 열에 적용 : A가 [Z,X] B가 [1,2] 면 모든 경우의 수는 [Z1,Z2,X1,X2]이다.
    ```
    SELECT DISTINCT A , B ... FROM _;
    ```
#### 중복데이터 삭제와 DEFAULT END


### 기타
#### 사용이 권고되지 않는 QUERY, 효율때문에(서비스 응답 시간 DELAY)
1. ORDER BY 1_ 2_, 1_ 2_, ...: 1_열을 기준으로 데이터를 정렬한다. 사용시 데이터가 일단 많아지면 정렬시간이 매우 오래 걸림으로서 출력시간도 증가하게 됨.
  - 2_옵션
    1. ASC : DEFAULT, 오름차순 
    2. DESC : 내림차순
  - KNOWLEDGE
    : VARCHAR2 같은 문자 데이터 역시 알파벳 순, DATE 역시 이전/이후 날짜로 정렬 가능
  - 사용법
    - EX 1: 한 개의 열만 사용
    ```
    SELECT * FROM EMP ORDER BY SAL ASC/DESC;
    ```
    - EX 2: 두개 이상의 열 사용
    ```
    SELECT * FROM EMP ORDER BY DEPTNO ASC,SAL DESC;
    -- DEPTNO의 정렬은 먼저 고정되고 고정된 상태에서 같은 LEVEL의 DEPTNO끼리 SAL기준으로 정렬하는 것임.
    ```
####
  


1. NULL : NULL 의 AND OR 와의 관계
  - NULL WITH AND
    1. NULL AND TRUE = NULL
    2. NULL AND FALSE = FALSE
    3. NULL AND NULL = NULL

  - NULL WITH OR
    1. NULL OR TRUE = TRUE
    2. NULL OR FALSE = NULL
    3. NULL OR NULL = NULL

  - 이외 ?><=>..NULL 은 모두 NULL이 결과이다.

2. FROM _ : FROM 뒤에는 열과 행만 제대로 갖추어져 있으면 굳이 테이블이 아니더라도 다 올 수가 있다.

#### TRANSACTION과 SESSION과 LOCK

- SESSION > TRANSACTIO, 트랜은 세션에 속한다.

1. 트랜잭션 : ACID원칙이 지켜지는 더 이상 분할불가한 최소 수행 단위. 한 개 이상의 "DML"로 이루어져 있음. 어떤 기능 "한 가지"를 수행하는 SQL문 덩어리의 개념이다. 하나의 트랜잭션 내에 있는 여러 명령어를 한 번에 수행하여 작업을 완료하거나 아예 모두 수행하지 않는 상태를 가진다. " 트랜잭션은 SCOTT 같은 DB계정을 통해 접속 동시에 시작된다. 그리고 트랜잭션을 제어하는 명령 "TCL"을 실행할 때 기존 트랜잭션은 끝나고 다시 새로운 트랜잭션이 시작 된다.
  - DML : INSERT,UPDATE,DELETE

  - ROLLBACK과 COMMIT, 사용시 기존 트랜잭션은 끝나고 새로운 트랜잭션이 생성된다.
    - ROLLBACK : DML로 구성된 트랜잭션 안에 모든 DML을 삭제한다. 오직 DML과정만 담아있기 때문에 중간에 DML아닌 명령이 실행돼도 상관없다. ROLLBACK;만 쳐주면 된다.

    - COMMIT : DML, 트랜잭션을 모두 DB에 확정. 실행시 다시는 ROLLBACK 불가능하다. COMMIT; 만 쳐주면 된다.

2. SESSION : 일반적으로 활동위한 시간/기간을 뜻한다. DB에선 DB접속을 시작으로 DB관련 작업 수행후 종료하기 까지의 전체 기간을 의미.
  - 세션이 여러개 : 오라클 DB에 접속,사용중인 연결이 여러개.
  - 하나의 세션 안에는 여러개의 트랜잭션이 존재한다. 트랜잭션이 끝나고 새로 만들어지는 걸 계속해도 세션은 이 모든 과정을 포함한 하나이다.
  - #읽기 일관성의 중요성# : 특정 세션에서 테이블의 데이터를 변경 중일 때 다른 세션에서는 테이터의 변경이 "확정 되기" 전까지는 변경되어가고 있는 테이블이 아닌 변경되기 전 테이블을 보는 것.
    + [매우중요한내용]어떤 데이터 조작(DML)이 완료(COMMIT OR ROLLBACK)되기 전까지 데이터를 직접 조회하는 세션 외 다른 세션은 데이터 조작 전 상태 내용을 조회/출력/검색되는 특성이 " 읽기 일관성 (READ CONSISTENCY) "
  - 2중 세션을 통한 트랜잭션 예시
    1. 오라클 DB (A)와 CMD로 SQLPLUS SCOTT/scott (B)로 두개 세션 만들기.

3. LOCK : 조작중인 데이터를 다른 세션은 조작할 수 없도록 접근을 " 보류 "시키는 것.
  - 행(hang) : 특정 "세션"에서 데이터 조작이 완료될 때까지 다른 세션에서 해당 데이터 조작을 기다리는 현상

  - EX) 1번 컴퓨터가 "먼저"DML작동시 2번 컴터가 DML 사용하게되면
    1. NUM1COMPUTER
      ```
      INSERT INTO TABLE (~~~) VALUES(~~~)
      ```
    2. NUM2COM , HANG 현상 발생
      ```
      DELETE ..~
      -> 아무런 동작이 일어나지 않고 화면이 멈춘 듯 가만히 있다,
      ```

    3. 1번 컴터가 확정시켜야만 LOCK이 풀린고 즉시 2번 컴터의 DML이 작동한다.
  - TIP
    1. LOCK 의 종류도 좀 있는데, DML도 WHERE로 조건을 지정할 수 있어서 그 행만 DML하는 "ROW LEVEL LCOK"이 된다. WHERE가 없으면 모든 행을 LOCK한다
    2. 비록 모든 행이 LOCK이어도 "INSERT"는 수행이 가능하다. 그런데 INSERT는 수행이 됨에도 불구하고 변경되는 행의 수와 상관없이 TABLE LEVEL LOCK이 걸려서 "DDL"로는 테이블의 구조를 변경할 수 없다 한다.

#### 객체종류 

- 오라클 객체의 종류[사용빈도가높은것만]
  1. 테이블[SQL문오라클에서가장많이사용]
  2. 데이터사전
  3. 인덱스
  4. 뷰
  5. 시퀀스
  6. 동의어

1. 데이터사전
  - 사용 가능한 데이터 사전을 알고싶다면 SELECT * FROM DICT;
    + 종류가 매우 많아서 자주 사용하는 몇 개 정도만 알아둘 것이다.
  - 사용자 테이블 : DB를 통해 관리할 데이터를 저장하는 테이블을 뜻한다. EMP,DEPT,SALGRADE 테이블들이 바로 사용자 테이블이다.
  - 테이터 사전 : DB를 구성하고 있는 운영을 위해 필요한 모든 정보를 저장하는 특수한 테이블이다. DB가 생성되는 시점에 자동으로 만들어진다.
    + 데이터 사전에는 메모리/성능/사용자/권한/객체 등 오라클 DB운영에 중요한 데이터가 보관되어 있다. 만약 이 데이터에 문제가 발생하면 오라클 DB 사용이 불가능 해질 수 있다.

    + 오라클 DB 사용자가 데이터 사전 정보에 직접 접근/작업은 불허이지만 " 데이터 사전 뷰(DATA DICTIONARY VIEW) " 를 SELECT문을 통해 정보 열람이 가능하댄다.

  - 데이터사전뷰 : 용도에 따라 이름 앞에 접두사가 붙는다. 더불어 대부분의 데이터 사전 끝에는 접미사로 "-S" 복수형태 접미사가 붙는다.
    
    1. USER_XXXX : 현재 DB 접속한 사용자가 소유한 객체 정보
    2. ALL_XXXX : 현 DB 접속한사용자가 소유한 객체 또는 다른 사용자가 소유한 객체 중 사용 허가를 받은 객체 정보
    3. DBA_XXXX : DB관리를 위한 정보(오직 DB 관리권한을 가진 SYSTEM,SYS사용자만 열람가능)
    4. V$_XXXX : DB 성능 관련 정보(X$_XXXX 테이블의 뷰)

    1. USER_XXXX
      1. USER_TABLES : 현재 DB에 접속한 계졍이 소유하는 테이블 정보 출력
        - 사용방법 : USER_TABLES또한 테이블이다, 테이블 안에 TABLE_NAME이라는 열이 존재한다. * 로 출력해도 가능
          ```
          SELECT TABLE_NAME FROM USER_TABLES;
          ->아래처럼 나온다.
          DEPT
          1.EMP
          2.BONUS
          3.SALGRADE
          ...귀찮MY_TABLE
          DEPT_TEMP
          KBS
          EMP_DDL
          EMP_DDL2
          ALT_PRAC
          ```
    2. ALL_XXXX : ALL접두어는 접속한 DB뿐 아니라 다른 사용자의 소유 객체중 허락된 모든 정보를 볼 수 있다. ALL_ 접두어의 데이터 사전 뒤 객체를 명시할 때 "복수형"단어를 사용합니다.
      - 사용방법
        ```
        SELECT OWNER,TABLE_NAME FROM ALL_TABLES;
        -> TABLE이름이 나오고 그 테이블의 소유 계정주가 나온다.
        ->어래엣 보다시피 DUAL 테이블은 SYS 소유였다.
        OWN TABLE_NAME
        SYS	DUAL
        SYS	SYSTEM_PRIVILEGE_MAP  
        SYS	TABLE_PRIVILEGE_MAP 
        ```
    3. DBA_XXXX : DB 관리 권한을 가진 사용자만 조회할 수 있는 테이블이다. SCOTT계정으로는 조회가 불능이다. DB 자체를 관리하는 목적이 아니고선 오라클 DB를 사용하여 데이터를 보관하고 관리하는 업무시엔 잘 사용하지 않는다.
        - 사용방법
          1. 우선 SYSTEM 계정으로 진입한다.
            ```
            1. ORACLE 녹색십자를 클릭하고 SYSTEM , SYSTEM/ORCL, XE->ORCL 해서 들어간다.
            ```
          2. 
            ```
            SELECT * FROM DBA_USERS WHERE USERNAME='SCOTT';
            ```
2. 인덱스
  1. 설명 : 테이블 열에 사용하는 객체,. 테이블에 보관된 특정 행 데이터의 주소를 책의 페이지 목록처럼 만들어 놓은 것. 인덱스가 지정된 열들은 출력속도가 빨라질 것이라 예상할 수 있지만 항상 그런 것도 아니다. 열의 선정은 데이터의 구조와 데이터의 분포도 등의 여러 조건을 고려해서 이루어져야 하기 때문이다.

    - 인덱스 사용 여부에 따른 검색방식
      1. Table Full Scan : 테이블 데이터를 처음부터 끝까지 검색해서 원하는 데이터를 찾기
      2. Index Scan : 인덱스를 통해 데이터를 찾는 방식
        - 인덱스도 오라클 db 객체이다.
        - 소유사용자와 사용 권한이 존재한다

        - 사용법
          1. USER_INDEXES

          2. USER_IND_COLUMNS
      3. 인덱스는 열이 기본키,고정키 일 경우 자동으로 생성된다.
    - 인덱스 생성 : 오라클 DB에서 자동생성 하는 것 외 사용자 지정 생성.
      - 사용법
        ```
        CREATE INDEX _INDEX NAME_
        ON _TABLE NAME_(열 이름1 ASC[DEFAULT] OR DESC,
        열 이름2 ASC OR DESC,
        ...);
        ```
      - 그외 인덱스 생성방법이 있는데 336PAGE 참고.

    - 인덱스 삭제
      - 사용법
        ```
        DROP INDEX _INDEX NAME_;
        ```
  2. 마치며
    - 인덱스 생성이 항상 좋은 결과로 이어지지는 않는다. 337PAGE 참고하시고, SQL 튜닝 관련 서적을 참고해서 인덱스 사용 백그라운드를 늘릴 수도 있다.

3. 뷰