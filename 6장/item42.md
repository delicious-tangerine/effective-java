# `아이템 42: 익명 클래스보다는 람다를 사용하라`

### `낡은 기법의 익명 클래스 인스턴수 함수 객체`
```Java
Collections.sort(words, new Comparator<String>) {
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
}
```

### `람다를 사용하면?`
```Java
Collections.sort(words, (s1, s2) -> Integer.compare(s1.length(), s2.length()));
```

- 타입을 명시해야 코드가 더 명확할 때만 제외하고는, 람다의 모든 매개변수 타입은 생략하자.

### `람다로 열거타입의 인스턴스 필드를 손쉽게 구현하기`
```Java
public enum Operatopm {
    PLUS("+") {
        public double apply(double x, double y) { return x + y; };
    }
}

// PLUS를 아래와 같이 변경 가능
PLUS("+", (x, y) -> x + y);
```

- 이렇게만 보면 람다는 만능인것 같다. 하지만 람다는 이름이 없고 문서화도 못 한다. 따라서 코드 자체로 동작이 명확히 설명되지 않거나 코드 줄 수가 많아지면 람다를 쓰지 말아야 한다.
- 람다가 세 줄을 넘어간다면 람다를 쓰지 않는쪽으로 리팩터링 하길 고려하라.
- 함수 객체가 자신을 참조해야 한다면(this 키워드) 람다 대신 익명 클래스를 이용하라.
- 람다의 직렬화 형태는 구현별로( ex 가상머신별로) 다를 수 있으니 람다를 직렬화하는 일은 극히 삼가라.

<br>

- 익명 클래스는 (함수형 인터페이스가 아닌) 타입의 인스턴스를 만들 때만 사용하라.