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
- Advice : 언제 공통 관심 기능을 핵심 로직에 적용할 지를 정의하고 있다.
  - Before: 메서드 호출 전
  - After Returning: 예외 없이 메서드가 실행된 이후
  - After Throwing: 예외가 발생한 경우 실행한다.
  - After: 예외 여부와 관계 없이 실행
  - Around: 메서드 실행, 전, 후, 예외 발생 시점에 실행

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

``` java
import java.util.Arrays;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;

// Aspect 어노테이션을 통해 ExeTimeAspect를 Aspect로 등록한다.
@Aspect
@Component
public class ExeTimeAspect {

    // execution 명시자를 통해 적용 대상을 지정할 수 있다.
    @Pointcut("execution()")
    public void target() {}

    // 원하는 시점, Advice를 선택하고 만들어둔 Pointcut을 적용한다.
    @Around("target()")
    public Object mesure(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.nanoTime();

        try {
          	// 핵심 로직 실행 
            Object result = joinPoint.proceed();
            return result;
        } finally {
            long finish = System.nanoTime();
            Signature sig = joinPoint.getSignature();
            System.out.println("실행시간: " + (finish - start));
            System.out.println(joinPoint.getTarget().getClass().getSimpleName());
            System.out.println(sig.getName() + " " + Arrays.toString(joinPoint.getArgs()));
        }
    }
}
```
