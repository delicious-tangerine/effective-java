# Item 61 : 박싱된 기본 타입보다는 기본 타입을 사용하라

> 💡 
> 기본타입 : ```int```, ```double```, ```boolean```
> 참조타입 :  ```String```, ```List```
> > 박싱된 기본타입 : ```Int```, ```Double```, ```Boolean```

#### 1. 기본타입의 차이점

1. 기본타입은 값만 가지고 있으나, 박싱된 기본 타입은 값에 더해 ```식별성(identity)```을 가짐
   - 박싱된 기본타입의 두 인스턴스는 서로 식별 가능
2. 기본타입의 값은 언제나 유효하나, 박싱된 기본타입은 유효하지 않은 값(```null```)을 가질 수 있다
3. 기본타입이 박싱된 기본타입보다 시간과 메모리 사용면에서 더 효율적

#### 2. 박싱된 기본자에 == 연산자를 사용한다면 ?? 

1. 객체의 식별성
```java
Comparator<Integer> naturalOrder =
    (i, j) -> (i < j) ? -1 : (i == j ? 0 : 1);
naturalOrder.compare(new Integer(42), new Integer(42)); // 객체의 식별성으로 인해, 0이 아닌 1 출력

/**********************************************************************/

Comparator<Integer> naturalOrder = (iBoxed, jBoxed) -> {
            int i = iBoxed, j = jBoxed; // 오토박싱
            return i < j ? -1 : (i == j ? 0 : 1);
        };


```
<br/>

2. NullPointerException

```java
static Integer i;//그냥 int로 선언해주면 해결될 문제...

    public static void main(String[] args) {
        if (i == 42)
            System.out.println("믿을 수 없군!");
    }
```

- 여기서 ```i == 42```는 박싱된 기본타입(```Integer```)와 기본타입(```int```)를 비교하는 것
- 이 두 타입을 혼용한 연산에서는 박싱된 기본 타입의 **박싱이 자동으로 풀린다.**

<br/>


#### 3. 넘 오래걸리는 시간과 불필요한 메모리 사용

```java
Long sum = 0L;
for (long i = 0; i<= Integer.MAX_VALUE; i++>){
    sum += i; //여기서 박싱과 언박싱이 반복해서 일어나 성능 하락
}
```


#### 4. 박싱된 기본 타입은 언제 써야하는가?
1. 컬렉션의 원소, 키, 값
2. 리플렉션 아이템을 통해 메서드를 호출할때