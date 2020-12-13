# `아이템 38: 확장할 수 있는 열거 타입이 필요하면 인터페이스를 사용하라`

- 열거 타입은 확장을 할 수 없게 되어 있다.
- 설계상의 실수는 아니고 대부분의 상황에서 열거 타입을 확장하는건 좋지 않은 생각이다.
- 확장 할 수 있는 열거타입이 어울리는 쓰임이 하나정도는 있다. ( 아래의 Operation 예시)

### `인터페이스를 이용해 확장 가능 열거 타입을 흉내`
```Java
public interface Operation {
    double apply(double x, double y);
}
```

```Java
public enum BasicOperation implements Operation {
    PLUS("+") {
        public double apply(double x, double y) {return x + y;}
    }

    ~~~
}
```


```Java
public enum ExtendedOperation implements Operation {
    EXP("^") {
        public double apply(double x, double y) {return Math.pow(x, y);}
    }

    ~~~
}
```


### `제네릭을 이용한 모든 operaton 테스트`
```Java
public static void main(String[] args) {
    double x = Double.parseDouble(agrs[0]);
    double y = Double.parseDouble(agrs[1]);
    test(ExtendedOperation.class, x, y);
}

private static <T extends Enum<T> & Operation> void test () {
    for (Operation op : opEnumType.getEnumConstants())
        System.out.printf("%f %s %f = %f%n", x, op ,y, op.apply(x, y));
}
```