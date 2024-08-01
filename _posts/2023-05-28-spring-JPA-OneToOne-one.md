---
title: Spring JPA[1:1]
date: 2023-05-28 20:31:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Relationship, OneToOne]
---

# 일대일 [ 1:1 ]
일대일 관계는 반대로 해도 일대일 입니다.  
주 테이블이나, 대상 테이블 중에 외래 키 선택이 가능합니다.
외래 키에는 데이터베이스 유니크(UNI) 제약조건을 추가해야 합니다. 
## 일대일 [1:1] - 단방향 [ 주 테이블 ]
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

    @OneToOne
    @JoinColumn(name = "Job_id")
    private Job job;
}

```
### Job.class
```java
@Entity
@Getter
@Setter
public class Job {
    @Id
    @GeneratedValue
    private Long id;

    private int jobname;

}
```
일대일 주 테이블 단방향 방법은 다애일 단방향 매핑과 유사합니다.

## 일대일 [1:1] - 양방향 [ 주 테이블 ]
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

    @OneToOne
    @JoinColumn(name = "Job_id")
    private Job job;
}

```
### Job.class
```java
@Entity
@Getter
@Setter
public class Job {
    @Id
    @GeneratedValue
    private Long id;

    private int jobname;
    
    @OneToOne(maapedBy = "job")
    private Job job;

}
```
일대일 주 테이블 양방향도 다대일 양방향 매핑 처럼 외래 키가 있는 곳이 연관관계의 주인입니다.  
반대편은 mappyedBy를 사용하여 적용합니다.

## 일대일 [1:1] - 단방향 [ 대상 테이블 ]
단방향 관계는 JPA를 지원하지 않습니다.
그로므로 일대일 대상테이블 단방향은 되도록 사용을 지향합니다.

## 일대일 [1:1] - 양방향 [ 대상 테이블 ]
일대일 주 테이블 외래 키 양방향 매핑과 방법은 똑같습니다.

## 결론
일대일 주 테이블
- 장점
    - 대상 테이블에 데이터가 있는지 확인이 가능하다는 장점이있습니다.
- 단점
    - 값이 없으면 외래 키에 null값을 허용한다는 단점이 존재합니다.

대상 테이블에 외래 키
- 장점
    - 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조를 유지할 수 있습니다.
- 단점
    - 프록시 기능의 한계로 지연 로딩으로 설정해도 항상 즉시로딩이 됩니다.

이러한 각각의 장단점들이 있습니다.
주로 주 테이블에 외래키를 두는 방법은 객체지향 개발자들이 많이 사용 합니다.