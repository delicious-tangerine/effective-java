# `아이템 90: 직렬화된 인스턴스 대신 직렬화 프록시 사용을 검토하라`

- Serializable을 구현하기로 결정한 순간 언어의 정상 메커니즘인 생성자 이외의 방법으로 인스턴스를 생성할 수 있게 된다.
- 버그와 보안 문제가 일어날 가능성이 커진다.
<br>

## `직렬화 프록시 패턴`
- 바깥 클래스의 논리적 상태를 정밀하게 표현하는 중첩 클래스를 설계해 private static으로 선언 한다.
- 중첩 클래스의 생성자는 단 하나여야 하며, 바깥 클래스를 매개변수로 받아야 한다.

### `중첩 클래스`
```Java
private static class SerializationProxy implements Serializable {
    private final Date start;
    private final Date end;

    SerializationProxy(Period p) {
        this.start = p.start;
        this.end = p.end;
    }

    private static long serialVersionUID = <무작위 long값>;
}
```

### `직렬화 프록시 패턴용 writeReplace 메서드`
```Java
private Object writeReplace() {
    return new SerializationProxy(this);
}
```
- 직렬화 시스템이 바깥 클래스 인스턴스 대신 proxy 인스턴스를 반환하게 한다.

### `왜 이런 패턴을 쓰는가?`
- 직렬화는 생성자 없이 인스턴스를 생성 한다. 즉 생성시의 제약 조건을 정하기가 까다롭다.
- Proxy객체를 이용해 Proxy객체에서 제어를 해준다.
- Proxy를 이용해 안전하고 정확한 객체를 만들어 내자!