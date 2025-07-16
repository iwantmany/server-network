# server-network

NHN클라우드를 이용한 인스턴스 생성

윈도우 : 192.168.0.56, 133.186.202.16
리눅스 : 192.168.0.63, 133.186.200.103


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
<img width="1398" height="317" alt="image" src="https://github.com/user-attachments/assets/0bcefcb2-174b-4809-8b36-889c519a4810" />

<img width="752" height="402" alt="image" src="https://github.com/user-attachments/assets/73c96012-b87c-4c89-a748-235b15cb0df4" />

② 리눅스 측 설정
JDK 설치

Tomcat 설치 (또는 Java 프로젝트 실행 환경)

Microsoft JDBC 드라이버 다운로드


mssql-jdbc-xxx.jar를 Tomcat의 lib 폴더에 복사

DB 연결 테스트

간단한 JSP/Servlet 코드 or Java CLI 코드로 SQL 접속

▶ 1. Microsoft 리포지터리 등록

sudo curl -o /etc/yum.repos.d/mssql-tools.repo https://packages.microsoft.com/config/rhel/8/prod.repo
▶ 2. mssql-tools + unixODBC 설치

sudo dnf install -y mssql-tools unixODBC
설치 중간에 라이선스 동의 메시지가 나오면 YES 입력

▶ 3. sqlcmd 경로 등록 (선택)

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
source ~/.bash_profile

✅ 2단계: sqlcmd로 MSSQL 접속 시도

sqlcmd -S 192.168.0.56 -U webapp -P '비밀번호'


항목	입력 값
-S	윈도우 SQL 서버의 사설 IP 주소 (예: 192.168.0.10)
-U	SQL 로그인 계정 (예: webapp, sa, 등)
-P	SQL 로그인 비밀번호

✅ 3단계: 성공 시 화면
1>

<img width="781" height="226" alt="image" src="https://github.com/user-attachments/assets/24e5de61-2d84-4bf4-ae5b-7f289e474437" />

<img width="1004" height="292" alt="image" src="https://github.com/user-attachments/assets/5ddd3b15-1a5a-45af-90c9-085147dd9619" />

<img width="1496" height="163" alt="image" src="https://github.com/user-attachments/assets/1bcf6c0d-6552-40fb-812f-be04c5f8e299" />

방법: JDBC 드라이버 ZIP 다운로드 + 수동 설치
📦 1. JDBC 드라이버 ZIP 다운로드

cd /opt/tomcat9/lib
sudo curl -L -O https://github.com/microsoft/mssql-jdbc/releases/download/v12.4.2/mssql-jdbc-12.4.2.jre11.jar
✅ 이 명령은 실제 .jar만 받아오는 명령입니다.
(버전 확인: java -version이 11대 → .jre11.jar)

✅ 2. 다운로드 확인

ls -lh /opt/tomcat9/lib/mssql-jdbc-12.4.2.jre11.jar
→ 파일 크기가 수 MB 이상이면 OK (정상 다운로드)

<img width="1657" height="664" alt="image" src="https://github.com/user-attachments/assets/89c62b7d-8845-4aac-a20a-9af4cc182934" />

<%@ page import="java.sql.*" %>
<html>
<head><title>MSSQL 연결 테스트</title></head>
<body>
<%
    String url = "jdbc:sqlserver://192.168.0.56:1433;databaseName=AdventureWorks2016;encrypt=true;trustServerCertificate=true;";
    String user = "webapp";
    String password = "rjdls123!";

    try {
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        Connection conn = DriverManager.getConnection(url, user, password);
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery("SELECT TOP 5 name FROM HumanResources.Department");

        out.println("<h3>✅ 연결 성공! 부서 리스트:</h3>");
        while (rs.next()) {
            out.println("<p>📌 " + rs.getString("name") + "</p>");
        }

        rs.close();
        stmt.close();
        conn.close();
    } catch(Exception e) {
        out.println("<h3>❌ 연결 실패</h3>");
        out.println("<pre>" + e.toString() + "</pre>");
    }
%>
</body>
</html>


sudo vi /opt/tomcat9/webapps/ROOT/mssqltest.jsp 입력 후 편집화면에서 저장

http://<133.186.200.103>:8080/mssqltest.jsp

<img width="963" height="457" alt="image" src="https://github.com/user-attachments/assets/2fb58a54-3856-4bd4-93a9-1f8401c3af3d" />

<img width="558" height="850" alt="image" src="https://github.com/user-attachments/assets/48f75f52-1955-41e1-a0b3-96485d124d59" />

<img width="489" height="271" alt="image" src="https://github.com/user-attachments/assets/79956469-07fd-4526-8677-4349045be20b" />

✅ [1단계] 부서 상세 조회 기능 추가

vi /opt/tomcat9/webapps/ROOT/mssqltest.jsp

<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<html>
<head>
    <meta charset="UTF-8">
    <title>부서 목록</title>
