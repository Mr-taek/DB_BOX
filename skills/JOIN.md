- 집합 연산자와 조인의 차이점 : 집합연산자는 세로로, 조인은 가로로 확장

- 조인이라고 해서 특별한 함수를 사용하는 것이 아니다. 구조적으로 조인을 정의한 것이 조인이다. 일종에 SKILL이다.




### 조인은 WHERE로 같은 열을 명시하는 것.
- 조인시 테이블 구분을 위한 방식 : "테이블이름.열이름"
1. 열 이름을 비교로 조건식 조인
    -   EX) : SELECT * FROM EMP,DEPT WHERE EMP.DEPTTO=DEPT.DEPTNO ORDER BY EMPNO;

- FROM 절에 지정한 테이블엔 별칭 지정이 가능 : " FROM TABLE1 ALIAS1,TABLE2 ALIAS2 "
2. 별칭으로 비교한 조건식 조인
    - EX) SELECT * FROM EMP E, DEPT D WHERE E.DEPTNO=D.DEPTNO;


### 등가 조인, 비등가 조인, 자체 조인, 내/외부 조인

- 중요 핵심은 Insight 3번을 읽고 읽도록 한다.

- 조인을 사용한다 -> 일반적으로 등가조인을 사용한다.

- 모든 조인시 , 각 테이블을 " 정확히 연결하는 조건식 "이 최소 N(전체테이블수)-1 만큼 존재해야 한다.

- 등가/비등가 조인은 모두 WHERE로 범위를 지정해줘야 한다. 안 그러면 데카르트 곱 된다.

1. [가장일반적인조인] 등가조인 : 열 이름을 비교로 한 조건식과 동일하다.
    - 주의사항
        1. 조인할 테이블 간 같이 존재하는 열이 있다면 반드시 어느 테이블에 속한 열인지 명시 해야한다. 이외 열이 겹치는 게 없다면 굳이 정해줄 필요는 없다.
            - EX) 겹치는 열만 어떤 테이블의 열인지 명시하고 이 외엔 그대로 사용한 경우. 어느 테이블의 열을 명시하던 결과는 같다.
            ```
            SELECT EMPNO,D.DEPTNO FROM EMP E,DEPT D WHERE E.DEPTNO=D.DEPTNO;
            -- D.DEPTNO로 DEPT 테이블의 DEPTNO열을 명시했다
            ```
        2. 겹치지 않는 열이라도 모든 열이 어느 테이블에서 왔는 지 명시해준다. 조인 테이블 수가 10를 넘기도 하는데, 이 때 열이 어느 테이블에서 왔는 지 헛갈리기 쉽기 때문이다.
    - 사용예
        1. 조인후 GROUP BY와 조건절로 행 축약, 이때 모든 중복되는 열은 반드시 어느 테이블에서 왔는 지 명시해 줘야 한다. GROUP BY에서 테이블 명시 안해 줘서 " 열의 정의가 애매하다 " 라고 떳다.
            ```
            SELECT D.DEPTNO,COUNT(D.DEPTNO),AVG(SAL) FROM EMP E,DEPT D WHERE E.DEPTNO=D.DEPTNO AND SAL>1000 GROUP BY D.DEPTNO ORDER BY DEPTNO;
            ```
        2. WHERE을 사용해서 행을 개축약
            ```
            SELECT E.ENAME FROM EMP E,DEPT D WHERE E.DEPTNO=D.DEPTNO AND E.SAL<3000 AND E.EMPNO<9999;
            ```
2. 비등가 조인 : 등가조인 외의 방식

3. 자체 조인 : 하나의 테이블 안에서 열들의 값을 비교해서 새로운 테이블을 만드는 방법.

    - 사용법 : 같은 테이블을 별칭으로 나누면 OK
        ```
        SELECT E1.MGR,E2.EMPNO,E2.MGR FROM EMP E1,EMP E2 WHERE E1.MGR=E2.EMPNO;
        ```
    ### (아주 중요) 반드시 집고 넘어가야 할 것 : 반드시 각 열이 서로 어떤 관계에 있는 지를 짚고 넘어 가야 한다. 예를들어 mgr 열은 empno에 종속적인 관계인데, 이게 의미 해석이 제대로 안 되어 있어서 결과 해석이 재대로 안 된다. 그러니 꼭 짚고 넘어가자.


    - 주의점 : WHERE 조건에서 같은 테이블이더라도 다른 값이면 행으로 출력하지 않는다.
    
    - 예시
        1. 
        ```
        SELECT E1.ENAME,E1.JOB,E2.ENAME as mgr_name ,E2.JOB as job1 FROM EMP E1,EMP E2 WHERE E1.MGR=E2.EMPNO;
        -- 여기서 where e1.mgr과 e2.empno의 e1,e2가 바뀌면 해석도 변한다.
        ```
        2. E1.EMPNO가 왼쪽, E2.MGR이 오른쪽. 즉 E1이 매니저정보 E2가 부하직원 정보
        ```
        SELECT E1.ENAME,E1.JOB,E2.ENAME as mgr_name ,E2.JOB as job1 FROM EMP E1,EMP E2 WHERE E1.EMPNO=E2.MGR;

        ```
    
4. 내/외부 조인 : 등가/자체 조인은 내부조인, ㅈㄴ 어렵다.
    - 내부조인 : WHERE 조건에 해당하는 데이터가 존재할 경우에만 출력하는 조인
    
    - 외부조인 : WHERE 조건에 해당하지 않은 데이터들은 NULL로 모두 출력하는 형태. 밴다이어 그램으로 항상 해석하는 것이 가장 쉽다.
        - 사용법 : WHERE 절에서 각 테이블의 열을 비교할 때 실제로 구하고 싶은 기준의 집합의 반대 집합에 +를 붙혀준다.
            - EX) WHERE T.COL1=E.COL1(+), T.COL1의 모든 집합을 포함하면서 E.COL1과 겹쳐져는 부분집합까지 출력.
        
        - 사용 예
            1. 모든 매니저가 관리하고 있는 직원들을 보고싶다
            ```
            SELECT E2.ENAME,E1.ENAME FROM EMP E1,EMP E2 WHERE E2.MGR=E1.EMPNO(+);

            ```