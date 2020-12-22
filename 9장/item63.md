# Item 63 : 문자열 연결은 느리니 주의하라

> 💡 문자열 연결 연산자(+)로 문자열 n개를 잇는 시간은 n^2에 비례한다.
> > 왜냐규? **불변**이기때문...!

1. 성능을 포기하고 싶지 않을 경우 StringBuilder 사용

```java
StringBuilder sb = new StringBuilder();
sb.append("웅앵다릐");
```
