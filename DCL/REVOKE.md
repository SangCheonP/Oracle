# REVOKE
```
REVOKE 는 이미 부여한 권한을 회수하는 명령어로, GRANT 와 반대 개념이다
```

<br>

## ✅ 기본 문법
```sql
REVOKE 권한 ON 객체 FROM 사용자;
```
- 권한 → 회수할 권한 (SELECT, INSERT, CREATE TABLE 등)
- 객체 → 특정 테이블, 뷰 등 (시스템 권한일 경우 생략 가능)
- 사용자(또는 롤, PUBLIC) → 권한을 빼앗을 대상

<br>

## ✅ 객체 권한 회수
```sql
-- hr.employees 테이블에 대한 SELECT 권한 회수
REVOKE SELECT ON hr.employees FROM dev_user;

-- 여러 권한 동시에 회수
REVOKE INSERT, UPDATE ON hr.employees FROM dev_user;

-- PUBLIC(모든 사용자) 에게 부여된 권한 회수
REVOKE SELECT ON hr.employees FROM PUBLIC;
```
👉 `dev_user` 는 더 이상 `hr.employees` 테이블을 조회할 수 없음

<br>

## ✅ 시스템 권한 회수
```sql
-- CREATE TABLE 권한 회수
REVOKE CREATE TABLE FROM dev_user;

-- CREATE SESSION 권한 회수 (로그인 불가)
REVOKE CREATE SESSION FROM dev_user;

-- DBA 롤(Role) 회수
REVOKE DBA FROM dev_user;
```
👉 시스템 권한도 GRANT 때와 동일하게 `ON 객체` 부분이 없음

<br>

## ✅ 롤(ROLE) 회수
```sql
-- 롤(Role) 회수
REVOKE dev_role FROM dev_user;
```
👉 `dev_user` 는 이제 `dev_role` 이 가진 모든 권한을 사용할 수 없음

## ✅ WITH GRANT OPTION 회수
```
WITH GRANT OPTION 으로 권한을 받은 사용자가 다른 사용자에게 권한을 부여한 경우,
권한을 회수하면 그 하위로 전파된 권한도 함께 사라진다.
```
```sql
-- dev_user 가 user_b 에게 SELECT 권한을 준 경우
REVOKE SELECT ON hr.employees FROM dev_user;
```
👉 `user_b` 도 SELECT 권한을 잃게 됨

<br>

## ⚠️ 주의 사항
1. `WITH GRANT OPTION` 으로 전파된 권한도 함께 회수된다.
2. 시스템 권한 회수 시 사용자가 실행 중인 세션에는 즉시 반영되지 않을 수 있다.