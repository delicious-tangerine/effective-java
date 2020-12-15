# Item 30 : 이왕이면 제네릭 메서드로 만들라

> 💡 메서드를 제네릭으로 만들어 타입안전하게 하라
> 
> → 명시적 형변환해야 하는 메서드보다 제네릭 메서드가 더 안전하고 사용하기 쉽다. 

### 제네릭 메서드
```java
public static <E> Set<E> union(Set <E> s1, Set<E> s2) {
  Set<E> result = new HashSet<>(s1);
  result.addAll(s2);
  return result;
}
```

한정적 와일드카드를 이용해 이렇게 더 유연하게 작성할 수도 있다.
```java
public static <E> Set<E> union(Set <? extends E> s1, Set<? extends E> s2) {
  Set<E> result = new HashSet<>(s1);
  result.addAll(s2);
  return result;
}
```

### 제네릭 싱글턴 Factory 패턴
요청한 타입 매개변수에 맞게 그 객체의 타입을 바꿔주는 정적 Factory
```java
private static UnaryOperator<Object> IDENTITY_FN = (t) -> t;

@SuppressWarnings("unchecked")
public static <T> UnaryOperator<T> identityFunction() {
  return (UnaryOperator<T>) IDENTITY_FN;
}
```

### 재귀적 한정 타입(recursive type bound)
자기 자신이 들어간 표현식을 사용하여 타입 매개변수의 하용 범위를 한정할 수 있다.
```java
public interface Comparable<T> {
  int compareTo(T o);
}

// <E extends Comparable<E>> : 모든 타입 E는 자신과 비교할 수 있다.
public static <E extends Comparable<E>> E max(Collection<E> c);
