# 문자열(String) 함수

## 1. `UPPER`, `LOWER`, `INITCAP`
```
문자열 대/소문자 변환
```
```sql
SELECT UPPER('oracle') AS upper_str,   -- ORACLE
       LOWER('ORACLE') AS lower_str,   -- oracle
       INITCAP('oracle database') AS initcap_str  -- Oracle Database
FROM dual;
```

<br>

## 2. `CONCAT`, `||`
```
문자열 연결
```
```sql
SELECT CONCAT('Hello', ' World') AS concat_str,  -- Hello World
       'Hello' || ' World' AS pipe_str           -- Hello World
FROM dual;
```

<br>

## 3. `SUBSTR`
```
문자열 일부 추출
```
```sql
SUBSTR(문자열, 시작위치, 추출할 크기)
```
```sql
-- 1) 기본 사용
SELECT SUBSTR('OracleDatabase', 1, 6) AS result FROM dual;
-- 결과: 'Oracle' (1번째 문자부터 6글자 추출)

SELECT SUBSTR('OracleDatabase', 7) AS result FROM dual;
-- 결과: 'Database' (7번째부터 끝까지)

-- 2) position = 0이면 1로 취급
SELECT SUBSTR('ABC', 0, 2) AS result FROM dual;
-- 결과: 'AB'

-- 3) 음수 position
SELECT SUBSTR('abcdef', -2, 2) AS result FROM dual;
-- 결과: 'ef' (끝에서 두 번째 문자부터 2글자)

-- 4) substring_length > 남은 문자 수
SELECT SUBSTR('abc', 2, 10) AS result FROM dual;
-- 결과: 'bc'

-- 5) position 범위 초과
SELECT SUBSTR('abc', 10, 2) AS result FROM dual;
-- 결과: NULL

-- 6) substring_length <= 0
SELECT SUBSTR('abc', 1, 0) AS result FROM dual;
-- 결과: NULL

-- 7) 멀티바이트 문자
SELECT SUBSTR('가나다라', 2, 2) AS result FROM dual;
-- 결과: '나다' (문자 단위 기준)
```

<br>

## 4. `INSTR`
```
문자열 내 특정 문자의 위치
```
```sql
INSTR(문자열, 패턴, 시작 위치, n번째)
```
```sql
-- 1) 기본 사용 (첫 번째 발생 위치)
SELECT INSTR('Oracle Database', 'a') AS pos FROM dual;
-- 결과: 3 (세 번째 문자에 'a')

-- 2) 두 번째 발생 찾기 (occurrence 지정)
SELECT INSTR('Oracle Database', 'a', 1, 2) AS pos FROM dual;
-- 결과: 9 (두 번째 'a' 위치)

-- 3) 검색 시작 위치 변경
SELECT INSTR('Oracle Database', 'a', 5, 1) AS pos FROM dual;
-- 결과: 9 (5번째부터 검색)

-- 4) 뒤에서부터 검색 (start_position 음수)
SELECT INSTR('Oracle Database', 'a', -1, 1) AS pos FROM dual;
-- 결과: 14 (뒤에서부터 첫 번째 'a')
```

<br>

## 5. `LAPD`, `RPAD`
```
문자열 채우기
```
```sql
SELECT LPAD('123', 5, '0') AS lpad_str,   -- 00123
       RPAD('123', 5, '*') AS rpad_str    -- 123**
FROM dual;
```

<br>

## 6. `LTRIM`, `RTRIM`, `TRIM`
```
공백/특정 문자 제거
```
```sql
TRIM([LEADING | TRAILING | BOTH] 제거할 문자 FROM 대상 문자열)
```
- `LEADING` → 문자열 앞쪽에서 제거
- `TRAILING` → 문자열 뒤쪽에서 제거
- `BOTH` (기본값) → 문자열의 앞뒤 모두에서 제거

```sql
SELECT LTRIM('   Oracle') AS ltrim_str,          -- 'Oracle'
       RTRIM('Oracle   ') AS rtrim_str,          -- 'Oracle'
       TRIM('x' FROM 'xxxOraclexxx') AS trim_str -- 'Oracle'
FROM dual;
```

<br>

## 7. `REPLACE`
```
특정 문자열 치환
```
```sql
-- 1) 문자열 치환
SELECT REPLACE('Hello World', 'World', 'Oracle') AS result FROM dual;
-- 결과: 'Hello Oracle'

-- 2) 문자열 삭제 (replacement_string 생략)
SELECT REPLACE('Hello World', 'World') AS result FROM dual;
-- 결과: 'Hello ' (World 삭제)

-- 3) 부분 치환
SELECT REPLACE('2025-09-01', '-', '/') AS result FROM dual;
-- 결과: '2025/09/01'

-- 4) 여러 번 반복되는 문자열 치환
SELECT REPLACE('ababab', 'ab', 'cd') AS result FROM dual;
-- 결과: 'cdcdcd'

-- 5) 공백 제거
SELECT REPLACE(' Oracle Database ', ' ', '') AS result FROM dual;
-- 결과: 'OracleDatabase'
```

<br>

## 8. `TRANSLATE`
```
문자 단위로 변환 (복수 매핑)
```
```sql
TRANSLATE(변환할 대상 문자열, 바꾸고 싶은 문자들의 집합, 변환될 문자들의 집합)
```
- `from_string` 의 i번째 문자가 있으면 → `to_string` 의 i번째 문자로 변환됨
- 만약 `to_string`에 해당 자리가 없으면 → 삭제됨
```sql
-- 1) 문자 치환
SELECT TRANSLATE('12345', '123', 'abc') AS result FROM dual;
-- 결과: 'abc45'
-- (1→a, 2→b, 3→c로 변환)

-- 2) 문자 삭제 (to_string이 더 짧으면)
SELECT TRANSLATE('12345', '123', 'ab') AS result FROM dual;
-- 결과: 'ab45'
-- (1→a, 2→b, 3→삭제)

-- 3) 특수문자 제거
SELECT TRANSLATE('010-1234-5678', '-_', '') AS result FROM dual;
-- 결과: '01012345678'
-- (-와 _ 모두 제거됨)

-- 4) 영문 대문자를 소문자로 치환
SELECT TRANSLATE('ORACLE', 'ORACLE', 'oracle') AS result FROM dual;
-- 결과: 'oracle'

-- 5) 여러 문자 동시에 변환
SELECT TRANSLATE('2025/09/01', '/-', '  ') AS result FROM dual;
-- 결과: '2025 09 01'
-- (/ → 공백, - → 공백)
```

<br>

## 9. `LENGTH`
```
문자열 길이
```
```sql
SELECT LENGTH('Oracle') AS str_len  -- 6
FROM dual;
```

<br>

## 10. `REGEXP_SUBSTR`, `REGEXP_REPLACE`, `REGEXP_LIKE`
```
정규식 활용
```
```sql
-- 특정 패턴 추출
SELECT REGEXP_SUBSTR('abc123xyz', '[0-9]+') AS only_number  -- 123
FROM dual;

-- 특정 패턴 치환
SELECT REGEXP_REPLACE('2025-08-27', '[^0-9]', '') AS only_digits -- 20250827
FROM dual;

-- 특정 패턴 존재 여부
SELECT CASE WHEN REGEXP_LIKE('abc123', '[0-9]') THEN '숫자 있음'
            ELSE '숫자 없음' END AS check_num
FROM dual;
```