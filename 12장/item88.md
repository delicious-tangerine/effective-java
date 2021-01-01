# `아이템 88: readObject 메서드는 방어적으로 작성하라`

### `방어적 복사를 사용하는 불변 클래스`
```Java
public final class Period {
    private final Date start;
    private final Date end;

    public Period(Date start, Date end) {
        this.start = new Data(start.getTime());
        this.end = new Data(end.getTime());

        if (this.start.compareTo(this.end) > 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

- 위 코드는 serializable을 추가하는것으로 직렬화 할 수 있다.
- 하지만 serializable을 추가하는것으로는 불변식을 더는 보장하지 못한다.


### `readObject 메서드는 실질적으로 또다른 public 생성자이다.`
- readObject 메서드에서도 인수가 유효한지 검사해야 하고 필요하다면 매개변수를 방어적으로 복사 해야 한다.
- 잘못된 객체를 생성할 수 있는 바이트 스트림은 불변식을 깨뜨리는 객체를 생성 할 수 있다<br>

<br>

- 불변클래스를 해칠 수 없도록 readObject를 다시 선언하자.
```Java
private void readObject(ObjectInputStream s) throws IoException{
    s.defaultReadObject();

    if (start.compareTo(end) > 0) {
        throw new Exception();
    }
}
```

### `readObject를 재정의 하는것만으로는 완벽하게 방어할 수 없다.`
- 직렬화된 바이트 스트림에서 private 필드를 참조 할 수 있다.
- 인스턴스는 불변식을 유지한 채 생성됐지만, 의도적으로 내부의 값을 수정 할 수 있다.
- ### 객체를 역직렬화할 때는 클라이언트가 소유해서는 안되는 개체 찾모를 갖는 필드를 모두 방어적으로 복사해야 한다.

```Java
private void readObject(ObjectInputStream s) throws IoException{
    s.defaultReadObject();

    start = new Date(start.getTime());
    end = new Date(end.getTime());

    if (start.compareTo(end) > 0) {
        throw new Exception();
    }
}
```
