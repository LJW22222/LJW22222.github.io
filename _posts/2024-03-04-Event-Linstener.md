---
title: EntityListeners
date: 2024-03-04 20:40:00 +0800
categories: [Spring, JPA]
tags: [Annotation, Transactional]
---
---
# 영속성 컨텍스트 이벤트 리스너
영속성 컨텍스트에 대해 잘 모르신다면 [영속성 컨텍스트](/_posts/2023-05-08-spring-JPA-two-one.md)를 참조하시면 됩니다.  

프로젝트를 진행하던 중, DB에 데이터가 새로 추가가 될 때, 특정 기능이 작동해야하는 기능을 개발하고 있었습니다.  

이때 어떤 기술을 활용해야 할지 고민하던 중에, 영속성 컨텍스트의 변경감지를 이용해 보자는 생각이 떠올라 이를 활용하기로 하였습니다.  

영속성 컨텍스트의 변경감지는, JPA가 제공하는 기능 중 하나로, Entity의 상태 변화를 감지하여 자동으로 DB의 변경 사항을 반영하는 메커니즘입니다.  

## 사용 방법

### Application.java

Application.java에 먼저 어노테이션을 추가해줍니다.

```java
@SpringBootApplication
@EnableJpaAuditing 
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Entity 정의

그런다음 Entity Class를 정의합니다. [ User.calss ]

```java
@Entity
@EntityListeners(value= UserListeners.class)
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
		
		private String name;
		
		private String age;
}

```

### Repository 정의

Entity에 대한 CRUD작업을 수행할 수 있는 JPA 레포지토리 인터페이스를 정의합니다.

```java
public interface UserRepository extends JpaRepository<YourEntity, Long> {
    // 추가적인 메서드가 필요한 경우 정의합니다.
}
```

### Service 정의

DB에 새로운 데이터가 추가될 때 실행될 특정 기능을 수행하는 서비스 클래스를 작성합니다.

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    @Transactional
    public void processNewData(User user) {
        System.out.println("Hi");
    }
}
```

### UserListeners 정의

여기서 핵심 클래스인 UserListeners.class를 작성합니다.

```java
@Component
public class UserListeners {

    @Autowired
    private UserService userService;

    @PostPersist
    public void handlePostPersist(User user) {
        userService.processNewData(user);
    }
}
```

이러한 설정을 하고나면, 새로운 데이터가 DB에 추가 및 변경될 때마다 영속성 컨텍스트의 변경감지를 통해 지정해놓은 특정 기능( 여기서는 processNewData메서드가 실행됩니다. )을 수행합니다.  

### 종류

| 이름 | 설명 |
| --- | --- |
| @PrePersist | 엔티티가 영속화 되기 전에 호출됩니다. 
주로 엔티티가 DB에 저장되기 전에 선행 작업을 수행하는데 사용됩니다. |
| @PostPersist | 엔티티가 영속화된 후에 호출합니다. 
DB에 엔티티가 저장된 후에 후속 작업을 수행하는데 사용됩니다. |
| @PreUpdate | 엔티티의 상태가 업데이트 되기 전에 호출됩니다. 
엔티티의 상태 변경 전에 선행 작업을 수행하는데 사용됩니다.  |
| @PostUpdate | 엔티티의 상태가 업데이트된 후에 호출됩니다. 
엔티티의 상태가 변경된 후 후속 작업을 수행하는데 사용됩니다.  |
| @PreRemove | 엔티티가 제거 되기 전에 호출됩니다.
엔티티가 삭제되기 전에 선행 작업을 수행하는데 사용됩니다. |
| @PostRemove | 엔티티가 제거된 후에 호출됩니다.
엔티티가 삭제된 후 후속 작업을 수행하는 데 사용됩니다. |
| @PostLoad | 엔티티가 DB에서 로드된 후에 호출됩니다.
엔티티가 로드된 후 후속 작업을 수행하는데 사용됩니다. |

## 결론

이렇게 다양한 어노테이션들을 사용하여 엔티티의 라이프사이클 이벤트에 대한 동작을 정희할 수 있습니다.  

상황에 맞게 사용하면 됩니다.