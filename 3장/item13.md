## Item 13 : clone 재정의는 주의해서 진행하라

> 💡 Cloneable : 복제해도 되는 클래스임을 명시하는 용도의 Mixin interface
> - clone() 이 protected로 Cloneable이 아닌 Object에 선언되어 있음 → *Cloneable을 구현하는 것만으로 외부 객체에서 clone() 호출 불가*

#### 1. 메서드 하나 없는 Cloneable interface가 하는 일
: clone()의 동작방식을 결정한다
- **Cloneable을 구현한 클래스**의 인스턴스에서 clone을 호출하면 그 객체의 필드들을 하나하나 복사한 객체를 반환
- **Cloneable을 구현 안한 클래스**의 인스터스에서 호출 시 CloneNotSupportedException을 던짐

⭐️ Cloneable 인터페이스 : *상위 클래스에 정의된 protected 메서드의 동작 방식을 변경한것* → 즉, 매우 위험하고 모순적인 매커니즘 **"생성자를 호출하지 않고도 객체를 생성할 수 있게 됨"** 탄생

<br/>

#### 2. clone()의 일반 규약


```java
/* .clone() : 객체의 복사본을 생성해 반환 */

//참
x.clone() != x

//참, 필수 아님
x.clone().getClass == x.getClass()

//참, 필수 아님
x.clone().equals(x)

// [관례] 해당 클래스와 모든 상위 클래스가 super.clone()을 호출받아 객체를 반환한다면,
// 아래도 참
x.clone().getClass() == x.getClass()
```

- 강제성이 없다는 점만 빼면 생성자 연쇄(Constructor Chaining)과 비슷

> Class B extends A
> B.clone() → B 타입 객체 반환해야함

> 💡 그런데 A.clone()이 자신의 생성자 new A(..)로 생성한 객체를 반환
> - A.clone() → new A(..) → A 타입 객체 반환
> - B.clone() → super.clone() → A 타입 객체를 반환
> **하위 클래스에서 super.clone 호출시 잘못된 클래스의 객체가 만들어져서 .clone() 제대로 동작X**

- final 클래스라면 clone()을 재정의 했어도 하위클래스가 없으므로 이 [관례]를 무시해도 안전 (그래도 성능 최적화 관점, 특히 thread,, 에서 검토 후 판단)

<br/>

#### 3. 가변 상태를 참조한 clone 메서드 : 재앙

[Example] Stack 클래스
```java
public class Stack {
    private Object[] elements;
    private int size = 0;
    ...
}
```
* 이 클래스가 clone 된다면...
  * 반환된 Stack 인스턴스의 size 필드 : OK
  * 반환된 Stack 인스턴스의 elements 필드 : **원본의 배열과 같은 배열 참조**
    → 원본/복제본 수정시 다른 하나도 수정되어 **불변식을 해침**

<br/>

* 제대로 동작하려면...
  * 스택 내부 정보를 복사 → elements 배열의 **clone을 재귀적으로 호출**

```java
   @Override
    public Stack clone() {
        try {
            Stack stack = (Stack) super.clone();
            stack.elements = this.elements.clone(); //요렇게
            return stack;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
```

* 기존 객체와 복사된 객체의 객체 배열이 서로 다른 주소를 가지고 있어도 **배열의 원소가 가지고 있는 객체는 같은 객체**
  *  ex) HashTable의 예
* ⚡️ 기존 elements의 원소들을 **반복문으로 새로 생성**하여 복사된 배열에 넣으면 해결 가능
  * HashTable.Entry (깊은복사 지원) 을 사용해도 되지만, 재귀함수로 인해 스택오버플로우가 날 가능성 있다. -> 반복자 사용


#### 4. 즉,  Cloneable보다는 복사 생성자와 복사 팩터리를 사용하자

```java
// 복사 생성자
public Sonic (Sonic sonic)

//복사 팩터리
public static Sonic newInstance(Sonic Sonic)
```

- 복사 생성자 : 자기 자신과 같은 클래스의 인스턴스를 인수로 받는 생성자
- 복사 팩터리 : 복사 생성자를 모방한 정적 팩터리

[장점]
- Cloneable의 단점 모두 커버
⚡️ 추가적으로 ⚡️
- 복사 클래스가 구현한 인터페이스 타입의 인스턴스를 인수로 받을 수 있다.
- 변환 생성자, 변환 팩터리
- 원본의 구현 타입에 얽매이지 않고 복제본의 타입 직접 선택 가능

<br/>

-----

<br/>

##### 🍊 QUIZ 

1. Cloneable을 사용하려면 이를 interface로 확장하거나, 새 클래스로 이것을 구현하기만 하면 된다. [O | X]
2. 가변 상태를 참조한 clone 메서드는 클래스를 복제했을 시, 불변성을 해친다. [O | X]
3. 배열을 clone하지 않는다면 대체로 Cloneable보다는 [　　　　] 과(와) [　　　　] 과(와)를 사용하자