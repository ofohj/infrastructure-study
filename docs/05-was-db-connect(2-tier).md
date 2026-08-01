# 주제: WAS와 DB 연결해 2티어 아키텍처 만들기
## 학습 목표
- **실습 내용:** 이전에 만든 WAS(Tomcat)가 DB(MariaDB)에 직접 접속해서 데이터를 가져와 화면에 뿌려주는 jsp 페이지 띄우기
- **배우는 개념:** 2, 3-Tier 아키텍처의 흐름 이해, DB 커넥션 풀

## 1. 톰캣에 마리아db 자바 커넥터 파일(JDBC 드라이버) 설치
```bash
# 톰캣의 라이브러리 폴더로 이동
cd /opt/tomcat/lib

# 마리아db 자바 커넥터 파일 설치
wget https://repo1.maven.org/maven2/org/mariadb/jdbc/mariadb-java-client/3.1.4/mariadb-java-client-3.1.4.jar
```

## 2. DB에서 데이터 가져와서 보여주는 웹페이지(jsp) 코드 입력하기
- **JSP(Java Server Pages)**: HTML 문서 안에 자바 코드를 넣어 동적 웹페이지를 만들어주는 기술
```bash
# 톰캣이 웹사이트 화면을 저장하는 기본 폴더로 이동
cd /opt/tomcat/webapps/ROOT

# jsp 파일 생성
nano dbtest.jsp
```

## 3. tomcat 재시작하기
```bash
# 톰캣 실행 파일이 있 폴더로 이동
cd /opt/tomcat/bin

# 톰캣 종료
./shutdown.sh

# 톰캣 재실행
./startup.sh
```

## 4. 웹브라우저에서 연동 결과 확인하기

- 웹 브라우저에 url 입력
- `http:[ip주소]=8080(톰캣 포트번/[jsp 파일명]`

<img width="905" height="309" alt="스크린샷 2026-07-17 162331" src="https://github.com/user-attachments/assets/2327adea-aff7-4529-acf9-a36f459cf5cc" />