</head>
<body>
<h2>📋 부서 목록</h2>
<%
    String url = "jdbc:sqlserver://192.168.0.56:1433;databaseName=AdventureWorks2016;encrypt=true;trustServerCertificate=true;";
    String user = "webapp";
    String password = "rjdls123!";

    try {
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        Connection conn = DriverManager.getConnection(url, user, password);

        String sql = "SELECT DepartmentID, Name, GroupName, ModifiedDate FROM HumanResources.Department ORDER BY DepartmentID DESC";
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery(sql);

        while (rs.next()) {
            String id = rs.getString("DepartmentID");
            String name = rs.getString("Name");
            String group = rs.getString("GroupName");
%>
            <p>📁 <a href="deptDetail.jsp?id=<%=id%>"><%=name%></a> - <%=group%></p>
<%
        }

        rs.close();
        stmt.close();
        conn.close();
    } catch(Exception e) {
        out.println("❌ 에러 발생: " + e.toString());
    }
%>
</body>
</html>

vi /opt/tomcat9/webapps/ROOT/deptDetail.jsp

<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<html>
<head><title>부서 상세 정보</title></head>
<body>
<%
    String deptId = request.getParameter("id");

    String url = "jdbc:sqlserver://192.168.0.56:1433;databaseName=AdventureWorks2016;encrypt=true;trustServerCertificate=true;";
    String user = "webapp";
    String password = "rjdls123!";

    try {
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        Connection conn = DriverManager.getConnection(url, user, password);

        String sql = "SELECT * FROM HumanResources.Department WHERE DepartmentID = ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, deptId);
        ResultSet rs = pstmt.executeQuery();

        if (rs.next()) {
%>
            <h2>📌 부서 상세 정보</h2>
            <p><strong>부서 ID:</strong> <%=rs.getString("DepartmentID")%></p>
            <p><strong>부서명:</strong> <%=rs.getString("Name")%></p>
            <p><strong>그룹명:</strong> <%=rs.getString("GroupName")%></p>
            <p><strong>수정일자:</strong> <%=rs.getString("ModifiedDate")%></p>
            <br>
            <a href="mssqltest.jsp">🔙 목록으로 돌아가기</a>
<%
        } else {
            out.println("<p>해당 부서를 찾을 수 없습니다.</p>");
        }

        rs.close();
        pstmt.close();
        conn.close();
    } catch(Exception e) {
        out.println("<h3>❌ 오류 발생</h3>");
        out.println("<pre>" + e.toString() + "</pre>");
    }
%>
</body>
</html>

<img width="1478" height="968" alt="image" src="https://github.com/user-attachments/assets/ab87fcae-2e27-4a43-a6ab-7671eb0e3794" />

<img width="670" height="510" alt="image" src="https://github.com/user-attachments/assets/cfc3f029-3507-459b-94e3-691030df9aaf" />

✅ [2단계] 부서 삭제 기능

1. deptDetail.jsp에 삭제 버튼 추가
<a href="deleteDept.jsp?id=<%=rs.getString("DepartmentID")%>" onclick="return confirm('정말 삭제하시겠습니까?');">🗑️ 삭제</a>

2. deleteDept.jsp 만들기
vi /opt/tomcat9/webapps/ROOT/deleteDept.jsp

<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<html>
<head><title>부서 삭제</title></head>
<body>
<%
    String deptId = request.getParameter("id");

    String url = "jdbc:sqlserver://192.168.0.56:1433;databaseName=AdventureWorks2016;encrypt=true;trustServerCertificate=true;";
    String user = "webapp";
    String password = "rjdls123!";

    try {
        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
        Connection conn = DriverManager.getConnection(url, user, password);

        String sql = "DELETE FROM HumanResources.Department WHERE DepartmentID = ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, deptId);
        int result = pstmt.executeUpdate();

        if (result > 0) {
            out.println("<h3>🗑️ 부서 삭제 성공!</h3>");
        } else {
            out.println("<h3>❗ 삭제할 부서를 찾을 수 없습니다.</h3>");
        }

        pstmt.close();
        conn.close();
    } catch(Exception e) {
        out.println("<h3>❌ 에러 발생</h3>");
        out.println("<pre>" + e.toString() + "</pre>");
    }
%>

<a href="mssqltest.jsp">🔙 목록으로 돌아가기</a>
</body>
</html>

<img width="520" height="920" alt="image" src="https://github.com/user-attachments/assets/8299c62f-656c-425d-877f-a9ef3b44f9a4" />

<img width="733" height="447" alt="image" src="https://github.com/user-attachments/assets/ab7f28da-d9e6-46a1-b6f7-82a453709329" />

<img width="1564" height="408" alt="image" src="https://github.com/user-attachments/assets/06f90378-3571-4b71-8412-bd21707030bb" />

<img width="2141" height="145" alt="image" src="https://github.com/user-attachments/assets/76259a96-bd37-4b1b-b0a6-f6cca3117ee2" />

❌ DELETE가 실패한 이유: EmployeeDepartmentHistory.DepartmentID 컬럼이 Department 테이블의 DepartmentID를 참조하고 있기 때문

DELETE FROM HumanResources.EmployeeDepartmentHistory
WHERE DepartmentID = [삭제할 ID];

DELETE FROM HumanResources.Department
WHERE DepartmentID = [삭제할 ID];

<img width="474" height="111" alt="image" src="https://github.com/user-attachments/assets/836d14bc-2cc9-4426-9512-160ae10c6c7b" />

<img width="538" height="184" alt="image" src="https://github.com/user-attachments/assets/6eefa239-d8e7-4b31-a0e8-caa962703a2c" />

