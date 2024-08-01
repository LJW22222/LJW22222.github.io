---
title: Entity_Mapping[기본키 매핑]
date: 2023-05-18 20:41:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Entity, Mapping]
---

# Entity Mapping
## 기본키 매핑
Spring에서 기본키 매핑은 JPA을 통해 이루어집니다.  
```java
@Entity(name = "testUser")
@Table
public class testUser {
    @Id
    @Column(name = "id")
    private Long id;

    @Column(name = "user_name")
    private String name;

    private Integer age; 

    @Enumerated(EnumType.STRING) 
    private RoleType roleType; 

    @Temporal(TemporalType.TIMESTAMP) 
    private Date createdDate; 

    @Temporal(TemporalType.TIMESTAMP) 
    private Date lastModifiedDate; 

    @Lob 
    private String description; 
    //Getter, Setter… 
}
```
## 매핑 어노테이션 정리
## @Id
- 직접 할당하는 어노테이션입니다.
- 옵션은 없습니다.

## @GeneratedValue(strategy = GenerationType.XXX)
### 1. IDENTITY
#### 특징
- 기본 키 생성을 데이터베이스에 위임합니다.
- 주로 DB가 MYSQL, PostgreSQL, SQL Server, DB2일때 사용합니다.
- em.persist() 시점에 즉시 INSERT SQL을 실행하고, DB에서 식별자를 조회 합니다.

### 2. SEQUENCE
#### 특징
- 데이터베이스 시퀸스 오브젝트에 사용됩니다.
- 주로 DB가 ORACLE, PostgreSQL, DB2, H2일떄 사용됩니다.
- @SequenceGenerator가 필요합니다.
- 유일한 값을 순서대로 생성하는 특별한 DB 오브젝트 입니다.
```java
@Entity 
@SequenceGenerator( 
 name = “MEMBER_SEQ_GENERATOR", 
 sequenceName = “MEMBER_SEQ", //매핑할 데이터베이스 시퀀스 이름
 initialValue = 1, allocationSize = 1) 
public class Member { 
 @Id 
 @GeneratedValue(strategy = GenerationType.SEQUENCE, 
 generator = "MEMBER_SEQ_GENERATOR") 
 private Long id;
}
```
#### @SequenceGenerator 속성
- name : 식별자 생성기 이름 입니다.
    - 필수입니다.

- sequenceName : DB에 등록되어 있는 시퀸스 이름입니다.
- initialValue : DDL 생성시에만 사용됩니다.
    - 기본값은 1입니다.
- allocationSize : 시퀸스 한 번 호출에 증가하는 값 입니다.
    - 기본값은 50입니다.
- catalog, schema : DB catalog, Schema 이름입니다.



### 3. TABLE
#### 특징
- 키 생성용 테이블을 사용합니다.
- 모든 DB에서 사용이 가능합니다.
- @TableGenerator가 필요합니다.
```java
@Entity 
@TableGenerator( 
 name = "MEMBER_SEQ_GENERATOR", 
 table = "MY_SEQUENCES", 
 pkColumnValue = "MEMBER_SEQ", allocationSize = 1) 
 public class Member { 
 @Id 
 @GeneratedValue(strategy = GenerationType.TABLE, 
 generator = "MEMBER_SEQ_GENERATOR") 
 private Long id;
 }
```
#### @TableGenerator 속성
- name : 식별자 생성기 이름을 지정합니다.
    - 필수입니다.

- table : 키생성 테이블명을 지정합니다.
- pkColumnName : 시퀸스 컬럼명을 지정합니다.
- valueColumnNa : 시퀸 스 값 컬럼명을 지정합니다.
- pkColumnValue : 키로 사용할 값 이름을 지정합니다.
- initialValue : 초기 값, 마지막으로 생성된값이 기준입니다.
    - 기본값은 0 입니다.
- allocationSize : 시퀸스 한 번 호출에 증가하는 수 입니다.
    - 기본값은 50입니다.
- uniqueConstraints(DDL) : 유니크 제약 조건을 지정할 수 있습니다.

### AUTO
- 방언에따라 자동으로 지정합니다.
- Default 입니다.

## 결론
Spring에서 JPA를 사용하여 엔터티 클래스의 기본키를 매핑하는 방법은 @Id 어노테이션을 통한 직접 할당과, @GeneratedValue 어노테이션을 통한 자동 생성 전략을 사용하는데, 자동 생성 전략에는 IDENTITY, SEQUENCE, TABLE, 그리고 AUTO가 있습니다.  

IDENTITY는 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용되며, 데이터베이스에 기본 키 생성을 위임하고 즉시 INSERT SQL을 실행하여 DB에서 식별자를 조회합니다.  

SEQUENCE는 ORACLE, PostgreSQL, DB2, H2와 같은 데이터베이스에서 활용되며, 데이터베이스 시퀸스 오브젝트를 활용하여 유일한 값을 순차적으로 생성합니다.   

TABLE은 모든 데이터베이스에서 사용 가능하며, 키 생성용 테이블을 활용합니다.  

마지막으로 AUTO는 방언에 따라 자동으로 적절한 전략을 선택하며, 주로 기본값(Default)으로 사용됩니다.






