# `아이템 83: 지연 초기화는 신중히 사용하라`


## `지연 초기화`
- 필드의 초기화 시점을 그 값이 처음 필요할 때까지 늦추는 기법이다.
- 그 값이 전혀 쓰이지 않으면 초기화도 결코 일어나지 않는다.
- 지연초기화로의 성능 향상이 일어날지를 알고싶다면 안타깝게도 직접 측정해보는 방법밖에없다.

<br>

- 멀티스레드 환경에서의 지연 초기화는 까다롭다.

### `인스턴스 필드를 초기화하는 방법`
```Java
// 일반적인 방법
private final FieldType field = computeFieldValue();

// 지연초기화
private FieldType field;

private synchronized FieldType getField() {
    if (field == null)
        field = computeFieldValue();
    return value;
}


// 지연 초기화 홀더 클래스 관용구
private static class FieldHolder {
    static final FieldType field = computeFFieldValue();
}

private statif FieldType getField() {
    return FieldHolder.field;
}

// double-check 관용구
private volatile FieldType field;

private FieldType getField() {
    FieldType result = field;
    if (result != null)
        return result;

    synchronized(this) {
        if (field == null)
            field = computeFieldValue();
        return field;
    }
}

```

### `핵심정리`
- 대부분의 필드는 지연시키지 말고 곧바로 초기화해야 한다.
- 성능 때문에 혹은 위험한 초기화 순환을 막기 위해 꼭 지연초기화를 써야 한다면 올바른 지연 초기화 기법을 사용하자.