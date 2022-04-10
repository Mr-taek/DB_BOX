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