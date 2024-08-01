---
title: Spring JPA[N:N]
date: 2023-05-28 20:31:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, Relationship, OneToOne]
---

# 다대다 [ N:N ]
다대다 매핑 방법은, RDB에서 정규화된 테이블 2개로 다대다 관계를 표현할 수 없습니다.  

그래서 중간에 테이블을 하나 추가해서, 일대다, 다대일 관계로 풀어나가야 합니다.  

## 다대다 [N:N->1:N,N:1] - 단방향
### Student Class
```java
@Entity
public class Student {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses = new HashSet<>();
}
```
### Cource Class
```java
@Entity
public class Course {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

}
```
## 다대다 [N:N->1:N,N:1] - 양방향
### Student Class
```java
@Entity
public class Student {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses = new HashSet<>();
}
```
### Cource Class
```java
@Entity
public class Course {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
}
```
이처럼 ManyToMany를 어노테이션을 사용해서 다:다 연관관계 매핑을 하는 방법입니다.  

JoinTalbe에 들어있는 여러가지 옵션을 사용해 편리하게 만들 수 있습니다.

하지만 편리하기 만들기 위해 이러한 방법을 사용한다면 실무에서 사용하기에는 분명한 한계가 존재합니다.

이러한 한계점을 돌파하기 위해서는 중간에 테이블을 하나 두고 연결하는 방법이 존재합니다.

## 다대다 [N:N->1:N,N:1] - 한계돌파
### Student Class
```java
@Entity
public class Student {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "student")
    private Set<StudentCourse> studentCourses = new HashSet<>();
}
```
### Course Class
```java
@Entity
public class Course {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "course")
    private Set<StudentCourse> studentCourses = new HashSet<>();
}
```
### StudentCourse Class
```java
@Entity
public class StudentCourse {

    @Id
    @GeneratedValue
    private Long id;

    @ManyToOne
    @JoinColumn(name = "student_id")
    private Student student;

    @ManyToOne
    @JoinColumn(name = "course_id")
    private Course course;
}
```
StudentCourse라는 새로운 테이블을 만들고, 기존에 ManyToMany였던 테이블을 OneToMany -> ManyToOne -> OneToMany 라는 관계로 만들어 줍니다.

이렇게 하면 한계점이었던 단방향에서의 값 추가/삭제 어려움을 극복할 수 있고, 양방향에서는 양쪽 엔티티에서 서로를 참조할 수 있도록 하여 편리성을 높일수 있습니다.  

## 결론
다대다를 연관관계를 맺고 싶을때는, @ManyToMany를 사용하는 것보다,  중간에 테이블을 하나 추가해줘서 OneToMany -> ManyToOne -> OneToMany로 풀어주면 @ManyToMany의 한계점을 돌파할 수 있습니다.
