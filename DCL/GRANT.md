# GRANT
```
GRANT 문은 오라클에서 사용자에게 권한을 부여하는 명령어인데, 크게 시스템 권한과 객체 권한으로 나눌 수 있다.
```

<br>

## ✅ 객체 권한(Object Privilege)
```
특정 스키마의 테이블, 뷰 등에 대한 권한
```
### ✅ 기본 문법
```sql
GRANT 권한 ON 객체 TO 대상
```
```sql
-- HR 사용자가 EMPLOYEES 테이블을 가지고 있음
-- DEV_USER 에게 권한을 부여

-- SELECT 권한 부여
GRANT SELECT ON hr.employees TO dev_user;

-- INSERT 권한 부여
GRANT INSERT ON hr.employees TO dev_user;

-- UPDATE 권한 부여
GRANT UPDATE ON hr.employees TO dev_user;

-- DELETE 권한 부여
GRANT DELETE ON hr.employees TO dev_user;

-- 모든 권한 부여 (실무에서는 지양!)
GRANT ALL ON hr.employees TO dev_user;
```
👉 `dev_user` 는 이제 `hr.employees` 테이블에서 SELECT, INSERT, UPDATE 가능

<br>

## ✅ 시스템 권한
```
데이터베이스 전체에 영향을 주는 권한 (예: CREATE TABLE, CREATE USER 등)
```

### ✅ 기본 문법
```sql
GRANT 권한 TO 대상
```
```sql
-- 사용자 생성
CREATE USER dev_user IDENTIFIED BY dev1234;

-- 접속 권한 (세션 생성)
GRANT CREATE SESSION TO dev_user;

-- 테이블 생성 권한
GRANT CREATE TABLE TO dev_user;

-- 뷰(View), 시퀀스(Sequence), 프로시저 권한
GRANT CREATE VIEW, CREATE SEQUENCE, CREATE PROCEDURE TO dev_user;

-- DBA 권한 (최고 권한, 매우 주의!)
GRANT DBA TO dev_user;
```
👉 시스템 권한은 ON 객체 부분이 없음

<br>

## ✅ ROLE
```
여러 권한을 모아 롤(Role)을 만들어 사용자에게 부여
```
```sql
-- 롤 생성
CREATE ROLE dev_role;

-- 롤에 권한 부여
GRANT CREATE SESSION, CREATE TABLE TO dev_role;
GRANT SELECT, INSERT ON hr.employees TO dev_role;

-- 사용자에게 롤 부여
GRANT dev_role TO dev_user;
```
👉 `dev_user` 는 이제 `dev_role` 이 가진 모든 권한 사용 가능
👉 권한 관리가 편해짐 (여러 사용자에게 일괄 적용 가능)

<br>

## ✅ WITH GRANT OPTION
```
부여받은 권한을 다른 사용자에게도 줄 수 있는 옵션 (⚠️ 보안 주의!)
```
```sql
-- dev_user 가 SELECT 권한을 다른 사람에게도 줄 수 있음
GRANT SELECT ON hr.employees TO dev_user WITH GRANT OPTION;
```
👉 `dev_user` → `user_b` 에게 다시 SELECT 권한을 줄 수 있음  
👉 하지만 잘못 쓰면 권한이 무분별하게 퍼져 보안 이슈 발생