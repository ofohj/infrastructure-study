# 주제: 3 티어 아키텍처 구성하기(server-was-db)

💡실제로 3-Tier 구현하려면?

- 가상머신을 최소 2대 이상 켜서 1번에는 톰캣만, 2번에는 마리아db만 올려둔 후 db를 통해 두 vm이 통신하도록 해야함
- 이 때, 방화벽 포트를 열어줘야 함!

---

## 0. 실습 전, 알아둘 개념

## ★Proxy★

💡클라이언트와 서버 간의 통신을 중계하는 역할을 하는 서버나 소프트웨어

- 클라이언트는 서버에게 직접 요청을 보내지 않고 프록시를 통해 요청을 보낸다
- 프록시는 해당 요청을 서버에 전달하여 응답을 받아 클라이언트에게 전송한다

⇒ 프록시 서버는 프록시의 기능을 수행하기 위해 사용되는 서버이다!!

▼ 프록시 구조에는 두 가지 유형이 있다>> Forward Proxy, Reverse Proxy

### ☆Forward Proxy☆

- 일반적으로 프록시 하면 이 포워드 프록시를 의미
- 구조: 클라이언트 - 프록시 서버 - 인터넷 - 서버
- 효능
    - 익명성, 개인정보보호: 클라이언트의 ip 주소 안알려줌
    - 캐싱: 자주 요청되는 정보를 저장해 빠른 응답 제공
    - 엑세스 제어: 특정 웹사이트 접근 제어. 네트워크 보안 강화
    - 로깅 및 모니터링: 사내 네트워크 사용 현황 분석 가능

### ☆Reverse Proxy☆

- 구조: 클라이언트 - 인터넷 - 프록시 서버 - 서버
- 클라이언트가 특정 자원을 요청하면, 프록시 서버는 이 요청을 받아 서버로 전달하고, 서버의 응답을 클라이언트에게 다시 전달
- 효능
    - 보안 강화
    - 로드 밸런싱
    - 프록시 서버 단에서 SSL 암호화 가능

---

## 1. Nginx(프록시 및 웹 서버용) 설치

- ip 주소 입력 후 연결 확인

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/1b383d2d-9467-4bb9-9216-e4a8987e3eb6" />

---

## 2. VMware에서 서버 복제해 3개로 만들기

<img width="189" height="88" alt="스크린샷 2026-07-19 150852" src="https://github.com/user-attachments/assets/26b00a5c-da14-4e75-85d6-e5c6baa4fa5e" />

---

## 3. MobaXterm에서도 창 3개 켜주기

<img width="1451" height="556" alt="image" src="https://github.com/user-attachments/assets/daeb0aab-5b37-4247-8135-af5e2da06d95" />

| 역할 | 설명 | 사용 프로그램 |
| --- | --- | --- |
| 웹 서버 | 클라이언트의 요청을 가장 먼저 받는 문지기 | Nginx |
| WAS 서버 | 자바 코드를 실행하고 비즈니스 로직을 처리 | Tomcat |
| DB 서버 | 데이터를 보관하는 금고 역할 | MariaDB |

---

## 4. WAS 서버랑 DB 서버 연결하기

1. WAS 서버에서 작성해뒀던 `dbtest.jsp` 파일에 있는 db의 localhost ip 주소를 DB 서버 ip 주소로 수정하기
2. DB 서버 방화벽을 열어서 WAS 서버가 접근할 수 있게 만들기
    
    ▼ (참고) 소프트웨어별 기본 포트번호
    
    | 소프트웨어명 | 기본 포트번호 |
    | --- | --- |
    | Nginx | 80 |
    | Tomcat | 8080 |
    | MariaDB | 3306 |
3. DB 서버에 WAS 서버의 ip 주소 접근 허용하는 권한 주기
    
    ```sql
    -- 외부 접속자용 비밀번호를 정확하게 입력
    GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.226.129' IDENTIFIED BY '****';
    
    -- 권한 새로고침
    FLUSH PRIVILEGES;
    
    -- 종료
    EXIT;
    ```
    
4. Web 서버 ip 주소 입력하기
    
<img width="584" height="246" alt="image" src="https://github.com/user-attachments/assets/2f4ce0e8-4bbd-40e8-90da-df499e914f8d" />
