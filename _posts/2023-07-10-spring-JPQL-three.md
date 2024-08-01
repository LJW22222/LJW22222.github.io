---
title: JPQL[경로 표현식]
date: 2023-07-10 16:34:00 +0800
categories: [Spring, JPQL]
tags: [Spring, JPQL]
---
# JPQL 경로표현식
JPQL에서 경로 표현식은 .을찍어 객체 그래프를 탐색하는 것입니다.  

이떄 사용되는 용어는 상태필드, 단일 값 연관 필드, 컬렉션 값 연관 필드가 있는데 이에 대해서 알아보겠습니다.
## 예제 코드
```sql
select m.username -> 상태 필드
 from Member m 
 join m.team t -> 단일 값 연관 필드
 join m.orders o -> 컬렉션 값 연관 필드
where t.name = '팀A'
```
- 상태 필드
    - 단순히 값을 저장하기 위한 필드입니다.
- 연관 필드
    - 연관관계를 위한 필드입니다.
        - 단일 값 연관 필드
            - @ManyToOne, @OneToOne, 대상이 엔티티입니다.
        - 컬렉션 값 연관 필드
            - @OneToMany, @ManyToMany, 대상이 컬렉션 입니다.

## 특징
- 상태필드
    - 경로 탐색의 끝입니다.
    - 탐색하지 않습니다.
- 연관 필드
    - 단일 값 연관 필드
        - 묵시적 내부 조인이 발생합니다.
        - 탐색을 합니다.
    - 컬렉션 값 연관 필드
        - 묵시적 내부 조인이 발생합니다.
        - 탐색하지 않습니다.
단, FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색이 가능합니다.

## 경로 탐색
### 상태 필드
```SQL
select m.username, m.age from Member m
```
상태필드는 JPQL이나 SQL이나 방식이 똑같습니다.

### 단일 값 연관 필드
```SQL
JPQL
select o.member from Order o

SQL
select m.* from Orders o inner join Member m on o.member_id = m.id
```
하지만 단일 값 연관 필드는 조금 다릅니다.  
JQPL은 객체를 직접 참조하고 있지만, SQL에서는 inner join을 사용하여 연관된 엔티티를 가져오고 있습니다.  

이처럼 join을 사용하는 방법을 **명시적 조인**이라고 하고, SQL처럼 객체를 참조하는 방식을 **묵시적 조인**이라고 합니다. ( 단, 내부 조인만 가능합니다.)

### 주의점
```sql
select t.members.username from Team t
```
위의 쿼리를 실행하면 실패합니다.  
member의 경우 컬렉션 값 연관필드이기 때문에 실패할 수 밖에 없습니다.  
이를 해결하려면 Join방법을 써야합니다.  
```sql
select m.username from Team t join t.members m
```

## 결론
묵시적 조인은 간편한 방법으로 실행할 수 있는 장점이 있지만, 연관 필드에서는 묵시적 조인 방법으로 사용하면 쿼리문이 실패합니다.  

하지만, 명시적 조인 (join)을 사용하면 연관된 필드를 조회할 수 있기 때문에 이를 사용하여 해결 할 수 있습니다.

이로 인해 가급적 무시적 조인 대신에 명시적 조인을 사용하는 걸 권장합니다.  
