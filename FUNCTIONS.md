# 함수는 어느 위치던 사용할 수가 있다.

### 내장함수의 종류
- 행단위로 데이터가 처리된다.

1. 단일행 함수 : 한 행 입력 한 행 출력

2. 다중행 함수 : 여러 행 입력 한 행 출력

### 문자 함수
1. UPPER(STRING) : ALL CHARACTER 대문자화

2. LOWER(STRING) : 모든 문자 소문자화.

3. INITCAP(STRING) : 문자열의 첫 글자는 대문자 나머지 소문자.실무에선 덜 쓰임
- EX)
    ```
    SELECT * FROM TABLE WHERE LOWER(NAME)=UPPER('lalala')
    ```
4. LENGTH(STRING) : STRING의 문자 길이를 출력.

5. LENGTHHP(STRING) : STRING의 총 byte 값을 출력. (한글은 문자당 2byte)

6. SUBSTR(STRING,START,LENGTH[DEFAULT는_끝까지]) : 문자열을 시작위치에서부터 LENGTH만큼 자르기.
    - EX 
        ```
        1. LENGTH 함수를 사용
        SELECT *, SUBSTR(COL,LENGTH(COL)-LENGTH(COL))FROM TABLE;
        2. 음수사용으로 문자 맨 뒤 인덱스부터 시작
        SUBSTR(COL,-2)
        -- 문자열 맨뒤에서 2번째부터 마지막까지만 출력.
        ```
7. INSTR : 별로 유용할 것 같지는 않아서 자체적 PASS

8. REPLACE(COL,ORIGINAL CHR,REPLACAED CHR) : 문자열 안에 원본 문자를 대체 문자로 변환시킨다.
    - PARAMETER
        1. COL : TABLE의 COL이나 문자열
        2. ORGINAL CHR : 문자열 안에 있는 대체될 문자
        3. REPLACED CHR : 대체될 문자
    - EX) DATE TYPE의 '/'를 '-'로 교체
        ```
        SELECT REPLACE(HIREDATE,'/','-') AS NEW,HIREDATE FROM EMP;
        ```
9. LPAD/RPAD(COL/STR,데이터총자리수,채울문자(선택)) : LEFT,RIGHT PADDING. 자리수만큼 문자열을 채우고 남은 자리에 빈 공간을 채울 문자로 채우기. 데이터 가명화에 사용가능

10. CONCAT(STR1,STR2) : 두 문자열을 하나로 합친다
    - 응용
        ```
        CONCAT(STR1,CONCAT(':',STR2));
        ```
11. || 연산자 : CONCAT과 유사하게 열, 문자열을 연결한다.
    - EX
        ```
        SELECT EMPNO||':'||ENAME AS PD FROM EMP;
        ```

12. 특정 문자(들)지우기
    - 아래 함수들의 공통점은 왼쪽이던 오른쪽이던 어디던 삭제할 문자열 안 문자들을 시작 지점에서부터 하나씩 대입해서 삭제가능하면 삭제하고 다시 다음 문자를 문자열 그냥 예를들면
    ```
    RTRIM('ABCD','CDB');
    -- 'CDB'중에 하나를 대입해서 D가 있으니 D삭제, ABC만남고 다시 'CDB'안에 C가 있으니 C삭제 다시 B삭제 A만 남는다.
    ```
    1. TRIM(LEADING/TRAILING/BOTH 삭제할문자 FROM COL) : 문쟈열 데이터의 양쪽 공백 제거할 때 사용한다. 유저의 로그인 시 SPACE를 모르고 누른 경우를 제거하기 위해서이다.
    2. R/LTRIM(COL/STR,삭제할문자가있는문자열)


### 숫자 함수