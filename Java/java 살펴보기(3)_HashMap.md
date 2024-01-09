# java 살펴보기(3)_HashMap_Set

## HashMap
### HashMap의 선언 및 기본적인 사용
```java
Map<String, String> map = new HashMap();
System.out.println(map); // {}
```
- HashMap은 Map 인터페이스를 구현하는 클래스로, 키-값 쌍으로 데이터를 저장한다.
-  HashMap은 데이터를 빠르게 검색할 수 있도록 해시 테이블을 사용하는 자료 구조이다.

### HashMap CRUD
CREATE
```java
map.put("name", "kim");
System.out.println(map); // {name=kim}
map.put("age", "30");
System.out.println(map); // {name=kim, age=30}
```

READ
```java
System.out.println(map.get("name")); // kim
System.out.println(map.keySet()); // [name, age]
System.out.println(map.values()); // [kim, 30]
System.out.println(map.entrySet()); // [name=kim, age=30]
```

UPDATE
```java
map.put("name", "lee");
System.out.println(map); // {name=lee, age=30}
```
DELETE
```java 
map.remove("name");
System.out.println(map); // {age=30}
```

### 특정 키 및 값의 존재 여부 확인
```java
System.out.println(map.containsKey("age")); // true
System.out.println(map.containsValue("lee")); // false
```

## HashSet

### 선언
```java
Set<String> set = new HashSet<>();
```
- Set은 Java의 컬렉션 프레임워크에 속하는 인터페이스이다.
- 중복된 요소를 허용하지 않으며, 저장된 요소 간의 순서를 보장하지 않는다.
- HashSet은 Set 인터페이스를 구현한 가장 일반적인 클래스 중 하나이다.


### 사용
```java
set.add("신짱아");
set.add("신짱구");
set.remove("신짱아");
System.out.println(set.isEmpty()); //false
System.out.println(set.contains("신짱구")); //true
```
- add 메소드는 새로운 요소를 집합에 추가한다. 만약 추가하려는 요소가 이미 집합에 존재한다면, 중복을 허용하지 않기 때문에 추가되지 않는다.
- remove 메소드는 집합에서 특정 요소를 제거한다.
- isEmpty 메소드는 집합이 비어있는지 여부를 반환한다.
- contains 메소드는 집합이 특정 요소를 포함하고 있는지 여부를 확인한다.


### 활용
```java
Set<String> set3 = new HashSet<>(Arrays.asList("양", "염소", "염소", "염소", "염소", "양", "알파카", "알파카", "흑염소", "흑염소", "염소(흑)", "양(아기)", "산양"));
System.out.println(set3); // [흑염소, 염소, 알파카, 양, 산양, 염소(흑), 양(아기)]
```
- Arrays.asList(...) 메서드를 사용하여 문자열 요소들을 리스트로 만든다.
- 그런 다음, new HashSet<>(...)을 사용하여 중복 요소를 제거하고, 중복이 제거된 문자열 집합을 생성한다.