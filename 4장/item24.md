# Item 24 : 멤버 클래스는 되도록 static으로 만들라

> 💡 중첩 클래스에 대한 개념을 이해하고 왜 static으로 만들어야 하는지 이해하자.

### 중첩 클래스(nested class)
#### 뜻 : A 클래스 안에 정의되어 있는 또 다른 클래스 B
<br>
따라서 B 클래스(inner class)는 A 클래스(outer class)에서만 쓰여야 하며, 다른 쓰임새가 있다면 Top-Level로 만들어야 한다.
<br>
(참고로 static member class는 inner class라고 부를 수 없다. 아래에서 설명한다.)
<br>
<em>왜?</em> 멤버 클래스가 다른 곳에서 쓰이게 되면 멤버일 이유가 없다. 사실상 A 클래스의 캡슐화를 망치는 셈이다.

#### 종류
static member class, non-static member class, anonymous class, local class

<br>

### static member class VS non-static member class
비정적 멤버 클래스의 인스턴스는 생성될 때 바깥 클래스와 관계가 생기게 된다.

(드물게 직접 A.new B(args)를 호출할 수도 있다.)


어댑터를 만들 때 보통 비정적 멤버 클래스를 사용한다.

```java
public class MySet<E> extends AbstractSet<E> {
  ...
  
  @Override public Iterator<E> iterator() {
    return new MyIterator();
  }
  
  private class MyIterator implements Iterator<E> {
    ...
  }
}
```

<br>

실제 HashMap 구현에서는 EntrySet 같은 비정적 멤버 클래스가 있다.

```java
public class HashMap<K,V> extends AbstractMap<K,V>
  implements Map<K,V>, Cloneable, Serializable {
  ... 
  public Set<Map.Entry<K,V>> entrySet() {
    Set<Map.Entry<K,V>> es;
    return (es = entrySet) == null ? (entrySet = new EntrySet()) : es;
  }
  
  final class EntrySet extends AbstractSet<Map.Entry<K,V>> {
    public final int size() { return size; }
    public final void clear() { HashMap.this.clear(); }
    ...
  }
}
```

### 멤버 클래스에서 바깥 인스턴스에 접근할 일이 없다면 무조건 static을 붙여라!
[이유 1] 참조 저장에 시간과 공간 낭비

[이유 2] GC가 바깥 클래스의 인스턴스를 수거하지 못할 수 있음 => Memory leak

### 익명 클래스 => 람다로 대체
```java
public class Fruit {
  Lambda LambdaClass(){
    return (String a) -> {
      System.out.println("Tangerine" + a);
    };
  }

  interface Lambda {
    void tangerine(String a);
  }
}
```
### 지역 클래스
- 사실상 지역변수랑 같다.
- 이름이 있고 반복 사용 가능
- non-static에서만 바깥 인스턴스 참조 가능
- 정적 멤버를 가질 수 없음

    
