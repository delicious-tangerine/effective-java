# Item 25 : Top-level Class는 한 파일에 하나만

> 💡 한 파일에 여러 클래스를 작성하는 것은 아무런 이득이 없다.
> 
> → 어느 소스 파일을 컴파일하냐에 따라 결과가 달라질 수 있다.

굳이 쓰고 싶으면, 이렇게 써라.

```java
public class Test {
  public static void main(String[] args) {
    System.out.println(Utensil.NAME + Dessert.NAME);
  }
  
  private static class Utensil {
    static final String NAME = "pan";
  }
  
  private static class Dessert {
    static final String NAME = "cake";
  }
}
