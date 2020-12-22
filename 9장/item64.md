# Item 64 : 객체는 인터페이스를 사용해 참조하라

> 💡 적합한 인터페이스만 있다면 매개변수뿐 아니라 반환값, 변수, 필드를 전부 인터페이스 타입으로 선언하라

[O]
```java
Set<Son> sonSet = new LinkedHashSet<>();
```

[x]
```java
LinkedHashSet<Son> sonSet = new LinkedHashSet<>();
```

#### 1. 왜 이렇게 써야하는 것일까?
- ***프로그램의 유연성을 높이기 위해서***
- 나중에 구현 클래스를 교체하고자 한다면 그저 새 클래스의 생성자(또는 다른 정적 팩터리)를 호출해주기만 하면 되기 때문
- ex) HashMap -> EnumMap (속도 빨라짐..)
  -  키 타입에 상관 없을 떄 (LinkedHashMap)

> 단, 원 클래스가 특별한 기능을 제공해서 이 기능에 기대어 동작한다면 새 클래스도 반드시 같은 기능 제공
> > ```LinkedHashSet```의 순서 정책 <-호환안됨-> ```HashSet```

#### 2. 적합한 인터페이스가 없다면 클래스로 참조하자

1. ```String```이나 ```BigInteger```과 같은 값 클래스
2. 클래스 기반으로 작성된 프레임워크 제공 객체
   - ```OutputStream```, ```java.io```
3. 인터페이스에는 없는 특별한 메서드를 제공하는 클래스
   - ```PriorityQueue``` : comparator보유
   - ```Queue``` : 없음