- 다중행함수 : 그룹 함수, 복수행 함수로 불린다. 여러 행을 바탕으로 하나의 결과를 도출하는 함수이다.
{
1. SUM(DISTINCT/ALL[선택],COL) : COL 안에 있는 모든 값을 더한다.
    - NULL은 자동으로 무시한다.
    - PARAMETER
        1. DISTINCT/ALL[DEFAULT] : DISTINCT는 중복되는 데이터는 포함하지 않는 다는 것이다. 즉 더할 값 중 2500나오고 나중에 2500이 나오면 이 값은 더 이상 더하지 않는 것이다.

2. COUNT(DISTINCT/ALL[선택],COL) : 개수를 새준다.
    - 자동으로 NULL은 무시한다.

    - EX 1) SELECT COUNT(*) ~ : SELECT문의 결과로 나온 총 행의 개수를 반환한다.
    - EX 2) 조건을 만족하는 데이터 개수. 특정 회원이 작성한 글, 댓글 수, 글에서 받은 찬/반 수등을 조합해서 회원 등급,레벨 관리 가능
        ```
        SELECT COUNT(*) FROM EMP WHERE SAL>10000;
        -- SAL이 1만을 넘기는 행의 개수를 리턴
        ```
#### oracle은 날짜,문자 데이터 역시 크기 비교가 가능해서 MAX,MIN을 사용해서 원하는 값을 추출 가능
3. MAX(DISTINCT/ALL[선택],COL),MIN(DISTINCT/ALL[선택],COL) : 최대값 반환,최소값 반환
    - EX) 제일 큰 DATE 출력
        ```
        SELECT MAX(HIREDATE),MIN(HIREDATE) FROM EMP;
        ```
####

4. AVG(DISTINCT/ALL[선택],COL) : 행의 평균을 계산

} OVER(분석을 위한 여러 문법을 지정)[선택] , 위에 4개는 OVER라는 걸 사용가능.



#### 결과 값을 열로 묶어 출력

1. GROUP BY 1_ 2_ 3_ .. : 1_ 2_ 3_ .. 그룹화할 열을 지정한다.
    - 그룹 : 한 데이터(열) 안에 동질(같은) 값들 끼리 모아 놓는다.
    - 사용법
        ```
        SELECT ~ FROM ~ WHERE ~ GROUP BY ~ ORDER BY ~
        -- WHERE 나 ORDER BY가 꼭 있어야 하는 건 아니다.
        ```
    - 원리
        1. 1_ 을 먼저기 준으로 그룹을 묶는다 예를들어 1_에서 범주를 10~100 , 10개 씩 나뉘어져 있다면 {10,20,30....100} 으로 그룹을 짜게 된다. 여기서 {10:4개,20:30개,..} 이런식으로 생각해볼 수 있다(물론 반드시 모든 열의 개수는 같아야 함으로 위에 예는 있을 수 없다).
        2. 1_에서 {10:4개,20:30개..}로 묶였는데 여기서 4개 30개가 바로 2_에 해당한다. 다시 2_를 기준으로 또 그룹화 한다.
        3. 최종적으로 GROUP BY에 묶여진 열들을 기준으로 다중행함수의 값이나

    - 주의사항 : 그룹된 열을 사용해서 SELECT 한 것들은 반드시 단일 값이거나 그룹으로 묶여있어야 한다.

    - EX)
        ```
        select AVG(SAL),JOB,DEPTNO from emp group by JOB,DEPTNO;
        ->
        AVG(SAL) JOB    DEPTNO
        2975	MANAGER	    20
        5000	PRESIDENT	10
        1300	CLERK	    10
        1400	SALESMAN	30
        3000	ANALYST	    20
        2850	MANAGER	    30
        2450	MANAGER	    10
        950	    CLERK	    30
        950	    CLERK	    20

        -- 가만히 보면 규칙이 안 보이지만 뜯어보면 이렇다
        -- JOB으로만 묶이면 직업은 총 5개로 5개 안에서 평균치가 나온다
        -- 여기서 DEPTNO라는  새로운 변수가 그룹으로 묶이면서
        -- 언뜻보면 중복되어 보이는데 아니다
        -- 잘 보면 중복되는 것이 하나도 없다, JOB + DEPTNO를 합한것중에 중복되는 것을 찾아보라
        -- 즉 이는 " JOB이면서 DEPTNO 인 것들의 묶음 " 인 것이다.
        ```

2. HAVING 1_[조건식] : 오직 GROUP BY 절이 있을 때만 사용할 있다. HAVE, 가지다라는 의미로 해석해서 GROUPB BY 로 묶인 것 중에 1_식이 참인 것만 출력할 것이다.
    - WHERE 절과 HAVING의 우선순위 차이[가장먼저숙지하고아래읽기]
        : WHERE은 SELECT된 열중에서 조건식을 통해 걸러내고 HAVING은 GROUP BY가 사용됐을 때만 사용가능하다. 즉 GROUP BY는 WHERE로 걸러진 데이터들을 토대로 그룹을 짜는 것이고 HAVING은 걸러진 릴레이션을 가지고 그룹의 결과에 조건식에 따라 새 테이블을 만든다.


    - 사용법 : WHERE과는 차별됨을 기억. 오직 그룹 용으로 HAVING을 사용한다
        - 사용 패턴
            ```
            SELECT ~ FROM ~ WHERE ~ GROUP BY ~ HAVING ~ ORDER BY ~
            ```
        - 수정 예
            ```
            SELECT NAME,AVG(NUMBER)
            FROM TABLE
            GROUP BY NAME,NUMBER
            HAVING AVG(NUMBER)>1000;
            ```
        - 의미 없지만 사용법은 맞는 예
            ```
            SELECT ENAME, AVG(SAL) FROM EMP GROUP BY ENAME, SAL HAVING AVG(SAL)>1000 OR SAL<2000;
            -- 어차피 이름이 겹치는 경우는 없어서 총 14개 그룹이 만들어진다. 단지 HAVING절에서 걸러질 뿐
            ```



### Advanced GROUPING FUNCTIONS
'SELECT DEPTNO,JOB,COUNT(*),AVG(SAL)FROM EMP GROUP BY ROLLUP(DEPTNO,JOB) ORDER BY DEPTNO,JOB;'
1. ROLLUP,CUBE

2. GROUPING SETS, GROUPING_ID


3. LISTAGG(col,sep) WITHIN GROUP(ORDER BY condi DESC/ASC) : 그룹으로 묶인 내용들을 보고 싶다면 기존에는 group by 로 하나를 묶고 그 다음 대그룹에 묶인 열을 소그룹해서 order by로 정렬해야만 했다. 그러나 이건 보기가 귀찮다, 나는 하나의 대그룹으로 묶고 그 대그룹에 속한 소그룹 애들을 가로로 보기 편하게 하고 싶다.
    - 사용방법
        : SELECT DEPTNO,LISTAGG(ENAME,' ') WITHIN GROUP(ORDER BY SAL) FROM EMP GROUP BY DEPTNO;

