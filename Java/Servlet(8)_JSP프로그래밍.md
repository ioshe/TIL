# Servlet(8)_JSP프로그래밍

## 9. JSP 프로그래밍

### 9-1. JSP란?

- JSP(JavaServer Pages)는 HTML,DHTML,XHTML,XML과 같은 동적 웹 콘텐츠를 생성하는 애플리케이션을 만들기 위한 J2EE 플랫폼에 속하는 자바 기술이다.
- JSP의 장점
    - Write Once, Run Anywhere properties
        - jsp는 플랫폼에 무관하고, JSP 스펙을 지원하는 어떠한 웹 애플리케이션 서버에서도 동작한다.
    - 역할 분리
        - JSP는 프레젠테이션 기능과 비즈니스 로직 기능을 분리할 수 있어서 개발자와 디자이너 역할을 분리할 수 있다.
    - 컴포넌트와 태그 라이브러리의 재사용
        - JSP는 자바빈즈 컴포넌트(재사용하는 조각) EJB, 태그 라이브러리를 사용하여 재사용성을 높일 수 있다.
    - 정적 콘텐츠와 동적 콘텐츠 분리
    - 액션, 표현식, 스크립팅 제공
        - 액션(Action)은 JSP에서 사용되는 요소의 하나로 내장 객체, 혹은 서버 측 객체와 상호 동작할 수 있도록 유용한 기능을 추상화한 표준 태그이다.
    - N-tier 엔터프라이즈 애플리케이션을 위한 웹 접근 레이어

### 9-2. JSP 동작 원리

- JSP는 응답정보를 만들기 위해 요청을 어떻게 처리할 것인가를 명세한 태그 기반의 문서이다.
    1. 개발자는 HTML 태그와 JSP 태그를 사용하여 페이지를 작성한 후 확장자를 .jsp 로 저장한다. 
    2. 클라이언트로부터 JSP 요청이 들어오면 JSP 컨테이너는 태그로 만들어진 JSP 파일을 자바 소스로 변환하여 *.java 파일로 만든다.
    3. JSP 컨테이너는 *.jsp 파일을 변환한 *.java 파일을 컴파일하여 *.class 파일을 만든다.
    4. 컴파일된 자바 실행 파일은 서블릿 컨테이너에 의해 서블릿으로서 동작한다.
    5. 변환과 컴파일 작업은 최초의 요청이나 JSP가 변경되었을 때만 수행된다.

### 9-3. JSP 동작

```html
<html>
<body>
	 <!-- HTML 주석문 -->
	<%
		String user = request.getParameter("name");
		if (user == null)
		  // user = "Guest";
	%>
	<h1>
		Hello
		<%-- <%=user%>! --%>
	</h1>
</body>
</html>
```

<img src="../data/Servlet(8)_JSP결과.png">

- JSP 파일이 웹 브라우저에서 실행되었다는 것은 JSP 파일이 자바 소스로 변환되었고, 자바 소스가 컴파일되어 클래스 파일이 생성되었으며, 서블릿 컨테이너가 이 클래스 파일을 실행했다는 것이다.
- 따라서 우리는 이클립스에서 JSP를 실행하였으므로 이클립스에서 지정된 위치에서 example1_jsp.java 파일을 찾을 수 있다.
    
    ```java
    public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
            throws java.io.IOException, javax.servlet.ServletException {
    ```
    
- _jspService() 메소드는 JSP가 실행될 때마다 자동으로 호출되는 메소드이다.
- 모든 JSP가 자바 소스로 변환될 때 _jspService() 메소드가 삽입되는데, 이때 _jspService()메소드 내에 항상 삽입되는 코드가 있고 그 코드는 아래와 같다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a8f5871-50ff-4968-ab22-97950ee212cc/bedecf0a-aa51-49bd-9e5e-11cbfee1aad6/Untitled.png)
    

### 9-4. JSP 태그

- **`<%@ ... %>`**: 지시어(Directive) : JSP 페이지에 대한 설정 정보를 제공한다. 가장 흔한 예는 페이지 지시어(**`<%@ page ... %>`**)로, JSP 페이지의 전체적인 구조와 속성을 정의한다.
    - Page 지시자
        - errorPage와 isErrorPage 속성 :
            - `<%@ page isErrorPage=”true” %>` : 현재 페이지는 오류 처리 페이지임을 설정한다.
            - `<%@ page errorPage=”example3.jsp” %>` **:** 현재 페이지가 실행될 때 오류가 발생하면 해당 오류를 처리할 페이지를 지정한다.
        - `<%@ page trimDirectiveWhitespaces=*"true"* %>` : JSP 소스의 첫 번째 줄 `<%@ page contentType=*"text/html; charset=UTF-8"*%>` 로 생기는 빈 줄을 조절하기 위한 속성
        - `pageEncoding="UTF-8”` : JSP 소스 저장 시 사용할 문자코드를 지정한다. 이를 생략하면 contetntType속성의 charset에 저장된 값으로 설정된다.
        - `session = “true/false”` : JSP 페이지의 세션 관리 처리 여부를 지정하고 할 때 사용한다. 기본값은 true이다.
    - include 지시자
        - `<%@ include file = “파일명” %>` : 다른 페이지에서 include 되어 사용될 페이지를 작성한다.
- **`<% ... %>`**: 스크립틀릿(Scriptlet)
    - JSP 페이지가 요청될 때마다 수행되어야 하는 자바 코드를 추가하고자 할 때 사용하는 태그이다.
    - `<% %>` 사이의 코드는 자바 소스로 변환 시 _jspService() 메소드 내로 그대로 옮겨진다.
- **`<%= ... %>`**: 표현식(Expression)
    - 예를 들어, 변수의 값을 출력하거나 간단한 계산 결과를 표시하는 데 사용된다.
    - 동적인 데이터를 응답 결과에 포함하기 위해 사용한다.
- **`<%! ... %>`**: 선언(Declaration) : 클래스 레벨의 변수나 메소드를 선언하는 데 사용된다. JSP 페이지가 컴파일될 때 이 코드는 서블릿 클래스의 멤버로 변환된다.