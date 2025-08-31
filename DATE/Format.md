# 날짜 포맷
## 1. 연도 (Year) 
```sql
-- YYYY  : 4자리 연도 → 2025
-- YY    : 2자리 연도 → 25
-- RR    : 2자리 연도 (세기 유연) → 25=2025, 75=1975
-- YEAR  : 영어 연도 → TWENTY TWENTY-FIVE
```

<br>

## 2. 월 (Month)
```sql
-- MM     : 월 (01–12) → 08
-- MON    : 월 약어 (영문) → AUG
-- MONTH  : 월 전체 이름 (영문) → AUGUST
-- RM     : 로마 숫자 월 → VIII
```

<br>

## 3. 일 (Day) 
```sql
-- DD   : 월 기준 일 (01–31) → 27
-- DDD  : 연 기준 일 (001–366) → 239
-- DY   : 요일 약어 → WED
-- DAY  : 요일 전체 이름 → WEDNESDAY
-- IW   : ISO 주차 → 35
```

<br>

## 4. 시간 (Hour/Minute/Second)
```sql
-- HH24 : 24시간 기준 시 (0–23) → 16
-- HH   : 12시간 기준 시 (1–12) → 04
-- MI   : 분 (0–59) → 35
-- SS   : 초 (0–59) → 52
-- FF   : 밀리초 → 123456
```

<br>

## 5. 특수 포맷 
```sql
-- AM / PM      : 오전/오후 → PM
-- A.M. / P.M.  : 오전/오후(점 포함) → A.M.
-- TZD          : 타임존 약어 → UTC
-- TZH:TZM      : 타임존 오프셋 → +09:00
```

<br>

## 📌 사용 예제
```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') AS full_date,
       TO_CHAR(SYSDATE, 'YY/MM/DD') AS short_date,
       TO_CHAR(SYSDATE, 'DY') AS day_of_week,
       TO_CHAR(SYSDATE, 'Month DD, YYYY') AS long_format
FROM dual;

-- 실행 결과 예시
-- FULL_DATE         SHORT_DATE   DAY_OF_WEEK   LONG_FORMAT
-- ----------------  -----------  ------------  ----------------------
-- 2025-08-27 16:35  25/08/27     WED           August   27, 2025
```