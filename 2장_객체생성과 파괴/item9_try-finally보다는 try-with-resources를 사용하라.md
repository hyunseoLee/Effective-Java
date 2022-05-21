## 아이템9. try-finally보다는 try-with-resources를 사용하라

* 자원이 제대로 닫힘을 보장하는 수단으로 try-finally 가 많이 쓰였다.

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
