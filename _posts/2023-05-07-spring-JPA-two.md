---
title: Spring JPA[영속성 컨텍스트-1]
date: 2023-05-05 22:15:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, PersistenceContext]
---

# 영속성 컨텍스트
JPA에서는 테이블과 매핑되는 엔티티 객체정보를 **영속성 컨텍스트**를 통해 애플리케이션 내에서 오래 지속되도록 보관합니다.<br/>

여기서 **영속성 컨텍스트**란, 엔티티를 영구 저장하는 환경이라는 뜻을 가지고 있습니다.<br/>

**영속성 컨텍스트**는 논리적인 개념이여서 눈에 보이지가 않습니다.<br/>

또한 엔티티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있습니다.<br/>

## 엔티티의 생명주기
![spring-JPA-Entity-png](/assets/img/spring/spring-JPA-Entity.png){: width="700" height="600" }
1. 비영속( new/transient )
    - 영속성 컨텍스트외 전혀 관계가 없는 새로운 상태입니다.

2. 영속( managed )
    - 영속성 컨텍스트에 관리되는 상태입니다.

3. 준영속 ( detached )
    - 영속성 컨텍스트에 저장되었다가 분리된 상태입니다.

4. 삭제 ( removed ) 
    - 삭제된 상태입니다.

## 비영속
![spring-JPA-Entity-out-png](/assets/img/spring/spring-JPA-Entity-out.png){: width="700" height="600" }<br/>
```java
TestEntity testentity = new TestEntity();
testentity.setName("test");
testentity.setNumber("1");
```
아직은 영속성 컨텍스트에 엔티티가 영속되지 않고 있습니다.

## 영속
![spring-JPA-Entity-in-png](/assets/img/spring/spring-JPA-Entity-in.png){: width="700" height="600" }<br/>
```java
TestEntity testentity = new TestEntity();
testentity.setName("test");
testentity.setNumber("1");

EntityManger em = emf.createEntityManger();
em.gettTransaction().begin();

em.persist(testentity);
```
엔티티가 엔티티매니저를 사용해 객체를 저장하였으므로, 영속상태로 전환되었습니다.

## 준영속
```java
em.detach(testentity); // 특정 엔티티만 준영속 상태로 전환
em.clear() // 영속성 컨텍스트를 완전히 초기화
em.close() // 영속성 컨텍스트 종료
```
testenetity라는 엔티티 객체를 영속성 컨텍스트에서 분리하였으므로 준영속 상태로 전환되었습니다.

해당 엔티티는 이제 더이상 관리되지 않습니다.

## 삭제
```java
em.remove(testentity);
```
testenetity라는 엔티티 객체를 삭제하였으므로, 영속성 컨텍스트에서 삭제된 상태입니다.

다음 포스터에서 이어서 설명하도록 하겠습니다.