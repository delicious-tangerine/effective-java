## Item 12 : toString을 항상 재정의 하라


> 💡 **toString**의 규약 : 모든 하위 클래스에서 이 메서드를 재정의 하라.
> - 해당 객체가 가진 주요 정보 모두를 반환하는 게 좋다.
> - 스스로를 완벽히 설명하는 문자열

<br/>

#### 1. format을 명시했을 경우와 명시하지 않았을 경우

```java
/*
 * 전화번호의 문자열을 반환
 * "XXX-YYY-ZZZZ"의 12자리
 * 웅앵따리
 * 
 */
@Override
public String toString(){
    return String.format("%03d-%03d-%04d",
            areaCode, prefix, lineNum);
}
```
- 문서화 하는 것을 권한다 (Strandardization, Specification, Readable)
- 단점: 포맷에 얽매일 수도 있다. -> 수정시, 엉망이 될 수도 있다.

<br/>

#### 2. toString이 반환한 값에 포함된 정보를 얻어올 수 있는 API 제시

- 즉, 클래스 멤버변수의 접근자를 제공해야한다.
- 접근자가 없을시, 프로그래머는 toString의 반환값을 파싱할 수밖에 없다.


> ⚡️ **[toString 재정의가 필요한 경우]**
> 추상클래스에서 하위클래스들이 공유해야 할 문자열 표현이 있을때
> 
> ⚡️ **[toString 재정의가 필요없는 경우]**
> 정적 유틸리티 클래스, 대부분의 열거타입 => Java가 이미 완벽한 toString제공

<br/>

-----

<br/>

##### 🍊 QUIZ 

1. 구체 클래스에서 Object의 toString을 재정의는 필요하다 [O | X]
2. 상위클래스에서 이미 알맞게 정의한 경우에도 재정의가 필요하다 [O | X]
3. toString을 재정의한 클래스는 해당 클래스의 [　　　　]이(가) 편리해진다



