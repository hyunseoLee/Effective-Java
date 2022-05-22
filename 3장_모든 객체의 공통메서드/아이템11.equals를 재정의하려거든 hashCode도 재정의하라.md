## 아이템11. equals를 재정의하려거든 hashCode도 재정의하라

*  equals를 재정의한 클래스 모두에서 hashCode도 재정의해야 한다.

Object 명세서 규약
> * equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 hashCode메서드는 몇번을 호출해도 일관된 값을 반환해야한다.
> * **equals(Object)가 두 객체를 같다고 판단했다면, hashCode도 똑같은 값을 반환해야 한다.**
> * equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 

* 명세서 규약 2번째를 보면 논리적으로 같은 객체는 같은 해시코드를 반환해야 하기 때문에, equals재정의했다면 hashCode도 재정의해야 한다! 


### hashCode를 작성하는 간단한 요령
1. Int 변수 result를 선언한 후 값 c로 초기화한다. 이때 c는 해당 객체의 첫번째 핵심필드를 단계 2.a 방식으로 계산한 해시코드다.
    * 여기서 말하는 핵심필드란 equals 비교에 사용되는 필드를 말한다.
2. 해당 객체의 나머지 핵심필드 f 각각에 대해 다음작업을 수행한다.
  * 해당 필드의 해시코드 c를 계산한다.
  * 계산한 해시코드 c로 result를 갱신한다.
    * result = 31* result + c;  ( 31인 이유는 홀수이면서 소수이기 때문에, ..) 
3. result를 반환한다.

```java
// 전형적인 hashCode 메서드 
@Override public int hashCode(){
  int result= Short.hashCode(areaCode);
  result = 31 * result + Short.hashCode(areaCode);
  result = 31 * ressult + Short.hashCode(lineNum);
  return result;
}
```

* hashCode 구현 후, 동치인 인스턴스에 대해 똑같은 해시코드를 반환할지 자문해보자.

* Objects 클래스는 임의의 개수만큼 객체를 받아 해시코드를 계산해주는 정적 메서드인 hash를 제공한다.
* 하지만, 아쉽게도 속도가 느리기 때문에 hash메서드는 성능에 민감하지 않은 상황에서만 사용하자.

```java
// 전형적인 hashCode 메서드 
@Override public int hashCode(){
   return Objects.hash(lineNum,prefix, areaCode); 
}
```

* 클래스가 불변이고 해시코드를 계산하는 비용이 크다면, 캐싱하는 방식을 고려해야 한다.
* 이 타입의 객체가 해시의 키로 사용되지 않는 경우라면 hashCode가 처음불릴때 계산하는 지연초기 전략 고려 

```java
// 해시코드를 지연 초기화하는 hashCode 메서드 - 스레드 안정성까지 고려해야 한다. 
private int hashCodel // 자동으로 0으로 초기화된다. 

@Override public int hashCode(){
  int result= hashCode;
  if(result==0){
    result= Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(areaCode);
    result = 31 * ressult + Short.hashCode(lineNum);
    hashCode= result;
   }
    return result;
}
```

* 성능을 높인다고 해시코드를 계산할 때 핵심코드를 생략해서는 안된다.
* hashCode가 반환하는 값의 생성규칙을 API 사용자에게 자세히 공표하지 말자.
  * 그래야 클라이언트가 이 값에 의지하지 않게 되고, 추후에 계산방식을 바꿀 수도 있다.

>정리
  
    equals를 재정의할 때는 hashCode도 반드시 재정의해야 한다. 그렇지 않으면 프로그램이 제대로 동작하지 않을 것이다.
    재정의한 hashCode는 Object의 API 문서에 기술된 일반 규약을 따라야 하며, 서로 다른 인스턴스라면 되도록 해시코드도 서로 다르게 구현해야 한다. 
