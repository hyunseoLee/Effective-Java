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
