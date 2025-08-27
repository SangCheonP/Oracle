# PL/SQL 

```
PL/SQL은 Oracle 데이터베이스에서 사용하는 절차적 확장 언어이다.
SQL에 변수, 조건문, 반복문, 예외 처리 등을 추가해 프로그래밍처럼 작성할 수 있다.
```

<br>

## ✅ 기본 문법
```sql
DECLARE
    -- 변수 선언부
BEGIN
    -- 실행부
EXCEPTION
    -- 예외 처리부 (선택)
END;
```
- `DECLARE` : 변수, 상수, 커서 선언
- `BEGIN` : 실행할 SQL 문장
- `EXCEPTION` : 오류 발생 시 처리 로직
- `END` : 블록 종료

<br>

## ✅ 변수 선언
```sql
DECLARE
    v_name VARCHAR2(50); -- 선언만
    v_price NUMBER := 1000;  -- 선언 및 초기화
BEGIN
    v_name := '상품A';
END;
```

<br>

## ✅ 조건문 (IF)
```sql
DECLARE
    v_stock NUMBER := 10;
BEGIN
    IF v_stock > 0 THEN
        DBMS_OUTPUT.PUT_LINE('재고 있음');
    ELSE
        DBMS_OUTPUT.PUT_LINE('재고 없음');
    END IF;
END;
```
`IF ~ END IF`로 구문 작성

<br>

## ✅ 반복문
### 기본 LOOP
```sql
DECLARE
    i NUMBER := 1;
BEGIN
    LOOP
        EXIT WHEN i > 5;
        DBMS_OUTPUT.PUT_LINE('i = ' || i);
        i := i + 1;
    END LOOP;
END;
```
- `LOOP ~ END LOOP`로 구문 작성
- `EXIT`로 루프 탈출


<br>

### FOR LOOP
```sql
BEGIN
    FOR i IN 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE('i = ' || i);
    END LOOP;
END;
```
- `FOR 변수 IN 시작..끝 LOOP ~ END LOOP`로 구문 작성
- 기본 LOOP와 같이 `EXIT`로 루프 탈출

<br>

## ✅ 커서(Cursor)
```
SELECT문으로 여러 행을 가져올 때, PL/SQL에서 한 줄씩 처리할 수 있도록 도와주는 도구이다.
```
```sql
DECLARE
    -- EMP 테이블에서 사번과 이름을 가져오는 SELECT 쿼리를 커서 c_emp에 정의
    CURSOR c_emp IS SELECT EMPNO, ENAME FROM EMP;
    v_empno EMP.EMPNO%TYPE; -- 컬럼 타입을 그대로 사용하기 위해 %TYPE 사용
    v_ename EMP.ENAME%TYPE;
BEGIN
    OPEN c_emp;
    LOOP
        FETCH c_emp INTO v_empno, v_ename; -- 결과 집합에서 한 행씩 꺼내 변수에 저장
        EXIT WHEN c_emp%NOTFOUND; -- 더 이상 읽을 행이 없으면 LOOP 종료
        DBMS_OUTPUT.PUT_LINE('사번: ' || v_empno || ', 이름: ' || v_ename);
    END LOOP;
    CLOSE c_emp;
END;

출력 예시
사번: 7369, 이름: SMITH
사번: 7499, 이름: ALLEN
...
```
작동 순서: `OPEN` → `SELECT` 실행 → `FETCH`로 한 행씩 → `CLOSE`로 커서 종료

### FOR LOOP + 커서 방식
```sql
BEGIN
    FOR rec IN (SELECT EMPNO, ENAME FROM EMP) LOOP -- 각 행은 rec 레코드 변수에 저장
        DBMS_OUTPUT.PUT_LINE('사번: ' || rec.EMPNO || ', 이름: ' || rec.ENAME);
    END LOOP;
END;
```

<br>

## ✅ 예외 처리
```sql
DECLARE
    v_num NUMBER := 0;
BEGIN
    v_num := 10 / v_num;
EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('0으로 나눌 수 없습니다.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('기타 오류 발생');
END;
```