# 날짜 함수
## 1. SYSDATE
```
현재 DB 서버의 날짜와 시간을 반환
```
```sql
-- 현재 날짜와 시간
SELECT SYSDATE FROM dual;
-- 결과: 2025-08-27 15:40:25

-- 현재 연도만 출력
SELECT TO_CHAR(SYSDATE, 'YYYY') AS year FROM dual;
-- 결과: 2025

-- 날짜를 원하는 포맷으로 출력
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI') AS now_time FROM dual;
-- 결과: 2025-08-27 15:40
```

<br>

## 2. ADD_MONTHS(date, n)
```
특정 날짜에 개월 수 더하기/빼기
```
```sql
-- 오늘 기준 +6개월
SELECT ADD_MONTHS(SYSDATE, 6) AS six_months_later FROM dual;
-- 결과: 2026-02-27

-- 오늘 기준 -2개월
SELECT ADD_MONTHS(SYSDATE, -2) AS two_months_ago FROM dual;
-- 결과: 2025-06-27

-- 특정 날짜 +1년
SELECT ADD_MONTHS(TO_DATE('2025-01-15', 'YYYY-MM-DD'), 12) AS next_year FROM dual;
-- 결과: 2026-01-15
```

<br>

## 3. MONTHS_BETWEEN(date1, date2)
```
두 날짜 간 개월 수 차이
```
```sql
-- 오늘과 2025-01-01 차이
SELECT MONTHS_BETWEEN(SYSDATE, TO_DATE('2025-01-01', 'YYYY-MM-DD')) AS months_diff
FROM dual;
-- 결과: 7.87 (개월)

-- 두 날짜 차이
SELECT MONTHS_BETWEEN(TO_DATE('2025-08-27', 'YYYY-MM-DD'),
                      TO_DATE('2025-06-15', 'YYYY-MM-DD')) AS diff_months
FROM dual;
-- 결과: 2.39 (개월)

-- 반올림해서 개월 수
SELECT ROUND(MONTHS_BETWEEN(SYSDATE, TO_DATE('2024-12-31', 'YYYY-MM-DD'))) AS rounded_months
FROM dual;
-- 결과: 8
```

<br>

## 4. NEXT_DAY(date, '요일')
```
특정 날짜 이후의 가장 가까운 요일
```
```sql
-- 오늘 이후 가장 가까운 월요일
SELECT NEXT_DAY(SYSDATE, 'MONDAY') AS next_monday FROM dual;
-- 결과: 2025-09-01

-- 특정 날짜 이후 가장 가까운 일요일
SELECT NEXT_DAY(TO_DATE('2025-08-01', 'YYYY-MM-DD'), 'SUNDAY') AS next_sun FROM dual;
-- 결과: 2025-08-03

-- 오늘 이후 가장 가까운 금요일
SELECT NEXT_DAY(SYSDATE, 'FRIDAY') FROM dual;
-- 결과: 2025-08-29
```
👉 오늘이 그 요일이면 `오늘은 포함하지 않고`, 그 다음 주의 같은 요일을 반환

<br>

## 5. LAST_DAY(date)
```
특정 달의 마지막 날짜
```
```sql
-- 이번 달 마지막 날
SELECT LAST_DAY(SYSDATE) AS end_of_month FROM dual;
-- 결과: 2025-08-31

-- 특정 날짜의 마지막 날
SELECT LAST_DAY(TO_DATE('2025-02-10', 'YYYY-MM-DD')) AS feb_end FROM dual;
-- 결과: 2025-02-28 (윤년이 아님)

-- 이번 달 마지막 날 + 1일 → 다음 달 1일
SELECT LAST_DAY(SYSDATE) + 1 AS first_of_next_month FROM dual;
-- 결과: 2025-09-01
```

<br>

## 6. ROUND(date, format) / TRUNC(date, format)
```
날짜 반올림/자르기
```
```sql
-- 이번 달 반올림 (15일 기준)
SELECT ROUND(SYSDATE, 'MONTH') AS round_month FROM dual;
-- 결과: 2025-09-01 (27일 → 올림)

-- 이번 달 1일로 자르기
SELECT TRUNC(SYSDATE, 'MONTH') AS trunc_month FROM dual;
-- 결과: 2025-08-01

-- 올해 반올림 (6월 30일 기준)
SELECT ROUND(SYSDATE, 'YEAR') AS round_year FROM dual;
-- 결과: 2026-01-01

-- 올해의 1월 1일로 자르기
SELECT TRUNC(SYSDATE, 'YEAR') AS trunc_year FROM dual;
-- 결과: 2025-01-01
```

<br>

## 7. EXTRACT(field FROM date)
```
날짜에서 특정 요소 추출
```
```sql
-- 연, 월, 일 추출
SELECT EXTRACT(YEAR  FROM SYSDATE) AS year,
       EXTRACT(MONTH FROM SYSDATE) AS month,
       EXTRACT(DAY   FROM SYSDATE) AS day
FROM dual;
-- 결과: YEAR=2025, MONTH=8, DAY=27

-- 시, 분, 초 추출 (SYSTIMESTAMP 필요)
SELECT EXTRACT(HOUR   FROM SYSTIMESTAMP) AS hour,
       EXTRACT(MINUTE FROM SYSTIMESTAMP) AS minute,
       EXTRACT(SECOND FROM SYSTIMESTAMP) AS second
FROM dual;
-- 결과: HOUR=15, MINUTE=40, SECOND=25.123

-- 주문일 기준 연도별 매출 합계
SELECT EXTRACT(YEAR FROM order_date) AS year, SUM(amount)
FROM orders
GROUP BY EXTRACT(YEAR FROM order_date);
-- 결과: (예시)
-- YEAR | SUM
-- 2024 | 1520000
-- 2025 | 1830000
```

<br>

## 8. TO_CHAR / TO_DATE
```
문자열 <-> 날짜 변환
```
```sql
-- 날짜 → 문자열
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') AS today FROM dual;
-- 결과: 2025-08-27

SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS') AS now_time FROM dual;
-- 결과: 2025/08/27 15:40:25

-- 문자열 → 날짜
SELECT TO_DATE('2025-08-27', 'YYYY-MM-DD') AS to_date_ex FROM dual;
-- 결과: 2025-08-27

-- 주문일자 한국어 표기
SELECT TO_CHAR(order_date, 'YYYY"년" MM"월" DD"일"') AS korean_date
FROM orders;
-- 결과: 2025년 08월 27일
```