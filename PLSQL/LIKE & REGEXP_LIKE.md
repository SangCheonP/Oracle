# LIKE & REGEXP_LIKE
## ✅ LIKE
```
%, _를 사용하여 간단한 패턴 매칭때 사용
```

<br>

### ✅ 예제
```sql
-- 'A'로 시작하는 이름
SELECT * FROM EMP
WHERE ENAME LIKE 'A%';

-- 'L'이 두 번째 글자인 이름
SELECT * FROM EMP
WHERE ENAME LIKE '_L%';
```
- `%` : 0개 이상의 문자
- `_` : 정확히 1개의 문자

<br>

## ✅ REGEXP_LIKE
```
문자열이 정규표현식 패턴과 일치하는지 검사하여 LIKE보다 더 유연하게 패턴 매칭이 가능하다
```

<br>

### ✅ 문법
```sql
REGEXP_LIKE(column, 'pattern', 'match_parameter')
```
- `column` : 검사할 컬럼
- `pattern` : 정규표현식 패턴
- `match_parameter` : 옵션 (`i` = 대소문자 무시, `c` = 대소문자 구분 등)

<br>

### ✅ 패턴
### 1. 문자 클래스
```sql
[0-9]     -- 숫자 1개
[A-Z]     -- 대문자 알파벳
[a-z]     -- 소문자 알파벳
[A-Za-z]  -- 영문자
[^0-9]    -- 숫자가 아닌 문자 (부정: ^)
```
```sql
-- 1) 이름에 숫자가 포함된 경우
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '[0-9]');

-- 2) 이름이 대문자 A~Z 로 시작
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^[A-Z]');

-- 3) 이름에 숫자가 아닌 문자가 포함된 경우
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '[^0-9]');

-- 4) 대소문자 구분 없이 'smith' 검색
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, 'smith', 'i');
```

<br>

### 2. 수량자(Quantifier)
```sql
+     -- 1개 이상
*     -- 0개 이상
{n}   -- 정확히 n개 (예: 123)
{n,m} -- 숫자 n~m개 (예: 12, 123, 1234)
```
```sql
-- 1) 이름이 숫자로만 이루어지고, 최소 1개 이상
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^[0-9]+$');

-- 2) 이름이 정확히 3자리 숫자
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^[0-9]{3}$');

-- 3) 이름이 2자리 ~ 4자리 숫자
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^[0-9]{2,4}$');
```

<br>

### 3. 앵커(Anchor)
```sql
^A        -- A로 시작
A$        -- A로 끝남
^...$     -- 전체 일치
```
```sql
-- 이름이 A 또는 S로 시작하는 사원
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^(A|S)');

-- 2) 이름이 N으로 끝남
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, 'N$');

-- 3) 이름이 전체가 대문자 알파벳으로만 구성
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^[A-Z]+$');
```

<br>

### 4. 특수 클래스
```sql
\d        -- 숫자 (0-9와 동일)
\w        -- 영문자 + 숫자 + _(언더바)
\s        -- 공백(스페이스, 탭, 줄바꿈)
.         -- 임의의 한 문자
```
```sql
-- 1) 이름에 숫자가 포함된 경우 (\d)
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '\d');

-- 2) 이름이 알파벳/숫자/언더바만 포함된 경우 (\w)
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '^\w+$');

-- 3) 이름에 공백이 포함된 경우 (\s)
SELECT * FROM EMP
WHERE REGEXP_LIKE(ENAME, '\s');
```
