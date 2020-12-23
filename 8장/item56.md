# Item 56 : 공개된 API 요소에는 항상 문서화 주석을 작성하라

> 💡 API 문서화시 모든 클래스, 인터페이스, 메서드, 필드 선언에 주석을 달아야한다.

- 각 문서화 주석의 첫 번쨰 문장은 해당 요소의 요약설명 ```summary```이여야한다.\
- java 10부터 ```{@summary}``` 라는 요약 설명 전용 태그가 추가되었다.
- 스레드 안전 수준을 반드시 포함



#### 1. 메서드용 문서화 주석
: how가 아닌 what을 기술

```java

/**
 * 이 리스트에서 지정한 위치의 원소를 반환한다.
 *
 * <p>이 메서드는 상수 시간에 수행됨을 보장하지 <i>않는다</i> 구현에 따라
 * 원소의 위치에 비례해 시간이 걸릴 수도 있다.
 *
 * @param  index 반환할 원소의 인덱스; 0 이상이고 리스트 크기보다 작아야 한다.
 * @return 이 리스트에서 지정한 위치의 원소
 * @throws IndexOutOfBoundsException index가 범위를 벗어나면,
 * 즉, ({@code index < 0 || index >= this.size()})이면 발생한다.
 */
 E get(int index) {
     return null;
 }
```
1. 
2. 마침표를 붙이지 않는 태그 : ```@param``` ```@return``` ```@throws```
3. ```@param``` 태그 : 해당 매개변수가 뜻하는 값을 설명하는 명사구 기술
4. ```@return``` 태그 : 반환값을 설명하는 명사구 기술
5. ```@throws``` 태그 : 비검사 예외를 이 태그로 선언하여 전제조건 모두 기술
6. 자바독 유틸리티에서는 주석을 HTML로 변환하므로 태그 사용
7. ```{@code}```를 통해 코드용 폰트로 렌더링 + 태그로 감싼 내용에 HTML 메타문자 및 자바독 태그 무시

<br/>

####2. 상속용 클래스의 문서화

```java
/**
 * 이 컬렉션이 비었다면 true를 반환한다.
 *
 * @implSpec 이 구현은 {@code this.size() == 0}의 결과를 반환한다.
 * {@literal size < 0} 은 성립하지 않는다
 *
 * @return 이 컬렉션이 비었다면 true, 그렇지 않으면 false
 */
public boolean isEmpty() {
    return false;
}
```
1. ```@implSpec``` 태그 : 해당 메서드와 하위 클래스 사이의 계약을 설명하고, 메서드를 재정의 하는 방법을 알린다.
2. 자바독 명령줄에서 ```-tag "implSpec:a:Implementation Requirements:"``` 스위치를 켜줘야  ```@implSpec```사용가능
3. ```{@literal}```태그로 HTML메타문자나 온점을 무시 (코드폰트로 렌더링하지 않음)  

<br/>

#### 3. 열거타입 문서화 주석
: 상수들에도 주석을 달자

```java
/**
 * 심포니 오케스트라의 악기 세션.
 */
public enum OrchestraSection {
    /** 플루트, 클라리넷, 오보 같은 목관악기. */
    WOODWIND,

    /** 프렌치 호른, 트럼펫 같은 금관악기. */
    BRASS,

    /** 탐파니, 심벌즈 같은 타악기. */
    PERCUSSION,

    /** 바이올린, 첼로 같은 현악기. */
    STRING;
}
```
