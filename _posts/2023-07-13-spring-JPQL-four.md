---
title: JPQL[FetchJoin]
date: 2023-07-13 23:21:00 +0800
categories: [Spring, JPQL]
tags: [Spring, FetchJoin, JPQL]
---
# Fetch Join
Fetch Join은 명시적 조인, 묵시적 조인과 똑같을 거라 생각할 수 있지만, 페치 조인은 SQL 조인의 종류가 아닙니다.  

단지 JPQL에서 성능 최적화를 위해 제공되는 기능일 뿐 입니다.

페치 조인은, 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능입니다.

예를 들어서, 만약에 회원과 팀이 있다고 생각해봅시다. 회원은 팀에 속해 있을 수 있습니다.  
여기서 만약에 페치 조인을 사용해서 회원을 조회하면, 이에 연관된 팀도 함께 조회됩니다.  

이 기능이 바로 페치조인의 기능입니다.

## 엔티티 페치 조인
엔티티 페치 조인은 단일 값 연관 필드를 사용할 때 사용하는 방법입니다.
### 예시 코드
```sql
JPQL
select m from Member m join fetch m.team

SQL
SELECT M.*, T.* FROM MEMBER M
INNER JOIN TEAM T ON M.TEAM_ID=T.ID
```
위의 JPQL쿼리문을 SQL쿼리문으로 변환한 쿼리문을 보면 회원(M)뿐만 아니라 팀(T)까지 같이 조회하는 모습을 볼 수 있습니다.

### Java 코드
```java
String jpql = "select m from Member m join fetch m.team";
List<Member> members = em.createQuery(jpql, Member.class)
 .getResultList();
for (Member member : members) {
 //페치 조인으로 회원과 팀을 함께 조회해서 지연 로딩X
 System.out.println("username = " + member.getUsername() + ", " +
 "teamName = " + member.getTeam().name()); 
}

// 결과
username = 회원1, teamname = 팀A
username = 회원2, teamname = 팀A
username = 회원3, teamname = 팀B
```

## 컬렉션 페치 조인
일대다 관계에서 사용되는 컬렉션 페치 조인방법입니다.
컬렉션 값 연관필드를 사용할 때 컬렉션 패치 조인을 사용합니다.
### 예시 코드
```sql
JPQL
select t
from Team t join fetch t.members
where t.name = '팀A'

SQL
SELECT T.*, M.*
FROM TEAM T
INNER JOIN MEMBER M ON T.ID=M.TEAM_ID
WHERE T.NAME = '팀A'
```
### Java 코드
```java
String jpql = "select t from Team t join fetch t.members where t.name = '팀A'"
List<Team> teams = em.createQuery(jpql, Team.class).getResultList();
for(Team team : teams) {
 System.out.println("teamname = " + team.getName() + ", team = " + team);
 for (Member member : team.getMembers()) {
 //페치 조인으로 팀과 회원을 함께 조회해서 지연 로딩 발생 안함
 System.out.println("-> username = " + member.getUsername()+ ", member = " + member);
}
}

// 결과
teamname = 팀A, team = Team@0x100
-> username = 회원1, member = Member@0x200
-> username = 회원2, member = Member@0x300
teamname = 팀A, team = Team@0x100
-> username = 회원1, member = Member@0x200
-> username = 회원2, member = Member@0x300
```

## DISTINCT
페치조인의 기능중에 DISTINCT라는 명령어가 있습니다.
DISTINCT느 중복된 결과를 제거하라는 명령어 입니다.

하지만 하이버네이트6 부터는 DISTINCT 명령어를 사용하지 않아도 애플리케이션에서 중복 제거가 자동으로 적용되니 이부분은 넘어가겠습니다.

## 결론 
일반 조인은 연관된 엔티티를 함께 조회하지 않지만, 페치조인은 일반조인과 다르게 연관된 엔티티를 함꼐 조회한다는 것 입니다. ( 즉시 로딩 )

보기에는 좋아보이지만, 명확한 단점이 존재합니다.
1. 별칭을 부여할 수 없습니다.
2. 둘 이상의 컬렉션은 페치 조인이 불가능합니다.
3. 컬렉션을 페치 조인하면 페이징 API를 사용할 수 없습니다.
    - 일대일, 다대일 같은 단일 값 연관 필드들은 페치 조인해도 페이징 가능합니다.
    - 경고 로그를 남기고, 메모리에서 페이징합니다.

이러한 단점들이 존재합니다.

또한 즉시로딩된다는 점도 문제점입니다. ( N+1 )

모든 것을 페치 조인으로는 해결할 수 없으니, 객체 그래프를 유지할 때 사용하면 효과적이고, 최적화가 필요한 곳에만 페치 조인을 적용하는 것이 좋습니다.