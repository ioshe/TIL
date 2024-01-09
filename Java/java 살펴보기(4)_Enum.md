# java 살펴보기(4)_Enum

### Enum의 개념
```java
enum Language { 
    JAVA, PYTHON, SQL, JAVASCRIPT 
}
```

- Enum은 **`열거형`** 으로, 한정된 값만을 갖는 데이터 타입이다.
- 위의 코드에서는 **`Language`** 는 JAVA, PYTHON, SQL, JAVASCRIPT만을 값으로 가질 수 있다.
- 예를 들어, 특정 카테고리 내에서만 값을 제한할 때 유용하다 (예: SQL의 enum, 파이썬의 pandas category).
- **`상수의 개념`** 으로 이해하며, 주로 대문자로 표기한다.

### ENUM 사용 이유
-  반복되는 문자열 상수를 효율적으로 관리할 수 있게 한다.
- String 대신 Enum을 사용하면 오타나 잘못된 값 입력을 방지한다.

```java
ArrayList<Language> languageList = new ArrayList<>();
languageList.add(Language.JAVA);
languageList.add(Language.PYTHON);
```
- languageList는 Language 타입의 객체만 추가할 수 있으며, 이를 통해 데이터 타입의 안정성을 보장한다.
