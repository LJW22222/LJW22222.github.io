---
title: Entity_Mapping[필드와 컬럼 매핑]
date: 2023-05-17 23:41:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Entity, Mapping]
---

# Entity Mapping
## 필드와 컬럼 매핑
JPA에서 엔티티 클래스와 필드와 DB 테이블의 컬럼 간의 매핑은 **@Column**어노테이션을 사용하여 정의합니다.<br/>
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
## @Column
- 컬럼을 매핑할 때 사용합니다.
### 속성
- name
    - 필드와 매핑할 테이블의 컬럼 이름을 작성합니다.
    - 기본값은 필드 이름입니다.
- insertable, updatable
    - 등록, 변경 가능 여부를 설정합니다.
    - 기본값은 TRUE입니다.
- nullable(DDL)
    - null 값의 허용 여부를 설정합니다.
- unique(DDL)
    - 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용합니다.
- columnDefinition(DDL)
    - 데이터베이스 컬럼 정보를 직접 줄 수 있습니다.
- length(DDL)
    - 문자 길이 제약조건을 설정합니다. ( String 타입에만 사용이 가능합니다. )
- precision, scale(DDL)
    - BigDecimal타입에서만 사용이 가능합니다.
    - 아주큰 숫자, 정밀한 소수를 다룰 때 사용합니다.

## @Temporal
- 날짜 타입(Date, Calendar)을 매핑할 때 사용합니다.
### 속성
- TemporalType.DATE
    - 날짜, DB date 타입과 매핑할 때 사용합니다.
- TemproalType.TIME
    - 시간, DB time타입과 매핑할 때 사용합니다.
- TemproalType.TIMESTAMP
    - 날짜와 시간, DB timestamp 타입과 매핑할 때 사용합니다.

## @Enumerated
- enum타입을 매핑할 때 사용합니다.
### 속성
- EnumType.ORDINAL
    - enum 순서를 DB에 저장할때 사용합니다.
- EnumType.STRING
    - enum이름을 DB에 저장합니다.
하지만 @Enumerated을 사용할 때는 ORDINAL을 사용하면 안됩니다. ( 기본값이 ORDINAL)

## @Lob
- BLOB, CLOB을 매핑 때 사용합니다.
@Lob에는 지정할 수 있는 속성이 없습니다.
매핑하는 필드 타입이 문자면 CLOB를 사용하고, 나머지는 BLOB를 사용합니다.

## @Transient
- 특정 필드를 컬럼에 매핑하지 않습니다.<br/>

- 필드에 매핑하지 않습니다.<br/>

- 또한 DB에 저장하지도 않고, 조회되지도 않습니다.<br/>

- 주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을때 사용합니다.<br/>

