 ## 아이템36. 비트 필드 대신 EnumSet을 사용하라 
 
 서로 다른 2의 거듭제곱 값을 할당한 정수 열거 패턴이다.
 아래와 같은 식으로 비트별 OR을 사용해 여러 상수를 하나의 집합으로 모을 수 있으며, 이런 집합을 **삐트 필드**라 한다. 
 ```java
 public class Text{
  public static final int STYLE_BOLD = 1 << 0 ; //1
  public static final int STYLE_ITALIC = 1 << 1 ; //2
  public static final int STYLE_UNDERLINE = 1 << 2; //4
  public static final int STYLE_STRIKETHROUGH = 1 << 3; //8
 
  // 매개변수 styles는 0개 이상의 STYLE_ 상수를 비트별 OR한 값이다.
  public void applyStyles(int styles) { ... } 
 }
```
 
 - 비트 필드를 사용하면 비트별 연산을 사용해 합집합,교집합 같은 집합 연산을 효율적으로 수행할 수 있지만,
 정수 열거 상수의 단점을 그대로 지닌다.
 - 또한, 비트 필드값이 그대로 출력되면 단순한 정수 열거 상수를 출력할 때보다 해석하기 훨씬 어렵다.
 - 최대 몇 비트가 필요한지를 API 작성 시 미리 예측하여 적절한 타입을 선택해야 한다. 
 
 #### 해결책  : EnumSet 클래스
 - EnumSet 클래스는 열거 타입 상수의 값으로 구성된 집합을 효과적으로 표현해준다.
 
 ```java
 public class Text{
  public enum Style {BOLD, ITALIC, UNDERLINE, STRIKETHROUGH }
 
 // 어떤 Set을 넘겨도 되나, EnumSet이 가장 좋다.
 public void applyStyles(Set<Style> styles) { ... }
 }
 ```
 
 > 정리

  열거할 수 있는 타입을 한데 모아 집합 형태로 사용한다고 해도 비트 필드를 사용할 이유는 없다. 
  EnumSet클래스가 비트 필드 수준의 명료함과 성능을 제공하고 아이템34에서 설명한 열거 타입의 장점까지 선사하기 때문이다.
  EnumSet 의 유일한 단점이라면 불변 EnumSet을 만들 수 없는 것이다. 
