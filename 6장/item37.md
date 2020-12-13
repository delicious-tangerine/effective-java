# `아이템 37: ordinal 인덱싱 대신 EnumMap을 사용하라`

```Java
class Item {
    enum Type { T1, T2, T3 };

    ~~~
}
```


### 이따금 배열이나 리스트에서 원소를 꺼낼 때 ordinal 메소드로 인덱스를 얻는다.

```Java
Set<Item>[] items = new Set<>;
for (int i=0; i<items.length; i++) {
    itemList[i] = new Set<>;
}
for (Item i : allItem) {
    itemList[i.type.ordinal()].add(i);
}
```

- 안정성에 대한 보장이 없다.
- 레이블을 직접 달아야 한다.
- 그냥 안좋다. 쓰지 말자.

### `EnumMap을 사용해보자`

```Java
Map<Item.enum, Set<Item>> items = new EnumMap<>;
for (Item.type t : Item.Type.valules())
    items.put(t, new HashSet<>);
for (Item i : allItem) {
    items.get(i.type).add(i);
}
```

- 타입 안정성이 있다!
- 타입이 출력용 문자열을 제공 하니 레이블을 달 일도 없다!


### `Stream을 사용한 코드
```Java
Arrays.stream(allItem)
    .collect(groupingBy(i -> i.type, () -> new EnumMap<>(Type.class), toSet()));
```

- 위의 코드보다는 좀 더 최적화가 되었다.<br>
=>  없는 Item Type에 대해선 맵을 만들지 않는다.