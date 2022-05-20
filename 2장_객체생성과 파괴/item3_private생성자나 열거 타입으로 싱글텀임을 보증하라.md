  ## 아이템3. private생성자나 열거 타입으로 싱글턴임을 보증하라 
* 싱글턴(Singleton) : 인스턴스를 오직 하나만 생성할 수 있는 클래스
  + 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다. 
  + 싱글턴을 만드는 방식은 두가지가 있다. 
  + 1. 생성자는 private으로 감춰두고, 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 하나 마련한다. 
  
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
*   
  
  
  
