# Item 23 : 태그 달린 클래스보다는 클래스 계층 구조를 활용하라

> 💡 서로 다르게 발전할 수 있는 중복을 같은 중복이라 생각하지 말자.
> 
> → Subtyping을 통해 더 유연하게 코드를 작성해보자.

### 태그 달린 클래스
```java
class Figure {
  enum Shape { RECTANGLE, CIRCLE };
  
  final Shape shape;
  
  double length;
  double width;
  
  double radius;
  
  Figure(double radius) {
    shape = Shape.CIRCLE;
    this.radius = radius;
  }
  
  Figure(double length, double width) {
    shape = Shape.RECTANGLE;
    this.length = length;
    this.width = width;
  }
  
  double area() {
    switch (shape) {
      case RECTANGLE:
        return length * width;
      case CIRCLE:
        return Math.PI * (radius * radius);
      default:
        throw new AseertionError(shape);
    }
  }
}
    
```
보기만 해도 답이 없다.
[1] 쓸모 없는 코드가 한 가득이다.
[2] 여러 구현이 한 클래스에 있어 가독성이 매우 나쁘고, 이 때문에 메모리도 많이 차지한다.
[3] 초기화를 잘못해도 런타임에 발견된다.
**[4] 제일 최악인 것은, 새로운 개념이 추가될 때마다 새로운 태그, 새로운 case, 새로운 생성자들이 늘어나고 Instance 만으로 어떤 타입인지 알 수 없다.**

### 즉, 태그 달린 클래스는 Shit이다.
### 클래스 계층 구조를 설계해보자.
결론 : 공통적인 건 root로, 특징적인 것 각각에 추가한다.

```java
abstract class Figure {
  abstract double area();
}

class Circle extends Figure {
  final double radius;
  
  Circle(double radius) { this.radius = radius; }
  
  @Override double area() { return Math.PI * (radius * radius); }
}

class Rectangle extends Figure {
  final double length;
  final double width;
  
  Rectangle(double length, double width) {
    this.length = length;
    this.width = width;
  }
  
  @Override double area() { return length * width; }
}
```
보기만 해도 깔끔하고 달달하다.
<br>
아래처럼 유연하게 추가할 수도 있다.

```java
class Square extends Rectangle {
  Square(double side) {
    super(side, side);
  }
}
```
