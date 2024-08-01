---
title: Spring JPA[즉시로딩과 지연로딩]
date: 2023-06-24 15:28:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Proxy]
---

# 즉시로딩과 지연로딩
## 즉시로딩
Spring에서 즉시 로딩은 연관된 엔티티를 쿼리할 때 즉시 로딩되는 것을 의미합니다.
즉, 하나의 객체를 조회하면, 연관된 객체를 전부다 바로 조회하는 방법입니다.

## 지연로딩
즉시로딩과는 반대로, 지연로딩은 연관된 엔티티를 실제로 사용할 때까지 로딩을 미루는 것을 의미합니다.

그럼 둘의 차이는 뭘까요?
예시를 보면서 알아보도록 하겠습니다.

## 즉시 로딩 예시
### Author.class
```java
@Entity
public class Author {
    @Id
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "author", fetch = FetchType.EAGER)
    private List<Book> books;

}
```
### Book.class
```java
@Entity
public class Book {
    @Id
    private Long id;
    
    private String title;
    
    @ManyToOne
    @JoinColumn(name = "author_id")
    private Author author;
}
```
위의 두 객체를 살펴보면 서로가 서로를 참조하고 있으며, Author 클래스를 기준으로 Book 클래스와의 일대다 관계로 매핑되어 있습니다.  

Author 클래스를 살펴보면, 연관 관계에 사용된 어노테이션에 fetch = FetchType.EAGER 옵션이 사용되어 있습니다. 이 옵션은 바로 즉시 로딩을 의미합니다.  

즉시 로딩 옵션이 적용된 상태에서 Author 클래스를 조회하는 SQL문을 살펴보면 다음과 같습니다.    

```SQL
SELECT author0_.id AS id1_0_,
       author0_.name AS name2_0_,
       books1_.author_id AS author_i3_1_1_,
       books1_.id AS id1_1_1_,
       books1_.id AS id1_1_0_,
       books1_.author_id AS author_i3_1_0_,
       books1_.title AS title2_1_0_
FROM Author author0_
LEFT OUTER JOIN Book books1_ ON author0_.id=books1_.author_id
WHERE author0_.id=?
```  

이 쿼리문을 살펴보면 Author를 조회하는데 있어서 Book까지 함께 조회되는 것을 확인할 수 있습니다.

이처럼 즉시 로딩은 Author를 조회할 때, 연관된 Book 객체들도 함께 즉시 모두 조회되는 특성을 갖습니다.

## 지연 로딩 예시 
### Author.class
```java
@Entity
public class Author {
    @Id
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    private List<Book> books;

}
```
### Book.class
```java
@Entity
public class Book {
    @Id
    private Long id;
    
    private String title;
    
    @ManyToOne
    @JoinColumn(name = "author_id")
    private Author author;
}
```
Book 클래스에서는 ManyToOne 관계를 정의하고 있습니다.  

이 때 fetch = FetchType.LAZY 옵션이 사용되어 있어서, 연관된 엔티티인 Author을 지연 로딩으로 설정하고 있습니다.  

지연 로딩 옵션이 적용된 상태에서 Author 클래스를 조회하는 SQL문을 살펴보면 다음과 같습니다.  

```sql
SELECT author0_.id AS id1_0_,
       author0_.name AS name2_0_
FROM Author author0_
WHERE author0_.id=?
```  

이 쿼리문을 살펴보면, 즉시 로딩과는 달리 SQL문이 매우 간단한 것을 확인할 수 있습니다.  

또한, Author을 조회할 때, 연관된 객체는 조회하지 않고 Author 엔티티 자체만을 조회하는 것을 알 수 있습니다.  

이렇게 조회할 객체만을 조회하는 것을 지연 로딩이라고 합니다.  

## 결론
위에서 설명한 것처럼 즉시로딩과 지연로딩은 데이터베이스에서 연관된 엔티티를 로드하는 방식을 나타내며, 각각 FetchType.EAGER와 FetchType.LAZY 옵션으로 설정됩니다.  

그러나 실무에서는 즉시로딩을 사용하는 것을 권장하지 않습니다. 즉시로딩은 연관된 모든 엔티티를 즉시 로드하여 메모리를 많이 사용하고, 의도하지 않은 쿼리문이 발생하여 성능 문제를 야기할 수 있습니다.  

특히, 객체 간의 관계가 복잡하고 데이터 양이 많을 경우에는 N + 1 쿼리 문제가 발생할 수 있습니다. 이는 일대다 또는 다대다 관계에서 하나의 엔티티를 조회할 때 발생하는 추가적인 쿼리 호출로 인해 성능이 저하되는 문제입니다.  

따라서 실무에서는 지연로딩을 사용하여 필요한 시점에만 연관된 엔티티를 로드하고, 즉시로딩은 특별한 경우가 아니라면 피하는 것이 좋습니다. 지연로딩을 사용하면 필요한 데이터만을 로드하여 성능을 최적화할 수 있습니다.  