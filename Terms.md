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