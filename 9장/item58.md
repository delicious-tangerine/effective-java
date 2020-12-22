# Item 58 : 전통적인 for문보다는 for-each문을 사용하라

> 💡 전통적인 for문에서 위험요소를 줄이기 위해 반복자와 인덱스 변수를 사용하지 않는 for-each문을 사용하자 <br/>
>  또한, 하나의 관용구로 컬렉션과 배열 모두 처리 가능하다.


#### 컬렉션을 중첩해서 사용할때 for-each문의 장점

```java
public class DiceRolls {
    enum Face { ONE, TWO, THREE, FOUR, FIVE, SIX }

    public static void main(String[] args) {
        // 36개 조합이 아닌 6조합만 나오고 끝내버린다.
        Collection<Face> faces = EnumSet.allOf(Face.class);

        for (Iterator<Face> i = faces.iterator(); i.hasNext(); )
            //이곳에 Suit suit = i.next();를 넣으면 되지만...
            for (Iterator<Face> j = faces.iterator(); j.hasNext(); )
                System.out.println(i.next() + " " + j.next());

/********************************************************************/

        // 컬렉션이나 배열의 중첩 반복을 위한 권장 관용구 (훨씬 깔끔)
        for (Face f1 : faces)
            for (Face f2 : faces)
                System.out.println(f1 + " " + f2);
    }
}
```

<br/>

#### for-each문을 사용할 수 없는 경우

1. 파괴적인 필터링(destructive filtering) - 컬렉션 순회시 원소를 제거해야한다면 반복자의 remove 호출
   - But, java8부터 removeIf라는 메서드를 사용 가능
   ```java
    List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9));
    for (Integer i : numbers) {
        if(i % 3 == 0) {
            numbers.remove(i);
        }
    }
    /******************************************/
    numbers.removeIf(n -> (n % 3 == 0));
   ```
2. 변형(ransforming) - 순회하면서 리스트/배열 원소 값을 교체해야할때 반복자나 인덱스 사용
3. 병렬반복(parallel iteration) - 여러 컬렉션을 병렬로 순회해야 할 시, 반복자와 인덱스로 엄격하고 명시적으로 제어