### FROM 안에 TABLE
- SELECT * FROM (SELECT COL,COL2,COL3 FROM TABLE); 이게 가능!
    - EX) SELECT ENAME FROM(SELECT ENAME FROM EMP);
    
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

### PIVOT , UNPOVOT SKILLS
- ORACLE 11g 부터 제공되는 함수이다. 관계를 더욱 직관적이며 깔끔하게 만들어 준다.
1. PIVOT : 테이블의 행을 열로 바꾸고 
    - 사용법
        : SELECT * FROM (SELECT COL, ... FROM TABLE) PIVOT(GROUPFUNC(COL) FOR _COL_ IN (COL의 VALUE이름1 AS NAME1, COL의 VALUE이름2 AS NAME2,
        ...)) - 여기서 as name1,2는 생략해도 된다.
    - 원조 사용법, 명시를 잘 해줘야 함..
        ```
        SELECT DEPTNO,
        MAX(DECODE(JOB,'CLERK',SAL)) AS CLERK,
        MAX(DECODE(JOB,'SALESMAN',SAL)) AS SALESMAN,
        MAX(DECODE(JOB,'PRESIDENT',SAL)) AS PRESIDENT,
        MAX(DECODE(JOB,'MANAGER',SAL)) AS MANAGER,
        MAX(DECODE(JOB,'ANALYST',SAL)) AS ANALYST FROM EMP GROUP BY DEPTNO ORDER BY DEPTNO;
        ```
### 상호 연관 서브쿼리
- 사용법
    ```
    SELECT*
    FROM EMP E1
    WHERE SAL>(SELECT MIN(SAL) FROM EMP E2 WHERE E2.DEPTNO=E1.DEPTNO)
    ORDER BY DEPTNO,SAL;
    ```