---
title: Spring JPA[DataType-1]
date: 2023-07-02 21:19:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, DataType]
---

# JPA_Data_Type
Spring JPA 값 타입에는 여러가지 타입이 존재합니다.
주로 JPA 값 타입은 2가지 분류로 나뉘게 됩니다.

## 엔티티 타입
첫번째는 엔티티 타입 입니다.
엔티티 타입은 개별적으로 식별이 가능한 개체이며, 생명주기를 갖고 있습니다.
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
}
```

## 값 타입
두번째는 값 타입 입니다.
단순한 데이터이며 엔티티의 속성으로만 사용되며, 엔티티의 생명주기에 종속적 입니다.
주로 엔티티의 속성으로 사용되며, 엔티티의 상태를 나타내는데 사용됩니다.
```java
@Embeddable
public class Point {
    private double latitude;
    private double longitude;

}
```

즉 엔티티는 @Entity로 정의하는 객체이고, 값 타입은 int, Integer, String 처럼 단순히 값으로만 사용하는 자바 타입입니다.

값 타입에는 3가지의 타입이 분류가 되는데, 이 3가지에 대해서 알아보겠습니다.

## 기본값 타입
### 자바 기본 타입 
자바의 기본 타입은 자바의 원시 데이터 타입을 의미합니다.  
데이터를 단순하게 저장하고, 처리하기 위한 기본적인 타입들로 구성 되어있습니다.
- int, long, double, float, boolean 등
-  주의사항 : **자바 기본 타입은 절대 공유되면 안됩니다.**


### 래퍼 클래스
래퍼 클래스는 기본 타입을 객체로 감싸는 역할을 합니다.  
기본 타입에 해당하는 래퍼 클래스들은 대문자로 시작하며, 이를 사용하여 기본타입을 객체처럼 다룰 수 있습니다.
- Integer, Long, Double, Float, Boolean 등  

### 문자열
자바에서 텍스트 데이터를 다룰 때 사용되는 기본적인 데이터 타입입니다.  
Stirng 클래스는 자바에서 문자열을 나타내는데 사용되며, 문자열을 저장하고, 처리하는 다양한 기능을 제공합니다.  
- String

### 임베디드 타입
여러 엔티티에서 반복적으로 사용되는 복합적인 데이터 구조입니다.  
여러 엔티티에서 공통적으로 사용되는 속성들의 집함을 하나의 임베디드 객체로 정의하여 재사용성을 높이는 것을 목적으로 합니다.
```java
@Embeddable
public class Address {
    private String street;
    private String city;
    private String zipCode;
}

```

하지만 여기서 만약에, 엔티티 하나의 속성이 여러 개의 값을 가지는 경우에는 복합값 타입을 써야 합니다.

### 복합값 타입
복합값 타입은 임베디드 타입과 비슷해 보이지만, 하나의 속성이 하나의 값만 가지는게 아닌, 하나의 속성이 여러개의 값을 가질 수 있습니다.
```java
@Embeddable
public class OrderItem {
    private String productName;
    private BigDecimal price;
    private int quantity;
}

```

### 컬렉션 타입
JPA에서 엔티티의 속성으로 사용되는 컬렉션 형태의 복합값을 나타냅니다.
즉, String, int 등 이런 타입이 아닌 컬렉션 타입을 사용합니다
```java
@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

}
```
주로 List, Set, Map 같은 타입이 사용됩니다.

하지만 타입을 공유하는 상황에서는 큰 부작용이 존재합니다.  
부작용에 대해서는 다음 포스터에서 다루겠습니다.  

