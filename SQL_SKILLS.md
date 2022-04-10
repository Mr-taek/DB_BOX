

### IDE SHORT CUT
1. ctrl+enter : 해당 줄 실행

### LANGUAGE SKILLS

1. 전체 테이블 조회
    - ex) : SELECT * FROM TAB;

### 별칭(alias)

1. 별칭 : 기존 열들을 산술연산 등을 이용해 새로운 열을 만드는 skill. NAMEING 에서 용도가 명확해야함.

2. 사용법
    ```
    -- 이름사용없이
    1. SELECT COL1+COL2 FROM TAB;
    -- 이름 사용
    1. SELECT COL1+COL2 NEW_COL
    2. SELECT COL1+COL2 "NEW_COL"
    3. [선호됨] SELECT COL1+COL2 AS NEW_COL
    4. SELECT COL1+COL2 AS "NEW_COL"
    ```
    - EX 1) 한 개 별칭
    ```
    SELECT SAL,DEPTNO, SAL*DEPTNO AS GAME FROM EMP;
    -- SAL,DEPTNO는 NUMBER 타입, 산술연산은 +-*/ 로 하며 %은 없음.
    -- 이렇게 하면 질의 결과로 3개의 열이 등장
    ```
    - EX 2 : 2개 이상의 별칭
    ```
    SELECT EMPNO AS EMPP,ENAME AS EMPNAME FROM _;
    -- 중간에 ","이 핵심이다.
    ```