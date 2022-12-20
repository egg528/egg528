# AOP

### 프록시 객체

- 특정 객체를 감싼 객체를 뜻한다.

- 한 예시로, 특정 객체 A를 필드로 가지고 있어, A의 동작 앞 혹은 뒤에 추가적인 동작을 수행할 수 있다.

- ``` java
  public class ExeTimeCalculator implements Calculator{
    
    private Calculator delegate;
    
    public ExeTimeCalculator(Calculator delegate){
      this.delegate = delegate;
    }
    
    @Override
    public long factorial(long num){
      long start = System.nanoTime();
      long result = delegate.factorial(num);
      long end = System.nanoTime();
      
      System.out.println(end - start);
      
      return result;
    }
  }
  ```

  - factorial 메서드가 재귀 형식으로 구현되었다 가정해보자
  - 시간을 구하는 코드를 추가하면 메서드가 실행된 횟수만큼 시간을 구할 것이다.
  - 이때 프록시 객체를 활용하면 1번만 시간을 구할 수 있다.(ExeTimeCalculator 클래스는 factorial() 앞 뒤에 시간 구하는 것만 추가하는 역할)
