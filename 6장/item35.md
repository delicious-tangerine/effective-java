# `아이템 35: ordinal 메서드 대신 인스턴스 필드를 사용하라`



### `ordinal 사용의 예`
```Java
public enum Ensemble {
    SOLO, DUET, TRIO, QUARTET, QUINTET;

    public int getOrdinal() {return this.ordinal(); }
}
```

- 유지 보수 하기 끔찍한 코드<br>
=> 상수 선언 순서를 바꾸는 순간 오작동 한다.<br>
=> 예를 들어 JPA의 EnumType.ORDINAL을 사용하면 상수 순서가 바뀌는 순간 디비를 갈아 엎어야 한다...

- 해결책은 간단하다. 쓰지 말자.<br>
=> 그럼 왜있나? EnumSet, EnumMap과 같은 범용 자료구조에 쓸 목적으로 만들어 졌다.

- 인스턴스 필드를 사용하자

```Java
public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5);

    private final int numberOfMusicians;
    Ensemble(int num) {this.numberOfMusicians = num;}
}
```