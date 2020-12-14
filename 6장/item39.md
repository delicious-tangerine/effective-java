# `아이템 39: 명명 패턴보다 애너테이션을 사용하라`

### `명명 패턴`
-  명명패턴 이란 ? Junit3의 메서드들 처럼 prefix를 test 로 시작하게 해야 작동하게 되는 방식
<br><br>
- 올바른 프로그램 요소에서만 사용되리라 보증 할 방법이 없다.<br>
=> 개발자의 실수로 프로그램이 동작을 안할수도 있다.
- 프로그램 요소를 매개변수로 전달할 마땅한 방법이 없다.<br>
=> test_return_runtime_exception 보다는 **@Test(expected = RuntimeException.class)**

### `마커 애너테이션 타입`
```Java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
}
```

### `배열 매개변수를 받는 애너테이션 타입`
```Java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTest {
    Class<? extends Throwable>[] value();
}
```


**애너테이션으로 할 수 있는 일을 명명 패턴으로 처리할 이유는 없다.**<br>
**자바 프로그래머라면 예외 없이 자바가 제공하는 애너테이션 타입들은 사용해야 한다.**