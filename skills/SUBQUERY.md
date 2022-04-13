## 여러 데이터를 하나의 SQL문에서 처리하기 위해 사용한다.

## 실무에서도 자주 사용된다.

### 서브쿼리의 이해
- MAINQUERY : 전체적인 기능을 최종적으로 수행하는 SQL문, 서브쿼리의 가장 바깥에 존재하는 쿼리.

- SUBQUERY : SQL 실행 위해 필요한 데이터를 추가 생성한 SQL문 내부에서 사용하는 "SELECT 문".

    - 서브쿼리 사전 사용 설명서
        1. 서브쿼리는 OPERAND같이 비교,조회 대상이고 괄호로 묶임
        2. 특수케이스 제외시 ORDER BY는 사용불능
        3. 서브쿼리가 비교 대상이 되는 열의 DATA TYPE, 개수가 똑같이 일치해야 한다. 단일 값일 경우엔 반드시 데이터 타입을 일치 시켜야한다.
        4. WHERE SAL > SAL2 이거나 SAL > 231면 모두 3번의 특징을 만족하는데, 더 뜯어보면 각 행끼리의 값을 하나씩 비교하는 것임으로 서브쿼리의 결과도 이처럼 똑같이 만들어 줄 수 있어야 한다. SAL2를 서브쿼리로 대체하면 결과도 SAL2와 같은 형식으로 도출시켜야 한다.
    - 사용 예
        1. SELECT * FROM(SELECT * FROM EMP); : FROM 안에 있는 것이 서브쿼리
        2. SELECT * FROM EMP WHERE COL (SELECT COL FROM EMP WHERE COL>200) : WHERE 문 안에 있는 SELECT문이 서브쿼리


    - EX 1) JONES보다 급여가 높은 사원 출력하기. 서브쿼리 활용.242PAGE
        ```
        SELECT *
            FROM EMP
            WHERE SAL >(SELECT SAL FROM EMP WHERE ENAM='JONES');
        ```

### 실행 결과가 하나인 서브쿼리

- 단일행 서브쿼리(SINGLE-ROW SUBQUERY) : 하나의 행으로 나오는 서브쿼리. 데인쿼리와 서브쿼리의 결과는 "단일행 연산자"를 사용하게 된다.
    1. 단일행 연산자
        - ">","<","<=",">=","!=","<>","^=","="

    2. 예시들
        1. 단일행 서브쿼리와 날짜데이터, 빨리 입사한 사원 뽑아내기
        ```
        SELECT * FROM EMP WHERE HIREDATE<(SELECT HIREDATE FROM EMP WHERE ENAME='SCOTT');
        ```
        2. 1번에다가 SCOTT이 입사한 날짜 테이블을 붙이기.
        ```
        SELECT E.ENAME,E.HIREDATE,SCOTT.HIREDATE FROM EMP E,(SELECT HIREDATE FROM EMP WHERE ENAME='SCOTT') SCOTT 
        WHERE E.HIREDATE<=(SELECT HIREDATE FROM EMP WHERE ENAME='SCOTT');

        -> 불필요한 것 삭제 수정
        SELECT E.ENAME,E.HIREDATE,SCOTT.HIREDATE FROM EMP E,(SELECT HIREDATE FROM EMP WHERE ENAME='SCOTT') SCOTT 
        WHERE E.HIREDATE<=scott.hiredate;
        ```
        3. EMP 테이블서 전체 사원의 평균 급여보다 작거나 같은 급여를 받고 있는 20번 부서의 사원 및 부서의 정보 구하기
        ```
        SELECT E.ENAME,E.DEPTNO FROM EMP E, DEPT D WHERE E.DEPTNO=D.DEPTNO AND E.DEPTNO=20 AND SAL<=(SELECT AVG(SAL) FROM EMP);
        ```

### 실행 결과가 여러 개인 다중행 서브쿼리

- 다중행 서브쿼리(Multiple-row subquery) : 실행 결과 행이 여러개로 나오는 서브쿼리. 단일항 연사자는 사용이 불가능이다.

    1. 다중행 연산자 , 자세한 것은 Terms의 다중항 연산자 참고
        - IN
        - ANY,SOME
        - ALL
        - EXISTS
    
    2. 다중열 서브쿼리(Multiple-column subquery) : 굉장히 유용할 것같은 SKILL인데 솔직히 아직 어떡해 쓸 지는 감이 안 잡힌다.
        - 사용법 : 비교할 열을 두개 이상 묶고 서브 쿼리 괄호에는 묶은 데이터 타입과 일치되는 자료형을 출력해야한다.
            ```
            WHERE (DEPTNO,SAL) IN (SELECT DEPTNO,MAX(SAL) FROM EMP GROUP BY DEPTNO)
            ```

    3. WITH : 여러 테이블(서브쿼리포함)을 FROM 처리해야할 때 가독성과 성능향상을 위해사용 된다.
        - 인라인 뷰(Inline view) : 일단 view는 특정 테이블에서 열들을 따온 것이다. FROM 절에 사용하는 서브쿼리를 지칭한다.
            - EXAMPLE OF Inline view
                ```
                SELECT * FROM (SELECT * FROM DEPT) D,(SELECT * FROM EMP) E WHERE D.DEPTNO=E.DEPTNO;
                ```
        - WITH : 오라클 9i부터 사용가능.
            - 사용법
                ```
                WITH
                E AS (SELECT * FROM EMP WHERE DEPTNO=10),
                D AS (SELECT * FROM DEPT)
                SELECT E.DEPTNO,D.LOC FROM E,D WHERE E.DEPTNO=D.DEPTNO;
                ```


#### SELECT 절에 사용가능한 서브쿼리

- 설명 : 흔히 SCALAR 서브쿼리라고 하는데, 단일행 연산자 처럼 하나의 값만을 받아 하나를 출력하기 때문이다. 일반 열과 다를 바가 없는 출력을 내는 서브쿼리라는 말이다.

- 예시
    1. 이 정도면 다 한거라 생각한다. 반드시 아래의 원리대로 해야함. 
    ```
    SELECT 
    E.DEPTNO,
    (SELECT GRADE FROM SALGRADE WHERE E.SAL BETWEEN LOSAL AND HISAL) AS ASALGRADE
    FROM EMP E;
    ```