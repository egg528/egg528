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





### AOP

- 흩어져 있는 공통의 로직을 모듈화 하여 재사용 할 수 있는 기능
  - 주로 부가기능을 모듈화 한다.
- 왜 쓰는가?
  - 부가기능 로직은 AOP를 활용해 줄이고 핵심 로직만을 남기기기 위해서



### 주요 개념

- Aspect : 흩어진 관심사를 모듈화 한 것.
- Target : Aspect를 적용하는 곳 (클래스, 메서드 .. )
- Advice : 실질적으로 어떤 일을 해야할 지에 대한 것, 실질적인 부가기능을 담은 구현체
- JointPoint : Advice가 적용할 위치
- PointCut : JointPoint의 상세한 스펙을 정의한 것.



### 의존성

``` java
// for gradle
implementation 'org.springframework.boot:spring-boot-starter-aop' 
  
// for maven
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```





### 사용법

`@Aspect`
