# 주제: VMware에 DB 설치하기

## 학습 목표
- **실습 내용:** RDBMS인 MariaDB 설치
- **배우는 개념:** DBMS 설치 및 초기 보안 설정, 데이터베이스 및 사용자 생성, 권한 부여(Grant), 방화벽 설정

---

## 1. mariaDB 설치하기
```bash
# 1. mariaDB 설치하기
dnf install -y mariadb-server

# 2. DB 서비스 시작 및 상태 확인
systemctl start mariadb
systemctl status mariadb

# 3. DB 보안 설정
mysql_secure_installation

# 4DB 접속 및 테이블 확인
mysql -u root -p
show databases;
```
<img width="325" height="150" alt="스크린샷 2026-07-17 161204" src="https://github.com/user-attachments/assets/293e5235-3623-48ae-86c5-6ade29b76d3a" />

## 2. 실습용 DB 및 사용자 계정 생성하기
```bash
# 1. 새 데베 만들기
create database mydb;

# 2. 로컬 접속용 계정 생성 및 권한 부여
grant all privileges on mydb.* to 'myuser'@'localhost' identified by '****';

# 3. 외부 접속용 계정 생성 및 권한 부여
grant all privileges on mydb.* to 'myuser'@'%' identified by '****';

# 4. 변경된 권한 적용
flush privileges;
```

## 3. 나가기
`exit`
