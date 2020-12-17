# Item 29 : 이왕이면 제네릭 타입으로 만들어라

> 💡 제네릭을 코드를 좀 더 유연하고 견고하게 만들어준다.
> 
> → 클라이언트에서 직접 형변환하는 타입보다 제네릭 타입이 더 안전하고 쓰기 편하다.

### 배열을 사용한 코드를 제네릭으로 만드는 방법 1
코드에 쓰인 Object를 다 E로 바꾼다.

```java
// 배열 elements는 push(E)로 넘어온 E 인스턴스만 담는다.
// 따라서 타입 안전성을 보장하지만,
// 이 배열의 런타임 타입은 E[]가 아닌 Object다!
@SuppressWarnings("unchecked")
public Stack() { // 참고로 생성자다.
  elements = (E[]) new Object[DEFAULT_INITIAL_CAPACITY];
}
```

### 배열을 사용한 코드를 제네릭으로 만드는 방법 2
elements 필드의 타입을 E[]에서 Object[]로 바꾼다.

```java
public E pop() {
  if (size == 0)
    throw new EmptyStackException();
    
    // push에서 E 타입만 허용하므로 이 형변환은 안전하다.
    @SuppressWarnings("unchecked") E result = (E) elements[--size];
    
    elements[size] = null;
    return result;
}
```

### 두 방법 비교
- 첫 번째는 가독성이 좋고 코드도 더 짧다.
- 첫 번째는 형변환을 배열 생성시 단 한 번만 해주면 되지만 두 번째는 배열에서 원소를 읽을 때마다 해줘야 한다.
- 하지만 첫 번째 방식은 배열의 런타임 타입이 컴파일 타입과 달라 힙 오염(item 32)을 일으킨다.
