# `아이템 89: 인스턴스 수를 통제해야 한다면 readResolve 보다는 열거 타입을 사용하라`

```Java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }
}
```
- 아이템 3에서 이야기 했듯 이 클래스는 implements Serializable을 추가하는 순간 더 이상 싱글턴이 아니게 된다.
- readObject 메소드를 사용하면 별개의 instance를 반환하게 된다.
- readResolve를 통해 인스턴스를 통제 할 수 있다.
```Java
private Object readResolve() {
    return INSTANCE;
}
```
- readResolve를 인스턴스 통제 목적으로 사용한다면 객체 참조 타입 인스턴스 필드는 모두 transient로 선언해야 한다. => transient가 아니면 공격의 여지를 남겨둔다.

### `싱글턴을 보장하는 열거타입`
```Java
public enum Elvis {
    INSTANCE;
}
```

<br><br>

---
readResolve를 사용하는 방식이 완전히 쓸모없는 것은 아니다. 직렬화 가능 인스턴스 통제 클래스를 작성해야 하는데, 컴파일타임에는 어떤 인스턴스들이 있는지 알 수 없는 상황이라면 열거 타입으로 표헌하는 것이 불가능 하기 때문이다.

---
- 불변식을 지키기 위해 인스턴스를 통제해야 한다면 가능한 한 열거 타입을 사용하자.
- 여의치 않은 상황에서 직렬화와 인스턴스 통제가 모두 필요하다면 readResolve 메서드를 작성해 넣어야 하고, 그 클래스에서 모든 참조타입 인스턴스 필드를 transient로 선언 해야 한다.
