# `아이템 81: wait와 notify보다는 동시성 유틸리티를 애용하라`

## `wait와 notify는 올바르게 사용하기가 아주 까다로우니 고수준 동시성 유틸리티를 사용하자.`

고수준 유틸리티<br>
1. 실행자 프레임워크 (item 80에서 살펴본)
2. 동시성 컬렉션
3. 동기화 장치

<br>

## `동시성 컬렉션`
- 동시성 컬렉션은 표준 컬렉션 인터페이스에 동시성을 가미해 구현한 고성능 컬렉션이다.
- 동시성 컬렉션에서 동시성을 무력화하는건 불가능하며 외부 락을 추가로 사용하면 오히려 느려진다.
- putIfAbsent(key, value) 같은 상태 의존적 수정 메서드가 있다.
- 동기화된 컬렉션을 동시성 맵으로 교체하는것만으로 성능이 극적으로 개선된다.

## `동기화 장치`
### CountDownLatch
- int값을 받아 countDown 메서드를 몇 버 호출해야 대기중인 스레드를 꺠우는지 결정
- 사용 예시
```Java
public static long time(Executor executor, int concurrency, Runnable action) throws InterruptedException {
    CountDownLatch ready = new CountDownLatch(concurrency);
    CountDownLatch start = new CountDownLatch(1);
    CountDownLatch done = new CountDownLatch(concurrency);

    for (int i=0; i< concurrency; i++) {
        executor.execute(() -> {
            ready.countDown();
            try {
                start.await();
                action.run();
            } catch(InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                done.countDown();
            }
        });
    }

    ready.await();
    long startNanos = System.nanoTIme();
    start.countDown();
    done.await();
    return System.nanoTime() - startNanos;
}
```

- `await와 notify를 직접 사용하는 것을 동시성 '어셈블리언어'로 프로그래밍하는 것에 비유할 수 있다.`
- `코드를 새로 작성한다면 await와 notify를 쓸 이유가 거의 없다.`