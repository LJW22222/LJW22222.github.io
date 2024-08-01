---
title: Spring JPA[JPA]
date: 2023-04-30 21:15:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA]
---
# JPA
간단한 프로젝트를 진행하면서 SQL문을 일일이 작성하고 관리하는 것이 번거로웠습니다.<br/>

특히 중복된 SQL문이 늘어남에 따라 코드의 양이 증가하고 가독성이 떨어지는 문제가 발생했습니다.<br/>

이런 어려움들을 극복하기 위해 ORM(Object-Relational Mapping) 기술인 JPA를 도입하게 되었습니다.<br/>
## JPA?
JPA란 Java Persistence API 의 약자입니다.<br/>

JPA는 개발자가 객체지향적인 코드로 작성하고, 이를 관계형 데이터 베이스(RDBS)에 저장하고 조회할 수 있도록 도와주는 인터페이스 입니다.<br/>

JPA를 사용하면 중복된 SQL문을 줄이고 객체 지향적인 코드를 유지할 수 있어서 코드의 양을 최적화하고 가독성을 높일 수 있고 데이터베이스와 객체 간의 매핑을 자동으로 처리할 수 있어서 SQL문을 직접 작성하지 않아도 됩니다.<br/>

이로인해 개발자는 데이터베이스에 대한 세부적인 처리보다 비즈니스 로직에 집중할 수 있고, 유지보수가 향상된다는 장점이 있습니다.<br/>

## JPA 구동 방식
![spring-JPA-png](/assets/img/spring/spring-JPA.png){: width="700" height="600" }
1. 설정 정보 조회 
    - META-INT 폴더 안에 있는 persistence.xml 설정 정보를 참고 합니다.
2. 생성
    - EntityManagerFactory를 생성합니다
3. 생성
    - 생성된 EntityManagerFactory안에 EntityManager를 생성하여 요청을 실행합니다.
 
 - 엔티티 매니저 팩토리는 하나만 생성하여 애플리케이션 전체에서 공유합니다.
 - 엔티티 매니저는 쓰레드 간에 공유하지 않아서, 사용하고 버려야 합니다
 - JPA의 모든 데이터 변경은 트랜잭션안에서 실행됩니다.

## JPA 설정
JPA예제 코드를 작성하기 전에 DB에 연결해주는 설정을 해주어야 합니다.<br/>
그렇지 않으면 connect 오류가 발생합니다.<br/>
![spring-basic-jpa-db-setting-png](/assets/img/spring/spring-basic-jpa-db-settting.png){: width="700" height="600" }
- org.springframework.boot:spring-boot-starter-data-jpa
    - Spring Boot에서 JPA를 사용하는데 필요한 스타터 의존성입니다.
    
    - 이 의존성은 Hibernate 같은 JPA 구현체를 내장하고 있어 JPA를 쉽게 설정하고 사용할 수 있게 합니다.<br/>
- mysql:mysql-connector-java:8.0.28
    - MySQL 데이터베이스와 연동하기 위한 MySQL Connector/J 라이브러리입니다.
    - 이 라이브러리는 JDBC 드라이버로서 MySQL과의 연결을 가능하게 해줍니다.<br/>


![spring-basic-jpa-db-properties-png](/assets/img/spring/spring-basic-jpa-db-properties.png){: width="700" height="600" }
- spring.datasource.url=jdbc:mysql://localhost:3306/webdata
    - db에 연결할 주소를 설정합니다. 
        - localhost:[db Port]/[db 스키마]
- spring.datasource.username='user 이름'
    - DB에 접근할 때 필요한 사용자 이름을 적어야 합니다.
- spring.datasource.password='비밀번호'
    - DB에 접근할 때 필요한 사용자의 비밀번호를 적어야 합니다. 
- spring.jpa.show-sql=true
    - SQL 쿼리를 로그에 출력하도록 하는 속성입니다.
- spring.jpa.hibernate.ddl-auto=update
    - Hibernate에게 데이터베이스 스키마를 자동으로 생성 또는 업데이트할 것인지 지정하는 속성입니다. 
- spring.jpa.properties.hibernate.format_sql=true
    - Hibernate가 출력하는 SQL을 보기 쉽게 포맷하는 속성입니다. 

## JPA 예제 코드
```java
@Entity 
public class Member { 
 @Id 
 @GeneratedValue(strategy = GenerationType.IDENTITY)
 private Long id; // Entitiy의 PK에 해당되는 부분입니다.
 private String name;
 //Getter, Setter … 등등 추가할 수 있습니다.
}
```
- @Entity의 어노테이션을 사용하여 이 클래스는 JPA Entity라는 것을 명시해 줍니다.<br/>

- @Id는 private Long id라는 변수가, PK(Private Key)라는 것을 명시해 줍니다.<br/>

- @GeneratedValue(strategy = GenerationType.IDENTITY)는 값을 자동으로 생성하는 방법을 설정하는 어노테이션 입니다.<br/>

즉, DB에서 자동으로 증가하는 기능을 사용하겠다는 의미입니다.<br/>
이렇게 Member라는 Entity 클래스를 생성해주면, DB에는 이를 기반으로 테이블이 생성됩니다.<br/>

## 결론
JPA를 통한 간편한 객체-데이터베이스 매핑을 통해 SQL 작성과 관리의 번거로움을 줄이고, 중복 코드를 최소화하여 가독성을 향상시킬 수 있습니다.<br/>
지금까지는 엔터티 클래스를 정의한 초기 단계이며, 앞으로 예외 처리 및 나머지 프로세스에 대한 구현이 필요합니다.<br/>

