# Item 32 : 제네릭과 가변인수를 함께 쓸 때는 신중하라

> 💡 가변인수와 제네릭은 궁합이 좋지 않다.
> 
> → 타입 안전한지 확인해야 한다! 꼭!

### Heap Pollution
- `단순히 힙에 나쁜 것들이 있다.` 정도의 의미다.
- 여기서는 매개변수화 타입의 변수가 다른 타입 객체를 참조하는 경우를 이야기했다.

### 가변인수 (varargs)
- 가변인수는 배열 형태로 생성되게 된다.

```java
static void dangerous(List<String>... stringLists) {
  List<Integer> intList = List.of(42);
  Object[] objects = stringLists;
  objects[0] = intList; // 힙 오염 발생
  String s = stringLists[0].get(0); // ClassCastException
}
```

제네릭 varargs 배열 매개변수에 값을 저장하는 것은 안전하지 않다.

=> 그러나 실무에서 매우 유용하다. 그래서 허용되었다.

### 아니 그러면, varargs를 쓰는 메서드는 안전한가?
- `@SafeVarargs`는 메서드 작성자가 그 메서드가 타입 안전함을 보장하는 장치다.
- 타입 안전을 확신하는 방법 : 메서드가 배열에 아무것도 저장하지 않고 그 배열의 참조가 밖으로 노출되지 않는다면 타입 안전하다.
- 즉, 이 varargs 매개변수 배열이 호출자로부터 그 메서드로 순수하게 인수들을 전달하는 일만 한다면 안전하다!

```java
// Not Safe!!!
static <T> T[] toArray(T... args) {
  return args;
}
```

제너릭이나 매개변수화 타입의 varargs 배개변수를 받는 모든 메서드에 @SafeVarargs를 달아라.

```java
// Safe!
@SafeVarargs
static <T> List<T> flatten(List<? extends T>... lists) {
  List<T> result = new ArrayList<>();
  for (List<? extends T> list : lists)
    result.addAll(list);
  return result;
}
```

### Two rules for varargs
- varargs 매개변수 배열에 아무것도 저장하지 않는다.
- 그 배열(혹은 복제본)을 신뢰할 수 없는 코드에 노출하지 않는다.

### 제네릭 varargs 매개변수를 대체하는 방법
(실체는 배열인) varargs 매개변수를 List 매개변수로 바꾸자.

```java
static <T> List<T> flatten(List<List<? extends T>> lists) {
  List<T> result = new ArrayList<>();
  for (List<? extends T> list : lists)
      result.addAll(list);
  return result;
}
```
**장점** : 이 방식을 통해 컴파일러가 메서드의 타입 안전성을 검증할 수 있다.

**단점** : 클라이언트코드가 지저분해질 수 있으며 속도가 조금 느려질 수 있다

<br>

실제로 가변인자가 나오기 전까지는 Vector 같은 것에 인자를 담아서 넘겨줬다고 하니 충분히 가능한 일이라고 생각된다.
