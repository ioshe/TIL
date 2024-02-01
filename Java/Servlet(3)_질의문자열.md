# Servlet(3)_질의 문자열

## 4. 질의 문자열

### 4-1. 개요

- **질의문자열(Query String) :** 클라이언트가 웹서버에 서비스를 요청할 때 추가로 전달하는 데이터를 의미한다.
- **질의문자열 전송 규칙 :** 질의 문자열이 클라이언트에서 서버로 전달 될 때 데이터는 정해진 규칙으로 인코딩(encoding)되어 전달된다.
    1. `name=value` 형식으로 전달, 여러개의 `name=value` 쌍은 `&`으로 구분한다.
    2. 영문자, 숫자, 일부 특수문자는 그대로 전달, 이를 제외한 나머지 자는 `%`기호와 함께 16진수로 바뀌어 전달된다.
    3. 공백문자는 `+` 기호로 변경되어 전달된다.

### 4-2. HTML 입력방식

- **<form>** 태그 : 웹 브라우저에서 다양한 정보를 전달할 수 있도록 입력 양식을 제시한 것.
    - `<form action=”서버프로그램 경로” method=”요청방식”>`

### 4-3. 요청방식에 따른 처리

- GET
    - GET : 요청시 `<form>` 에서 입력한 데이터 들이 `name=value&name=value` 형태로 URL 전송한다
    - 질의문자열을 요청정보 헤더에 포함하여 전달한다.
    - **데이터의 크기에 제한이 있다** : 서버에서 인식할 수 있는 URI 길이가 제한적이기 때문이다. 만약 서버가 처리할 수 있는 이상의 길이가 전달되면 414 오류 코드를 보내도록 정의하고 있다.
- GET 방식으로 요청되는 경우
    - `<a>` 태그를 클릭하여 요청하는 경우 : 기본 요청방식은 GET이다.
    - 브라우저 주소 줄에 URL을 입력하여 요청하는 경우
    - `<form>` 태그에서 method 속성을 생략하여 요청하는 경우
- POST
    - 전달되는 질의 문자열이 요청정보의 몸체에 포함되어 전달된다.
    - `<form>` 태그를 사용해야만 요청할 수 있다.

### 4-4. 서블릿 작성

- member.html코드

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="queryTest" method="GET">
       	이름 :  <input type="text" name="input1"><br><br>
		취미 :  <input type="text" name="input2"><br><br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

- form 의 submit 버튼을 클릭하면 queryTest의 파일의 `method`방식으로 요청되게 된다.
- form action을 html에 정의한 `@WebServlet("/queryTest")`에 맞춰서 작성해야 한다.

```html
package ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/queryTest")
public class QueryTestServlet extends HttpServlet {
	@Override
	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();
		out.print("<html><body>");
		out.print("<h1>GET 방식으로 호출되었습니다.</h1>");
		out.println("</body></html>");
		out.close();
	}

	@Override
	public void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();
		out.print("<html><body>");
		out.print("<h1>POST 방식으로 호출되었습니다.</h1>");
		out.println("</body></html>");
		out.close();
	}
}
```

### 4-5. 질의 문자열 추출

- **String getParameter(String name)**
    - 질의 문자열이 중복되지 않았을 때 사용`(id=guest&pwd=amy) `는 겹치지 않지만 `(color=red&color=yellow)`와 같은 경우는 color가 중복되어 사용불가
- **String[] getParameterValues(String name)**
    - 같은 이름으로 여러 개의 변수가 전달되었을 때 사용
- **String getQueryString()**
    - 질의 문자열 전체를 하나의 STring으로 추출한다. 하지만 이는 GEt 방식 요청에만 사용할 수 있다. post는 요청정보가 몸체에 포함되어 전달되기 떄문이다.
- **ServletInputStream getInputStream() throws IOExeption**
    - HTTP의 요청정보 몸체와 연결된 입력 스트림을 생성하여 변환한다.
    - POST 방식의 질의 문자열 전체를 한번에 추출할 떄 사용한다.

- 한글로 질의어 추출
    - POST 방식
        
        ```java
        resp.setContentType("text/html;charset=UTF-8");
        req.setCharacterEncoding("UTF-8");
        ```
        
        - **resp**의 타입은 `HttpsServletResponse`이다. 따라서 `setContetnType()`메소드는 서버에서 보내는 문서의 타입과 보내는 문자들을 처리할 문자코드를 지정한다.
        - **req**의 타입은 `HttpServletRequest`이다. 따라서 `setCharacterEncoding()` 메소드는 클라이언트가 보낸 요청정보의 몸체에 있는 문자열들을 처리할 문자코드를 지정한다.
    - GET 방식
        - `setCharacterEncoding`은 요청정보의 몸체에 있는 데이터만 인코딩해준다.  따라서 **`GET`** 방식으로 전달된 질의 문자열은 인코딩 되지 않는다.
        
        ```html
        <head>
        <meta charset="UTF-8">
        </head>
        ```
        
        - 따라서 소스에서 현재 페이지를 인코딩할 문자코드를 지정한다.
        - 웹세버에서 바꾸는 방법