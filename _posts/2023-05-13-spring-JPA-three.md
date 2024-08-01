---
title: Spring JPA[Schema]
date: 2023-05-13 23:13:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Schema]
---

# Schema
DB에서 테이블, 뷰, 인덱스 등 과 같은 객체들의 집합을 나타내는 방법입니다.<br/>

DB내의 여러 사용자들 간의 DB객체의 소유 및 구조를 정의하는 것 입니다.

## JPA Schema
JPA에서는 DDL을 애플리케이션 실행 시점에 자동으로 생성합니다.

이는 테이블 중심이 아닌 객체중심입니다.

DB방언을 활용해서 DB에 맞는 적절한 DDL을 생성합니다.

이렇게 생성된 DDL은 개발 장비에서만 사용합니다.

### 사용법
application.properties
```java
spring.jpa.hibernate.ddl-auto=create
```
이 외에도 여러가지 옵션이 존재합니다.
1. auto = create
    - 기존 테이블을 삭제 후 다시 생성합니다. ( DROP + CREATE )

2. create-drop
    - create와 같으나 종료 시점에 테이블을 DROP 합니다.

3. update
    - 변경분만 반영합니다. ( 웅영 DB에서는 사용하면 안됩니다! )

4. validate
    - 엔티티와 테이블이 정상 매핑되었는지만 확인합니다.

5. none
    - 사용하지 않습니다.

## 결론
이처럼 JPA는 DDL도 자동으로 해주는데 주의점이 존재합니다.<br/>

개발 초기단계에는 create이나 update를 사용하고,<br/>

테스트 단계에서는 update, validate를,<br/>

스테이징과 운영 서버는 validate 또는 none을 사용하면 좋습니다.<br/>