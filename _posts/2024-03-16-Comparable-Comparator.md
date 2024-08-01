---
title: Comparable,Comparator
date: 2024-03-16 16:19:00 +0800
categories: [Java, Annotation]
tags: [Java, Comparable, Comparator]
---
# Comparable, Comparator

Comparable와 Comparator은 자바의 정렬 기준을 정의 하는 인터페이스중 하나로 , 본질적으로 의미는 똑같습니다.  

서로 공통점은 사용자 정의 클래스의 객체들을 비교할 수 있게 해주며, 인터페이스들을 구현한 사용자 정의 클래스의 객체들을 정렬하고 싶을때 임의의 정렬기준을 쉽게 정의할 수 있도록 도와주는 역할을 수행하고 있습니다.  

하지만 여기서 다른점은 Comparable은 자기 자신과 매개변수 객체를 비교하는 인터페이스 입니다.  

즉, 자기자신과 파라미터로 들어오는 객체를 비교하는 것입니다.  

이에 반대로 Comparator은 두 매개변수 객체를 비교 하는 인터페이스 입니다.  

즉, 파라미터로 들어오는 두 객체를 비교하는 것 입니다.  

## Comparable

comparable은 자바에서 객체를 정렬할 때 사용되는 인터페이스 입니다.  

### Interface

우선 Comparable을 사용할 사용자 정의 클래스에 구현합니다.  

```java
class sort implements Comparable<sort>{
}
```

implements로 구현을 정의했으면 이제 필수 메소드를 오버라이딩 해야하는데 Comparable은 단 하나의메서드만 구현하면 됩니다.  

```java
class sort implements Comparable<sort>{
    int value;
    int index;

    public sort(int index, int value) {
        this.value = value;
        this.index = index;
    }

    @Override
    public int compareTo(sort o) {
        return this.value - o.value;
    }
}
```

여기서 compareTo 메서드가 정렬 기준을 정하는 메서드 입니다.  

## 정렬 기준 - compareTo(T o1)

- 현재 객체가 파라미터로 받아온 객체보다 우선순위가 높다면 양수(+)를 반환합니다.  

- 현재 객체가 파라미터로 받아온 객체보다 우선순위가 작다면 음수(-)를 반환 합니다.  

- 현재 객체가 파라미터로 받아온 객체랑 같다면 0을 반환합니다.  

하지만 compareTo()메서드를 정의하여 사용하였다고 해도, 정렬이 되지는 않습니다.  

그 이유는 compareTo()는 단지 정렬 기준을 정의하는 메서드이기 떄문입니다.  

그리하여 실제로 정렬을 하고싶으면 compareTo()메서드를 정의하고나서 정렬 알고리즘을 실행해야 합니다.  

대표적으로 정렬하는 메소드들은 Arrays.sort(), Collections.sort()가 있습니다.  

정렬메소들을 적용한 코드예시를 하나 보여드리겠습니다.  

### 사용법

```java
import java.util.*;

class Sort implements Comparable<sort>{
    String dataname;
    int number;

    public sort(String dataname, int number) {
        this.dataname = dataname;
        this.number = number;
    }

    @Override
    public int compareTo(Sort o) {
        return this.number - o.number;
    }
}

public class Main {
    public static void main(String[] args) {
        List<sort> data= new ArrayList<>();
        data.add(new Person("A", 30));
        data.add(new Person("B", 25));
        data.add(new Person("C", 35));
        
        Collections.sort(people);
        
        for (Sort sort: data) {
            System.out.println(sort.name + ": " + sort.age);
        }
    }
}
```

위의 코드를 살펴보면, Sort 클래스가 Comparable을 구현하였습니다.  

그리고 compareTo 메서드를 구현하여 정렬 기준을 정의했습니다.  

실행 클래스(Main)에서는 List의 타입을 Sort로 지정하고 데이터를 추가한 후, Collections.sort를 사용하여 정렬을 수행합니다.  

## Comparator

Comparable은 객체 자신이 정렬 가능하도록 구현하는 것이었다면, Comparator은 타입이 동일한 객체 2개를 매개변수로 전달 받아 우선순위를 비교하는 정렬기준을 만드는것에 차이가 있습니다.  

## 정렬 기준 - compare(T o1, T o2)

- o1이 o2보다 우선순위가 높으면 양수를 반환합니다. ( + )  

- o1이 o2보다 우선순위가 낮다면 음수를 반환합니다. ( - )  

- o1과 o2의 우선순위가 같다면 0을 반환합니다.  

Comparator은 Comparable과 마찬가지로 필수 메서드는 1개지만, Comparator에서 제공하는 기능은 여러가지가 존재합니다.  

## 메서드

### compare()

두 객체를 비교하여 순서를 결정하는데 사용됩니다.  

### reversed

정렬 규칙의 반대로 정렬합니다.  

```java
Collections.sort(strings, new LengthComparator().reversed());
```

### Collections.reverseOrder, Collections.naturalOrder

내림차순 / 오름차순 정렬을 수행합니다.  

```java
Collections.sort(strings, Collections.reverseOrder(new LengthComparator()));
strings.sort(new LengthComparator());
```

### nullsFirst, nullslast

null을 맨 앞 / 뒤에 정렬 합니다.  

```java
Collections.sort(strings, Comparator.nullsFirst(new LengthComparator()));
Collections.sort(strings, Comparator.nullsLast(new LengthComparator()));
```

### comparing

비교 함수를 간단하게 구현할 수 있습니다.  

## 사용법

Comparable과 같이 Interface를 구현하고 필수 메서드를 오버라이딩 합니다.  

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> strings = new ArrayList<>();
        strings.add("apple");
        strings.add("banana");
        strings.add("grape");
        strings.add("orange");
        
        // Comparator를 사용하여 문자열을 길이에 따라 정렬
        Collections.sort(strings, new LengthComparator());
        
        // 정렬된 문자열 출력
        for (String str : strings) {
            System.out.println(str);
        }
    }
}

// Comparator를 구현한 클래스
class LengthComparator implements Comparator<String> {
    // 문자열의 길이를 기준으로 비교
    @Override
    public int compare(String str1, String str2) {
        return str1.length() - str2.length();
    }
}
```

Comparable처럼 class에 구현하고 하는 방법은 똑같지만, compare의 메서드가 조금 다릅니다.  

compare메서드 부분을 보면 파라미터로 2개의 매개변수를 전달받아서 정렬 기준을 정의하고 있습니다.  