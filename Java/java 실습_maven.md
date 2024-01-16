# java 실습_Maven

## 1. 함수형 인터페이스
### 개념 
- **`DBUtil`** 은 데이터베이스 관련 작업을 편리하게 수행하기 위한 유틸리티 클래스를 의미한다.
- 이 클래스는 데이터베이스 연결, 쿼리 실행, 자원 해제와 같은 반복적인 작업을 간소화하기 위해 사용한다.

### Class.forName
```java
// get Connection
        public static Connection getConnection() throws SQLException{
                String url = "jdbc:mysql://localhost:3306/your_database_name?characterEncoding=UTF-8&serverTimezone=UTC";
                String id = "ID";
                String pw = "password";
                
                return DriverManager.getConnection(url, id, pw);
        }
```
- localhost:3306/your_database_name 의 부분을 고쳐서 사용가능하다.
- characterEncoding과 serverTimezone 매개변수는 UTF-8 문자 인코딩과 시간대 설정을 정의한다.
- 위의 URL을 이용하여 **`DriverManager.getConnection()`** 메서드를 호출하여 MySQL DB에 연결한다.

### Close 
```java
public static void close(Connection conn, Statement stmt, ResultSet rs) throws SQLException{
                rs.close();
                stmt.close();
                conn.close();
        }

        public static void close(Connection conn, PreparedStatement pstmt)  throws SQLException{
//              rs.close();
                pstmt.close();
                conn.close();
        }
}
```
- **`ResultSet`** 객체는 데이터베이스 쿼리의 결과를 저장하는 데 사용된다.
- **`Statement`** 객체는 SQL 명령을 실행하는 데 사용된다.
- 첫번째 메소드는 기본적인 SQL **`SELECT`** 쿼리를 실행한 후 결과를 **`ResultSet`**으로 받았을 때 사용된다.
- **`PreparedStatement`** 는 미리 컴파일된 SQL 문을 나타내며, 매개변수화된 쿼리에 사용
- 두번째 메소드는 **`ResultSet`** 이 포함되어 있지 않기 때문에, 이 메소드는 결과 집합을 반환하지 않는 SQL 명령(예: INSERT, UPDATE, DELETE)을 실행한 후에 사용

## DataController
```java
public static String insert(int deptno, String dname, String loc) throws Exception{
			Connection conn = null;
			PreparedStatement pstmt = null;
			
			try {
				conn = Util.getConnection();
				
				pstmt = conn.prepareStatement("insert into dept values (?, ?, ?)");
				
				pstmt.setInt(1, deptno);
				pstmt.setString(2, dname);
				pstmt.setString(3, loc);
				
				int result = pstmt.executeUpdate();
				
				if(result == 1) {
					return "저장 성공";
				}	
				
			} catch (Exception e) {
				e.printStackTrace();
			} finally { 
				Util.close(conn, pstmt);
			}
			
			return "저장 실패";
		}
```
- **`PreparedStatement`** 를 사용하여 SQL 삽입(INSERT) 명령을 실행한다. ?는 매개변수화된 쿼리에서 값을 대체하기 위한 플레이스홀더이다.
- **`pstmt.setInt(1, deptno)`** 을 통해 플레이스홀더에 각각의 값을 설정한다.
- **`executeUpdate`** 메소드는 삽입, 업데이트, 삭제 등을 수행하고, 영향 받은 행의 수를 반환한다.

```java		
		public static DeptDto select(int deptno) throws Exception{
			Connection conn = null;
			PreparedStatement pstmt = null;
			ResultSet rs = null;
			
			DeptDto dept = null;	
			
			try {
				conn = Util.getConnection();
				pstmt = conn.prepareStatement("select * from dept where deptno=?");
				pstmt.setInt(1, deptno);
				rs = pstmt.executeQuery();
				if(rs.next()) {
					dept = new DeptDto(rs.getInt("deptno"), rs.getString("dname"), rs.getString("loc") ); 
				}	
				
			} catch (Exception e) {
				e.printStackTrace();
			} finally { 
				Util.close(conn, pstmt, rs);
			}
			
			return dept;
		}
```
- **`PreparedStatement`** 를 사용하여 SQL 조회(SELECT) 명령을 실행한다.
- **`executeQuery`** 메소드는 쿼리를 실행하고 ResultSet 객체에 결과를 반환한다.