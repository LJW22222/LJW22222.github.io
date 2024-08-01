---
title: Entity_Mapping[객체와 테이블 매핑]
date: 2023-05-16 21:41:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Entity, Mapping]
---

# Entity Mapping
## 객체와 테이블 매핑
```java
@Entity(name = "testUser")
@Table
public class testUser {
    @Id
    @Column(name = "id")
    private Long id;

    @Column(name = "user_name")
    private String name;
}
```
**@Entity**가 붙은 클래스는 JPA가 관리하고, 엔티티라고 불립니다.

JPA를 사용해서 테이블과 매핑할 클래스는 **@Entity**가 필수입니다.

### 주의점
- JPA가 엔티티를 생성할 때 기본 생성자를 사용하기 때문에 **기본 생성자는 반드시 필수**입니다.

- JPA가 프록시를 생성하고, 상속과 관련된 기능을 사용하기 위해 기본적으로 클래스를 상속하기 때문에**final클래스, enum, interface, inner 클래스는 사용하면 안됩니다.**

- **저장할 필드에 final을 사용하면 안됩니다.**

### @Entity 속성
- name
    - JPA에서 사용할 엔티티의 이름을 정의합니다.

    - 기본값은 클래스의 이름을 그대로 사용합니다.

    - 같은 클래스 이름이 없으면 가급적 기본값을 사용합니다.
### @Table 속성
- name
    - 매핑할 테이블 이름을 지정합니다.

- catalog
    - DB catalog를 매핑합니다.
- schema
    - DB schema를 매핑합니다
- uniqueConstraints
    - DDL 생성 시에 유니크 제약 조건을 생성합니다.

## 결론
JPA는 편리하게 많은 어노테이션을 제공하여 객체와 데이터베이스 간의 매핑을 쉽게 할 수 있게 도와줍니다.<br/>

그러나 이러한 어노테이션들을 사용할 때에는 주의해야 할 사항들이 있으며, 특히 기본 생성자, 클래스 및 필드의 제약사항, 어노테이션의 속성 등을 적절히 고려하여 사용해야 합니다.<br/>