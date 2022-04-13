- sqlplus 접속후 사용자명 " sys as sysdba " , 비번은 " orcl " -> 초기 비밀번호 설정한 것

- identified by _ : _는 비밀번호를 뜻하는데, 비밀번호 초기화를 의미하는 것임!
```
alter user scott identified by _;
```

- connect _ = conn _ : _ 다른 계정으로 이동

- select * from tab; : 모든 column 가져오기. tab=table

- desc _ : _은 column 이름이고, 위에 tab를 통해 알 수가 있다.

- 계정 잠금을 풀고 비밀번호 바꾸기
    - alter user _ identified by _ account lock/unlock

1. SYSTEM 접속 , sqlplus system/orcl


2. CREATE USER ORACLE IDENTIFIED BY ORACLE;
-> 사용자가 생성되었습니다.
3. conn oracle , 비번 oracle
-> 연결되지 않았다. 권한이 부여되지 않아서

4. conn system/orcl

5. grant create session to oracle;

6. conn oracle/oracle
-> 연결되었습니다.
--temp 테이블 만들기, 이 또한 권한이 필요하다.
7. conn system/orcl -> 그래서 다시 테이블을 만들 권한을 주기 위해 system 이동

8. grant create table to oracle;
-> 권한이 부여되었습니다.
9. conn oracle/oracle

10. create stable temp (
11. name varchar2(10)
12. , phone number(10)
13. ); 잘 써줘야한다. 오타나면 누락된 괄호 뜸.
-> 테이블이 생성되었습니다.

### 위에 과정은 정말 귀찮다. 일일히 권한을 줘야한다. 그래서 "롤"이 역할인데, 권한을 모아놓은 것이다.