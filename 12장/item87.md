# `아이템 87: 커스텀 직렬화 형태를 고려해보라`

- 먼저 고민해보고 괜찮다고 판단될 때만 기본 직렬화 형태를 사용하라.
- 객체의 물리적 표현과 논리적 내용이 같다면 기본 직렬화 형태라도 무방하다.

### `이상적인 직렬화 형태`
```Java
public class Name implements Serializable {
    /**
     * 성. null이 아니어야 한다.
     * @serial
     */
    private final Stirng lastName;

    /**
     * 이름. null이 아니어야 한다.
     * @serial
     */
    private final String firstName;

    /**
     * 중간이름. 중간이름이 없다면 null
     * @serial
     */
    private final String middleName;

    ... // 나머지 코드는 생략
}
```

### `기본 직렬화 형태에 적합하지 않은 클래스`
```Java
public final class StringList implements Serializable {
    private int size = 0;
    private Entry head = null;

    private static class Entry implements Serializable {
        String data;
        Entry next;
        Entry previous;
    }
    // ... 생략
}
```

- 너무 많은 시간과 공간을 차지
- 스택 오버 플로 발생 가능

### `합리적인 커스텀 직렬화`
```Java
public final class StringList implements Serializable {
    private transient int size = 0;
    private transient Entry head = null;

    // 이번에는 직렬화 하지 않는다.
    private static class Entry {
        String data;
        Entry next;
        Entry previous;
    }

    // 문자열을 리스트에 추가한다.
    public final void add(String s) { ... }

    /**
     * StringList 인스턴스를 직렬화한다.
     */
    private void writeObject(ObjectOutputStream stream)
            throws IOException {
        stream.defaultWriteObject();
        stream.writeInt(size);

        // 모든 원소를 순서대로 기록한다.
        for (Entry e = head; e != null; e = e.next) {
            s.writeObject(e.data);
        }
    }

    private void readObject(ObjectInputStream stream)
            throws IOException, ClassNotFoundException {
        stream.defaultReadObject();
        int numElements = stream.readInt();

        for (int i = 0; i < numElements; i++) {
            add((String) stream.readObject());
        }
    }
    // ... 생략
}
```

- 해당 객체의 논리적 상태와 무관한 필드라고 확신할 때만 transient 한정자를 생략해야 한다.

### `UID를 명시적으로 부여하자`
```Java
private static final long serialVersionUID = <무작위의 long 값>;
```

- 명시적으로 부여하지 않으면 런타임에 값을 생성하느라 오버헤드가 있다.
- 호환성을 위해서라도 명시적으로 UID를 부여하자.<br>
=> 호환성을 끊으려는 경우라면 UID를 수정하면 된다.