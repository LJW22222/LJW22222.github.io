---
title: JPQL[기초]
date: 2023-07-07 19:15:00 +0800
categories: [Spring, JPQL]
tags: [Spring, JPQL]
---
# JPQL
JPQL은 객체 지향 쿼리 언어 입니다.  
따라서 테이블을 대상으로 쿼리하는게 아닌, 엔티티 객체를 대상으로 쿼리를합니다.  

SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않습니다.  
즉, 다양한 데이터베이스에서 사용이 가능합니다.  

JPQL은 결국에는 SQL로 변환 됩니다.  
## JPQL 문법
```SQL
select m from Member as m where m.name = 'Hi'
```
위의 쿼리는 Member Entity를 m으로 지정하고, Entity의 name이 Hi인 객체를 조회하는 쿼리문입니다.  

엔티티와 속성은 대소문자를 구분합니다.  
하지만 JPQL 키워드는 대소문자를 구분 하지 않습니다.  
별칭 즉 m부분은 필수이지만 as는 생략이 가능합니다.  

## 반환 값 타입
- TypeQuery
    - 반환 타입이 명확할때 사용합니다.
    ```java
    TypedQuery<Member> query =
    em.createQuery("SELECT m FROM Member m", Member.class);
    ```
- Query
    - 반환 타입이 명확하지 않을때 사용합니다.
    ```java
    Query query =
    em.createQuery("SELECT m FROM Member m", Member.class);
    ``` 

## 결과 조회 타입
- query.getResultList()
    - 결과가 하나 이상일 때 사용합니다.
    - 만약, 결과가 없으면 빈 리스트를 반환합니다.
- query.getSingleResult()
    - 결과가 정확히 하나, 단일 객체일때 사용합니다.
    - 없거나, 둘 이상이면 Exception이 발생됩니다.

## 프로젝션
쿼리결과로 반환되는 데이터의 형태를 제어하는 기술입니다.
즉, SELECT 절에 조회할 대상을 지정하는 것 입니다.  
종류는 엔티티, 임베디드, 스칼라 총 3가지가 있습니다.  

### 엔티티 프로젝션
JPQL 쿼리에서 엔티티를 직접 선택하는 방법입니다.  
이 경우 결과는 엔티티의 인스턴스로 반환됩니다.
```SQL
SELECT e FROM EntityName e

SELECT e.subentity FROM EntityName e
```

### 임베디드 타입 프로젝션
엔티티에서 임베디드 타입의 특정 필드를 선택하는 기술입니다.
```SQL
SELECT m.subentity.entity FROM EntityName e
```

### 스칼라 타입 프로젝션
엔티티의 특정 필드, 또는 여러필드를 선택하는 기술입니다.  
결과는 엔티티의 필드 값이거나, 배열일 수 있습니다.
```SQL
SELECT m.type, e.filed FROM EntityName e
```

## 페이징 API
페이징은 N번부터 M번까지만 쿼리결과에서 가져오는 방법입니다.
```JAVA
 String jpql = "select e from EntityName e order by e.name desc";
 List<Member> resultList = em.createQuery(jpql, Member.class)
 .setFirstResult(10)
 .setMaxResults(20)
 .getResultList();
```
- setFirstResult()
    - 조회 시작위치를 정합니다.
- setMaxResult()
    - 조회할 데이터 수를 정합니다.

## 조인
RDB에서 두 개 이상의 테이블 간에 데이터를 결합하여 하나의 결과 집합으로 반환하는 작업입니다.
조인의 종류는 내부조인, 외부조인, 세타조인이 있습니다.  
조인을 수행할 때 조인 조건을 명시하는 방법으로는 ON절을 사용합니다.

### 내부 조인
두 테이블 간의 공통된 값을 기준으로 데이터를 조회합니다.
```SQL
SELECT *
FROM Table1
INNER JOIN Table2 ON Table1.column = Table2.column;
```
### 외부 조인
두 테이블 간의 연관된 값을 모두 반환합니다.  
그리고 연관된 값이 없는 경우에도 결과를 반환합니다. (NULL)
#### 왼쪽
```JAVA
SELECT *
FROM Table1
LEFT OUTER JOIN Table2 ON Table1.column = Table2.column;
```
#### 오른쪽
```JAVA
SELECT *
FROM Table1
RIGHT OUTER JOIN Table2 ON Table1.column = Table2.column;
```
#### 전체
```JAVA
SELECT *
FROM Table1
FULL OUTER JOIN Table2 ON Table1.column = Table2.column;
```
#### 세타 조인
조건이 관계 연산자(>,<,= 등)을 사용하여 조인을 수행합니다.
별도의 키워드 없이 조인 조건만 명시됩니다.
```JAVA
SELECT *
FROM Table1
INNER JOIN Table2 ON Table1.column = Table2.column;
```
#### 조인 ON 절
조인 조건을 명시하는데 사용됩니다.
이외에도 조인 대상 필터링, 연관관계 없는 엔티티 외부 조인에도 사용됩니다.

##### 조인대상필터링
특정 조건만 만족하는 조인된 엔티티만 가져올 수 있습니다.
```Java
SELECT e
FROM Entity1 e
JOIN Entity2 e2 ON e.id = e2.entity1Id AND e2.status = 'ACTIVE'
```
##### 엔티티 외부 조인
연관관계가 없는 엔티티 간의 외부조인을 수행할 때도 사용됩니다.
```java
SELECT e1, e2
FROM Entity1 e1
LEFT JOIN Entity2 e2 ON e1.id = e2.entity1Id AND e2.status = 'ACTIVE'
```

다음 포스터에서는 서브쿼리에 대해 다뤄보도록 하겠습니다.