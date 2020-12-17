# Item 31 : 한정적 와일드카드를 사용해 API 유연성을 높여라

> 💡 다른 거 몰라도 된다. PECS만 기억하자
> 
> → PECS : producer-extends, consumer-super

### PECS

#### producer-extends

```java
// E의 하위 타입의 Iterable
public void pushAll(Iterable<? extends E> src) {
  for (E e : src)
    push(e);
}
```

#### consumer-super
```java
// E의 상위 타입의 Collection
public void popAll(Collection<? super E> dst) {
  while (!isEmpty())
    dst.add(pop());
}
```

<br>

### 반환 타입에는 한정적 와일드카드 타입을 사용하면 안 된다.
- 유연성을 높여주기는 커녕 클라이언트 코드에서도 와일드카드 타입을 써야 하기 때문 
- 클래스 사용자가 와일드카드 타입을 신경써야 한다면 그 API에 무슨 문제가 있을 가능성이 크다.

<br>

### In Java 7
아래와 같이 명시적 타입 인수를 적어줘야 한다.
```java
Set<Number> numbers = Union.<Number>union(integers, doubles);
```

<br>

### 메서드 선언에 타입 매개변수가 한 번만 나오면 와일드 카드로 대체하라.
```java
// 원래 버전
public static void swap(List<?> list, int i, int j) {
  list.set(i, list.set(j, list.get(i)));
} // 오류 메시지가 발생한다!

// 변경된 버전
public static void swap(List<?> list, int i, int j) {
  swapHelper(list, i, j);
}

private static <E> void swapHelper(List<E> list, int i, int j) {
  list.set(i, list.set(j, list.get(i)));
}
```
