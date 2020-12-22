# Item 65 : 리플렉션보다는 인터페이스를 사용하라

> 💡 리플렉션은 아주 제한된 형태로 사용해야 단점을 피하고 이점을 취할 수 있다.

<br/>

* **리플렉션 기능 사용시**
1. 클래스의 생성자, 메서드, 필드 인스턴스 가져올 수 있음
2. 이를 이용해 각 인스턴스에 연결된 실제 생성자, 메서드, 필드 조작가능

* **단점**
1. 컴파일타입 검사가 주는 이점을 누릴 수 없음
2. 코드가 더러워지고 장황해짐
3. 성능이 떨어짐


#### 1. 리플렉션은 인스턴스 생성에만 사용

이렇게 만든 인스턴스는 상위 클래스나 인터페이스를 참조하여 사용

```java
package com.github.sejoung.codetest.general;


import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.util.Arrays;
import java.util.Set;

// 리플렉션으로 활용한 인스턴스화 데모
public class ReflectiveInstantiation {
  // 코드 65-1 리플렉션으로 생성하고 인터페이스로 참조해 활용한다. (372-373쪽)
  public static void main(String[] args) {


    // 클래스 이름을 Class 객체로 변환
    Class<? extends Set<String>> cl = null;
    try {
      cl = (Class<? extends Set<String>>)  // 비검사 형변환!
          Class.forName("java.util.HashSet");
    } catch (ClassNotFoundException e) {
      fatalError("클래스를 찾을 수 없습니다.");
    }

    // 생성자를 얻는다.
    Constructor<? extends Set<String>> cons = null;
    try {
      cons = cl.getDeclaredConstructor();
    } catch (NoSuchMethodException e) {
      fatalError("매개변수 없는 생성자를 찾을 수 없습니다.");
    }

    // 집합의 인스턴스를 만든다.
    Set<String> s = null;
    try {
      s = cons.newInstance();
    } catch (IllegalAccessException e) {
      fatalError("생성자에 접근할 수 없습니다.");
    } catch (InstantiationException e) {
      fatalError("클래스를 인스턴스화할 수 없습니다.");
    } catch (InvocationTargetException e) {
      fatalError("생성자가 예외를 던졌습니다: " + e.getCause());
    } catch (ClassCastException e) {
      fatalError("Set을 구현하지 않은 클래스입니다.");
    }

    // 생성한 집합을 사용한다.
    s.addAll(Arrays.asList("a","b","c").subList(1, 3));
    System.out.println(s);
  }

  private static void fatalError(String msg) {
    System.err.println(msg);
    System.exit(1);
  }
}

```

단점
1. 런타임에 6가지 예외 던질 수 있음
2. 클래스 이름만으로 인스턴스 생성하기 위해 25줄 사용 