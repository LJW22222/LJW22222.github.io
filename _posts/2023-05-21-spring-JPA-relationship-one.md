---
title: Spring JPA[연관관계]
date: 2023-05-21 22:31:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Relationship]
---

# 연관관계
연관관계랑 객체들 간의 관계를 나타내는 개념으로 일종의 연결고리라고 생각하면 됩니다.  

즉, 아파트에 살고있으면, 나와 내 이웃들간의 관계라고 생각하면 됩니다.  

각각의 아파트 주민들은 서로에게 연결되고 있고, 이로인해 이웃 간에 상호작용이 발생할 수 있습니다.  

이러한 관계를 연관관계라고 생각하시면 됩니다.

연관관계에는 주로 2종류가 있습니다.

## 단방향 연관관계
한 객체에서 다른 객체로의 관계가 한 방향으로만 설정 되어 있는 관계입니다.
### 객체지향 모델링 [ 객체 연관관계 사용 ]
![spring-JPA-one-way-relationship-png](/assets/img/spring/spring-JPA-one-way-relationship.png){: width="700" height="600"}
### 객체지향 모델링 [ ORM 매핑 ]
![spring-JPA-one-way-relationship-2-png](/assets/img/spring/spring-JPA-one-way-relationship-2.png){: width="700" height="600"} 

## 단방향 예제 코드
### Entity 작성 및 연관관계 설정 
#### Member.class
```java
@Entity
 public class Member { 
 @Id @GeneratedValue
 private Long id;
 @Column(name = "USERNAME")
 private String name;
 private int age;
// @Column(name = "TEAM_ID")
// private Long teamId;
 @ManyToOne
 @JoinColumn(name = "TEAM_ID")
 private Team team;
 } 
```
#### Team.class
```java
@Entity
 public class Team {
 @Id @GeneratedValue
 private Long id;
 private String name; 
 … 
 }
```

## 양방향 연관관계
두 객체 간의 연관관계가 양쪽으로 서로를 참조하도록 설정된 상태를 의미합니다.
즉, 단방향은 한 객체가 한쪽밖에 참조를 못하지만, 양방향은 한 객체에서 다른 객체를 참조하고, 동시에 반대 방향에서도 서로를 참조가 가능한 상태입니다.
### 객체지향 모델링 [ 객체 연관관계 사용 ]
![spring-JPA-two-way-relationship-png](/assets/img/spring/spring-JPA-two-way-relationship.png){: width="700" height="600"} 
## 양방향 예제 코드
### Entity 작성 및 연관관계 설정 
#### Member.class
```java
@Entity
 public class Member { 
 @Id @GeneratedValue
 private Long id;
 @Column(name = "USERNAME")
 private String name;
 private int age;
 @ManyToOne
 @JoinColumn(name = "TEAM_ID")
 private Team team;
 }
```
#### Team.class
```java
 @Entity
 public class Team {
 @Id @GeneratedValue
 private Long id;
 private String name;
 @OneToMany(mappedBy = "team")
 List<Member> members = new ArrayList<Member>();
 … 
 }
```

## 결론
연관관계는 객체 간의 관계를 나타내는 개념으로, 단방향 연관관계는 한 객체에서 다른 객체로의 관계가 한 방향으로만 설정된 것이며, 양방향 연관관계는 두 객체 간의 관계가 양쪽으로 서로를 참조하도록 설정된 상태입니다.  

이를 객체지향 모델링에서는 일종의 연결고리로 비유할 수 있습니다.  

단방향 연관관계에서는 한 객체가 다른 객체를 참조하고, 양방향 연관관계에서는 두 객체가 서로를 참조함으로써 양방향으로의 이동이 가능해집니다.  

이러한 연관관계를 JPA에서는 객체 간의 관계를 매핑하는데 사용되며, 이를 통해 객체를 데이터베이스에 저장하고 조회할 수 있습니다.  