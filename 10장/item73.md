# Item 73 : 추상화 수준에 맞는 예외를 던지자. (Exception Translation)

> 💡 상위 계층에서는 저수준 예외를 잡아 자신의 추상화 `Level`에 맞는 예외로 바꿔 던져야 한다.
> 
> → 내부 구현 방식을 드러내어 Top-Level API를 오염시킨다.

<br>

### Exception Translation Example
```java
try {
  ... // 저수준 추상화를 이용한다.
} catch (LowerLevelException e) {
  // 추상화 수준에 맞게 번역한다.
  throw new HigherLevelException(...);
}
```

<br>

### Exception chaining
저수준 예외가 디버깅에 도움이 된다면, 문제의 근본 원인(저수준 예외)를 고수준 예외에 실어보낸다.

```java
try {
  ... // 저수준 추상화를 이용한다.
} catch (LowerLevelException cause) {
  // 저수준 예외를 고수준 예외에 실어보낸다.
  throw new HigherLevelException(cause);
}

// 고수준 예외의 생성자는 다음과 같다.
class HigherLevelException extends Exception {
  HigherLevelException(Throwable cause) {
    super(cause);
  }
}
```

**그러나, 예외 번역은 우수한 방법이지만 그렇다고 남용해서는 곤란하다.**

**Why?** 하위 계층에서는 예외가 발생하지 않도록 하는 것이 최선이기 때문.

<br>

### 만약 API 호출자에게까지 문제를 전파하고 싶지 않다면,
java.util.logging 같은 로깅 기능을 이용하여 기록해두자.
