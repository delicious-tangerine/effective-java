# `아이템 79: 과도한 동기화는 피하라`

### `과도한 동기환는 동기화 성능을 떨어뜨리고, 교착상태에 빠드리고, 심지어 예측할 수 없는 동작을 낳기도 한다.`

- 응답 불가와 안전 실패를 피하려면 동기화 메서드나 동기화 블록 안에서는 제어를 절대로 클라이언트에 양도하면 안 된다.
- 클라이언트가 넘겨준 객체의 외계인 메서드가 하는 일에 따라 동기화된 영역은 예외를 일으키거나, 교착상태에 빠지거나, 데이터를 훼손할 수도 있다.

### `구체적인 예`
```Java
public class ObservableSet<E> extends ForwardingSet<E> {
    public ObservabletSet(Set<E> set) { super(set); }

    private final List<SetObserver<E>> observers = new ArrayList<>();

    public void addObserver(SetObserver<E> observer) {
            synchronized(observers) {
                observers.add(observer);
            }
    }

    public boolean removeObserver(SetObserver<E> observer) {
            synchronized(observers) {
                return observers.remove(observer);
            }
    }

    public void notifyElementAdded(E elemet) {
            synchronized(observers) {
                for (SetObserver<E> observer : observers)
                    observer.added(this, element);
            }
    }

    @Override
    public boolean add(E element) {~}

    @Override
    public boolean addAll(Collection<? extends E> c) {~}
}
```

<br>

- ConcurrentMOdificationException 발생!
```Java
set.addObserver(new SetObserver<>() {
    public void addded(ObservableSet<Integer> s, Integer e) {
        System.out.println(e);
        if (e == 23) s.removeObserver(this);
    }
})
```

<br>

- 데드락 발생!
```Java
set.addObserver(new SetObserver<>() {
    public void addded(ObservableSet<Integer> s, Integer e) {
        ExecutorService exec = Executors.newSingleThreadExecutor();

        ~~ executor service에서 구독 취소 ~~
    }
})
```

### `동기화로 인해 생기는 문제의 대안`
- 외계인 메서드 호출을 동기화 블록 밭깥으로 옮기자.
- 자바의 동시성 컬렉션 라이브러리인 CopyOnWriteArrayList를 사용해보자.

<br>

### `가변 클래스를 작성하려거든 다음 두 선택지 중 하나를 따르자`

- 동기화를 전혀 하지 말고 그 클래스를 동시에 사용해야 하는 클래스가 외부에서 알아서 동기화 하게 하자 (java.util)

- 동기화를 내부에서 수행해 스레드 안전한 클래스로 만들자. (java.util.concurrent)