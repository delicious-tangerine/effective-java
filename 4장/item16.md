## Item 16 : public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라

#### 1. 퇴보한 클래스란?
: 캡슐화 없이 그저 인스턴스 필드만을 모아 놓은 것
```java
class Point {
    public double x;
    public double y;
}
```

#### 2. public 클래스에서의 접근자(getter)

- 필드를 모두 private으로 바꾸고 getter를 추가
- 유연성 :패키지 바깥에서 getter를 통해 클래스 내부 표현 방식을 변경 가능

> public 클래스의 필드가 final이라면?
> 직접 노출할때 -> 단점이 좀 줄어들지만 그래도 지양

#### 3. 데이터 필드를 getter로 접근하는게 아니라 그냥 노출해도 상관 없을때

- package-private 클래스
  > 클라이언트도 어차피 해당 클래스를 포함한 패키지 안에서만 동작
- private 중첩 클래스
  > private 중첩 클래스를 포함하는 외부 클래스까지만 수정 가능
- 추상개념만 바르게 표현하면 된다
- 오히려 클래스 선언 또는 클라이언트 코드 면에서 접근자 방식보다 깔끔


