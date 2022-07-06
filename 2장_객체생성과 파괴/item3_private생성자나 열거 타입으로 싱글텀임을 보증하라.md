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
- 생성자를 private으로 감춰두고, 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 마련한다. 
- private 생성자는 pulic static final 필드인 E lvis.INSTANCE를 초기화할 때 딱 한번만 호출된다.
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
  
* 1번과 2번으로 만든 싱글턴 클래스를 직렬화 하기위해서는 단순히 Serializable을 구현한다고 선언하는 것만으로 부족하다.
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
