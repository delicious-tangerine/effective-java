# `아이템 36: 비트 필드 대신 EnumSet을 사용하라`

### `구닥다리 기법!`
```Java
public class Text {
    public static final int STYLE_BOLD          = 1 << 0;
    public static final int STYLE_ITALIC        = 1 << 1;
    public static final int STYLE_UNDERLINE     = 1 << 2;
    public static final int STYLE_STRIKETHROUGH = 1 << 3;
}
```

- OR를 이용한 비트 필드를 통해 집합을 모을 수 있다.<br>
=> 집합 연산이 효율적이다! 그런데 굳이 . . .? 해석하기도 어렵고 순회하기도 까다롭다!

<br>

### `비트 필드를 대체하는 현대적 기법`
```Java
public class Text {
    public enum Style { BOLD, ITALIC, UNDERLINE, STRIKEROUGH }
}
```

- enum set 을 사용 하자