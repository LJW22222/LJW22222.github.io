---
title: Spring JPA[영속성 컨텍스트-2]
date: 2023-05-05 22:15:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, PersistenceContext]
---

# 영속성 컨텍스트
## 1차 캐시
영속성 컨텍스트는 객체(Entity)를 처음으로 로딩할 때 해당 객체(Entity)에 대한 데이터를 보관하며, 트랜잭션 내에서 이를 관리하는 상태에 있습니다. 

이 특성으로 1차 캐시를 활용할 수 있습니다.

1차 캐시는 영속성 컨텍스트 내부에 존재하며, JPA에서 객체(Entity)를 조회할 때 해당 객체(Entity)를 캐시에 저장하여 재사용하는 역할을 합니다. 
## 1차캐시 작동 원리
```java
Testentity otherentity = em.find(Testentity.class, "otherentity");
```
![spring-JPA-find-database-png](/assets/img/spring/spring-JPA-find-database.png){: width="700" height="600" }
1. 데이터베이스 조회를 요청하면, 먼저 DB에 바로 접근하는게 아닌 영속성 컨텍스트의 1차캐시에서 조회합니다.

2. 1차캐시에 있으면 이를 활용해 반환하면 되지만, 없다면 DB에서 조회를 합니다

3. DB에서 조회를하면, 이를 다시 1차 캐시에 저장합니다.

4. 1차 캐시에 저장이 완료되면, 1차캐시에서 조회한 후, 반환합니다.

이러한 원리로, 계속해서 DB에서 조회를 하는 것을 피하고, 성능을 향상 시킬 수 있다는 장점이 있습니다.

### 동일성
```java
Testentity otherentity1 = em.find(Testentity.class, "otherentity");
Testentity otherentity2 = em.find(Testentity.class, "otherentity");
System.out.print(otherentity1 == otherentity2 ); // True
```
같은 Entity를 사용하고 있기 때문에 두 참조가 같은 객체를 가리키고 있습니다.

이를 동일성이라고 합니다.

### 쓰기 지연
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.
transaction.begin(); // [트랜잭션] 시작
em.persist(otherentity1);
em.persist(otherentity2);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```
![spring-JPA-Lazy-loading-png](/assets/img/spring/spring-JPA-Lazy-loading.png){: width="700" height="600" }<br/>
![spring-JPA-transaction-commit-png](/assets/img/spring/spring-JPA-transaction-commit.png){: width="700" height="600" }<br/>
쓰기 지연은 데이터베이스의 DML 작업을 트랜잭션의 커밋 시점까지 지연시키는 전략입니다.

트랜잭션의 커밋 전까지는 변경된 내용이 실질적으로 데이터베이스에 반영되지 않으며, 트랜잭션을 커밋하는 순간에 해당 트랜잭션 동안 쌓아둔 DML 작업이 일괄적으로 데이터베이스에 적용됩니다.

### 변경감지
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
transaction.begin(); // [트랜잭션] 시작
// 영속 엔티티 조회
Testentity otherentity1 = em.find(Testentity.class, "otherentityA");
// 영속 엔티티 데이터 수정
otherentity1.setName("test");
otherentity1.setNumber(1);
//em.update(member) 이런 코드가 있어야 하지 않을까?
transaction.commit(); // [트랜잭션] 커밋
```
![spring-JPA-DirtyChecking-png](/assets/img/spring/spring-JPA-DirtyChecking.png){: width="700" height="600" }<br/>
JPA에서 영속성 컨텍스트는 엔티티들을 관리하는데, 트랜잭션 범위 내에서 엔티티의 데이터 변화가 감지되면 자동으로 데이터베이스에 반영 하는것이 변경 감지 입니다.<br/>

즉, 여러 엔티티 매니저 메서드를 사용하여 DB로부터 엔티티를 읽어오면 해당 엔티티는 영속 상태가 됩니다.

그 후에 트랜잭션 내에서 엔티티의 필드 값을 변경하면, 트랜잭션이 커밋되기 전에 영속성 컨텍스트가 이 변경을 감지하고 자동으로 UPDATE 쿼리를 생성하여 데이터베이스에 반영하는것 입니다.<br/>

### 플러시
플러시는 영속성 컨텍스트의 변경사항을 DB에 동기화하여 상태를 일치시키는 과정입니다.<br/>

이때, 영속성 컨텍스트를 비우지 않고 변경내용을 DB에 동기화합니다.<br/>

다음과 같은 상황에서 발생합니다.<br/>

1. 트랜잭션 커밋 [ 자동 호출 ]
    - 트랜잭션이 커밋될 때 자동으로 플러시를 수행합니다.

2. JPQL 쿼리 실행 [ 자동 호출 ]
    - JPQL쿼리를 실행하기 전에 플러시가 발생합니다.

3. 명시적인 플러시 호출(em.flush()) [ 직접 호출 ]
    - JPA는 트랜잭션이 커밋될 때 자동으로 플러시를 수행합니다.

## 결론
영속성 컨텍스트는 객체를 효율적으로 관리하기 위한 메커니즘이며, 1차 캐시를 활용하여 반복적인 데이터베이스 조회를 최소화합니다.<br/>

이를 통해 성능 향상과 동일한 엔티티 참조의 유지가 가능하며, 쓰기 지연, 변경 감지, 그리고 명시적인 플러시를 통해 데이터베이스와의 동기화를 효과적으로 관리할 수 있습니다.