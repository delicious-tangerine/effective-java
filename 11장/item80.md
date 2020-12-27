# `아이템 80: 스레드보다는 실행자, 태스크, 스트림을 애용하라`

### `뛰어난 작업 큐 'ExecutorService'`

```Java
ExecutorService exec = Executors.newSingleThreadExecutor();

exec.executr(runnable);

exe.shutdown();
```

### `실행자 서비스의 대표적 기능`
- 특정 태스크가 완료되기를 기다린다.
- 태스크 모음 중 아무것 하나 혹은 모든 태스크가 완료되기를 기다린다.
- 실행자 서비스가 종료하기를 기다린다.
- 완료된 태스크들의 결과를 차례로 받는다.
- 태스크를 특정 시간에 혹은 주기적으로 실행하게 한다.



### `CachedThreadpool`
- 작은 프로그램이나 가벼운 서버라면 CachedThreadPool을 이용하라.
- 무거운 프로덕션 서버에는 좋지 못하다.
- CachedThreadpool에서는 요청받은 태스크들이 큐에 쌓이지 않고 즉시 스레드에 위임돼 실행된다.

### `ForkJoinPool`
- 병렬 처리에 매우 유용하다.