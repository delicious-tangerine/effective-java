# Item 22 : 인터페이스는 타입을 정의하는 용도로만 사용하라

> 💡 클래스가 어떤 인터페이스를 구현한다 == 자신의 인스턴스로 무엇을 할 수 있는지를 클라이언트에 얘기해주는 것.
> 
> → 인터페이스는 오직 이 용도로만 사용해야 한다.

### 상수 인터페이스 안티패턴은 인터페이스를 잘못 사용한 예
```java
// Anti-Pattern
public interface PhysicalConstants {
 
  static final double AVOGADROS_NUMBER = 6.022_140_857e23;
  static final double BOLTZMANN_CONST = 1.380_648_52e-23;
}
```
- 이유 : 내부 구현을 외부로 공개한 것이기 때문에, 오히려 사용자에게 혼란을 주고 클라이언트 코드가 이 상수에 종속될 수 있음

### How to change this?
유틸리티 클래스를 활용하여 다음과 같이 작성할 수 있다.

```java
public class PhysicalConstants {
  private PhysicalContstants() { } // 인스턴스화 방지
  
  public static final double AVOGADROS_NUMBER = 6.022_140_857e23;
  public static final double BOLTZMANN_CONST = 1.380_648_52e-23;
}
```
혹은 열거 타입으로 작성할 수 있다.

```java
    public enum PhysicalConstants {
        AVOGADROS_NUMBER(6.022_140_857e23), 
        BOLTZMANN_CONST(1.380_648_52e-23);

        private final double number;

        PhysicalConstants (double num) {
            this.number = num;
        }

        public double getConstant() { return number; }
    }
```
