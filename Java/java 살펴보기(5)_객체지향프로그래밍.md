# java 살펴보기(5)_객체지향프로그래밍


### 1. 객체 지향 프로그래밍 (Object-Oriented Programming, OOP) 개념
- 객체 지향 프로그래밍은 변수(데이터)와 함수(행동)를 객체로 캡슐화하여 프로그램을 구성하는 방법이다.
- 특징: 코드의 재사용성, 유지보수의 편리함을 제공한다.
- 클래스와 객체: 클래스는 객체를 생성하기 위한 틀로, 객체는 클래스의 인스턴스이다.

### 2. 클래스와 인스턴스
```java
//객체
public class Student {
    String name; // 학생의 이름
    int age;     // 학생의 나이
    
    void enter() {
        System.out.println("학교에 들어갑니다.");
    }
    // ... (다른 기능들)
}

//인스턴스
Student cheolsu = new Student(); // '학생' 클래스로 '철수'라는 인스턴스 생성
```
- 클래스(Class) : 게임에서 캐릭터를 만들 때 구성요소이다.
- 인스턴스(Instance): 클래스를 바탕으로 만든 실제 '캐릭터'가 된다. 


### 3. 상속(Inheritance)
```java
public class AIStudent extends Student {
    int tossMoney;
    // 추가 메서드
}
AIStudent minsu = new AIStudent();
minsu.enter(); // Student 클래스의 메서드 사용
```
- 개념: 한 클래스의 속성과 **`메서드`** 를 다른 클래스가 물려받는 것이다.
- 특징: 코드의 **`재사용성`** 을 높이고 **`유지보수`** 를 용이하게 한다.
자바의 특징: 단일 상속만을 지원한다.
- 예시: AIStudent 클래스는 Student 클래스로부터 상속받아 구현되었다.

### 4. 오버로딩(Overloading)
오버로딩 (Overloading)

```java
class AIStudent extends Student {
    // 매개변수로 정수를 받는 plusAgeOne 메서드
    int plusAgeOne(int num) {
        this.age += num;
        return this.age;
    }

    // 매개변수로 문자열을 받는 plusAgeOne 메서드
    int plusAgeOne(String strNum) {
        this.age += Integer.parseInt(strNum);
        return this.age;
    }
}
```
- 정의: 같은 이름의 **`메서드`** 를 다양한 매개변수를 받도록 여러 번 정의하는 기법이다.
- 목적: **`메서드`** 에 다양한 매개변수 유형이나 수를 허용하여, 같은 기능을 여러 방식으로 수행할 수 있도록 한다.
- 특징: **`메서드`** 이름은 동일하지만, 매개변수의 타입, 개수, 순서가 달라야 한다.

### 5.오버라이딩(Overriding)
```java
class AIStudent extends Student {
    // 상속받은 plusAgeOne 메서드 오버라이딩
    @Override
    int plusAgeOne() {
        System.out.println("AIStudent의 메서드입니다.");
        return super.plusAgeOne();
    }
}
```
- 정의: 상속받은 메서드를 하위 클래스에서 **`재정의`** 하는 기법이다.
- 목적: 상위 클래스의 동작을 하위 클래스에서 다른 방식으로 구현할 수 있도록 한다.
- 특징: 메서드의 이름, 반환 타입, 매개변수 목록이 상위 클래스의 메서드와 **`동일`** 해야 한다.