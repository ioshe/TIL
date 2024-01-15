# java 살펴보기(9)_lambda


## 1. 함수형 인터페이스
### 개념 
- 단 하나의 추상 메서드만을 포함하는 인터페이스를 **`함수형 인터페이스`** 라고 한다.
- 함수형 인터페이스의 호출 및 기능을 구현하는 방법을 새롭게 정의한 문법이 **`람다식`** 이다.
```java
interface Calculator {
    int sum(int a, int b);
}
```

## 2. lambda 
### 개념
- 람다식(**`Labmda expression`**)은 함수형 프로그래밍 기법을 지원하는 자바의 문법 요소이다.
- 객체지향형프로그래밍에서 메서드를 사용하기 위해서는 클래스의 객체를 먼저 생성한 후에 메서드를 호출해야하는 방식이었기 때문에 **`labmda`** 를 통해 함수를 구현한다.
```java
Calculator mc = (int a, int b) -> a + b;
```

- 여기서 (int a, int b) -> a + b는 두 정수 a, b를 입력받아 합을 반환하는 람다 표현식이다.


## 3. 람다식의 활용
### 3-1. 정의돼 있는 메서드 참조
- 추상 메서드를 직접 구현하는 대신, 이미 구현이 완료된 메서드를 참조하는 것이다.
```java
// 사용법 : 클래스 객체::인스턴스 메서드 명
Calculator mc4 = Integer::sum;
```
```java
interface A {
    void abc();
}
Class B {
    void bcd(){
        System.out.println("메서드");
    }
}
```
```java
// 익명 이너 클래스로 구현
A a = new A() {
    public void abc() {
        B b = new B();
        b.bcd();
    }
};
```
```java
// 람다식으로 구현
A a = () -> {
    B b = new B();
    b.bcd();
};
```
- 구현한 **`abc()`** 메서드에서는 B 클래스 객체를 생성해 인스턴스 메서드인 **`bcd()`** 를 호출했다. 이는 **`abc()`** 는 **`bcd()`** 와 동일하다는 것을 의미한다. 이는 다음과 같이 표시 할 수 있다.
```java
// 정의돼 있는 인스턴스 메서드 참조
B b = new B();
A a = b::bcd;
```
- 이는 **A인터페이스 내부의 abc()  메서드는 참조 변수 b 객체 내부의 인스턴스 메서드 bcd() 와 동일하다.** 라는 의미이다.
- 주의해야 할 점으로는 abc()가 bcd()를 참조하기 위해서는 **리턴 타입과 매개변수의 타입이 반드시 동일** 해야 한다.

```java
//람다식
A a = (k) -> {
    System.out.println(k);
}
//정의돼 있는 인스턴스 메서드 참조
A a = System.out::println;
```
- 이는 **인터페이스 A의 추상메서드인 abc()는 System.out.println();을 참조하라** 라는 의미이다.

### 3-2. 정의돼 있는 정적 메서드 참조
- 정적 메서드를 참조하는 것이라서 객체의 생성 없이 **`클래스명을 바로 사용`** 한다.
```java
//사용법 : 클래스명::정적 메서드명
```
```java
interface A {
    void abc();
}
Class B {
    static void bcd(){
        System.out.println("메서드");
    }
}
```
```java
//익명 이너 클래스
A a = new A() {
    public void abc() {
        B.bcd();
    }
}
```
```java
//정의돼 있는 정적 메서드 참조
A a = () -> {
    B.bcd();
};
```
- 이는 **인터페이스 A의 객체를 생성할 떄 구현해야 하는 abc()메서드를 B.bcd()와 동일하게 하라** 라는 의미이다.

### 3-3. 첫 번째 매개변수로 전달된 객체의 인스턴스 메서드 참조
```java
interface A {
    void abc(B b,int k);
}
Class B {
    static void bcd(int k){
        System.out.println(k);
    }
}

// 람다식
A a = (b,k) -> {
    b.bcd(k);
};
A a = B :: bcd;
//활용
A a= (str) -> str.length();
A a = String::lengnth;
```

### 3-4. 배열 생성자 참조
```java
//배열 타입::new
interface A {
    int[] abc(int len);
}
```
```java
//익명 이너 클래스
A a = new A() {
    public int[] abc(int len) {
        return new int[len];
    }
};
```
```java
// 람다식
A a = (len) -> new int[len];
```
```java
// 배열의 new 생성자를 참조할 때
A a = int[]::new;
```

### 3-5. 클래스 생성자 참조
```java
//클래스명::new
interface A {
    B abc();
}
Class B {
    B() {} //첫 번쨰 생성자
    B(int k) {} //두 번째 생성자
}
```
```java
// 익명 이너 클래스
A a = new A() {
    public B abc() {
        return new B();
    }
};
```
```java
// 람다식
A a = () -> new B();
A a = B::new;
```
- 이는 a.abc() 메서드를 호출하면 **new B()를 실행해 객체를 생성하라** 라는 의미이다.
```java
```