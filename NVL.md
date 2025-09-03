# Null 관련 함수
```
Oracle에서 NULL은 “값이 없음”을 의미하는 값이다. 0이나 '' (빈 문자열)과는 다르며, 특히 빈 문자열도 NULL로 간주한다.
그렇기 때문에 SQL 작성 시 NULL을 제대로 처리하지 않으면, 의도치 않은 결과가 나올 수 있다.
```
```yaml
| 함수       | 설명                                               
|------------|---------------------------------------------------
| `NVL`      | 값이 NULL이면 지정한 대체값을 반환한다.               
| `NVL2`     | 값이 NULL인지 아닌지에 따라 서로 다른 값을 반환한다.    
| `NULLIF`   | 두 값이 같으면 NULL을, 다르면 첫 번째 값을 반환한다.    
| `COALESCE` | 여러 값 중 처음 NOT NULL인 값을 반환한다.             
```
<br>

## 1. `NVL` — NULL이면 대체값 반환
### 기본 문법
```sql
NVL(expr1, expr2)
```
- `expr1`이 `NULL`이면 `expr2`를 반환한다.
- `expr1`과 `expr2`는 타입이 같거나 암시적으로 변환 가능해야 한다.

### 예제
```sql
WITH emp AS (
  SELECT 7369 empno, 300  comm FROM dual UNION ALL
  SELECT 7499,       NULL comm FROM dual
)
SELECT empno,
       comm,
       NVL(comm, 0) AS comm_nvl
FROM emp
ORDER BY empno;
```
```yaml
EMPNO | COMM | COMM_NVL
-----------------------
7369  | 300  | 300
7499  | NULL | 0
```
`comm` 컬럼이 NULL일 경우 0으로 치환하여 출력한다.

<br>

## 2. `NVL2` — NULL 여부에 따라 다른 값 반환
### 기본 문법
```sql
NVL2(expr1, expr2, expr3)
```
- `expr1`이 `NULL`이 아니면 `expr2`, `NULL`이면 `expr3`을 반환한다.

### 예제
```sql
WITH emp AS (
  SELECT 'A' ename,  100 comm FROM dual UNION ALL
  SELECT 'B',         NULL comm FROM dual
)
SELECT ename,
       NVL2(comm, 'Y', 'N') AS has_comm
FROM emp
ORDER BY ename;
```
```yaml
ENAME | HAS_COMM
---------------
A     | Y
B     | N
```
커미션이 있으면 `Y`, 없으면 `N`으로 반환한다.

<br>

## 3. `NULLIF` — 두 값이 같으면 NULL
### 기본 문법
```sql
NULLIF(expr1, expr2)
```
- 두 값이 같으면 `NULL`, 다르면 `expr1`을 반환한다.

### 예제
```sql
SELECT NULLIF(100, 100) AS r1,
       NULLIF(100, 200) AS r2
FROM dual;
```
```yaml
R1   | R2
-----|----
NULL | 100
```
`NULLIF(100, 100)`은 NULL을 반환하고, `NULLIF(100, 200)`은 100을 반환한다.


### 실전 활용 — 0으로 나누기 방지
```sql
WITH s AS (
  SELECT 100 amount,  5 qty FROM dual UNION ALL
  SELECT  80 amount,  0 qty FROM dual
)
SELECT amount / NULLIF(qty, 0) AS unit_price
FROM s;
```
```yaml
UNIT_PRICE
----------
20
NULL
```
0으로 나누는 오류를 NULL 처리로 회피할 수 있다.

<br>

## 4. COALESCE — 여러 값 중 첫 번째 NOT NULL 반환
### 기본 문법
```sql
COALESCE(expr1, expr2, ..., exprN)
```
- 왼쪽부터 처음 나오는 NOT NULL 값을 반환한다.

### 예제
```sql
WITH u AS (
  SELECT 'nick' nickname, NULL name_en, NULL name_kr FROM dual UNION ALL
  SELECT NULL,           'Scott',     NULL           FROM dual
)
SELECT COALESCE(nickname, name_en, name_kr, 'N/A') AS display_name
FROM u;
```
```yaml
DISPLAY_NAME
-------------
nick
Scott
```
NULL이 아닌 값을 왼쪽부터 탐색하여 반환한다.