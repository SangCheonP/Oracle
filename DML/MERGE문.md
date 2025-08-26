# MERGE 문 (Oracle)

```
Oracle의 MERGE 문은 하나의 쿼리문으로 INSERT, UPDATE, DELETE 작업을 해야하는 경우에 유용하다.
즉, 조건에 따라 UPDATE, INSERT, DELETE 작업을 하나의 쿼리로 수행할 수 있다.
```

<br>

## ✅ 기본 문법
```sql
MERGE INTO 대상테이블 t -- {테이블|뷰}
USING (
        SELECT :id AS id, :name AS name 
        FROM dual
      ) s -- {테이블|뷰|서브쿼리}
ON (t.id = s.id) -- 조건절
WHEN MATCHED THEN -- 일치 {UDPDATE|DELETE}
    UPDATE SET t.name = s.name
    DELETE
WHEN NOT MATCHED THEN -- 불일치 INSERT
    INSERT (id, name)
    VALUES (s.id, s.name)
;
```

<br>

## ✅ 예제
### 단일 데이터
```sql
MERGE INTO EMP e
USING TEMP_EMP t
ON (e.EMP_ID = t.EMP_ID)
WHEN MATCHED THEN
    UPDATE SET e.SALARY = t.SALARY,
               e.DEPARTMENT = t.DEPARTMENT
WHEN NOT MATCHED THEN
    INSERT (EMP_ID, NAME, SALARY, DEPARTMENT)
    VALUES (t.EMP_ID, t.NAME, t.SALARY, t.DEPARTMENT);
```

### 조건부 UPDATE/INSERT
```sql
MERGE INTO PRODUCTS p
USING NEW_PRODUCTS np
ON (p.PROD_ID = np.PROD_ID)
WHEN MATCHED THEN
    UPDATE SET p.PRICE = np.PRICE
    WHERE np.PRICE > 0  -- UPDATE 조건 추가
WHEN NOT MATCHED THEN
    INSERT (PROD_ID, NAME, PRICE)
    VALUES (np.PROD_ID, np.NAME, np.PRICE)
    WHERE np.STOCK > 0; -- INSERT 조건 추가
```
`PRODUCTS` 테이블에 `NEW_PRODUCTS` 데이터를 비교해 **같은 상품은 가격만 조건부 업데이트하고, 없는 상품은 재고 조건에 따라 새로 삽입**하는 MERGE 문


### INSERT / UPDATE / DELETE 통합 예제
```sql
MERGE INTO PRODUCTS p
USING NEW_PRODUCTS np
ON (p.PROD_ID = np.PROD_ID)
WHEN MATCHED THEN
    DELETE WHERE np.PRICE = 0 OR np.STOCK = 0 -- 가격이 0이거나 재고가 없는 상품은 삭제
    UPDATE SET p.PRICE = np.PRICE, -- 그렇지 않으면 가격 업데이트
               p.STOCK = np.STOCK
    WHERE np.PRICE > 0 AND np.STOCK > 0
WHEN NOT MATCHED THEN
    INSERT (PROD_ID, NAME, PRICE, STOCK) -- 새로운 상품 삽입
    VALUES (np.PROD_ID, np.NAME, np.PRICE, np.STOCK);
```
이미 존재하는 상품이 재고가 없으면 삭제하고, 그 외는 업데이트 한다. 아니면 새로운 상품을 추가  

<br>

## ❌ 주의 사항
### ON 조건 컬럼 업데이트 시, 오류
```sql
-- PRODUCTS 테이블: PROD_ID(PK), NAME, PRICE
MERGE INTO PRODUCTS p
USING NEW_PRODUCTS np
ON (p.PROD_ID = np.PROD_ID)  -- 기준 컬럼 PROD_ID
WHEN MATCHED THEN
    UPDATE SET p.PROD_ID = np.NEW_PROD_ID,  -- ❌ ON 컬럼을 업데이트
               p.PRICE = np.PRICE
WHEN NOT MATCHED THEN
    INSERT (PROD_ID, NAME, PRICE)
    VALUES (np.PROD_ID, np.NAME, np.PRICE);
```
MERGE 문에서는 ON 조건에 사용된 컬럼을 UPDATE 하면 안 됨, MERGE 실행 시 `ORA-30926: unable to get a stable set of rows in the source tables` 오류 발생