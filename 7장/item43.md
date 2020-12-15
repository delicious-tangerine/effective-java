# `아이템 43: 람다보다는 메서드 참조를 사용하라`


### `람다보다 더 간결하게 만드는 법 - 메소드 참조`
```Java
// 람다를 이용하면?
map.merge(key, 1, (count, incr) -> count + incr);

// 메소드 참조를 이용하면?
map.merge(key, 1, Integer::sum);
```

- **하지만 어떤 람다에서는 매개변수의 이름 자체가 프로그래머에게 좋은 가이드가 되기도 한다.**

- 람다로 할 수 없는 일이라면 메서드 참조로도 할 수 없다.

### `때론 람다가 더 간결할 때가 있다.`
```Java
service.execute(GoshThisClassNameIsHumongous::action);

service.execute(() -> action());
```

**메서드 참조 쪽이 짧고 명확하다면 메서드 참조를 쓰고, 그렇지 않을 때만 람다를 사용하라**