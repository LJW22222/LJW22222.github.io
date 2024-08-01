---
title: Spring JPA[N:1,1:N]
date: 2023-05-27 18:31:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Relationship, ManyToOne, OneToMany]
---

# 다대일 [N:1]
다대일은, 한쪽 객체가 N이고 반대쪽 객체가 1인 연관관계 입니다. 
## 다대일 [N:1] - 단방향
### Member.class
```java
@Entity
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;
}

```

### Team.class
```java
@Entity
@Getter
@Setter
public class Team {
    @Id
    @GeneratedValue
    private Long id;

    private int teamNumber;

}
```
Member.class안에 team이라는 객체가 참조 되어있습니다.
하지만 Team.class에서는 Member.class를 참조하고 있지 않습니다.
이런 연관관계를 다대일[N:1]연관관계라고 합니다.  

즉, 한 팀에 여러 멤버가 속할 수 있지만, 하나의 멤버는 하나의 팀에만 속할 수 있는 구조를 갖고 있습니다.  

여기서의 관점은 Member가 N, Team이 1입니다.

가장 많이 사용되는 연관관계입니다.  
다대일의 반대는 일대다 입니다.

## 다대일 [N:1] - 양방향
### Member.class
```java
@Entity
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;
}

```

### Team.class
```java
@Entity
@Getter
@Setter
public class Team {
    @Id
    @GeneratedValue
    private Long id;

    private int teamNumber;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
}
```
Member.class는 단방향과 코드가 같습니다.  

하지만 Team.class는 mappedBy라고 하는 연관관계 주인을 맺어주는 코드가 추가 되었고, 참조하고 있는 Member의 타입이 List로 변경 되었습니다.  

mappedBy속성으로 인해 Team.class는 Member.class를 참조 하고 있고, Member.class가 N이기 때문입니다.

즉, 여러명의 Member는 하나의 팀에 속해있을 수 있기 때문에 타입을 List로 설정했습니다.  

연관관계의 주인은 Team라고 명시하고 있습니다.  
이로 인해 외래키의 관리는 Team.class가 하고, Member.class는 단지 조회만 가능합니다.  

# 일대다 [1:N]
## 일대다 [1:N] - 단방향
### Member.class
```java
@Entity
@Getter
@Setter
public class Member {
    @Id
    @GeneratedValue
    private Long id;

    private String name;
}
```

### Team.class
```java
@Entity
@Getter
@Setter
public class Team {
    @Id
    @GeneratedValue
    private Long id;

    private int teamNumber;

    @OneToMany
    @JoinColumn(name = "team_id")
    private List<Member> members = new ArrayList<>();
}
```
일대다의 단방향은 다대일의 역으로 생각하면 됩니다.  
일대다 관계는 항상 N쪽에 외래키가 존재합니다.

여러명의 Member가 하나의 Team에 속해있을 수 있습니다.  
하지만 지금 관점은 Member가 N이고, Team이 1이기 때문에  
Team 입장에서는 Member가 N이여서 List타입으로 받아와야 합니다.

하지만 일대다 단방향 매핑에는 단점이 존재합니다.  
그러므로 일대다 단방향 매핑보다는 다대일 양방향 매핑을 사용하는걸 지향합니다.

# 일대다 [1:N]
## 일대다 [1:N] - 양방향
이런 매핑은 공식적으로 존재하지 않습니다.  
다대일 양방향을 사용해야합니다.

## 결론
일대다 단방향 매핑 방법은 단점이 존재하고, 일대다 양방향 매핑방법은 공식적으로 존재하지 않는 방법입니다.  

그러므로 일대다 보다는 다대일 단방향,양방향 매핑 방법을 지향하고 있습니다.  

무분별하게 양방향 매핑을 하는 것 보다는, 초기 설계시 잘 설계하여야 합니다.  
그리고 단방향 매핑으로해도 연관관계 설정은 됩니다.  
꼭 필요한 경우에 양방향 매핑을 설정하는 걸 권장합니다.  

양방향 매핑에는 mappedBy라는 옵션이 꼭 사용되야 하는데, 연관관계의 주인은 외래키가 있는쪽으로 설정해야 합니다.