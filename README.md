# server-network

NHN클라우드를 이용한 인스턴스 생성

1. LINUX 서버생성
![image](https://github.com/user-attachments/assets/c75bfa0b-c250-4f9c-86af-91e0eee96633)
![image](https://github.com/user-attachments/assets/14409337-ae8d-4b64-af36-c02039a3a170)
![image](https://github.com/user-attachments/assets/15e5c5df-b8f3-4d12-afad-2a976f7336ab)

2. WINDOWS 서버생성
![image](https://github.com/user-attachments/assets/b6f45400-c7d0-41b3-bd24-9938d96b3a14)

![image](https://github.com/user-attachments/assets/cf9ed09b-729e-4d5b-8fef-effd7ce2c64b)

<img width="1641" height="981" alt="image" src="https://github.com/user-attachments/assets/eec01d10-0e81-4511-9146-7dec08ce0042" />
<img width="624" height="45" alt="image" src="https://github.com/user-attachments/assets/d926a3f4-b768-41e6-8a7e-af13f8484a6d" />
<img width="677" height="224" alt="image" src="https://github.com/user-attachments/assets/0005ae13-2a0a-440a-9d2f-9a96e69b3eab" />
<img width="1893" height="1013" alt="image" src="https://github.com/user-attachments/assets/0a572b43-cbf1-4676-924f-a91b511b9deb" />

<img width="791" height="729" alt="image" src="https://github.com/user-attachments/assets/022436be-4d40-4f73-bfb3-18c963d160b2" />

① Windows SQL Server 설정
TCP/IP 포트 1433 열기

SQL Server Configuration Manager > TCP/IP 활성화

포트 1433 수동 설정

Windows 방화벽 예외 등록

TCP 1433 포트 허용

SQL 인증 모드(sa)도 활성화 (선택 사항)

SQL Server 인증 + 계정 생성 (sa 또는 webapp 계정)

② 리눅스 측 설정
JDK 설치

Tomcat 설치 (또는 Java 프로젝트 실행 환경)

Microsoft JDBC 드라이버 다운로드


mssql-jdbc-xxx.jar를 Tomcat의 lib 폴더에 복사

DB 연결 테스트

간단한 JSP/Servlet 코드 or Java CLI 코드로 SQL 접속
