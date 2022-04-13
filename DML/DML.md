- INSERT : " 특정 테이블 "에 데이터를 새로 추가.

- CREATE : 새로운 테이블 만들기,DML은 아니고 DDL이며 필요해서 여기서 정의했다.
    - SKILLE
        1. 급하게 열 구조만 만들어야 할 때
            ```
            CREATE TABLE EMP_TEMO AS SELECT * FROM EMP WHERE 1<>1;
            -- 억지스럽지만 각 행을 조건식에 대입시 FALSE랜다 그래서 데이터 없고 열만 있는 테이블이 나온다.
            ```

- DROP TABLE _TABLE이름_ : 기존에 존재하는 테이블을 삭제할 때 사용한다 사용에 주의하자 복구가 안된다.

# DML은 굉장히 위험하다. 잘못해서 정보를 다 날려버릴 수 있기 때문이다. 모든 작업은 항상 복사한 테이블에서 진행하고 나중에 덮어쓰는 것으로 진행한다.

### 사용중 프로그램 종료후 COMMIT이 뜨면 그냥 눌러준다.

- INSERT 와 CREATE의 사용
```
1. DEPT 테이블을 그대로 가져와서 DEPT_TEMP 라는 이름으로 테이블 만들기.
CREATE TABLE DEPT_TEMP AS SELECT * FROM DEPT;

2. SELECT * FROM DEPT_TEMP; 시 정확히 DEPT와 같음을 확인

3. INSERT INTO DEPT_TEMP (DEPTNO,DNAME) VALUES(50,'MORNING');

4. DEPT_TEMP의 모든 열은 DEPTNO,DNAME,LOC임으로 새로 삽인한 데이터는
50 , 'MORNING' , NULL 값을 갖게 된다.
```


### INSERT

1. INSERT
    - 사용법
        ```
        INSERT INTO _INSERT할 TABLE이름_ (열이름1,열이름2,...)
        VALUES (열이름1에 해당하는 VALUE1,열이름1에 해당하는 VALUE2,....)
        ```
    - 추가 사용법 : VALUES를 서브쿼리로 대체
        - INSERT SUBQUERY유의사항
            1. VALUES절은 절대 사용하지 않는다.
            2. 데이터가 추가되는 테이블 열 개수와 서브쿼리 열 개수 일치
            3. 테이블 자료형과 서브쿼리의 자료형이 정확히 일치.
            4. 추가하려는 열의 이름이 서로 다를지라도 2,3 만 만족하면 다 넣을 수가 있다 이것.
        ```
        INSERT INTO _TABLE_ (NAME,JOB,LOC,BIRTH)
        SELECT T.NAME,T.JOB,T.LOC,T.BIRTH
        FROM TOKO T,SOULE S
        WHERE T.PAY BETWEEN S.LOPAY AND S.HIPAY;
        ```
    - 주의사항 : 반드시 열이름의 개수와 값의 개수는 일치해야하며, 당연히 기존 테이블의 열에서 요구하는 DATA TYPE과도 일치해야 한다. 안 그러면 오류난다.

    - 사용법2 : VALUES의 순서사 INSERT할 테이블의 열 순서와 같다는 전제라면 굳이 열이름을 선언하지 않고 사용해도 된다.
        ```
        -- DEPT의 열순서는 DEPTNO,DNAME,LOC
        INSERT INTO DEPT_TEMP VALUES(60,'IDONTKNOW','SEOUL')
        ```
    - 사용법 3 NULL의 관하여
        1. 삽입할 테이블의 열의 개수만큼 INSERT에서 열선언을 하지 않을 경우 주의사항에 위반되지 않는다면 나머지 열은 NULL로 자동으로 채워진다.
        2. 만약 VALUE는 없어도 해당 열은 선언 했을 경우 ''만 써도 NULL로 인식하지만 " 모든 경우에는 NULL이라고 명시한다 ". 새로운 개발자들이 잘 이해할 수 있게 하기 위해서 이다.

2. INSERT 로 DATE DATA 넣기.
    - 사용법
        1. 'YYYY/MM/DD'
        2. 'YYYY-MM-DD'
    - 주의 : 사용법과 다르게 날짜를 넣을 경우 운영체제에 따라 달라서 위에 형식을 오라클 내부서 정했다.
        - 사용법과 다르게 유입하려면 TP_DATE('DD/MM/YYYY')함수를 사용한다.

3. 그외 사용법 ALL,FIRST,MERGE는 직접 찾아서 하라.

### 테이블에 있는 데이터 수정 UPDATE

- SELECT 와 INSERT에 비해 위험이 큰 명령어. 항상 UPDATE를 실행하기전에 WHERE가 제대로 먹히는 지 SELECT문에서 검증하고 UPDATE의 WHERE절에 지정해준다. 변경할 행만 제대로 뽑아내기 위해서이다.
    - EX : 검증의 예, 한 개이지만 실무에선 아주 복잡하다.
        ```
        1. UPDATE TEMP SET DNAME="DATABASE",LOC="SEOUL"
        WHERE DEPTNO=40

        2. SELECT * FROM TEMP WHERE DEPTNO=40
        ```

- 수정한 내용을 되돌릴 때 " ROLLBACK; " 을 사용. DML의 명령을 취소한다.
1. UPDATE
    - 사용법 : 전체 수정, 일부 수정은 WHERE로 구분
        ```
        UPDATE _변경할테이블_
        SET 변경할열1=데이터 , 변경열2=데이터, ...
        [옵션]WHERE 변경할 열의 데이터 중 특별히 바꿀 값;
        -- EX) 테이블의 모든 값을 FUCKYOU로 바꿈
        UPDATE MY_TABLE SET JOB='FUCKYOU';
        ```
2. 서브쿼리로 데이터 수정하기
    - 사용법
        ```
        1. UPDATE DEPT_TEMP SET DNAME='DATA',LOC='SS' WHERE DEPTNO=40;
        
        2. UPDATE DEPT_TEMP SET (DEPT,LOC) =
        (SELECT DNAME,LOC FROM DEPT WHERE DEPTNO=40)
        WHERE DEPTNO=40;
        ```
    - 열 하나하나를 수정하는 서브쿼리, 당연히 열과 타입 일치는 기본
        ```
        UPDATE DEPT_TEMP
        SET DNAME=(SELECT DNAME FROM DEPT WHERE DEPTNO=40),
        LOC=(SELECT LOC FROM DEPT WHERE DEPTNO=40)
        WHERE DEPTNO=40;
        ```

### 테이블에 있는 데이터 삭제하기 DELETE

1. DELETE
    - 사용법
        ```
        DELETE FROM/없음 _TABLE NAME_
        WHERE _WHERE없으면전체데이터삭제_;
        ```
    - 실무 TIP
        1. WHERE을 지정하지 않아 전체 데이터를 삭제하는 경우는 연습뺴곤 없다.
        2. 반드시 UPDATE와 마찬가지로 WHERE 절의 사전 검증이 확실해야 한다.


