# Item 71 : 필요 없는 검사 예외 사용은 피하라.

> 💡 과한 검사 예외는 불편한 API를 만든다.
> 
> → API를 제대로 사용해도 발생할 수 있는 예외, 의미있는 조치를 취할 수 있는 예외가 아니라면 비검사 예외를 사용하자.

<br>

### 검사 예외를 싫어할 수 있지만..
검사 예외는 비검사 예외와 다르게 프로그래머가 처리하여 안전성을 높인다.

다만, 수가 늘어날 수록 API 사용자에게 부담을 준다.

<br>

### Is it the best?

<br>

```java
} catch (TheCheckedException e) {
  throw new AssertionError();
}
```

```java
} catch (TheCheckedException e) {
  e.printStackTrace();
  System.exit(1);
}
```

 **Answer: Nope..**
 
 <br>
 
 ### 검사 예외로 인한 부담
 차라리 여러 검사 예외를 던진다면 덜 부담스럽지만,
 
 메서드가 단 하나의 검사 예외만 던질 때 부담이 특히 크다.
 
 <br>
 
 ### 검사 예외를 회피하는 가장 쉬운 방법
 **적합한 결과 타입을 담은 옵셔널을 반환하는 것**
 
 **=> 검사 예외 대신 옵셔널을 던져라!**
 
 **단점?** 예외가 발생한 이유를 알려주는 부가정보 제공이 불가능.
 
 <br>
 
 ### Other way to 검사 예외 회피
 검사 예외를 던지는 메서드를 2개로 쪼개 비검사 예외로 바꾼다.
 
 ```java
 // Original Code - Before Refactoring
 try {
   obj.action(args);
 } catch (TheCheckedException e) {
   ... // 예외 상황에 대처한다.
 }
 ```
 
 ```java
 // After Refactoring
if (obj.actionPermitted(args)) {
  obj.action(args);
} else {
  ... // 예외 상황에 대처한다.
}
```

이 방식은 외부 동기화 없이 여러 스레드가 동시에 접근할 수 있거나 외부 요인에 의해 상태가 변할 수 있다면 적절하지 않다.

**Why?** actionPermitted와 action 호출 사이에 객체의 상태가 변할 수 있기 때문.

<br>

### 결론
- 검사 예외를 남용하지 말자.
- 정보가 필요없는 상황이라면 Optional로 바꾸는 방법을 먼저 고민한다.
- Optional만으로는 상황을 처리하기 적절하지 않다면 검사 예외를 던지자.
