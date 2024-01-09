## java 살펴보기(2)_배열_ArrayList
```java
// 기본 배열 선언
int[] numbers = {1, 2, 3, 4, 5};
// 배열 선언과 동시에 초기화
String[] words = {"Hello", "World", "Java"};
// 배열 선언 후 크기 지정
String[] phrases = new String[3];
``````
- 자료형의 데이터를 **`순차적으로`** 저장하는 자료구조
- 배열의 길이는 생성 시에 결정되며, 이후에 변경할 수 없다.
- 배열의 각 요소는 초기화되지 않으면 기본값으로 설정

### 배열의 출력

```java
int[] arr = {1,2,3,4,5};
System.out.println(arr); //[I@4fb64261 자료형@메모리주소 의 형태로 나온 것을 알 수 있다.
System.out.println(Arrays.toString(arr)); //[1, 2, 3, 4, 5] : 결과를 확인하기 위해 String으로 변환
````
- System.out.println(배열이름)을 사용하면 배열의 **`메모리 주소가`** 출력됨.
- Arrays.toString(배열이름) 메소드를 사용하여 배열의 내용을 **`문자열로`** 변환하여 출력할 수 있다.


### 자료형변환
```java
// double 타입 배열 생성. int 타입의 값은 double로 자동 형변환된다.
double[] arr3 = new double[]{3.14, 1.25, 7, 8.03}; 

// int[] arr3_1 = {1, 2, 3.1, 4, 5}; // 오류: double을 int로 자동 변환할 수 없음
// double은 int로 자동 형변환되지 않으므로, 이 코드는 컴파일 에러를 일으킨다.
````

- Java에서는 작은 데이터 타입에서 큰 데이터 타입으로 자동 형 변환이 가능하다. **`(예: int -> double).`**
- 그러나 반대로 큰 데이터 타입에서 작은 데이터 타입으로는 자동 형 변환이 불가능하며, 명시적으로 변환해야 한다. **`(예: double -> int).`**

## ArraryList


### ArrayList 특징 
```java
// ArrayList 생성
ArrayList<String> fruits = new ArrayList<>();
// 요소 추가
fruits.add("Apple");
fruits.add("Banana");

// ArrayList 출력
System.out.println(fruits); // [Apple, Banana]
```
- **`ArrayList`** 는 크기가 가변적인 배열로, 다양한 데이터 타입을 저장할 수 있다.
- **`제네릭`** 을 사용하여 특정 타입만 저장하도록 제한할 수 있다.

### ArrayList 메서드 사용
```java
// 특정 위치에 요소 삽입
fruits.add(1, "Orange");

// 요소 수정
fruits.set(2, "Grape");

// 요소 삭제
fruits.remove("Apple");

// ArrayList 크기 출력
System.out.println(fruits.size()); // 2
System.out.println(fruits); // [Orange, Grape]
```
- .add(값) 메서드를 사용하여 요소를 추가할 수 있으며, .add(인덱스, 값) 형식으로 특정 위치에 요소를 삽입할 수도 있다.
- .set(인덱스, 값) 메서드로 특정 인덱스의 값을 변경할 수 있다.
- .remove(인덱스) 또는 .remove(객체) 메서드로 요소를 삭제할 수 있다.
- .size() 메서드로 ArrayList의 현재 크기(요소의 수)를 알 수 있다.
