# Servlet(5)

## 6. 상태정보 유지 기술

### 6-1. 상태정보 유지

- **무상태(Stateless)** : HTTP 프로토콜의 통신 방식은 클라이언트와 서버 간의 연결을 클라이언트로부터 요청이 있을 때마다 매번 새롭게 연결한다. = 클라이언트와 서버 간에 연결상태가 유지되지 않는 통신방식이다.
- **상태정보(State Information)** : 클라이언트나 서버에 계속된 요청에서 사용할 수 있도록 저장한 정보들이다.

### 6-2. 저장위치 분류

- **클라이언트 측에 저장** : 웹에서 클라이언트는 웹 브라우저를 의미한다. 즉 웹 브라우저에 저장
    - `쿠키(Cookie)` : 클라이언트 측에 정보를 유지하는 기술이다. 웹브라우저가 종료된 이후에도 계속 유지된다.
- **서버 측에 저장** : 서버의 힙 메모리 영역에 만들어진 객체에 상태정보를 저장하는 것이다.
    - `javax.servlet.ServletContext` : 웹 애플리케이션 서비스가 시작될 때 생성되고 종료될 때 소멸한다.
    - `javax.servlet.http.HttpSession` : 클라이언트 단위로 유지한다.
    - `javax.servlet.http.HttpServletRequest` : 요청 단위 유지로 하나의 요청에만 상태정보를 유지한다.

### 6-3. ServletContext 정보공유

- **웹 애플리케이션 단위 정보 공유** : 웹 애플리케이션은 서버에 `한 번` 배포되며, 각 클라이언트는 필요한 정보와 기능을 서버로부터 받아와 사용자에게 표시하는 방식으로 동작한다.
    - `ServletContext` 객체를 사용하여 여러 페이지에서 사용할 데이터를 하나의 ServletContext객체에 접근하여 공유된 데이터를 사용할 수 있다.
        
        <img src = "../data/Servlet(5)_ServletContext 객체.png">
        
    - `void setAttribute(String name, Object value)`
        - 웹 애플리케이션 범위에서 공유할 데이터를 `ServletContext` 객체에 등록하는 메소드이다.
    - `Object getAttribute(String name)`
        - ServletContext 객체에 등록한 데이터를 추출하는 메소드이다.
    - `void removeAttribute(String name)`
        - ServletContext 객체에 setAttribute() 메소드로 등록한 데이터를 삭제하는 메소드이다.

### 6-4. 쿠키(Cookie) 정보 공유

- **클라이언트 단위**로 상태정보를 유지하는 상황일 때 사용한다. <br>
ex)몇 번째 방문인지 확인, 장바구니
- **쿠키란** : 서버가 클라이언트에 저장하는 정보로서 클라이언트 쪽에 필요한 정보를 저장해놓고 필요할 때 추출하는 것을 지원하는 기술이다.
- 클라이언트에 저장된 쿠키 정보는 서버에 방문할 떄 **자동으로** 요청정보의 헤더 안에 포함되어 전달된다.
- **쿠키 생성**
    - **쿠키생성** : `Cookie(String name, String value)`
        - javax.servlet.http.Cookie 객체를 생성한다.
    - **쿠키 유효시간 설정** : `setMaxAge(int expiry)`
        - 클라이언트로 전송되는 쿠키의 유효 시간을 설정한다. <br>expiry 값은 쿠키의 유효시간(초)를 의미한다.
        - 0은 쿠키 삭제를 의미하며, 음수 설정 시 브라우저가 종료되면 쿠키도 자동 삭제된다.
        - 유효시간을 설정하지 않으면 음수값이 적용된다.
    - **쿠기 경로 설정** : `setPath(String uri)`
        - 클라이언트가 접속한 기록이 있으면 기본적으로 요청 정보 헤더 안에 쿠키가 포함되어 서버 쪽으로 전송된다. 
        - 이 때 서버의 모든 요청에 대하여 쿠키가 서버 쪽으로 전송되는 것이 아니라, 특정 경로의 요청에서만 쿠키를 전송하고자 할 때 `setPath()`를 지정한다.
        - 이를 지정하면 지정된 경로와 그 하위 경로의 요청에 대해서만 클라이언트부터 쿠키가 전송된다.
    - **쿠키 도메인 설정** : `setDomain(String domain)`
        - 여러 대의 서버가 연결되어 서비스를 처리할 때, 하나의 서버에서 클라이언트로 전송된 쿠키를 다른 서버에서 읽어 들여 사용하는 메서드다.
    - **쿠기 전송** : `addCookie(Cookie cookie)`
        - 생성된 쿠키를 클라이언트로 보낼 때 HttpServletResponse 객체의 `addCookie()` 메서드를 이용한다.
- **쿠키 추출**
    - **쿠키 추출** : `Coockie[] getCookies()`
        - 클라이언트로부터 받은 HTTP 요청에서 쿠키를 추출한다.
        - HttpServletRequest 객체의 `getCookies()` 메서드를 사용하여 쿠키 배열을 반환받는다.
    - **쿠키 검색** : `String getName()`
        - 추출된 쿠키 배열에서 특정 쿠키를 찾기 위해 쿠키의 이름을 검색한다.
    - **쿠키 값 추출** : `String getValue()`
        - 특정 쿠키의 값을 추출한다.

### 6-5. 세션 정보 공유

- HTTP 기반으로 동작하는 클라이언트가 서버에 정보를 요청할 때 생성되는 `상태정보`를 세션이라고 한다.
- `HTTPSession` 객체가 생성될 때는 클라이어느의 정보를 조합한 세션 ID가 부여된다.
- 이 세션 ID는 클라이언트 측에 쿠키 기술로 저장되며, `HttpSession` 객체는 서버에 생성된다.

| 구분 | 쿠키 | 세션 |
| --- | --- | --- |
| 저장위치 | 클라이언트 | 서버 |
| 저장 데이터 타입 | 텍스트 | 객체 |
| 저장 데이터 크기 | 제한있음 | 서버에서 수용할 수 있는 만큼 |
- **HttpSession 생성**
    - `HttpSErvletRequest의 getSession()`
        - 클라이어트가 가지고 있는 세션 ID와 동일한 세션 객체를 찾아서 주솟값을 반환한다.
        - 만일 세션이 존재하지 않으면 새로운 HttpSession 객체를 생성하여 반환한다.
    - `HttpServeltRequest의 getsession(boolean creat)`
        - 클라이언트가 가지고 있는 세션 ID와 동일한 세션 객체를 찾아서 주솟값을 반환한다.
        - creat가 **true**이면 `getSession`과 동일한 동작이지만 **false**이면 새로운 `HttpSession` 객체를 생성하지 않고 `null`을 반환한다.
- **HttpSession 메소드**
    - `getAttribute(String name)` : HttpSession 객체에 name의 인자값으로 지정된 데이터 값을 반환한다.
    - `getAttrbuteNames()` : HttpsSession 객체에 등록되어 있는 모든 정보의 이름만 반환한다.
    - `getId()` : 지정된 세션 ID를 반환한다.
    - `invalidate()` : 현재의 세션을 삭제한다.
    - `isNew()` : 서버 측에 새로운 객체를 생성한 경우는 true, 기존 세션이 유지되고 있으면 false
    - `set Attribute(stirng name, Object value)` : HttpSession 객체에 name으로 지정된 이름으로 value 값을 등록한다.