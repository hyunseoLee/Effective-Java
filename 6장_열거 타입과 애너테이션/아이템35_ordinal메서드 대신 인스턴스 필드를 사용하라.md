## 아이템35. ordinal메서드 대신 인스턴스 필드를 사용하라 

열거 타입 상수는 열거 타입에서 몇 번째 위치인지를 반환하는 ordinal 메서드를 제공한다.
다만, 상수 선언의 순서가 바뀌거나 값의 중간이 없는 경우 ordinal을 쓰는 것은 좋지 않다.

해결책으로 열거 타입 상수에 연결된 값은 ordinal 메서드 대신, 인스턴스 필드에 저장하자. 

```java
// ordinal을 잘못 사용한 예 - 따라하지 말 것 
public enum Ensemble{
  SOlO, DUET, TRIO, OCTET,  NONET;
 
  public int numberOfMusicians() { return ordinal() + 1;  } 
} 

```


```java
public enum Ensemble{
  SOlO(1), DUET(2), TRIO(3), OCTET(8), DOUBLE_QUARTET(8), NONET(9);
  
  private final int numberOfMusicians;
  Ensemble(int size) { this.numberOfMusicians= size; }
  public int numberOfMusicians() { return numberOfMusicians; } 
} 

```

Enum API문서를 보면 ordinal 메서드는 EnumSet과 EnumMap 같이 열거 타입 기반의 범용 자료구조에 쓸 목적으로 설계되었다.
따라서 이런 용도가 아니라면 ordinal 메서드는 절대 사용하지 말자. 
