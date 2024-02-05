# java 살펴보기(9)_lambda


## 1. Statement
### 개념 
- **`Statement`** 는 SQL 문을 실행하기 위한 기본 인터페이스이다. 이는 SQL문을 문자열로 직접 제공한다.
- 단점으로는 매번 SQL 문을 실행할 때마다 데이터베이스에서 해당 SQL을 컴파일해야 하므로 성능상의 문제가 있을 수 있다. 
- 또한 SQL 인젝션 같은 보안 문제에 취약하다.

## 2. PreparedStatement 
### 개념
- **`PreparedStatement`** 는 **`Statement`** 의 하위 클래스로, 미리 컴파일된 SQL 문을 사용한다.
- SQL 문에서 파라미터가 필요한 부분에 **?** 를 사용하여 파라미터를 정의하고, **`setInt()`** , **`setString()`** 등의 메소드를 통해 실제 값을 주입한다.
```java
PreparedStatement pstmt = conn.prepareStatement("INSERT INTO table_name VALUES (?, ?)");
pstmt.setInt(1, 1);
pstmt.setString(2, "example");
```
- **`PreparedStatement`** 는 특히 반복적인 SQL 실행에서 성능이 우수하다.

## 3. Statment 가 SQL 인젝션 공격에 취약한 이유
### 3.1 - SQL 인젝션 공격의 메커니즘
- SQL 인젝션 공격은 애플리케이션의 보안 취약점을 이용하여 공격자가 데이터베이스 쿼리에 악의적인 코드를 "주입"하는 방식으로 발생한다.
- 이러한 공격은 주로 사용자 입력을 통해 발생하며, 입력 값이 검증되지 않거나 적절히 이스케이프되지 않은 경우 데이터베이스 쿼리의 일부가 될 수 있다.

### 3.2. Statement의 취약성
- Statement를 사용할 때, SQL 문은 문자열 연결을 통해 직접 구성된다.
```java
String query = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(query);
```
- 만약 사용자가 username 필드에 ' OR '1'='1와 같은 값 모든것을 참으로 만드는 값을 입력한다면, 구성된 쿼리는 모든 사용자 정보를 반환하도록 변경될 수 있다.
- 이처럼 악의적인 입력을 통해 원래의 쿼리 의도를 변경하거나, DB의 민감한 정보에 접근할 수 있기 때문에 Statement는 인젝션 공격에 취약하다.

### 3.3 PreparedStatement 보안 이점
- 반면, PreparedStatement는 미리 컴파일된 쿼리와 파라미터화를 사용하여 이러한 취약점을 해결한다.
- PreparedStatement는 쿼리의 구조를 먼저 정의하고, 나중에 각 파라미터를 안전하게 삽입합니다. 이 과정에서 사용자 입력은 일반 텍스트로 취급되어 쿼리의 일부로 해석되지 않는다.