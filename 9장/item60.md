# Item 60 : 정확한 답이 필요하다면 float와 double은 피하라

> 💡 ```float```과 ```double```은 과학과 공학 계산용이기 때문에 넓은 범위의 수를 빠르고 정밀하게 ```근사치```로 계산하도록 설계됨.
> ***특히 금융계산과 맞지 않다*** >> 음의 거듭수를 표현 못하기 때문

<br/>

#### ```float```이나 ```double```을 사용하면 잔돈으로 3.99999999..달러가 나오는 문제 발생
: 금융계산시 BigDecimal, int, long을 사용하자.

1. 금융계산시 ```BigDecimal```을 사용하는 경우
- 코딩 시의 불편함이나 성능 저하를 신경 쓰지 않겠다면 BigDecimal을 사용

```java
final BigDecimal TEN_CENTS = new BigDecimal(".10");
BigDecimal money = new BigDecimal("1.00");
```

BigDecimal은 8가지의 반올림 모드를 제공하기 때문에 반올림을 완벽히 제어 할 수 있다. 법으로 정해진 반올림을 수행해야하는 비즈니스 계산에서는 굉장히 유용한 기능이다.

1. 금융계산시 ```int```나 ```long```을 사용하는 경우
- 성능이 중요하고, 소수점을 직접 추적할 수 있고 숫자가 너무 크지 않다면 int나 long을 사용
- 9자리 십진수 이하 >> ```int```
- 18자리 십진수 이하 >> ```long```
- 그 이상 >> ```BigDecimal```