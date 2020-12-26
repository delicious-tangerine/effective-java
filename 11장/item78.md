# Item 78 : 공유 중인 가변 데이터는 동기화해 사용하라

> 💡 **여러 스레드가 가변 데이터를 공유한다면 그 데이터를 읽고 쓰는 동작은 반드시 동기화 해야 한다.**
> 
> → 동기화 실패는 알 수 없는 수많은 오류를 뿜는다!

<br>

### synchronized
해당 메서드나 블록을 한 번에 한 스레드씩 수행하도록 보장한다.

<br>

### 동기화의 기능
- 한 스레드가 변경하는 중이라서 상태가 일관되지 않은 순간의 객체를 다른 스레드가 보지 못하게 막는 용도(배타적 실행, Mutual Execution)
- 한 스레드가 만든 변화를 다른 스레드에서 확인하지 못할 수 있음 => 최종 결과의 보호로 이어짐

**동기화는 배타적 실행 뿐 아니라 스레드 사이의 안정적인 통신에 필요하다.**

<br>

### 원자적이기만 하면 되나?
공유 중인 가변 데이터를 원자적으로 읽고 쓸 수 있더라도, 동기화에 실패하면 망할 수 있다.

```java
public class StopThread {
  private static boolean stopRequested;
  
  public static void main(String[] args) throws InterruptedException {
    Thread backgroundThread = new Thread(() -> {
      int i = 0;
      while (!stopRequested)
        i++;
    });
    backgroundThread.start();
    
    TimeUnit.SECONDS.sleep(1);
    stopRequested = true;
  }
}
```

<br>

1초 후에 끝나야 할 것 가지만, 동기화 되지 않았기 때문에 백그라운드 스레드가 언제쯤에나 볼지 모른다.

<br>

```java
public class StopThread {
  private static boolean stopRequested;
  
  private static synchronized void requestStop() {
    stopRequested = true;
  }
  
  private static synchronized boolean stopRequested() {
    return stopRequested;
  }
  
  public static void main(String[] args) throws InterruptedException {
    Thread backgroundThread = new Thread(() -> {
      int i = 0;
      while (!stopRequested())
        i++;
    });
    backgroundThread.start();
    
    TimeUnit.SECONDS.sleep(1);
    requestStop();
  }
}
```

<br>

쓰기 메서드와 읽기 메서드 모두를 동기화 했다. => 쓰기와 읽기 모두가 동기화되지 않으면 동작을 보장하지 않는다!

<br>

### volatile
배타적 수행과는 상관없지만 항상 최근에 기록된 값을 읽게 됨을 보장함.

```java
public class StopThread {
  private static volatile boolean stopRequested;
  
  public static void main(String[] args) throws InterruptedException {
    Thread backgroundThread = new Thread(() -> {
      int i = 0;
      while (!stopRequested)
        i++;
    });
    backgroundThread.start();
    
    TimeUnit.SECONDS.sleep(1);
    stopRequested = true;
  }
}
```

아래는 될 것 같지만 안 되는 케이스다.

```java
private static volatile int nextSerialNumber = 0;

public static int generateSerialNumber() {
  // 1. 값을 읽고 나서,
  // 2. 값에 일을 더하고 저장한다.
  // 스레드가 이 사이에 접근하면....안전 실패(Safety Failure)
  return nextSerialNumber++;
}
```

가변 데이터를 멀티스레드에서 쓰긴 복잡하다 => **가변 데이터는 단일 스레드에서만 쓰자!**
