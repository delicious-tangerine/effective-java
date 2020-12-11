# `아이템 34: int 상수 대신 열거 타입을 사용하라`

### `정수 열거 패턴 - 다수의 취약점`

```Java
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN = 1;

public static final int ORANGE_NAVEL = 0;
public static final int ORANGE_TEMPLE = 1;
```

- 동등 연산자로 비교하더라도 컴파일러는 아무런 경고 메세지가 없다.<br>
=> APPLE_FUJI와 ORANGE_NAVEL은 분명 다른 의미를 가질텐데 같게 취급 된다.

- 클라이언트에 값이 그대로 새겨진다.<br>
=> 상수값 변경시 재컴파일 해야한다!

- 디버깅이 어렵다!<br>
=> 찍어봐도 뭐가 뭔지 ... ?

<br>

### `그래서 우리는 열거 타입을 사용 해야 한다.`

```Java
public enum Apple { FUJI, PIPPIN }
public enum Orange { NAVEL, TEMPLE }
```

- 열거 타입은 자체로 클래스이며 상수 하나당 하나의 인스턴스를 만든다.<br>
=> 싱글턴을 구현 할 수 있는 가장 손쉬운 방법

- 컴파일 타임 타입 안정성을 제공<br>
=> Apple, Orange 타입인지 검사를 한다. 다른 타입 끼리는 할당, 비교 등의 연산이 안된다.

- Object 메소드들을 구현 할 수 있다!<br>
=> Comparable, Serializable 구현 가능.

- 내부 메소드 구현도 가능!
```Java
public enum Category {
    FOOD, CAFE, SHOP;

    public String contains(String key) {
        Category[] values = values();
            for (Category value : values) {
                if (value.name().equals(key)) {
                    return true;
                }
            }
        return false;
    }
}
```

<br>

### `이대로 만족 하는가?`
```Java
public enum Operation {
    PLUS, MINUS, TIMES, DIVIDE;

    public double apply(double x, double y) {
        switch(this) {
            case PLUS: return x + y;
            case MINUS: return x - y;
            case TIMES: return x * y;
            case DIVIDE: return x / y;
        }
        throw new AssertionError("알 수 없는 연산");
    }

}
```

- 동작은 하지만 코드가 예쁘지 않다. . .

```Java
public enum Operation {
    PLUS {public double apply(double x, double y){return x + y ;}},
    MINUS {public double apply(double x, double y){return x - y ;}},
    TIMES {public double apply(double x, double y){return x * y ;}},
    DIVIDE {public double apply(double x, double y){return x / y ;}};

    public abstract double apply(double x, double y);
}
```


### `fromString 메서드를 제공하라!`
```Java
private static final Map<String, Operation> stringToEnum = Stream.of(values()).collect(Object::toString, e -> e);

public static Optional<Operation> fromString(String symbol) {
    return Optional.ofNullable(stringToEnum.get(symbol));
}
```