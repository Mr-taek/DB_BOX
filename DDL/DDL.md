# DDL은 DB 객체중 활용빈도가 가장 높은 테이블에 사용하게 된다.

- DDL (DATA DEFINITION LANGUAGE) : DB 데이터에 보관하고 관리하기 위해 제공되는 여러 객체의 생성-변경-삭제 관련 기능을 수행한다.

- [필!독!매우중요!!!]DDL의 치명적일 수 있는 특징
    1. DDL 사용즉시 COMMIT 되어서 이전으로 되돌아 가지 못한다.
        - EX
            ```
            1. INSERT..
            2. DELETE..
            3. DDL..
            -> 즉시 COMMIT 그리고 새로은 트랙잭션 시작. DDL 사용하는 즉시 DML이고 뭐고 다 COMMIT임.
            ```

1. CREATE : DB객체 생성 DDL.
    - 사용법 : 소유계정은 없어도 되는데 이때는 접속중인 계정 소유의 테이블이 만들어진다.
        ```
        CREATE 소유계정.테이블이름(
            열1 이름 열1 자료형,
            열2 이름 열2 자료형,
            열3 이름 열3 자료형,
            열4 이름 열4 자료형,
            열5 이름 열5 자료형,
            ...
        )
        ```
        - 테이블 이름 규칙
            1. 반드시 시작은 문자(한글포함)이며 숫자는 불능
            2. 이름은 30byte(E는 30글,K는 15글)까지,숫자,특수문자 모두 포함한다.
            3. 중복되는 테이블 이름은 불능
            4. SQL KEYWORD(SELECT,FROM ..etc)로 이름 지정 불능
        - 열 이름 생성 규칙
            1. 반드시 문자로 시작 하며 30byte이하이며 문자,숫자,특문 다 가능.
            2. 한 테이블의 열 이름은 중복불능(SAL이름의 열 두개는 존재불능)
            3. SQL 키워드로 열 이름 짓기 불능
    - 사용 예
        1. 열 추출
            ```
            CREATE TABLE EMP_DDL(
                EMPNO NUMBER(4),
                ENAME VARCHAR2(10),
                SAL NUMBER(7,2), -- 소수점 이하 2자리 포함 7자리 숫자 저장, 즉 자연수 5,소수 2
                HIREDATE DATE
            );
            ```
        2. 같은 테이블 복사 생성
            ```
            CREATE TABLE EMP_DDL AS SELECT * FROM EMP;

            ```
        3. WHERE로 일부 데이터(행)만 사하여 만들기
            ```
            CREATE TABLE EMPD_DDL
            AS SELECT * FROM EMP WHERE DEPTON=30;
            ```
        4. 테이블 구조만 같고 안에 데이터는 없이 만들기, 열명 중복이라고 자꾸 떠서 억지로 끼워맞춘 게 있다. 괄호는 없어도 됨.(서브쿼리)
            ```
            CREATE TABLE EMP_DDL2 AS 
            (SELECT E.ENAME AS NAME,E.DEPTNO,D.DNAME AS NAMEE,D.DEPTNO AS DD,D.LOC FROM EMP E,DEPT D 
            WHERE E.DEPTNO=D.DEPTNO AND 1<>1);
            ```

2. ALTER : 테이블에 "새 열"을 추가/삭제, 열의 자료형/길이 변경하는 기능이다.
    - 사용법 1 : 추가 ADD
        ```
        ALTER TABLE _TABLE NAME_
        ADD HP VARCHAR2(20);
        ```
    - 사용법 2 : 변경 RENAME COLUMN _ TO _
        ```
        ALTER TABLE EMP_ALTER
        RENAME COLUMN HP TO TEL;
        ```
    - 사용법 3 : 열의 자료형을 변경하는 MODIFY
        ```
        ALTER TABLE EMP_ALTER
        MODIFY EMPNO NUMBER(5);
        -- 사원수가 늘어서 1만대로 직원번호를 매겨야 할 때
        ```
    - 사용법 4 : 특정 열을 삭제할 때 사용하는 DROP,삭제시 돌아올 수 없다...
        ```
        ALTER TABLE EMP_ALTER
        DROP COLUMN TEL;
        ```
3. ALTER에 사용하는 RENAME,DROP의 다른 용도 + TRUNCATE
    1. RENAME : 테이블 이름을 변경하고 싶을 때 사용한다.
        - 사용법
            ```
            RENAME Original_tname TO New_tname
            -> 테이블 이름이 변경.
            ```
    2. TRUNCATE : 테이블의 데이터를 삭제한다. 테이터만 삭제해서 열 구조에는(데이터구조) 영향을 주지 않음. 
        - [사용시꼭숙지!!] : 여긴 DDL이다, 롤백따윈 없다. 삭제를 원하면 차라리 DELETE로 WHERE을 사용하는 것도 고려하라. 시발 무섭다 100백만개가 이걸로 날아간다면.... 끔찍하다. 
        - 사용법
            ```
            TRUNCATE TABLE EMP2;
            ->create로 emp를 복사한 emp2안에는 데이터가 없어진다.
            ```
    3. DROP : 테이블을 아예 삭제해버린다.
        - [사용시꼭숙지!!]사용법 : 한번 해버리면 안 되는데, 오라클 10G부터 윈도우는 휴지통기능으로 FLASHBACK 기능이 된대.

        - 사용법
            ```
            DROP TABLE _TABLE_;
            ```
        - FLASHBACK?
