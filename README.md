<style>
  /* summary 글자 들여쓰기 */
  summary {
    padding-left: 20px;
  }

  /* details 안 목록 점과 글자 들여쓰기 */
  details ul {
    padding-left: 60px; /* 점과 글자 모두 들여쓰기 */
  }

  /* 각 링크는 블록으로 표시해서 줄바꿈 적용 */
  details ul li a {
    display: inline;
  }
</style>


# Oracle 문법 정리 📘
```
이 저장소는 Oracle에서 자주 사용되는 SQL / PL/SQL 문법을 정리한 자료이다.
현업에서 자주 쓰이는 구문, 예제 코드, 주의사항 등을 카테고리별로 정리하였다.
```


## 📑 목차

### SQL
- [LIKE / REGEXP_LIKE](./LIKE%20&%20REGEXP_LIKE.md)
- [윈도우 함수(Window Function)](./Window%20Function.md)

### DDL
<details>
  <summary>테이블/컬럼/제약조건 수정</summary>
  <ul>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#1-컬럼-추가-add">컬럼 추가 (ADD)</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#2-컬럼-수정-modify">컬럼 수정 (MODIFY)</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#3-컬럼-이름-변경-rename-column">컬럼 이름 변경 (RENAME COLUMN)</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#4-컬럼-삭제-drop-column">컬럼 삭제 (DROP COLUMN)</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#5-제약조건-추가-add-constraint">제약조건 추가 (ADD CONSTRAINT)</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#6-제약조건-수정--삭제">제약조건 수정 / 삭제</a></li>
    <li><a href="/DDL/테이블_컬럼_제약조건_수정.md#7-테이블-이름-변경">테이블 이름 변경</a></li>
  </ul>
</details>

### DCL
- [GRANT](/DCL/GRANT.md)
- [REVOKE](/DCL/REVOKE.md)
- [권한 확인](/DCL/권한%20확인.md)

<details>
  <summary>MERGE</summary>
  <ul>
    <li><a href="./DCL/MERGE.md#1-기본-문법">1. 기본 문법</a></li>
    <li><a href="./DCL/MERGE.md#2-예제">2. 예제</a></li>
    <li><a href="./DCL/MERGE.md#21-단일-데이터">2.1 단일 데이터</a></li>
    <li><a href="./DCL/MERGE.md#22-조건부-updateinsert">2.2 조건부 UPDATE/INSERT</a></li>
    <li><a href="./DCL/MERGE.md#23-insert--update--delete-통합-예제">2.3 INSERT / UPDATE / DELETE 통합 예제</a></li>
    <li><a href="./DCL/MERGE.md#3-주의-사항-">3. 주의 사항 ❌</a></li>
    <li><a href="./DCL/MERGE.md#31-on-조건-컬럼-업데이트-시-오류">3.1 ON 조건 컬럼 업데이트 시 오류</a></li>
  </ul>
</details>

### DATE
<details>
  <summary>날짜 함수</summary>
  <ul>
    <li><a href="/DATE/Function.md#1-sysdate">SYSDATE</a></li>
    <li><a href="/DATE/Function.md#2-add_monthsdate-n">ADD_MONTHS</a></li>
    <li><a href="/DATE/Function.md#3-months_betweendate1-date2">MONTHS_BETWEEN</a></li>
    <li><a href="/DATE/Function.md#4-next_daydate-요일">NEXT_DAY</a></li>
    <li><a href="/DATE/Function.md#5-last_daydate">LAST_DAY</a></li>
    <li><a href="/DATE/Function.md#6-rounddate-format--truncdate-format">ROUND / TRUNC</a></li>
    <li><a href="/DATE/Function.md#7-extractfield-from-date">EXTRACT</a></li>
    <li><a href="/DATE/Function.md#8-to_char--to_date">TO_CHAR / TO_DATE</a></li>
  </ul>
</details>

<details>
  <summary>날짜 포맷</summary>
  <ul>
    <li><a href="/DATE/Format.md#1-연도-year">연도 (Year)</a></li>
    <li><a href="/DATE/Format.md#2-월-month">월 (Month)</a></li>
    <li><a href="/DATE/Format.md#3-일-day">일 (Day)</a></li>
    <li><a href="/DATE/Format.md#4-시간-hourminutesecond">시간 (Hour/Minute/Second)</a></li>
    <li><a href="/DATE/Format.md#5-특수-포맷">특수 포맷</a></li>
    <li><a href="/DATE/Format.md#-사용-예제">📌 사용 예제</a></li>
  </ul>
</details>

### String
<details>
    <summary>문자열 함수</summary>
    <ul>
        <li><a href="./String.md#1-upper-lower-initcap">UPPER / LOWER / INITCAP</a></li>
        <li><a href="./String.md#2-concat-">CONCAT / ||</a></li>
        <li><a href="./String.md#3-substr">SUBSTR</a></li>
        <li><a href="./String.md#4-instr">INSTR</a></li>
        <li><a href="./String.md#5-lpad-rpad">LPAD / RPAD</a></li>
        <li><a href="./String.md#6-ltrim-rtrim-trim">LTRIM / RTRIM / TRIM</a></li>
        <li><a href="./String.md#7-replace">REPLACE</a></li>
        <li><a href="./String.md#8-translate">TRANSLATE</a></li>
        <li><a href="./String.md#9-length">LENGTH</a></li>
        <li><a href="./String.md#10-regexp_substr-regexp_replace-regexp_like">REGEXP_SUBSTR / REGEXP_REPLACE / REGEXP_LIKE</a></li>
    </ul>
</details>


### PL/SQL
- [PL/SQL](./PLSQL/PLSQL.md)

