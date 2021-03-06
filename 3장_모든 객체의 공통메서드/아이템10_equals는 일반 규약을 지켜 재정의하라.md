## 아이템10. equals는 일반 규약을 지켜 재정의하라 

### <equals를 재정의하지 않는 것이 최선인 경우>

#### 1. 각 인스턴스가 본질적으로 고유하다.
- 값을 표현하는 게 아니라 동작하는 개체를 표현하는 클래스의 경우.
- ex. Thread - Object의 equals 메서드는 이러한 클래스에 딱 맞게 구현되었다. 

#### 2. 인스턴스의 '논리적 동치성(logical equality)'을 검사할 일이 없다.
- java.util.regex.Pattern은 equals를 재정의해서 두 Pattern의 인스턴스가 같은 정규표현식을 나타내는지 검사하는
- **논리적 동치성**을 검사하는 방법도 있다.
- 하지만 설계자가 논리적 동치성을 검사할 필요가 없다고 판단했다면 Object의 기본 equals만으로 해결된다.

#### 3. 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
- 대부분 Set 구현체는 AbstractSet이 구현한 equals를 상속받아 쓰고,
- List 구현체들은 AbstractList로부터, Map 구현체들은 AbstractMap으로부터 상속받아 그대로 쓴다.

#### 4. 클래스가 private이거나 package-private이고 equlas 메서드를 호출할 일이 없다.
- equlas가 실수로라도 호출되는 걸 막고싶다면 아래처럼 구현하자.

```java
@Override public boolean equals(Object o){
  throw new AssertionError(); //호출 금지!
} 
``` 

### <equlas를 재정의해야 하는 경우>
#### **논리적 동치성**을 확인해야 하는데, 상위 클래스의 equlas가 논리적 동치성을 비교하도록 재정의되지 않았을때 
- 주로 값 클래스(Integer, String)들이 해당 
- equals가 논리적 동치성을 확인하도록 재정의해두면, 값을 비교함 뿐만 아니라 Map의 키와 Set의 원소로 사용할 수 있게 된다.
- 값 클래스라 해도, 값이 같은 인스턴스가 둘 이상 만들어지지 않음을 보장하는 인스턴스 통제 클래스라면 equals를 재정의 하지 않아도 된다. Enum도 해당.
  - 인스턴스 통제 클래스(instance-controlled) : 반복되는 요청에 같은 객체를 반환하는 식으로 정적 팩터리 방식의 클래스느 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다.
    - 인스턴스를 통제하면 싱글턴으로 만들수도, 인스턴스화 불가로도 만들 수 있다.
    - 불변 값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수 있다. 


### <equals 메서드 재정의할 때 규약>
* equals 메서드는 동치관계를 구현하며, 다음을 만족한다. 
    - **동치관계** : 집합을 서로 같은 원소들로 이뤄진 부분집합으로 나누는 연산 
1. 반사성(reflexivity): null이 아닌 모든 참조 값 x에 대해, x.equals(x)= true 이다.
  - 객체는 자기 자신과 같아야 한다.
2. 대칭성(symmetry): null이 아닌 모든 참조 값 x,y에 대해, x.equals(y)= true 이면 y.equals(x)= true 이다.
  - 두 객체는 서로에 대한 동치여부에 똑같이 답해야 한다.
3. 추이성(transitivity) : null이 아닌 모든 참조값 x,y,z에 대해, x.equlas(y)= true이고, y.equals(z)= true이면 x.equlas(z)= true 이다.
   - 상위클래스에 없는 새로운 필드를 하위 클래서에 추가해야 하는 상황 
   - 구체 클래스를 확장해 새로우 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않는다.
      - 상속 대신 컴포지션을 사용하면 괜찮은 우회방법이다. 

```java
public class ColorPoint{
  private final Point point;
  private final Color color;
  
  public ColorPoint(int x, int y, Color color){
    point = new Point(x,y);
    this.color = Objects.requireNonNull(color);
  }
  
  /* 이 ColorPoint의 Point 뷰를 반환한다. */
  public Point asPoint(){
    return point;
  }
  
  @Override public boolean eqauls(Object o){
    if(!(o instanceof ColorPoint)) return false;
    ColorPoint cp = (ColorPoint) o;
    return cp.point.eqauls(point) && cp.color.equals(color); 
  }
}
```
   - 리스코프 치환원칙 : 어떤 타입에 있어 중요한 속성이라면 그 하위 타입ㅂ에서도 마찬가지로 중요하다.  
4. 일관성(consistency) : null이 아닌 모든 참조 값 x,y에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.
  - 두 객체가 같다면 (수정되지 않는 한) 앞으로도 영원히 같아야 한다. 
5. null-아님 : null이 아닌 모든 참조 값 x에 대해, x.equlas(null)은 false이다.


### equals 메서드 구현 방법

1. == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다. 자기 자신이면 true를 반환한다.
2. instanceof 연산자로 입력이 올바른 타입인지 확인한다. 
3. 입력을 올바른 타입으로 형변환한다. 2번에서 instanceof 검사를 했기 때문에 이 단계는 100% 성공한다.
4. 입력 객체와 자기 자신의 대으되는 '핵심'필드들이 모두 일치하는지 하나씩 검사한다. 
5. 대칭적인가? 추이성이 있는가? 일관적인가? 3가지를 자문한다. 

```java
// 전형적인 equals 메서드의 예 
public final class PhoneNumber{
 private final short areaCode, prefix, lineNum; 
 
 ...
 
 @Override public boolean equals(Object o){
  if(o==this) return true; // ==연산자르 사용해 입력이 자기 자신의 참조인지 확인한다. 
  if(!(o instanceof PhoneNumber)) return false;
  PhoneNumber pn = (PhoneNumber)o;
  return pn.lindNum == lineNum && pn.prefix==prefix && pn.areaCode== ardCode;
 }
}
```

* 어떤 필드를 먼저 비교하느냐가 equals의 성능을 좌우하기도 한다.
* 다를 가능성 크거나 비교하는 비용이 싼 피드를 먼저 비교하자. 
* equals를 다 구현했다몀ㄴ, 대칭적인가? 추이성이있는가? 일관적인가? 세가지르 자문하고, 단위테스트를 작성해 돌려보자. 
* equals를 재정의할 땐 hashCode도 반드시 재정의하자 (아이템 11)
* 너무 복잡하게 해결하려 들지 말자.
* Object 외의 타입을 매개변수로 받는 equals 메서드는 선언하지 말자.
```java
  //잘못된 예 - 입력타입은 반드시 Object여야 한다! 
  public boolean equlas(MyClas o){
  }
```

* equals,hashCode를 대신 작업해주는 오픈소스 AutoValue 프레임워크를 사용하면 훨씬 편리하다.

>정리

    꼭 필요한 경우가 아니라면 equals를 재정의하지 말자. 
    재정의해야 할 때는 그 클래스의 핵심 필드를 모두를 빠짐없이, 다섯가지 규약을 확실히 지켜가며 비교해야 한다. 
