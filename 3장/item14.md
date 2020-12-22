## Item 14 : Comparable을 구현할지 고려하라

Comparable 인터페이스와 그 메서드 compareTo()
- Java 라이브러리의 모든 값 클래스/ 열거타입이 Comparable을 구현
- 알파벳, 숫자, 연대 등의 명확한 값 클래스 작성시 반드시 Comparable 구현하자
- compareTo : Object 메서드가 아니며, equals와 다음 항목 빼고 성격이 동일하다
  - 단순 동치성 비교 가능
  - 순서 비교 가능

#### 1. compareTo 메서드의 일반 규약 : equals의 규약과 비슷하다.

```java
/*
 *    x < y    →   -1
 *    x == y   →   0
 *    x > y    →   1
 */

// 1. Exception
(x.compareTo(y) < 0) 이라면 (y.compareTo(x) > 0)
→ x.compareTo(y)가 Exception을 발생시 y.compareTo(x)도 Exception을 발생

// 2. A → B이고 B → C 라면 A → C 
x.compareTo(y) < 0 이고 y.compareTo(z) < 0 이라면 x.compareTo(z) < 0

// 3. 크기가 같은 객체들끼리는 어떤 객체와 비교하더라도 항상 같아야한다
x.compareTo(y) == 0
sgn(x.compareTo(z)) == sgn(y.compareTo(z))
// compareTo 메소드로 수행한 ==(동치성)테스트 결과가, equals와 같아야한다는 것

정렬된 컬렉션의 경우 equals가 아닌 compareTo를 이용하여 정렬을 시도한다.
```

* 정렬된 컬렉션 TreeSet, TreeMap
  * 정렬된 컬렉션의 경우 동치성 비교를 compareTo로 한다.
  * HashSet에서는 equals를 사용하기 때문에 1.0과 1.00을 다른 것으로 취급
  * TreeSet에서는 compareTo를 사용하므로 1.0, 1.00을 같은 것으로 취급


#### 2. compareTo를 작성하여 Comparable 구현

> 💡 compareTo 작성 요령은 equals와 비슷하나 몇가지 차이점에 주의

- Comparable은 제네릭 인터페이스이기 때문에 equals에서 행했던 type 확인/변환이 필요 없다.
- null을 인자로 받는다면 단순하게 NullPointerException을 발생시키면 된다
- 객체 참조 필드를 비교시, equals처럼 compareTo 또한 해당 변수에 대해 compareTo를 재귀호출하여 순서를 비교한다.

#### 3. Comparator 인터페이스의 비교자 생성 메서드 (Java 8)

- 메서드 연쇄방식으로 비교자 생성 가능

> ⚡️정수 타입을 비교시 **관계연산자 <, >를 사용하는 방식은 지양** 하고 **정적메서드인 compare, 또는 Comparator 인터페이스가 제공하는 비교자 생성 메서드 사용**하자

##### 1) 정적 메서드 compare
```java
static Comparator<Object> hashCodeOrder = new Comparator<>(){
    public int compare(Object o1, Object o2) {
    return Integer.compare(o1.hashCode(), o2.hashCode());
        }
};
```
##### 2) 비교자 생성 메서드
```java
static Comparator<Object> hashCodeOrder = Comparator.comparingInt(o -> o.hashCode())
```


<br/>

-----

<br/>

##### 🍊 QUIZ 

1. Cloneable을 사용하려면 이를 interface로 확장하거나, 새 클래스로 이것을 구현하기만 하면 된다. [O | X]
2. 가변 상태를 참조한 clone 메서드는 클래스를 복제했을 시, 불변성을 해친다. [O | X]
3. 배열을 clone하지 않는다면 대체로 Cloneable보다는 [　　　　] 과(와) [　　　　] 과(와)를 사용하자