# Item 69 : 예외는 진짜 예외 상황에만 사용하라.

> 💡 예외는 오직 예외 상황에서만 써야 한다.
> 
> → 절대로 일상적인 제어 흐름용으로 쓰여선 안 된다.

### You never do like this.

```java
try {
  int i = 0;
  while (true)
    range[i++].climb();
} catch (ArrayIndexOutOfBoundsException e) {
}
```

1. 예외는 예외 상황에 쓸 용도로 설계되었다. 따라서 빠르게 만들어야 할 동기가 약하다.

2. 코드를 try-catch 안에 넣으면 JVM이 적용할 수 있는 최적화가 제한된다.

등의 이유로 훨씬 느리다.

### 예외는 오직 예외 상황에서만 써야 한다.
만약 다른 예외가 있었는데, try-catch 구문으로 인해 잘 설계된 것으로 판단하고

런타임에서 다른 에러를 발생하는 경우를 찾아내지 못할 수도 있다.

### 잘 설계된 API라면 클라이언트가 정상적인 제어 흐름에서 예외를 사용할 일이 없게 해야 한다.
