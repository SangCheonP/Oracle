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

### DML
- [MERGE](./DML/MERGE.md)

### DCL
- [GRANT](/DCL/GRANT.md)
- [REVOKE](/DCL/REVOKE.md)
- [권한 확인](/DCL/권한%20확인.md)

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

- [날짜 포맷](/DATE/Format.md)

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

