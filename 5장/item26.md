# Item 26 : Raw Type은 사용하지 마라

> 💡 오류의 이상향 : 컴파일 할 때 발견
> 
> → Raw Type 쓰지마라.

### What's the `GENERIC`
선언에 타입 매개변수를 받는 클래스 혹은 인터페이스.

제네릭 타입을 정의하게 되면 Raw Type도 함께 정의된다.

**Raw Type** : 타입 선언에서 제네릭 타입 정보가 전부 지워진 것처럼 동작(하위 버전과의 호환성을 위해)

**ex) List<E>의 Raw Type은 List다.**

### Raw Type 쓰지 마라.
```java
// Raw Type으로 인한 side effect
private final Collection stamps = ...;

stamps.add(new Coin(...)); // warning에서 그친다.
// 만약 Runtime에서 실행하게 되면 오류를 늦게 찾게 된다.

// 정확하게 타입을 썼다면..
private final Collection<Stamp> stamps = ...;

stamps.add(new Coin(...));
// 위의 구문을 실행하게 된다면 type conversion에 실패했다는 에러를 뱉는다.
```

### 비한정적 와일드 카드 타입(unbounded wildcard type)
**Wildcard** : 어떤 용도로도 쓰일 수 있는 비장의 카드(joker가 이에 해당)

java의 Generic에서는 **'?'**를 비한정적 와일드카드 타입으로 사용할 수 있다.

물음표(?)는 모든 클래스나 인터페이스가 올 수 있다.

왜 써야 해? 실제 타입 매개변수가 무엇인지 궁금하지 않을 때 사용해야 한다. => **즉 안정적이다.**

일반적인 제네릭 방식과의 차이는 아래와 같다.

```java
public void sampleCode() {
    List<Integer> integerList = Arrays.asList(1, 2, 3);

    printList1(integerList);
    printList2(integerList);
}

static void printList1(List<?> list) {
    // 1. 와일드 카드는 list에 담긴 원소에는 전혀 관심이 없기 때문에 원소와 관련된 add 메소드를 사용할 수 없다.
    // 2. 단, null은 들어갈 수 있다.
        
    list.add(list.get(1)); // 컴파일 실패
}

static <T> void printList2(List<T> list) {
    // 1. 제네릭은 list에 담긴 원소에 관심을 갖기 때문에 원소와 관련된 add 메소드를 사용할 수 있다.
    // 2. 당연히 null도 들어갈 수 있다.
        
    list.add(list.get(1)); // 컴파일 성공
}
```

### Raw Type이 허용되기도 하는 경우
- class 리터럴 (List.class, String[].class)
- instanceof 사용 시
```java
if (o instance of Set) {
  Set<?> s = (Set<?>) o;
}
```

#### 참고 링크
https://vvshinevv.tistory.com/55



