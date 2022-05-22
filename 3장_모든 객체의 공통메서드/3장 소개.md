#### Objcect는 객체를 만들 수 있는 구체 클래스지만 기본적으로 상속해서 사용하도록 설계되었다.
* Object에서 final이 아닌 메서드(equlas, hashCode, toString, clone, finalize)는 모두 재정의를(overriding)를 염두해두고 설계되었다

* 그래서 Object를 상속하는 클래스, 즉 모든 클래스는 이 메서드들을 일반 규약에 맞게 재정의해야 한다.

#### 3장에서는 final이 아닌 Object 메서드들을 언제 어떻게 재정의해야 하는지를 다룬다. 
