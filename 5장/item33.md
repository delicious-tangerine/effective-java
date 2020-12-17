# Item 33 : 타입 안전 이종 컨테이너(type safe heterogeneous container pattern) 고려하라

> 💡 한 컨테이너가 다룰 수 있는 타입 매개변수의 수는 고정되어 있다.
> 
> → 따라서 컨테이너 자체가 아닌 키를 타입 매개변수로 바꾼다!

### Type Token
class 리터럴의 타입은 Class<T>다.

즉, String.class의 타입은 ```Class<String>```이고 Integer.class의 타입은 ```Class<Integer>```이다.

이처럼 컴파일타임 타입 정보와 런타임 타입 정보를 위해 사용하는 class 리터럴을 타입 토큰(Type Token)이라고 한다.

### 구현이 모든 걸 말해준다.

```java
public class Favorites {
  private Map<Class<?>, Object> favorites = new HashMap<>();
  
  public <T> void putFavorite(Class<T> type, T instance) {
    favorites.put(Objects.requireNonNull(type), instance);
  }
  
  public <T> T getFavorite(Class<T> type) {
    // 형변환 연산자의 동적 버전 == cast
    // 맞으면 반환, 틀리면 ClassCastException
    return type.cast(favorites.get(type));
  }
}
```

```java
public static void main(String[] args) {
  Favorites f = new Favorites();
  
  f.putFavorite(String.class, "Java");
  f.putFavorite(Integer.class, 0xcafebabe);
  f.putFavorite(Class.class, Favorites.class);
  
  String favoriteString = f.getFavorite(String.class);
  int favoriteInterger = f.getFavorite(Integer.class);
  Class<?> favoriteClass = f.getFavorite(Class.class);
  
  System.out.printf("%s %x %s%n", favoriteString, favoriteInteger, favoriteClass.getName());
  // 출력 결과 : Java cafebabe Favorites
}
```

### 제한
1. Client가 raw type으로 Class Object를 넘기면 타입 안전성이 깨진다.

해결하는 방법은 다음과 같이 동적 형변환을 하자.

```java
public <T> void putFavorite(Class<T> type, T instance) {
  favorite.put(Objects.requireNonNull(type), type.cast(instance));
}
```

2. 실체화 불가 타입에는 사용할 수 없다.

List<String>을 저장하려는 코드는 컴파일 되지 않는다. => List<String>용 Class 객체가 없기 때문에.

따라서 이에 대한 만족스러운 우회로는 없다.

### 한정적 타입 토큰?
asSubclass 메소드를 사용하자.

```java
public <T extends Annotation> T getAnnotation(Class<T> annotationType)

// Class<?> 타입의 객체가 있고, 한정적 타입 토큰을 받는 메서드에 넘기려면, asSubclass를 쓰자.
// 객체를 Class<? extends Annotation>으로 형변환하지 않고, 동적으로 변경한다.
// 실패하면 ClassCastException을 반환한다.
static Annotation getAnnotation(AnotatedElement element, String annotationTypeName) {
  Class<?> annoationType = null;
  try {
    annoationType = Class.forName(annotationTypeName);
  } catch (Exception ex) {
    throw new IllegalArgumentException(ex);
  }
  return element.getAnnotation(annotationType.asSubclass(Annotation.class));
}
```
