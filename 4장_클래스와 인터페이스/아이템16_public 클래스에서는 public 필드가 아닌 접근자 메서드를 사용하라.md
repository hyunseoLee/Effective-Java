## 아이템 16. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라


캡슐화의 이점을 사용하지 않는 퇴보한 클래스 
```java
Class Point{
  public double x;
  public double y;
}
```
- API를 수정하지 않고는 내부 표현을 바꿀 수 없고, 불변식을 보장할 수 없으며, 외부에서 필드에 접근할 때 부수작업을 수행할 수도 없다.
- 위 클래스를 객체 지향적으로 변경하기 위해 모든 필드를 private으로 바꾸고 public 접근자(getter)를 추가한다. 

```java
Class Point{
  private double x;
  private double y;
  
  public Point(double x, double y){
    this.x= x;
    this.y= y;
  }
  
  publid double getX(){ return x;}
  public double getY(){ return y;}
  
  public void setX(double x) { this.x= x;}
  public void setY(double y) { this.y= y;}
  
}
```
- 패키지 바깥에서 접근할 수 있는 클래스라면 접근자를 제공함으로써 클래스 내부 표현 방식을 언제든 바꿀 수 있는 우연성을 얻을 수 있ㄷ.


### package-private클래스 혹은 private 중첩 클래스라면 데이터 필드를 노출한다 해도 하등의 문제가 없다.

- public 클래스의 필드가 불변이라면 직접 노출할 때의 단점이 조금은 줄어들수 있지만, 좋은 생각은 아니다. API를 변경하지 않고는 표현방식을 바꿀 수 없고, 필드를 읽을 때 부수작업을 수행할 수 없다는 단점은 여전하다. 

>정리

  public 클래스는 절대 가변 필드를 직접 노출해서는 안 된다. 불변 필드라면 노출해도 덜 위험하지만 완전히 안심할 수는 없다. 하지만 package-private 클래스나 private 중첩 클래스에서는 종종(불변이든 가변이든) 필드를 노출하는 편이 나을 때도 있다. 
