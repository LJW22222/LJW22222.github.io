---
title: Transactional
date: 2024-03-05 14:24:00 +0800
categories: [Spring, JPA]
tags: [Annotation, Transactional]
---
# @Transactional
Transactional은 DB나 다른 리소스에 대한 작업을 수행할 때, 이러한 작업들을 여러 단계로 나누어 실행하고 관리하는 프로그래밍 개념입니다.  

예를 들어서, 은행에서 돈을 이체할 때, 돈을 보내는 쪽에서 자신의 계좌에서 돈을 빼고, 돈을 받는 쪽에서는 돈을 받는 곳에 넣는 것처럼, 두개 이상의 작업을 하나의 트랜잭션 단위로 묶어서 실행하는 것과 비슷합니다.  

Transactional을 사용하면 여러 개의 작업을 하나의 트랜잭션으로 묶어서 모든 작업이 성공하면 트랜잭션을 커밋하고, 하나라도 실패하면 모든 작업을 롤백하여 이전 상태로 되돌릴 수 있습니다.  

이렇게 함으로써 데이터의 일관성을 보장하고, 안정적인 데이터 처리를 할 수 있게 됩니다.   

간단하게 말하면 Transactional은 여러 개의 작업을 묶어서 한꺼번에 실행이 되고, 모든 작업이 성공하면 모두 반영하고, 하나라도 실패하면 모두 취소하는 것을 도와주는 개념입니다.  

## 트랜잭션

여기서 트랜잭션이란, DB나 다른 시스템에서 일련의 작업들을 논리적으로 묶어서 처리하는 단위를 의미합니다.  

트랜잭션은 DB에서 데이터를 읽고 쓰는 모든 작업을 포함할 수 있으며, 보통 다음과 같은 개념을 갖습니다.  

### 원자성

- DB나 다른 시스템에서 일련의 작업들을 논리적으로 묶어서 처리하는 단위를 의미합니다.  

- 즉, 트랜잭션 내의 작업중 하나라도 실패하면, 모든 작업은 롤백되어 이전 상태로 복구되고, 성공하였을 때만 트랜잭션은 커밋되어 영구적으로 반영됩니다.  

### 일관성

- 트랜잭션 전후의 DB상태는 항상 일관성을 유지해야합니다.  

- 즉, 트랜잭션이 실행되기 전과 후에도 DB는 일관된 상태여야 합니다.  

### 격리성

- 한 트랜잭션이 실행 중일 때, 다른 트랜잭션에 의해 수행되는 작업들이 그 영향을 받지 않아야 합니다.  

- 각각의 트랜잭션은 독립적으로 실행되는 것처럼 보여야 합니다.  

### 지속성

- 트랜잭션이 성공적으로 완료되면 해당 작업들이 영구적으로 DB에 저장되어야 합니다.  

- 이는 시스템 장애 또는 전원 공급 장애와 같은 예외 상황이 발생해도 유지 되어야 합니다.  

하지만 만약에 특정 트랜잭션이 처리중이고, 아직 커밋되지 않았는데 다른 트랜잭션이 접근할 경우 문제가 발생할 경우가 있습니다.  

하지만 이러한 상황을 방지할 수 있는 속성들이 있습니다.  

## Transactional 속성

속성에는 여러 속성들이 존재하지만, 대표적인 isolation과 propagation에 대해 알아 보겠습니다.  

### isolation

일관성 없는 데이터를 허용하도록 하는 수준입니다.  

- DEFAULT
    - 연결된 DB의 격리수준을 따릅니다.  

- READ_UNCOMMITTED( level 0 )
    - 다른 트랜잭션에서 커밋되지 않는 변경 사항을 볼 수 있습니다.  

- READ_COMMITTED ( level 1 )
    - 커밋된 변경 사항만 볼 수 있습니다.  

- REPEATABLE_READ ( level 2 )
    - 동일한 쿼리를 여러 번 실행해도 동일한 결과를 보장합니다.  

- SERIALIZABLE ( level 3 )
    - 동시에 실행되는 트랜잭션 간의 상호간섭을 방지하기 위해 트랜잭션을 순차적으로 실행합니다.  


```java
@Transactional(isolation = Isolation.READ_UNCOMMITTED)
@Transactional(isolation = Isolation.READ_COMMITTED) 
@Transactional(isolation = Isolation.REPEATABLE_READ) 
@Transactional(isolation = Isolation.SERIALIZABLE)
```

### propagation

여러 트랜잭션이 관련된 경우 전파 방식을 결정합니다.

- REQUIRED
    - 트랜잭션이 있는 경우 참여하고, 없으면 새 트랜잭션을 생성하며 propagation설정이 없는 경우 기본값입니다.  

- REQUIRES_NEW
    - 항상 새 트랜잭션을 만들고, 트랜잭션이 있다면 끝날때까지 일시중지 합니다.  

- NESTED
    - 기존 트랜잭션과 중첩된 트랜잭션을 생성하고, 없다면 새로 트랜잭션을 생성합니다.  

- SUPPORTS
    - 존재하는 트랜잭션이 있다면 지원하고, 없으면 메서드만 실생합니다.  

- MANDATORY
    - 반드시 트랜잭션이 존재해야 하는 유형으로 없으면 예외가 발생합니다.  

- NOT_SUPPORTED
    - 트랜잭션이 있어도 중단되며, 트랜잭션을 지원하지 않습니다.  
    
- NEVER
    - 트랜잭션이 존재하면 에외가 발생합니다.  

```java
@Transactional(propagation = Propagation.REQUIRED)       // Default
@Transactional(propagation = Propagation.REQUIRES_NEW) 
@Transactional(propagation = Propagation.NESTED) 
@Transactional(propagation = Propagation.SUPPORTS) 
@Transactional(propagation = Propagation.MANDATORY) 
@Transactional(propagation = Propagation.NOT_SUPPORTED)
@Transactional(propagation = Propagation.NEVER)
```

## 사용 방법

### 의존성 설정

가장 먼저 Spring Freamwork의 트랜잭션 기능을 사용하기 위해 의존성을 추가해야합니다.  

- Gradle 기준

```java
dependencies {
    implementation 'org.springframework:spring-tx:버전' // 사용 중인 Spring 버전에 맞게 버전을 지정해야 합니다.
}
```

- Maven 기준

```java
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>버전</version> <!-- 사용 중인 Spring 버전에 맞게 버전을 지정해야 합니다. -->
</dependency>
```

### 트랜잭션 관리 활성화

어노테이션 적용

```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {

    @Transactional
    public void save_user_data() {
        // 트랜잭션 내에서 수행되어야 하는 작업
    }
}

```

@Transactional은 주로 DB랑 관련이 있는 Service계층에 적용됩니다.

## 결론

트랜잭션이 좋아보여서 남발하여 사용하면 안됩니다.  

무작정 모든 곳에 적용하면 성능문제, 관리의 복잡성 등 여러 문제가 생길 수 있습니다.  

트랜잭션을 사용할떄는다른 AOP와의 충돌을 고려해야하고 트랜잭션을 적용하려는 메서드는 반드시 public으로 선언되어야 합니다.  

또한 DB와의 직접적인 연관을 가지는 Service 계층에서 사용되어야 합니다.  