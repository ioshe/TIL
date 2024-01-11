# java 살펴보기(8)_예외처리


## 1. Throwable
### 개념
- Java에서 **`Throwable`** 클래스는 모든 오류(Error)와 예외(Exception)의 **`최상위 클래스`** 이다.
- 모든 예외와 오류는 **`Throwable`** 을 상속받아 처리되며, 이를 확장해서 **`사용자 정의 예외 클래스`** 를 만들 수 있다.

### Error Class
- **`Error`** 클래스는 **`Throwable`** 의 하위 클래스 중 하나로, 시스템 오류를 나타낸다.
- 이러한 오류는 일반적으로 개발자가 처리할 수 없는 심각한 문제를 나타내며, 종종 프로그램의 정상적인 실행을 방해한다.

### Exception Class
- 또 다른 **`Throwable`** 의 하위 클래스로, 프로그램 실행 중 발생할 수 있는 예외 상황을 나타냄
-  Exception은 **체크 예외(checked exceptions)** 와 **언체크 예외(unchecked exceptions)** 두가지로 나뉜다.
    - **체크 예외** : 이들은 컴파일 타임에 체크되며, 개발자가 이를 명시적으로 처리해야 한다. 
        - 예를 들면, IOException, SQLException 등이 있다.
    
    - **언체크 예외** : 이들은 런타임에 발생하며, 대부분의 경우 프로그래머의 실수로 인해 발생한다.
        - RuntimeException 클래스와 그 하위 클래스들이 여기에 속한다. 예를 들면, NullPointerException, IndexOutOfBoundsException 등이 있다.


## 2. ArrayIndexOutOfBoundsException과 예외 처리

### 개념
- **`ArrayIndexOutOfBoundsException`** 개념: 배열의 범위를 벗어난 인덱스에 접근할 때 발생하는 예외이다.
- 상속 계층 구조: **`ArrayIndexOutOfBoundsException`** 은 **`IndexOutOfBoundsException`** 을 상속받고, 이는 **`RuntimeException`** 을 상속받으며, 최종적으로 **`Exception`** 클래스를 상속받는다.

### 코드
```java
try {
    int[] arr = {1, 2, 3, 4};
    for (int i = 0; i <= arr.length; i++) {
        System.out.println(arr[i]); // ArrayIndexOutOfBoundsException 발생 가능
    }
} catch(ArrayIndexOutOfBoundsException e) {
    System.out.println("ArrayIndexOutOfBoundsException 발생: " + e.getMessage());
    //ArrayIndexOutOfBoundsException 발생: Index 4 out of bounds for length 4
} 
```
- **`ArrayIndexOutOfBoundsException`** 은 배열의 유효 범위를 초과하는 인덱스에 접근할 때 발생한다. 위 코드에서 **`arr.length`** 는 4이므로, i가 4일 때 예외가 발생한다.
- try-catch 블록: 예외 발생 가능 구문을 try 블록 안에 넣고, catch 블록에서 해당 **`예외를 처리`** 하여 프로그램의 비정상 종료를 방지한다.

## 3. 예외처리 순서
```java
try { //예외를 발생시킬 우려가 있는 샐행문에만 try를 걸어준다.
			int[] arr = {1,2,3,4};

			for (int i=0; i <= arr.length; i++) {
				System.out.println(arr[i]);
			}
		}catch(Exception e) {
			System.out.println("예외가 발생했습니다.");
			System.out.println(e);
//		}catch(ArrayIndexOutOfBoundsException e){ //It is already handled by the catch block for Exception : Exception이 더 큰 범위이기 때문에 ArrayIndexOutOfBoundisException은 handled 할 수 없으므로 에러가 남. ㅌ
//			System.out.println(String.format("%s 예외가 발생했습니다.", e));
		}finally {
			//무조건 실행됨.
			System.out.println("try구문과 상관없이 무조건 실행됨");
		}

```
- Java에서 여러 **`catch`** 블록을 사용하는 경우, 예외 처리는 위에서부터 순서대로 진행된다.
- 이때 중요한 것은 예외 클래스의 계층 구조이다. **`Exception`** 클래스는 Java의 모든 예외 클래스의 상위 클래스로, 모든 종류의 예외를 포괄한다. 
- 이는 **`Exception`** 클래스를 사용하는 catch 블록이 상위에 위치하면, 그 아래에 위치한 더 구체적인 예외 클래스, 예를 들어 **`ArrayIndexOutOfBoundsException`** ,를 처리하는 catch 블록은 **절대 실행될 수 없다.** 이는 **`unreachable code`** 라는 오류로 이어질 수 있다.