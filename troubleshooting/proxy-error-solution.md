## 문제상황

<img width="1275" height="325" alt="image" src="https://github.com/user-attachments/assets/6311b8e8-cb45-40d8-961e-7195f627fb99" />
- 80번 포트로 요청을 받아 톰캣 서버로 넘기던 중 발생한 nginx error

<br>
<br>

---
## 원인: 네트워크 트래픽 차단 에러
- Rocky Linux의 보안 커널인 SELinux가 작동했기 때문

- 리눅스에는 방화벽 외에도 시스템 내부를 감시하는 커널 보안 시스템인 SELinux(Security-Enhanced Linux)가 켜져있음
  - Nginx가 내부 네트워크를 통해 다른 포트로 통신을 시도하는 행위를 해킹으로 판단하고 차단함
 
---
## 해결 방법: 환경 재설정
```bash
# 1. Nginx가 내부 네트워크 통신을 할 수 있도록 설정을 허용함
setsebool -P httpd_can_network_connect 1

# 2. 크롬 브라우저에서 새로고침
```

---

## 결과(문제상황2)
<img width="876" height="395" alt="image" src="https://github.com/user-attachments/assets/a565ed26-7cff-48a1-b9e8-f9fb1ca94fe5" />

- 사용자의 요청이 Nginx를 통과해서 패킷이 Tomcat 서버 내부를 거처 DB 앞까지 도달했으나 막힌 연결에 실패한 상황

---

## 원인
- DB 서버에 WAS 서버의 ip 주소 접근 허용하는 권한을 줄 때, 비밀번호를 잘못 입력함

---

## 해결방법: DB 서버에서 권한 재부여
```bash
# 1. MariaDB에 재로그인
mysql -u root -p

# 2. 권한 재부여
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'[ip주소]' IDENTIFIED BY '[비밀번호]';

# 3. 변경사항 새로고침
FLUSH PRIVILEGES;

# 4. 재대로 설정됐는지 확인
SELECT Host, User FROM mysql.user WHERE User='myuser';

EXIT;
```

---

## 결과

<img width="886" height="377" alt="image" src="https://github.com/user-attachments/assets/ab703260-ae22-4599-aa39-5f46d0f5c6ae" />

