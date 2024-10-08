---
title: Spring[의존관계 자동 주입]
date: 2023-02-20 23:12:00 +0800
categories: [Spring, SpringFrameWork]
tags: [Spring]
---

# 의존관계 자동 주입
의존관계 자동 주입은 객체 간의 관계를 코드에서 직접 설정하는 번거로움을 덜어주고, 유지 보수성을 향상 시키며 생산성을 높이기 위한 매커니즘입니다.      

이를 통해 코드의 모듈성이 강화되고, 객체의 독립성이 증가하여 재사용성이 향상된다는 이점이 있습니다.        

또한 스프링 컨테이너 객체간의 의존성을 관리하므로 개발자는 주로 비즈니스 로직에만 집중할 수 있어        

코드의 복잡성을 간소화하며 시스템의 유연성과 확장성을 제공하는 장점이 있습니다.

## 의존관계 주입 방법
주입 방법엔는 생성자 주입, 수정자 주입, 필드주입, 일반 메서드 주입 총 4가지 방법이 있습니다.        

다음각 각 방법에 대한 설명입니다.

### 1. 생성자 주입
이름 그대로 생성자를 통해서 의존관계를 주입 받는 방법입니다.        
```java
@Component
public class BeanServiceImpl implements BeanService {
 private final BeanRepository beanRepository;
 @Autowired
 public BeanServiceImpl(BeanRepository beanRepository) {
 this.beanRepository = beanRepository;
 }
}
```
생성자 주입은 다음과 같은 장단점이 존재합니다.
- 장점
   - 불변성: 생성자 주입을 통해 설정된 의존 관계는 객체가 생성된 후 변경되지 않으므로 불변성을 보장합니다.
   - 필수 의존성: 객체가 생성될 때 반드시 필요한 의존성을 명확하게 설정할 수 있습니다.
테스트 용이성: 생성자를 통해 의존성을 주입받기 때문에, 테스트 시 Mock 객체를 주입하기 쉬워집니다.
- 단점
   - 생성자의 매개변수가 많아질 경우: 많은 의존성을 가진 클래스는 생성자의 매개변수가 많아질 수 있습니다. 이 경우에는 빌더 패턴과 같은 추가적인 설계 패턴을 고려할 수 있습니다.

### 2. 수정자 주입
setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존 관계를 주입하는 방법입니다.
```java
@Component
public class BeanServiceImpl implements BeanService {
 private BeanRepository beanRepository;
 @Autowired
 public void setBeanServiceImpl(BeanRepository beanRepository) {
 this.beanRepository = beanRepository;
 }
}
```
수정자 주입은 다음과 같은 장단점이 존재합니다.
- 장점:
   - 선택적 의존성: 의존성을 선택적으로 주입할 수 있으므로, 일부 의존성이 없는 경우에도 객체를 생성할 수 있습니다.
   - 변경 가능성: 객체 생성 후에도 의존성을 변경할 수 있습니다.
- 단점:
   - 불변성 보장 불가: 객체 생성 후에도 의존성을 변경할 수 있어 불변성을 보장할 수 없습니다.
   - 의존성 누락 가능성: 수정자 메서드가 호출되지 않으면 의존성이 설정되지 않은 상태로 남을 수 있습니다.
#### Java Bean 프로퍼티 규약
- 필드의 값을 직접 변경하지 않고, setXxx, getXxx 메서드를 통해서 값을 읽거나 수정하는 규칙입니다.

### 3. 필드 주입
이름 그대로 필드에 바로 주입하는 방법입니다.      

하지만 외부에서 변경이 불가능해, 테스트 하기 힘들다는 치명적인 단점이 존재 합니다.     

그 외에도 의존성 숨김, 순환 의존성 문제등 다른 문제점도 존재하여 사용하지 않는걸 추천 합니다.      

### 4. 일반 메서드 주입
일반 메서드를 통해서 주입을 받는 방법입니.
```java
@Component
public class BeanServiceImpl implements BeanService {
 private BeanRepository beanRepository;
 @Autowired
 public void init(BeanRepository beanRepository) {
 this.beanRepository = beanRepository;
 }
}
```
다음과 같은 장단점이 존재합니다. 
- 장점:
   - 한 번에 여러 필드 주입: 여러 의존성을 한 메서드를 통해 주입할 수 있습니다.
- 단점:
   - 비표준 사용: 일반적으로 잘 사용되지 않으며, 코드의 가독성을 떨어뜨릴 수 있습니다.

## 결론
최근에는 스프링을 비롯한 DI(Dependency Injection) 프레임워크에서 생성자 주입을 강력히 권장하고 있습니다.  

이는 수정자 주입이나 필드 주입 대신 생성자 주입을 선호하는 이유가 있습니다.        

생성자 주입은 의존 관계가 한 번 설정되면 애플리케이션 종료까지 변경되지 않는 경우가 대부분입니다(불변성).
이로인해 객체를 안정적으로 사용할 수 있습니다.  

이로써 의존 관계의 불변성을 보장할 수 있으며, 객체 생성 시 한 번만 호출되므로 이후 호출되는 일이 없습니다.  

또한, 생성자 주입을 사용하면 해당 클래스의 의존 관계를 외부에서 주입받기 때문에 테스트 용이성이 높아집니다.        

수정자 주입과 필드 주입은 사용 용도가 다르며, 선택적 의존성이나 유연성을 요구하는 경우에는 수정자 주입이 유용할 수 있습니다. 그러나 필드 주입은 일반적으로 권장되지 않으며, 유지보수성과 테스트 용이성을 고려할 때 사용을 피하는 것이 좋습니다.

일반 메서드 주입은 특수한 경우에만 사용되며, 보통은 생성자 주입이나 수정자 주입을 사용하는 것이 바람직합니다.

결론적으로는 상황에 따라서 사용하는 방법이 다르지만 대게 일반적으로는 **생성자 주입**을 선택하는 것이 좋습니다.      