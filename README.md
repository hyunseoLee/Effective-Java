# Effective-Java 
이펙티브 자바 책 한권 끝내기 📚

 <img width="159" alt="스크린샷 2022-05-02 오후 6 36 36" src="https://user-images.githubusercontent.com/33533199/166214602-e95279b5-f239-4af3-8b53-20503c0cfa93.png">


<!-- TOC -->
# [목차](#목차)

### 📌 [2장. 객체 생성과 파괴](#2장-객체-생성과-파괴)
  - [아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라](#아이템1-생성자-대신-정적-팩터리-메서드를-고려하라) 
  - [아이템 2. 생성자에 매개변수가 많다면 빌더를 고려하라](#아이템2-생성자에-매개변수가-많다면-빌더를-고려하라)
  - [아이템 3. PRIVATE 생성자나 열거 타입으로 싱글턴임을 보증하라](#아이템3-private생성자나-열거-타입으로-싱글턴임을-보증하라)
  - [아이템 4. 인스턴스화를 막으려거든 PRIVATE 생성자를 사용하라](#아이템4-인스턴스화를-막으려거든-private-생성자를-사용하라)
  - 아이템 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라 
  - 아이템 6. 불필요한 객체 생성을 피하라 
  - 아이템 7. 다 쓴 객체 참조를 해제하라 
  - 아이템 8. FINALIZER와 CLEANER 사용을 피하라 
  - 아이템 9. TRY-FINALLY보다는 TRY-WITH-RESOURCES를 사용하라 

### 📌 3장. 모든 객체의 공통 메서드  
  - 아이템 10. EQUALS는 일반 규약을 지켜 재정의하라 
  - 아이템 11. EQUALS를 재정의하려거든 HASHCODE도 재정의하라 
  - 아이템 12. TOSTRING을 항상 재정의하라 
  - 아이템 13. CLONE 재정의는 주의해서 진행하라 
  - 아이템 14. COMPARABLE을 구현할지 고려하라 


### 📌 5장. 제네릭 
  - 아이템 26. 로 타입은 사용하지 말라 
  - 아이템 27. 비검사 경고를 제거하라 
  - 아이템 28. 배열보다는 리스트를 사용하라 
  - 아이템 29. 이왕이면 제네릭 타입으로 만들라 
  - 아이템 30. 이왕이면 제네릭 메서드로 만들라 
  - 아이템 31. 한정적 와일드카드를 사용해 API 유연성을 높이라 
  - 아이템 32. 제네릭과 가변인수를 함께 쓸 때는 신중하라 
  - 아이템 33. 타입 안전 이종 컨테이너를 고려하라 

### 📌 6장. 열거 타입과 애너테이션 
  - 아이템 34. INT 상수 대신 열거 타입을 사용하라 
  - 아이템 35. ORDINAL 메서드 대신 인스턴스 필드를 사용하라 
  - 아이템 36. 비트 필드 대신 ENUMSET을 사용하라 
  - 아이템 37. ORDINAL 인덱싱 대신 ENUMMAP을 사용하라 
  - 아이템 38. 확장할 수 있는 열거 타입이 필요하면 인터페이스를 사용하라 
  - 아이템 39. 명명 패턴보다 애너테이션을 사용하라 
  - 아이템 40. @OVERRIDE 애너테이션을 일관되게 사용하라 
  - 아이템 41. 정의하려는 것이 타입이라면 마커 인터페이스를 사용하라 

### 📌 7장. 람다와 스트림 
  - 아이템 42. 익명 클래스보다는 람다를 사용하라 
  - 아이템 43. 람다보다는 메서드 참조를 사용하라 
  - 아이템 44. 표준 함수형 인터페이스를 사용하라 
  - 아이템 45. 스트림은 주의해서 사용하라 
  - 아이템 46. 스트림에서는 부작용 없는 함수를 사용하라 
  - 아이템 47. 반환 타입으로는 스트림보다 컬렉션이 낫다 
  - 아이템 48. 스트림 병렬화는 주의해서 적용하라 
  
 <!-- /TOC -->
-----------------------------------------------------------------
  
  # 2장. 객체 생성과 파괴
  ## 아이템1. 생성자 대신 정적 팩터리 메서드를 고려하라. 
  * 클래스는 클라이언트에 public 생성자 대신 (혹은 생성자와 함께) 정적 팩터리 메서드를 제공할 수 있다. 
  * 정적 팩터리 메서드 (static factory method) :  클래스의 인스턴스를 반환하는 단순한 정적 메서드 (리턴값이 그 클래스의 인스턴스값인 메서드) 
```java
// 정적 팩터리 메서드의 예시: 기본타입인 boolean 값을 받아 Boolean 객체 참조로 변환해준다 
public static Boolean valueOf(boolean b) {
  return b ? Boolean.TRUE : Boolean.FALSE; 
}
```

**팩터리 메서드가 생성자보다 좋은 점 (= 장점)**
1. 이름을 가질 수 있다.
  - 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하지 못한다. ex) BigInteger(int, int, Random) 
  - 정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.  ex) BigInteger.probablePrime 
  - 한 클래스에 시그니처가 같은 생성자가 여러 개 필요할 것 같으면, 생성자를 정적 메서드로 바꾸고 각각의 차이를 잘 드러내는 이름을 지어주자. 
2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.
  - 불변 클래스(아이템 17)는 인스턴스를 미리 만들어 놓거나 새로 샏성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다. 
  - 반복되는 요청에 같은 객체를 반환하는 정적 팩터리방식의 클래스를 인스턴스 통제 클래스라 한다. 인스턴스를 통제하면 클래스를 싱글턴으로 만들 수도 있고, 인스턴스화 불가(아이템 4)로 만들수 있다. 또한 불변 값 클래스(아이템 17)에서 동치인 인스턴스가 단 하나뿐임을 보장할 수 있다. 
3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다. 
 - 반환할 객체의 클래스를 자유롭게 선택할 수 있게 하는 '엄청난 유연성'
 - API를 만들때, 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다. 이는 인터페이스를 정적 팩터리 메서드의 반환타입으로 사용하는 인터페이스 기반 프레임워크(아이템 20)을 만드는 핵심 기술. 
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다. 
- 반환 타입의 하위타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다.
- ex.EnumSet 클래스(아이템 36) 
5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다. (?) 

**단점**
1. 상속을 하려면 public 이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다. 

> 정리 

    정적 팩토리 메서드와 public생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋다. 
    그렇다고 하더라도 정적 팩터리를 사용하는게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치자. 
    
---------------------------------------------------------------------------------------
  ## 아이템2. 생성자에 매개변수가 많다면 빌더를 고려하라
  
* 정적 팩터리와 생성자에 선택적 매개변수가 많을 때 
### 1. 점층적 생성자 패턴 (telescoping constructor pattern) 
```java
// 점층적 생성자 패턴 - 확장하기 어려움
public class NutritionFacts{
  private final int servingSize;  // (ml, 1회 제공량) 필수
  private final int servings;     // (회, 총 n회 제공량) 필수
  private final int calories;     // (1회 제공량당) 선택
  private final int fat;          // (g/1회 제공량) 선택
  private fianl int sodium;       // (mg/1회 제공량) 선택 
  private final int carbohydrate; //(g/1회 제공량) 선택 
  
  public NutritionFacts(int servingSize, int servings) {
    this(servingSize,servings,0);
  }
  public NutritionFacts(int servingSize, int servings,int calories) {
    this(servingSize,servings,calories,0);
  }
  public NutritionFacts(int servingSize, int servings,int calories,int fat) {
    this(servingSize,servings,calories,fat,0);
  }
  public NutritionFacts(int servingSize, int servings,int calories,int fat,int sodium) {
    this(servingSize,servings,calories,fat, sodium,0);
  }
   public NutritionFacts(int servingSize, int servings,int calories,int fat,int sodium,int carbohydrate) {
    this.servingSize = servingSize;
    this.servings= servings;
    this.calories= calories;
    this.fat= fat;
    this.soduim= sodium;
    this.carbohydrate= carbohydrate;
  }
}
```
* 사용자가 설정하길 원치 않는 매개변수까지 값을 지정해줘야한다. 
* 점층적 생성자 패턴도 사용할 수 있으나, 매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다. 


### 2. 자바빈즈 (JavaBeans pattern)  
* 매개벼수가 없는 생성자로 객체를 만든 후, 세터메서드를 호출해 매개변수의 값을 설정하는 방식  
```java
// 자바빈즈 패턴 : 일관성이 깨지고, 불변으로 만들 수 없다. 
public class NutritionFacts{
  // 매개변수들은 ( 기본값이 있다면 ) 기본값으로 초기화된다. 
  private int servingSize= -1;  // (ml, 1회 제공량) 필수. 기본값 없음.
  private int servings= -1;     // (회, 총 n회 제공량) 필수. 기본값 없음.
  private int calories= 0;     // (1회 제공량당) 선택
  private int fat= 0;          // (g/1회 제공량) 선택
  private int sodium= 0;       // (mg/1회 제공량) 선택 
  private int carbohydrate= 0; //(g/1회 제공량) 선택 
  
  public NutritionFacts() {}
  public void setServingSize(int val)   { servingSize= val; }
  public void setServings(int val)      { servings= val;}
  public void setCalories(int val)      { calories= val;} 
  public void setFat(int val)           { sodium= val;}
  public void setCarbohydrate(int val)  { calgohydrate = val;} 
 
}
```

```java
// 자바빈즈 패턴에서 객체 생성시
  NutritionFacts cocaCola= new NutritionFacts();
  cocaCola.setServingSize(240);
  cocaCola.setServings(8);
  cocaCola.setCalories(100);
  cocaCola.setSodium(35);
  cocaCola.setCarbohydrate(27);
  
```
* 객체 하나를 만들기 위해서 여러 개의 메서드를 호출해야 하고, 객체가 완전히 생성되기 전까지는 **일관성**이 무너진 상태에 놓이게 된다.
* 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며, 스레드 안정성을 얻으려면 프로그래머가 추가 작업을 해줘야만 한다. 


### 3. 빌더패턴(Builder pattern)  : 점층적 생성자 패턴의 안정성 + 자바빈즈 패턴의 가독성 
* 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻는다. 
* 그런 다음 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 매개변수를 설정한다.
* 마지막으로 매개변수가 없는 build메서드를 호출해 우리에게 필요한 객체를 얻는다. 

```java
// 빌즈패턴 - 점층적 생성자 패턴과 자바빈즈 패턴의 장점만 취했다. 
public class NutritionFacts{
  private final int servingSize;  // (ml, 1회 제공량) 필수
  private final int servings;     // (회, 총 n회 제공량) 필수
  private final int calories;     // (1회 제공량당) 선택
  private final int fat;          // (g/1회 제공량) 선택
  private fianl int sodium;       // (mg/1회 제공량) 선택 
  private final int carbohydrate; //(g/1회 제공량) 선택 

  // 빌더는 생성할 클래스 안에 정적 멤버 클래스로 만들어두는게 보통이다. 
  public static class Builder{
  
      //필수 매개변수 
      private final int servingSize;      // (ml, 1회 제공량) 필수
      private final int servings;         // (회, 총 n회 제공량) 필수
      
      //선택 매개변수 - 기본값으로 초기화 한다. 
      private final int calories = 0;     // (1회 제공량당) 선택
      private final int fat= 0;           // (g/1회 제공량) 선택
      private fianl int sodium= 0;        // (mg/1회 제공량) 선택 
      private final int carbohydrate= 0;  //(g/1회 제공량) 선택 

    public Builder(int servingSize, int servings) {
      this.servingSize= servingSize;
      this.servings= servings; 
    }
    public Builder calories(int val)    { calories= val; return this; }
    public Builder fat(int val)         { fat= val; return this;}
    public Builder sodium(int val)      { sodium= val; return this;}
    public Builder carbohydrate(int val){ carbohydrate= val; return this;}
    public NutritionFacts build()    { return new NutritionFacts(this); } 
 }
    
  private NutritionFacts(Builder builder){
    servingSize = builder.servingSize;
    servings= builder.servings;
    calories= builder.calories;
    fat= builder.fat;
    soduim= builder.sodium;
    carbohydrate= builder.carbohydrate;
    
  }
}
```
* 해당 클래스는 불변이며, 모든 매개변수의 기본값들을 한곳에 모아뒀다.
* 빌더의 세터 메서드들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출할 수 있다 : **플루언트API** 또는 **메서드연쇄** 라 한다.

```java
NutritionFacts cocaCola= new NutritionFacts.Builder(240,8).calories(100).sodium(35).carbohydrate(27).build(); 
``` 

* 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기에 좋다. 
  * 하위클래스 메서드는 상위클래스 메서드가 정의한 반환 타입이 아닌, 그 하위 타입을 반환하는 기능을 사용할 수 있다 : 공변 반환 타이핑 기능
* 빌더 패턴은 상당히 유연해서, 하나의 빌더로 여러 객체를 순회하면서 만들 수 있고, 빌더에 넘기는 매개변수에 따라 다른 객체를 만들 수도 있다.

#### 빌더 패턴의 단점
* 객체를 만들려면, 빌더부터 만들어야 한다.
* 생성 비용이 크지는 않지만, 성능이 민감한 상황에서는 문제가 될 수 있다.
* 매개변수가 4개 이상은 되어야 값어치를 한다.

>정리

    생성자나 정적팩터리가 처리해야할 매개변수가 많다면(4개이상) 빌더 패턴을 선택하는게 더 낫다. 매개변수 중 다수가 필수가 아니라 같은 타입이면 특히 더 그렇다.
    빌더는 점층적생성자 보다 클라이언트 코드를 읽고 쓰기가 훨씬 간결하고, 자바빈즈보다 훨씬 안전하다.
    
----------------------------------------------------------------------------------------------------------------------------

## 아이템3. private생성자나 열거 타입으로 싱글턴임을 보증하라 
* 싱글턴(Singleton) : 인스턴스를 오직 하나만 생성할 수 있는 클래스
  + 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다. 
  + 싱글턴을 만드는 방식은 세가지가 있다. 
  
### 1. public static final 필드 방식의 싱글턴 
```java
  public class Elvis{
    public static final Elvis INSTACNE = new Elvis();
    private Elvis(){...}
    
    public void leaveTheBuilding(){...}
  } 
```
- private 생성자는 pulic static final 필드인 Elvis.INSTANCE를 초기화할 때 딱 한번만 호출된다.
- public 이나 protected 생성자가 없으므로 Elvis 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐임이 보장된다.
- 장점 
  - 클래스가 싱글턴임이 API에 명백히 드러난다. public static 필드가 final이니 절대로 다른 객체를 참조할 수 없다.
  - 간결함. 

### 2. 정적 팩터리 메서드를 pulic static 멤버로 제공한다.
```java
  public class Elvis{
    private static final Elvis INSTANCE= new Elvis();
    private Elvis(){...}  
    public static Elvis getInstance(){ return INSTANCE; }
    
    public void leaveTheBuilding(){...}
    
  }
```

*   Elvis.getInstance는 항상 같은 객체의 참조를 반환하므로 제2의 Elvis인스턴스란 결코 만들어지지 않는다.
*   장점
  * 마음이 바뀌면 API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다. 
  * 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다 (아이템 30) 
  * 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다. 
    * ex. Elvis::getInstance를 Supplier<Elvis>로 사용하는 식이다 ( 아이템43 , 44)
  
* 1번과 2번으로 만든 싱글턴 클래스를 직렬호 하기위해서는 단순히 Serializable을 구현한다고 선언하는 것만으로 부족하다.
* 모든 인스턴스 필드를 일시적이라고 선언하고 readResolve 메서드를 제공해야 한다 ( 아이템 89)

  
```java
  //싱글텀임을 보장해주는 readResolve 메서드 
  private Object readResolve(){
  //'진짜' Elvis를 반환하고, 가짜 Elvis는 가비지 컬렉터에 맡긴다. 
    return INSTANCE; 
  }
```
  
### 3. 원소가 하나인 열거타입을 선언하는 것
  
```java
  public enum Elvis{
    INSTANCE;

    public void leaveTheBuilding(){...}
    
  }
```
  
* public 필드 방식과 비슷하지만, 더 간결하고, 추가노력 없이 직렬화 할 수 있다.
* 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2 인스턴스가 생기는 일을 완벽히 막아준다.
* 대부분의 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다. 

 ----------------------------------------------------------------------------------------------------------------------------

 
 ## 아이템4. 인스턴스화를 막으려거든 private 생성자를 사용하라 
* 정적 멤버만 담은 유틸리티 클래스는 인스턴스로 만들어 쓰려고 설계한 게 아니다. 
* 하지만 생성자를 명시하지 않으면 컴파일러가 자동으로 기본 생성자를 만들어준다. 

* 추상 클래스로 만드는 것으로는 인스턴스화를 막을 수 없다. 
* 컴파일러가 기본 생성자를 만드는 경우는 오직 명시된 생성자가 없을 뿐이니 private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있다.


```java
//인스턴스를 만들 수 없는 유틸리티 클래스 
  public class UtilityClass{
    //기본 생성자가 만들어지는 것을 막는다 (인스턴스화 방지용)
    private UtilityClass(){
      throw new AssertionError();
      }
  } 
```

- 명시적 생성자가 private이니 클래스 바깥에서는 접근할 수 없다.
- 꼭 AssertionError를 던질 필요는 없지만, 클래스 안에서 실수로라도 생성자를 호출하지 않도록 해준다.
- 생성자가 존재하는데 호출할 수 없다니 직관적이지 않으니 혹시 모르니 위의 코드처럼 적절한 주석을 달아둔다.
- 위의 방식은 상속을 불가능하게 하는 효과도 있다. 하위클래스가 상위클래스의 생성자에 접근할 길이 막혀버리기 때문. 
 ----------------------------------------------------------------------------------------------------------------------------

 
