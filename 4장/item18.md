# Item 18 : 상속보다는 컴포지션을 사용하라

> 💡 이 책에서의 '상속' => 구현상속
> : 클래스가 다른 클래스를 상속
> (!= 클래스가 인터페이스를 구현 )

1. 상속의 위험성 : 캡슐화를 깨뜨린다

- 상위 클래스의 릴리즈마다 내부구현이 다를 수 있다
  → 하위클래스에 변경이 없어도 문제가 생길 수 있다

<br/>

2. 하위클래스 Overriding의 문제점

```java
s.addAll(List.of("웅","앵","웅")); // 이렇게 세 원소를 한꺼번에 Set에 추가한다면

public class InstrumentedHashSet<E> extends HashSet<E> {
    private int addCount = 0;
    
    ...중략...
    
    @Override //3.오버라이드한 .add()를 실행
    public boolean add(E e){
        addCount++;// 4. 여기서 또 추가
        return super.add(e)
    }
    @Override
    public boolean addAll(Collection<? extends E> c){
        addCount += c.size(); // 1. 3이 더해짐
        return super.add(e)
        // 2. HashSet의 addAll을 호출 => 이때 .add()로 원소 더하는데 위에 Override 되어있음
    
    ...중략
}
```

[1] Override로 하위클래스의 로직 방어 **(비효과적)**


- 상위클래스의 메서드 동작을 다시 구현하는게 어렵고 비용이 많이든다
- **오류**나 성능을 떨어뜨려 위험성이 증가한다.
- 상위클래스가 **private 클래스**를 쓰면 하위클래스에서 접근 불가 → 구현불가

[2] 상위 클래스에서 새로운 메서드를 추가할 경우
* 상위 클래스의 새 릴리즈에서 나온 메서드로 하위 클래스에서 '허용되지 않은' 원소를 추가할 경우를 고려해야한다 

[3] 하위클래스에 추가한 새 메서드가, 상위 클래스 다음 릴리즈에서 같은 시그니처에 리턴타입도 같은 것이 추가될 경우

<br/>

#### 3. 컴포지션과 전달 방식

> 💡 기존클래스 확장 대신 새로운 클래스를 만들고 private 필드로 기존 클래스의 인스턴스를 참조하는 설계패턴
> * 컴포지션 : 기존 클래스가 새로운 클래스의 구성요소로 쓰임 **(기존클래스-새클래스 : is-a관계)**
> * 전달 (forwarding) : 새 클래스의 인스턴스 메서드(= 전달 메서드)들은 기존 클래스(private로 참조됨)에 대응하는 메서드를 호출하고 return

* 장점
    - 새로운 클래스는 기존 클래스의 내부 구현 방식의 영향에서 벗어남
    - 기존 클래스에 새로운 메서드가 추가되어도 괜찮음

[1] 래퍼클래스(wrapper class)
```java
public class InstrumentedSet<E> extends ForwardingSet<E> {
    private int addCount = 0;

    public InstrumentedSet(Set<E> s) {
        super(s);
    }

    @Override
    public boolean add(E e) {
        addCount++;
        return super.add(e);
    }

    @Override public boolean addAll (Collection< ? extends E > c){
        addCount += c.size();
        return super.addAll(c);

    }

    public int getAddCount () {
        return addCount;
    }
}
```
- 상속방식 : 구체 클래스 각각을 따로 확장해야함
- 지원하고 싶은 상위 클래스의 생성자 각각에 대응하는 생성자를 별도 설정
- 다른 Set 인스턴스를 wrapping 하고 있어서 wrapper class (rapper아님ㅎ ㅈㅅ)
- Decorator Pattern : 다른 Set에 계측 기능을 덧씌운다는 뜻에서 데코레이터 패턴이라고 불린다

[2] 전달 클래스(Forwarding class)
```java
public class ForwardingSet<E> implements Set<E> {

    private final Set<E> s;
    public ForwardingSet(Set<E> s) { this.s = s;}

    public void clear() { s.clear(); }
    public boolean contains(Object o) {return s.contains(o); }
    public boolean isEmpty() { return s.isEmpty(); }
    public int size() { return s.size(); }
    public Iterator<E> iterator() { return s. iterator(); }
    public boolean add(E e) { return s.add(e); }
    public boolean remove(Object o) {return s.remove(o); }
    public boolean containsAll(Collection<?> c) { return s.containsAll(c); }
    public boolean addAll(Collection<? extends E> c) { return s.addAll(c); }
    public boolean removeAll(Collection<?> c) { return s.removeAll(c); }
    public boolean retainAll(Collection<?> c) { return s.retainAll(c); }
    public Object[] toArray() { return s.toArray(); }
    public <T> T[] toArray(T[] a) { return s.toArray(a); }
    @Override public boolean equals(Object o) { return s.equals(o); }
    @Override public int hashCode() {return s.hashCode(); }
    @Override public String toString() { return s.toString(); }

}
```
- Set 내부구현에 영향받지 않고 사용
- 전달 메서드로만 이루어져있다
- 재사용할 수 있는 전달 클래스를 인터페이스당 하나씩만 만들어두면 원하는 기능만 더해주는 것을 손쉽게 구현 가능
  
> 💡 위임(delegation)
> : composition과 forwarding의 조합
> - 넓은 의미이며, 래퍼 객체가 내부 객체에 자기 자신의 참조를 넘기는 경우만 위임

> 💡 래퍼클래스 주의점 : 콜백(callback) 프레임워크와는 어울리지 않음
> - 내부객체는 자신을 감싸고 있는 래퍼의 존재를 모르는 상태로 this의 참조를 넘김
> → 콜백 때는 래퍼가 아닌 내부 객체를 호출.
> - SELF의 문제
 
