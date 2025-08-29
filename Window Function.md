# 윈도우 함수(Window Function)
```
행(Row)들의 집합에 대해 "창(Window)"을 정의하고, 그 범위 안에서 집계 함수나 순위 함수 등을 수행할 수 있게 해준다.
일반 집계 함수(AVG, SUM 등)는 결과를 하나의 행으로 줄여주지만, 윈도우 함수는 원래 행을 유지하면서 결과를 컬럼으로 추가한다
```

## ✅ 기본 문법
```sql
윈도우 함수(인자) 
OVER (
    PARTITION BY 컬럼
    ORDER BY 컬럼
    ROWS BETWEEN 범위 지정
)
```

- `PARTITION BY` : 그룹을 나눌 기준 (GROUP BY와 유사)
- `ORDER BY` : 창(Window) 안에서 정렬 기준
- `ROWS BETWEEN` : 창(Window)의 범위 지정 (예: 현재 행 기준 앞뒤 몇 개 행)

<br>

## ✅ 주요 윈도우 함수 종류
### 1. 집계 함수 (Aggregate Window Functions)
일반 집계 함수들을 `OVER()`와 함께 사용
```sql
-- 사원별 급여, 부서별 평균 급여 같이 보기
SELECT empno, ename, deptno, sal,
       AVG(sal) OVER(PARTITION BY deptno) AS dept_avg_sal,
       SUM(sal) OVER(PARTITION BY deptno) AS dept_total_sal
FROM emp;

-- 예시 출력
-- EMPNO | ENAME  | DEPTNO | SAL  | DEPT_AVG_SAL | DEPT_TOTAL_SAL
-- 7369  | SMITH  | 20     | 800  | 2175         | 8700
-- 7566  | JONES  | 20     | 2975 | 2175         | 8700
-- 7788  | SCOTT  | 20     | 3000 | 2175         | 8700
-- 7902  | FORD   | 20     | 3000 | 2175         | 8700
-- 7499  | ALLEN  | 30     | 1600 | 1566.67      | 9400
-- 7521  | WARD   | 30     | 1250 | 1566.67      | 9400
```
👉 `PARTITION BY`로 부서를 나누고, 각 부서별 평균과 합계를 같이 출력

<br>

### 2. 순위 함수 (Ranking Functions)
- `ROW_NUMBER()` : 순차적으로 번호 부여 (중복 없음)
- `RANK()` : 동일 값은 같은 순위, 건너뜀 (1,2,2,4...)
- `DENSE_RANK()` : 동일 값은 같은 순위, 건너뛰지 않음 (1,2,2,3...)
```sql
-- 급여 기준으로 순위 매기기
SELECT empno, ename, sal,
       ROW_NUMBER() OVER(ORDER BY sal DESC) AS row_num,
       RANK()       OVER(ORDER BY sal DESC) AS rank,
       DENSE_RANK() OVER(ORDER BY sal DESC) AS dense_rank
FROM emp;

-- 예시 출력
-- EMPNO | ENAME   | SAL  | ROW_NUM | RANK | DENSE_RANK
-- 7839  | KING    | 5000 | 1       | 1    | 1
-- 7788  | SCOTT   | 3000 | 2       | 2    | 2
-- 7902  | FORD    | 3000 | 3       | 2    | 2
-- 7566  | JONES   | 2975 | 4       | 4    | 3
-- 7698  | BLAKE   | 2850 | 5       | 5    | 4
```

<br>

### 3. 누적/이동 집계 (Analytic Aggregate Functions)
- `SUM() OVER(ORDER BY ...)` : 누적 합계
- `AVG() OVER(ORDER BY ...)` : 누적 평균
- `LAG()` : 이전 행 값
- `LEAD()` : 다음 행 값
```sql
-- 사원의 급여에 대해 누적 합계, 이전/다음 급여 비교
SELECT empno, ename, sal,
       SUM(sal) OVER(ORDER BY sal) AS running_total,
       LAG(sal)  OVER(ORDER BY sal) AS prev_sal,
       LEAD(sal) OVER(ORDER BY sal) AS next_sal
FROM emp;

-- 예시 출력
-- EMPNO | ENAME  | SAL  | RUNNING_TOTAL | PREV_SAL | NEXT_SAL
-- 7369  | SMITH  | 800  | 800           | (NULL)   | 1100
-- 7876  | ADAMS  | 1100 | 1900          | 800      | 1250
-- 7521  | WARD   | 1250 | 3150          | 1100     | 1250
-- 7654  | MARTIN | 1250 | 4400          | 1250     | 1300
-- 7934  | MILLER | 1300 | 5700          | 1250     | 1500
```
👉 SUM() OVER(ORDER BY sal)은 급여 순으로 누적합

<br>

### 4. NTILE 함수 (구간 나누기)
```
데이터를 N등분해서 그룹 번호 부여
```
```sql
-- 급여를 4분위로 나누기
SELECT empno, ename, sal,
       NTILE(4) OVER(ORDER BY sal DESC) AS quartile
FROM emp;

-- 예시 출력
-- EMPNO | ENAME  | SAL  | QUARTILE
-- 7839  | KING   | 5000 | 1
-- 7788  | SCOTT  | 3000 | 1
-- 7902  | FORD   | 3000 | 2
-- 7566  | JONES  | 2975 | 2
-- 7698  | BLAKE  | 2850 | 3
-- 7654  | MARTIN | 1250 | 4
```
👉 급여 순으로 정렬한 뒤 4등분하여 그룹 번호를 부여

<br>

## ✅ ROWS BERWEEN
- `PRECEDING` : 이전 행
- `FOLLOWING` : 다음 행
```sql
-- 현재 행과 앞뒤 1개씩 포함한 평균 급여
SELECT empno, ename, sal,
       AVG(sal) OVER(
           ORDER BY empno
           ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
       ) AS moving_avg
FROM emp;

-- 예시 출력
-- EMPNO | ENAME  | SAL  | MOVING_AVG
-- 7369  | SMITH  | 800  | (800+1600)/2 = 1200.0
-- 7499  | ALLEN  | 1600 | (800+1600+1250)/3 = 1216.7
-- 7521  | WARD   | 1250 | (1600+1250+2975)/3 = 1941.7
-- 7566  | JONES  | 2975 | (1250+2975+1250)/3 = 1825.0
-- 7654  | MARTIN | 1250 | (2975+1250+2850)/3 = 2358.3
```
👉 현재 행 + 앞 행(1 PRECEDING) + 뒤 행(1 FOLLOWING)을 평균한 값