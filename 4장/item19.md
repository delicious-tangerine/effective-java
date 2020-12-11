# Item 19 : 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라

#### 1. 상속을 고려한 설계와 문서화란?

> 상속용 클래스
> : 상속용 클래스는 재정의할 수 있는 메소드들을 내부적으로 어떻게 이용하는지 문서로 남겨야 한다

- 호출의 순서 기입
- 각각의 **호출 결과가 이어지는 처리**에 어떤 영향을 주는지도 담아야한다
- 재정의 가능 메서드를 **호출할 수 있는 모든 상황**을 문서로 남겨야한다
  - ex) 백그라운드 스레드나 정적초기화 과정에서도 호출이  일어날 수 있다.

> Implementations Requirements
> : API 문서의 메서드 설명 끝에 해당 *메서드의 내부 동작 방식*을 설명하는 곳
> - **@implSpec** 태그 : javadoc으로 api 생성시 "Implementaion Requirements:"로 대체
> ```java
> javadoc -tag "implSpec:a:Implementation Requirements:" 
> ```

<br/>

..훅(hook)을 선별하여 protected 메서드 형태로 공개 >> 뭔말인지 잘 모르겠습니다.

#### 2. 상속용 클래스의 생성자는 재정의 가능 메소드를 호출해서는 안 된다.

- **상위 클래스의 생성자**가 하위 클래스의 생성자보다 **먼저 실행**됨
- 하위 클래스에서 **재정의한 메서드**가 하위 클래스의 **생성자**보다 먼**저 호출**됨
- 재정의한 메서드가 하위 클래스의 **생성자에서 초기화하는 값에 의존** 시, **오작동**

```java
public class Super {
    public Super() {
        overrideMe(); //1.생성자가 재정의 가능 메서드 호출, 제일 먼저 실행됨
    }

    public void overrideMe() {
        System.out.println("super method");
    }
}

public class Sub extends Super{

    private String str;
    public Sub() {
        str = "Sub String"; //3. 근데 재정의한 메서드가 생성자의 초기화하는 값에 의존
    }

    @Override
    public void overrideMe() {
        // 2. 재정의한 메서드, 상위 클래스의 생성자가 호출하므로 Sub()생성자보다 먼저 실행됨
        System.out.println(str);
    }

    public static void main(String[] args) {
        Sub sub = new Sub();
    }
}
```

> 위 코드의 결과
> :  String 값이 초기화가 되기도전에 접근하여 null이 출력이 된다
> Cloneable의 clone과 Serializable의 readObject에서도 발생

- 효율적인 하위 클래스를 큰 어려움 없이 만들 수 있게 하려면 



-----

<br/>

##### 🍊 QUIZ 

1. 문서화 한것은 그 클래스가 쓰이는 한 반드시 지켜야한다. [ O | X ]
2. 상속을 금지하려면 클래스를 [　　　　](으)로 선언하거나 생성자 모두를 외부에서 접근할 수 없도록 만든다.
