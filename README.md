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
  - [아이템 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라](#아이템5-자원을-직접-명시하지-말고-의존-객체-주입을-사용하라)
  - [아이템 6. 불필요한 객체 생성을 피하라](#아이템6-불필요한-객체-생성을-피하라)
  - [아이템 7. 다 쓴 객체 참조를 해제하라](#아이템7-다-쓴-객체-참조를-해제하라)
  - [아이템 8. FINALIZER와 CLEANER 사용을 피하라](#아이템8-finalizer와-cleaner-사용을-피하라)
  - [아이템 9. TRY-FINALLY보다는 TRY-WITH-RESOURCES를 사용하라](#아이템9-try-finally보다는-try-with-resources를-사용하라)

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

 
  ## 아이템5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라. 
- 사용하는 자원에 따라 동작이 달라지는 클래스에는 **정적 유틸리티 클래스** 나 **싱글턴 방식** 이 적합하지 않다.
- 그 이유는 우연하지 않고 테스트하기가 어렵기 때문. 
- (= 많은 클래스가 하나 이상의 자원에 의존하는 클래스) 
- 대신 클래스가 여러 자원 인스턴스를 지원해야 하며, 클라이언트가 원하는 자원을 사용해야 한다.
- 인스턴스를 생성할 때 생성자에게 필요한 자원을 넘겨주는 방식을 사용하면 된다 => **"의존객체주입"**


```java
//맞춤법 검사기 클래스 
  public class SpellChecker{
    private final Lexicon dictionary;
    
    // 맞춤법 검사기의 경우, 언어별로 따로 있고 특수어휘용 사전 등 별도의 버전이 있기 때문에 
    // 아래과정을 통해 여러사전을 사용할 수 있게 된다. 
    public SpellChecker(Lexicon dictionary){
      this.dictionary = Objects.requireNonNull(dictionary);
    }
    
    public boolean isValid(String word){...}
  } 
```

- 의존객체 주입이 유연성과 테스트 용이성을 개선해주긴 하지만, 의존성이 수천개나 되는 프로젝트에서는 코드를 어지럽게 만들기도 한다. 
- 스프링(Spring), 대거(Dagger), 주스(Guice) 같은 의존 객체 주입 프레임워크를 사용하면 어지러운 코드를 정교하게 사용 가능


> 정리

    클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는것이 좋다.
    이 자원들을 클래스가 직접 만들게 해서도 안 된다. 대신 필요한 자원을 생성자에 넘겨주자. 
    의존 객체 주입이라 하는 이 기법은 클래스의 유연성, 재사용성, 테스트 용이성을 기막히게 개선해준다.
 
  ----------------------------------------------------------------------------------------------------------------------------
 ## 아이템6. 불필요한 객체 생성을 피하라.
* 똑같은 기능의 객체를 매번 생성하기보다는 객체 하나를 재사용하는 편이 나을 때가 많다.
* 재사용은 빠르고 세련되다, 특히 불변객체는 언제든 재사용할 수 있다.

```java 
  // 하지 말아야 할 코드!
  // 실행될 때마다 String 인스턴스를 새로 만든다. 완전 쓸데없는 행위! 
  String s= new String("biki"); 
  
  //개선된 버전
  // 같은 가상머신 안에서 똑같은 문자열 리터럴을 사용하는 모든 코드가 같은 객체를 재사용함이 보장된다. 
  String s= "biki" 
```
* 생성자 대신 정적 팩터리 메서드(아이템 1)를 제공하는 불변 클래스에서는 정적 팩터리 메서드를 사용해 불필요한 객체 생성을 피할 수 있다.

```java
  Boolean(String) 생성자 보다는
  Boolean.valueOf(String) 팩터리 메서드를 사용하는 것이 좋다. 
``` 

* 생성자 메서드는 호출할 때마다 새로운 객체를 만들지만, 팩터리 메서드는 전혀 그렇지 않다.
* 생성 비용이 '비싼 객체'가 반복해서 피료하다면 캐싱하여 재사용하길 권장한다.
* 안타깝게도 자신이 만드는 객체가 비싼 객체인지를 매번 명확히 확인하기는 어렵다.

```java
//주어진 문자열이 유효한 로마숫자인지 확인하는 메서드 (정규식을 활용한 가장 쉬운 해법) 
  static boolean isRomanNumeral(String s){
    return s.match("^(?=.)M*(C[MD]|D?C{0,3})"
                + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
  }
```

* 위 방식의 문제점
* String.match 는 정규표현식으로 문자열 형태를 확인하는 가장 쉬운 방법이지만, 성능이 중요한 상황에서 반복해 사용하기엔 적합하지 않다.
* 정규표현식용 Pattern 인스턴스는 한번 쓰고 버려져서 곧바로 가비지 컬렉션 대상이 된다.
* Pattern은 입력받은 정규표현식에 해당하는 유한 상태 머신(final state machine)을 만들기 때문에 인스턴스 생성 비용이 높다.

* 성능을 개선하기 위해, (불변인) Pattern 인스턴스를 클래스초기화 (정적초기화) 과정에서 직접 생성해 캐싱해두고, 
* 나중에 isRomanNumeral 메서드가 호출될 때마다 인스턴스를 재사용한다.

```java
//값비싼 객체를 재사용해 성능을 개선한다.
public class RomanNumerals{
  private static final Pattern ROMAN = Pattern.compile(
    "^(?=.)M*(C[MD]|D?C{0,3})"
                + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
  static boolean isRomanNumeral(String s){
    return ROMAN.matcher(s).matches()l
  }
}
```

* 위 처럼 개선하면 isRomanNumeral이 빈번히 호출되는 상황에서 성능을 상당히 끌어올릴 수 있다.
  * 만약, 개선된 isRomanNumeral 방식의 클래스가 초기화된 후 이 메서드를 한번도 호출하지 않는다면 ROMAN 필드는 쓸데없이 초기화된 꼴이다.
* isRomanNumeral메서드가 처음 호출될 때 필드를 초기화하는 **지연초기화**(아이템 83)로 불필요한 초기화를 없앨 수는 있지만, 권하지는 않는다.
  * 지연초기화는 코드를 복잡하게 만드는데, 성능을 크게 개선되지 않을 때가 많기 때문이다.(아이템 67) 

* 객체가 불변이라면 재사용해도 안전함이 명백하다.

* 하지만 안전함이 명백하지 않은 경우도 있다.
  * 어댑터, KeySet 메서드, 오토박싱

1. 어댑터 (= 어댑터를 뷰(view)라고도 한다.)
- 실제 작업은 뒷단 객체에 위임하고, 자신은 2의 인터페이스 역할을 해주는 객체다.
- 어댑터는 뒷단 객체만 관리하면 된다.
- 즉, 뒷단 객체 외에는 관리할 상태가 없으므로 뒷단 객체 하나당 어댑터 하나씩만 만들어지면 충분하다.

2. KeySet 메서드
- KeySet 메서드는 Map 객체 안의 키 전부를 담은 Set뷰를 반환한다.
- KeySet을 호출할 때마다 사실 매번 같은 Set인스턴스를 반환한다. 
- 반환된 Set인스턴스가 일반적으로 가변이더라도 반환된 인스턴스들은 기능적으로 모두 똑같다.
- 즉, 반환된 객체 중 하나를 수정하면 다른 모든 객체가 따라서 바뀐다.
- **따라서 KeySet이 뷰 여러개를 만들어도 상관은 없지만, 그럴 필요도 없고 이득도 없다.** 

3. 오토박싱 (auto boxing)
- 프로그래머가 기본타입과 박싱된 기본 타입을 섞어 쓸 때 자동으로 상호 변환해주는 기술이다.
- 오토박싱은 기본 타입과 그에 대응하는 박싱된 기본 타입의 구분을 흐려주지만, 완전히 없애주는 것은 아니다

```java
  private static long sum(){
    Long sum=0L;
    for (long i=0;i<=Integer.MAX_VALUE;i++){
      sum+=i;
     }
  }
```
- sum 변수를 long이 아닌 Long으로 선언해서 불필요한 Long 인스턴스가 2의 31승 개나 더 만들어졌다.!
- 박싱된 기본 타입보다는 기본 타입을 사용하고, 의도치 않은 오토박싱이 숨어들지 않도록 주의하자.


> 정리

    "객체 생성은 비싸니 피해야 한다"는 뜻이 결코 아니다.
    프로그램의 명확성, 간결성, 기능을 위해서 객체를 추가로 생성하는 것이라면 일반적으로 좋은일이다. 
    방어적 복사(defensive copy)- 아이템 50에서는 "새로운 객체를 만들어야 한다면 기존 객체를 재사용하라"는 대조적인 이야기를 다루고있으니
    추후 복습해볼 것.
    
    "방어적 복사가 필요한 상황에서 객체를 재사용했을 때의 피해" 가 
    "필요없는 객체를 반복 생성했을 때의 피해" 보다 크다는 것을 명심하자.
    
    방어적 복사에 실패하면 언제 터져 나올지 모르는 버그와 보안 구멍으로 이어지지만,
    불필요한 객체 생성은 코드 형태와 성능에만 영향을 준다.
 
  ----------------------------------------------------------------------------------------------------------------------------
 ## 아이템7. 다 쓴 객체 참조를 해제하라
* **자바의 가비지 컬렉터를 통해 메모리 관리에 신경쓰지 않아도 된다고 오해할 수 있지만, 절대 아니다.** 

* 가비지 컬렉션 언어에서는 메모리 누수를 찾기가 아주 까다롭다.
* 객체 참조 하나를 살려두면 가비지 컬렉터는 그 객체뿐 아니라 그 객체가 참조하는 모든객체(그리고 또 그 객체들이 참조하는 모든객체..)를 회수해가지 못한다.

* 해법은 해당 참조를 다 썼을때 **null 처리**(참조해제)하면 된다. 
* null를 하면 null처리한 참조를 실수로 사용하려하면 NullPointerException을 던져 오류를 알 수 있다.
* 참조해제하는 가장 좋은 방법은 그 참조를 담은 변수를 유효범위(scope) 밖으로 밀어내는 것이다. 

* 모든 객체를 다 쓰지마자 null처리로 모두 바꾸는 일은 프로그램을 필요이상으로 지저분하게 만들 뿐이다.
* 객체 참조를 null 처리하는 일은 예외적인 경우여야 한다.
> 자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시 메모리 누수에 주의해야 한다.

### 1. Stack 클래스
  - 자기 메모리를 직접 관리하는 Stack은 메모리 누수가 발생한다.
  - 비활성 영역이 되는 순간 null 처리해서 해당 객체를 더는 쓰지 않을 것임을 가비지 컬렉터에 알려야 한다. 
### 2. 캐시
  - 캐시는 메모리 누수를 일으키는 주범이다. 
  - 캐시 외부에서 키를 참조하는 동안만 엔트리가 살아 있는 캐시가 필요한 상황이라면 WeakHashMap을 사용해 캐시를 만들자. 
### 3. 리스너 혹은 콜백 
  - 클라이언트가 콜백만 등록하고 명확한 해지를 하지 않는다면, 뭔가 조치해주지 않는 한 콜백을 계속 쌓여갈 것이다.
  - 이럴 때 콜백을 약한참조로 저장하면 가비지 컬렉터가 즉시 수거해 간다.


> 정리

    메모리 누수는 겉으로는 잘 드러나지 않아 시스템에 수년간 잠복하는 사례도 있다. 
    이런 누수는 철저한 코드 리뷰나 힙 프로파일러 같은 디버깅 도구를 동원해야만 발견되기도 한다.
    그래서 이런 종류의 문제는 예방법을 익혀두는 것이 매우 중요하다.
 
  ----------------------------------------------------------------------------------------------------------------------------
 
 ## 아이템8. finalizer와 cleaner 사용을 피하라

* 자바는 두 가지 객체 소멸자 제공을 제공한다 : finalizer / cleaner
### finalizer
* 예측할 수 없고, 상황에 따라 위험할 수 있어 일반적으로 불필요하다.
* 기본적으로 '쓰지 말아야'하며, 자바9에서는 finalizer을 사용자제 API로 지정했다.

### cleaner
* finalizer보다는 덜 위험하지만, 여전히 예측할 수 없고, 느리고 일반적으로 불필요하다.

### 사용을 피해야 하는 이유 
### 1. finalizer 와 cleaner은 즉시 수행된다는 보장이 없다.
* 둘다 제때 실행되어야 하는 작업은 절대 할 수 없다.

### 2. 수행 시점뿐 아니라 수행 여부조차 보장하지 않는다.
* 상태를 영구적으로 수정하는 작업에서는 절대 finalizer나 cleaner에 의존해서는 안 된다.

### 3. finalizer 동작 중 발생한 예외는 무시되며, 처리할 작업이 남았더라도 그 순간 종료된다.

### 4. finalizer와 cleaner는 심각한 성능 문제도 동반한다.
### 5. finalizer를 사용한 클래스는 finalizer 공격에 노출되어 심각한 보안 문제를 일으킬 수도 있다.
- finalizer의 공격 원리 : 생성자나 직렬화과정에서 예외가 발생하면, 이 생성자는 되다 만 객체에서 악의적인 하위 클래스의 finalizer가 수행될 수 있게 된다.
- 객체 생성을 막으려면 생성자에 예외를 던지는 것만으로 충분하지만, finalizer가 있다면 그렇지도 않다.
- final이 아닌 클래스를 finalizer 공격으로부터 방어하려면 아무 일도 하지 않는 finalize 메서드를 만들고 final로 선언하자. 


### AutoCloseable : finalizer와 cleaner의 묘안
- AutoCloseable을 구현해주고, 클라이언트에서 인스턴스를 다 쓰고 나면 close 메서드를 호출하면 된다. 
- close 메서드에서 이 객체는 더 이상 유효하지 않음을 필드에 기록하고, 다른 메서드는 이 필드를 검사해서 객체가 닫힌 후에 불렀다면   IllegalStateException을 던진다.

### 네이티브 피어 (native peer)와 연결된 객체 : cleaner와 finalizer을 적절히 활용하는 두번째 예 
- 네이티브 피어? 일반 자바 객체가 네이티브 메서드를 통해 기능을 위임한 네이티브 객체 
- 네이티브 피어는 잦바 객체가 아니라서 가비지 컬렉터는 그 존재를 알지 못한다.
  - 자바 피어를 회수 할때 네이티브 객체까지 회수하지 못하므로, 이 때 cleaner나 finalizer가 나서서 처리하기에 적당한 작업이다.
  - 다만, 성능저하를 감당할 수 있고 네이티브 피어가 심각한 자원을 가지고 있지 않을 때에만 해당한다. 
    - 성능 저하를 감당할 수 없거나 네이티브 피어가 사용하는 자원을 즉시 회수해야 한다면 close 메서드를 사용해야 한다.


> 정리
  
    cleaner(자바 8까지는 finalizer)은 안전한 역할이나 중요하지 않은 네이티브 자원 회수용으로만 사용하자.
    물론 이런 경우라도 불확실성과 성능 저하에 주의해야 한다. 
 

  ----------------------------------------------------------------------------------------------------------------------------
 
 ## 아이템9. try-finally보다는 try-with-resources를 사용하라

* 자원이 제대로 닫힘을 보장하는 수단으로 try-finally 가 많이 쓰였다.
* try-finally 보다 try-with-resouces를 사용하자. 훨씬 간결하고 문제 진단하기가 쉽다. 

### try-finally 를 이용한 자원회수 방법 
```java
 // try-finally를 이용한 자원회수 방법 - 하지만 최선의 방책이 아니다! 
  static String firstLineOfFile(String path) throws IOException{
    BufferdReader br= new BufferedReader( new FileReader(path));
    try{
      return br.readLine(); 
    } finally{
      br.close();
    }
  }
```
* 만약 예외가 try블록과 finally 블록 모두에서 발생한다면? (예시로 기기에 물리적인 문제가 생긴다면?)
  * 위 메서드의 readLine 메서드가 예외를 던지고, 같은 이유로 close 메서드도 실패할 것이다.
  * 이러한 경우에는 첫번째 예외에 관한 정보는 남지 않게 되어, 실제 시스템에서의 디버깅을 몹시 어렵게 한다.
    * 두 번째 예외 대신 첫 번째 예외를 기록하도록 코드를 수정할 수는 있지만, 코드가 너무 지저분해서 실제로 그렇게까지 하는 경우는 없다. 


### try-with-resource를 이용한 자원회수 방법 
```java
 // try-with-resource를 이용한 자원회수 방법 - 자원을 회수하는 최선책! 
  static String firstLineOfFile(String path) throws IOException{
    try(BufferdReader br= new BufferedReader( 
          new FileReader(path))){
    
      return br.readLine(); 

    }
  }
```
* try-with-resource 버전이 짧고 읽기 수월할 뿐만 아니라 문제를 진단하기도 훨씬 좋다.
* 아까와 같은 경우처럼 readLine과 close양쪽에서 예외가 발생하면
* close에서 발생한 예외는 숨겨지고 readLine에서 발생한 예외가 기록된다.

>정리
    
    꼭 회수해야 하는 자원을 다룰 때는 try-finally말고, try-with-resource를 사용하자.
    예외는 없다. 코드는 더 짧고 분명해지고, 만들어지는 예외 정보도 훨씬 유용하다.
    try-finally로 작성하면 실용적이지 못할만큼 코드가 지저분해지는 경우라도, try-with-resources로는 정확하고 쉽게 자원을 회수할 수 있다. 
 
  ----------------------------------------------------------------------------------------------------------------------------
