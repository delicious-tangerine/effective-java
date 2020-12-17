# `아이템 44: 표준 함수형 인터페이스를 사용하라`

- 자바가 람다를 지원하면서 API를 작성하는 모범 사례도 크게 바뀌었다.<br>
=> 템플릿 메서드 패턴의 매력이 크게 줄었다.<br>
=> 함수 객체를 받는 정적 팩터리나 생성자를 제공 한다.

### `LinkedHashMap의 예`
```Java
protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
    return size() > 100;
}
```
- 람다를 사용 한다면?
```Java
@FunctionalInterface interface EldestEntryRemovalFunction<K, V>{
    boolean remove(Map<K,V> map, Map.Entry<K,V> eldest)
}
```

- 필요한 용도에 맞는게 있다면 직접 구현하지 말고 표준 함수형 인터페이스를 활용하라.


### `기본 함수형 인터페이스의 종류`
|인터페이스|함수 기그니처|예|
|----|----|----|
|UnaryOperation<T>|T apply(T t)|String::toLowerCase|
|BinaryOperator<T>|T apply(T t)|BigInteger::add|
|Predicate<T>|boolean test(T t)|Collection::isEmpty|
|Function<T, R>| R apply(T t)|Arrays::asList|
|Supplier<T>|T get()|Instant::now|
|Consumer<T>|void accept(T t)|System.out::println|



### `@FunctionalInterface 애너테이션의 목적`
- @Override를 사용하는 이유와 비슷하다. 프로그래머의 의도를 명시
- 람다용으로 설계됨을 명시함.
- 추상 메서드를 오직 하나만 가지고 있어야 컴파일 되게 해준다. - 누군가 실수로 메서드를 추가 하지 못하도록