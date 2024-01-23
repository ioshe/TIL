# JavaScript(1)_Scope_Hoisting

## 스코프(Scope)
- 스코프는 변수와 함수가 정의되고 접근할 수 있는 범위를 말한다.

### 글로벌 스코프(Global Scope)
- 코드의 어디에서나 접근할 수 있는 변수나 함수가 정의된 스코프이다. 전역 변수는 어플리케이션의 어디서든 접근 가능하다.

### 함수 스코프(Function Scope)
- 함수 내부에서 정의된 변수는 해당 함수 내에서만 접근 가능하다. `var` 키워드로 선언된 변수는 함수 스코프를 가진다.

### 블록 스코프(Block Scope)
- {}로 둘러싸인 코드 블록(예: if 문, for 문, while 문 내의 블록) 내에서 정의된 변수는 해당 블록 내에서만 접근 가능하다. `let`과 `const`로 선언된 변수는 블록 스코프를 가진다.

## 호이스팅(Hoisting)
- `JavaScript`에서 변수와 함수 선언이 그들이 속한 스코프의 최상단으로 끌어올려지는 것을 의미한다.
- 이는 JavaScript가 코드를 `실행하기 전`에 변수와 함수 선언을 메모리에 저장하는 방식 때문에 발생한다.
- 호이스팅은 변수와 `함수 선언 자체만을 최상단`으로 끌어올리며, 초기화나 할당은 원래 위치에서 실행한다.
### 임시적 사각지대(Temporal Dead Zone, TDZ)
- 변수가 선언되었지만 `아직 초기화되지 않은` 상태의 코드 영역을 말한다.
- let과 const로 선언된 변수는 선언 지점에서 초기화 지점까지 TDZ에 있다. 

### 변수의 호이스팅
- **`var`** : var로 선언된 변수는 호이스팅되어 변수의 선언된 위치와 상관없이 함수의 최상단 또는 스크립트의 최상단으로 끌어올려진다.
    - 따라서 `var`로 선언된 변수를 초기화 전에 접근하면 `undefined` 값을 얻게 된다.
```java
console.log(x); // undefined
var x = 5;
console.log(x); // 5
```

- **`let`** 과 **`const`** 
    - `let`과 `const`로 선언된 변수도 호이스팅된다.
    - 하지만 `let` 과 `const`는 임시적 사각지대 **`(Temporal Dead Zone, TDZ)`** 가 존재한다.
    - 이로 인해 변수가 선언된 위치부터 초기화된 위치까지의 코드 영역에서 변수에 접근할 수 없다.
    - 따라서 let 또는 const로 선언된 변수에 TDZ 내에서 접근하려고 하면 참조 오류(ReferenceError)가 발생한다.
```java
console.log(y); // ReferenceError: y is not defined
let y = 5;
console.log(y); // 5
```

### 함수 선언의 호이스팅
- 함수 선언도 호이스팅된다. 다시 말해 함수가 선언된 위치와 상관없이 해당 스코프의 최상단으로 끌어올려진다.
- 함수 선언식은 전체가 호이스팅되므로 함수 선언 전에 함수를 호출할 수 있다.
```java
foo(); // "Hello, foo!"

function foo() {
    console.log("Hello, foo!");
}
```
- 반면에 함수 표현식은 호이스팅 되지 않는다.
- 함수 표현식은 변수에 할당된 함수로, 해당 변수의 호이스팅 규칙을 따른다.
```java
bar(); // TypeError: bar is not a function
var bar = function() {
    console.log("Hello, bar!");
}
```


### 궁금했던 코드
```java
var hello ="var 안녕"
const hello2 = "const 안녕"
let hello3 = "let 안녕"
console.log(hello)
const hello2 = "const 안녕"
console.log("두번째" + hello3)
```
- 이 코드는 var와 const를 사용하여 변수를 선언하고, console.log를 통해 그 값을 출력하는 코드이다. const로 선언된 변수는 같은 스코프 내에서 재선언될 수 없지만 cosnt로 중복변수를 선언하였다.
- 따라서 실행 전에 전체 코드를 분석하여 변수의 선언부분은 호이스팅으로 스코프의 최상단으로 올라가게 되고 실행 전 코드 분석 단계에서 문법적 오류로 감지된다. 
- 결과적으로 JavaScript는 이러한 오류를 발견하면 스크립트의 실행을 시작하지 않고 중단한다.