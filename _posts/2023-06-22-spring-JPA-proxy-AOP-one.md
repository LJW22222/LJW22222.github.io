---
title: Spring JPA[Proxy,AOP]
date: 2023-06-22 23:12:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Proxy, AOP]
---

# Proxy
Proxy란, 클라이언트가 서버로 요청을 할 떄, 객체를 대신하여 특정 작업을 수행하는 역할을 합니다.

즉, 대리자 역할을 하는 것 입니다.  

프록시가 대리자 역할을 하면서, 클라이언트는 실제 서버 객체의 세부 내용에 대해 알 필요가 없으며, 서버 역시 클라이언트에 대한 정보를 감추고 특정 작업을 수행할 수 있습니다.


주로 AOP, 트랜잭션 관리 등에 쓰입니다.

## AOP
프로그래밍에서 횡단 관심사를 모듈화하고 재사용 가능한 모듈로 만들기 위한 프로그래밍 패러다임입니다.

횡단 관심사는 여러 모듈이나 객체에 영향을 미치는 공통적인 관심사를 말합니다.

예를 들면 로깅, 트랜잭션 관리, 보안, 캐싱 등이 횡단 관심사에 해당합니다.

## AOP 동작 과정
![spring-jpa-aop-proxy-png](/assets/img/spring/spring-jpa-aop-proxy.png){: width="700" height="600"}
AOP를 적용하기 전에는 클라이언트가 직접 Service 객체를 사용하고 있는 모습이 보입니다.  

하지만, AOP를 적용하면 프록시 패턴을 사용하게 됩니다.
프록시 패턴을 사용함으로써, 클라이언트와 서비스 이에 Proxy가 배치되게 됩니다.  

클라이언트가 서비스를 사용하는 과정에서, 이제 서비스가 아닌 프록시를 사용하게 되었습니다.  

이때, 프록시가 핵심로직을 호출하기 전이나 후에 횡단 관심사를 적용할 수 있습니다.

## AOP 만드는 방법
AOP를 만드는 방법은 인터페이스를 구현하거나, 구체 클래스를 상속 받는 방식이 있습니다.  

하지만 이 방식들을 사용하면, 일일이 인터페이스나 클래스를 만들어야하는 번거로움이 존재합니다.  

하지만 Spring에서는 이런 번거로움을 줄이고자, Spring이 직접 프록시를 만들고 빈으로 등록하는 작업까지 모두 다 해주고 있습니다.

## Spring 어노테이션 종류
어노테이션 종류는 매우 다양합니다.  

여기서는 전부를 알아보기 보다는 자주 사용되는 종류로 알아보겠습니다
### @Aspect
- 해당 클래스가 Aspect역할을 한다는걸 명시하는 어노테이션 입니다.
- 어노테이션이 적용된 클래스는 어드바이스와 포인트 컷이 정의 됩니다.
- 주로 class위에 적용합니다.
#### 예시
    ```java
    @Aspect
    public class MyAspect {
    // Advice와 Pointcut이 여기에 정의됨
    }
    ```

### @Before
- 메서드가 실행되기 전에 어드바이스를 실행하도록 지정합니다.
#### 예시
    ```java
    @Before("execution(* com.example.service.MyService.*(..))")
    public void beforeAdvice() {
        // 메서드 실행 전에 실행되는 코드
    }
    ```

### @After
- 메서드가 실행된 후에 어드바이스를 실행하도록 지정합니다.
#### 예시
    ```java
    @After("execution(* com.example.service.MyService.*(..))")
    public void afterAdvice() {
        // 메서드 실행 후에 실행되는 코드
    }
    ```

### @AfterReturning
- 메서드가 성공적으로 실행된 후에 어드바이스를 실행하도록 지정합니다.
#### 예시
    ```java
    @AfterReturning(pointcut = "execution(* com.example.service.MyService.*(..))", returning = "result")
    public void afterReturningAdvice(Object result) {
        // 메서드가 성공적으로 실행된 후에 실행되는 코드
    }
    ```  

### @AfterThrowing
- 메서드에서 예외가 발생한 경우에 어드바이스를 실행하도록 지정합니다.
#### 예시
    ```java
    @AfterThrowing(pointcut = "execution(* com.example.service.MyService.*(..))", throwing = "ex")
    public void afterThrowingAdvice(Exception ex) {
        // 메서드에서 예외가 발생한 경우에 실행되는 코드
    }
    ```

### @Around
- 메서드를 감싸고 원하는 지점에서 어드바이스를 실행하도록 지정합니다.
- 가장 강력한 어드바이스로, 메서드 호출 저과 후에 모두 실행이 가능합니다.
#### 예시
    ```java
    @Around("execution(* com.example.service.MyService.*(..))")
    public Object aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
        // 메서드 호출 전에 실행되는 코드
        Object result = joinPoint.proceed(); // 메서드 호출
        // 메서드 호출 후에 실행되는 코드
        return result;
    }
    ```




## Spring 어노테이션 방법
### Proxy기반 AOP [ Interface ]
Proxy기반의 AOP를 만드는 방법은, 인터페이스를 구현한 클래스를 대상으로 AOP를 적용하는 방식입니다.  

이 방식은 타깃 객체가 인터페이스를 구현하고 있어야 합니다.  
#### MyService.Interface
```java
public interface MyService{
    void doSomething();

}
```
#### MyServiceImpl.class
```java
public class MyServiceImpl implements MyService{
    @Override
    public void doSomething(){
        System.out.println("doing something..");
    }
}
```
#### MyAOP.class
```java
@Aspect
@Component
public class MyAOP{
  
  @Before("execution(* com.example.service.MyService.*(..))")
  public void beforeAdvice(){
    System.out.println("before method execution - AOP Aspect");
  }
}
```

### CGLIB기반 AOP [ Class ]
CGLB기반의 AOP는 인터페이스를 구현하지 않아도 AOP를 적용할 수 있는 방식입니다.  

CGLIB을 사용하여 클래스를 상속받은 프록시 객체를 생성합니다

#### MyService.class 
```java
public class MyService{
    public void doSomething(){
        System.out.println("doing something..");
    }
}
```
#### MyAOP.class 
```java
@Aspect
@Component
public class MyAOP{
  @Before("execution(* com.example.service.MyService.*(..))")
  public void beforeAdvice(){
    System.out.println("before method execution - AOP Aspect");
  }
}
```

### 실행결과
doSomething 함수를 호출해서 실행하면, AOP의 beforeAdvice부분이 @Before로 설정 되어있기 때문에 AOP가 먼저 실행하고 그 다음에 함수가 호출됩니다.


## 결론
이렇게 Proxy의 역할에 대해 살펴봤습니다.  

AOP를 적용할 때 Proxy 패턴을 활용하여 다양한 기능을 Proxy에 추가할 수 있다는 것도 알아봤습니다.  

Spring에서는 어노테이션 기반 AOP를 사용할 때, CGLIB 방식이 편리하다고만 생각하기보다는 상황에 따라 적절한 방식을 선택해야 합니다. 인터페이스를 구현한 경우에는 Proxy 기반 AOP를 사용하고, 클래스를 상속받아 사용한 경우에는 CGLIB 방식을 선택하는 것이 바람직합니다.  

요약하면, 어떤 방식을 선택할지는 상황에 맞게 고려해야 합니다.  


