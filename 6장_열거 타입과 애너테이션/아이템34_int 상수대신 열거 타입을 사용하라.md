## 아이템 34. int 상수 대신 열거 타입을 사용하라 
 
 * 열거 타입은 일정 개수의 상수 값을 정의한 다음, 그 외의 값은 허용하지 않는 타입이다. 
 > [열거타입 정리 참조](https://hyun-1200.tistory.com/manage/newpost/93?type=post&returnURL=https%3A%2F%2Fhyun-1200.tistory.com%2Fmanage%2Fposts%2F)
 
 #### 1. 정수 열거 패턴 기법
 
 ```java
 public static final int APPLE_FUJI = 0;
 public static final int APPLE_PIPPIN = 1;
 public static final int APPLE_GRANNY_SMITH = 2;
 
 public static final int ORANGE_NAVEL =0;
 public static final int ORANGE_TEMPLE =1;
 public static final int ORANGE_BLLOD = 2; 
 ```
 
단점
* 타입 안전을 보장할 방법이 없으며 표현력이 좋지 않다.
```java
if(APPLE_FUJI == ORANGE_NAVEL) // 값은 같지만 타입이 다르다. 하지만, 컴파일러는 경고 메세지 출력하지 않는다. 
```
* 정수 열거 패턴을 위한 별도 이름공간(namespace)를 지원하지 않기 때문에, 어쩔수 없이 접두어를 써서 이름충돌을 방지한다.
ex. 사과용 상수 이름은 모두 APPLE_ 로 시작, 오렌지용 상수는 모두 ORANGE_로 시작
* 정수 열거패턴을 사용한 프로그램은 깨지기 쉽다.
  * 상수의 값이 바뀌게되면 다시 컴파일 해야 한다.
* 문자열로 출력하기 까다롭다.

#### 2. 문자열 열거 패턴 (String enum pattern) 
* 상수의 의미를 출력할 수 있다는 점은 좋지만, 문자열 상수 이름대신 문자열 값을 그대로 하드코딩 하게 만들 수 있다. 

### 3. 열거 타입 (enum type) : 열거패턴의 단점을 보완하고 여러 장점이 있는 대안 

```java
public enum Apple { FUJI, PiPPIN, GRANNY_SMITH }
public enum Orange { NAVEL, TEMPLE, BLOOD } 
```

 * 열거 타입 자체는 클래스이며, 상수 하나당 자신의 인스턴스를 하나씩 만들어 public static final 필드로 공개한다. 
 * 클라이언트가 인스턴스를 직접 생성하거나 확장할 수 없으니 열거 타입 선언으로 만들어진 인스턴스들은 딱 하나씩만 존재함이 보장된다.
    * 싱글턴은 원소가 하나뿐인 열거 타입이고, 열거타입은 싱글턴을 일반화한 형태라고 볼 수 있다. 
 * 열거 타입은 컴파일타임 타입 안전성을 제공한다. 
  * Apple 열거타입을 매개변수로 받는 메서드를 선언했다면, 건네받은 참조는 Apple의 세 가지 값 중 하나임이 확실하다.
  * 다른 타입의 값을 넘기려하면 컴파일 오류가 난다.
* 열거 타입에는 각자의 이름공간이 있어서 이름이 같은 상수도 공존할 수 있다.
* 열거 타입에 새로운 상수를 추가하거나 순서를 바꿔도 다시 컴파일 하지 않아도 된다.
* 열거 타입의 toString 메서드는 출력하기에 적합한 문자열을 내어준다. 

* 열거 타입에는 임의의 메서드나 필드를 추가할 수 있고, 임의의 인터페이스를 구현하게 할 수도 있다. 
  * 열거 타입 상수를 특정 데이터와 연결지으려면 생성자에서 데이터를 받아 인스턴스 필드에 저장하면 된다. 
* 열거 타입은 열거형의 모든 상수를 배열에 담아 반환하는 values를 제공한다. 
* values 는 값들을 선언된 순서로 저장하므르ㅗ toString을 사용하기에 편리하다. 
* 열거 타입의 상수마다 다른 메서드를 수행해야 한다면 switch조건식 대신 추상메서드를 선언하는 **상수별 메서드 구현**을 사용하자.

```java
public enum Operation{
 PLUS { public double apply(double x, double y) { return x+y;}
 MINUS { public double apply(double x, double y) { return x-y; }
 
 public abstract double apply(double x, double y);
}
   
```

* apply 메서드를 재정의하는것을 까먹지 않을 것이고, apply가 추상메서드이므로 재정의하지 않았다면 컴파일 오류로 알려준다. 

단점
- 상수별 메서드 구현에는 열거 타입 상수끼리 코드를 공유하기 어렵다. 

- switch문은 열거 타입의 상수별 동작을 구현하는 데 적합하지 않다.
- 기존 열거 타입에 상수별 동작을 혼합해 넣을 때는 switch문이 좋은 선택이 될 수 있다. 

- 필요한 원소를 컴파일 타임 
