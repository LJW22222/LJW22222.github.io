---
title: Java[StringSplitting]
date: 2024-03-21 17:34:00 +0800
categories: [CS, OS]
tags: [Java, String]
---
# 문자열 분리
Java에서 문자열 ( String ) 을 분리하는 방법에는 substring, split 이 존재합니다.

이 두개의 메서드에 대해서 알아보겠습니다.

## split(String regex)

## split(String regex, int limit)

이 두개의 메서드는 매개 변수로 정규표현식으로 문자열만 받거나 문자열과 limit(문자열을 나눌 최대 개수)를 받습니다.

둘다 return 값은 배열로 반환합니다

### split(String regex)

```java
String str = "Hi! Hello world";
String[] dataList1 = str.split("");
String[] dataList2 = str.split(" ");
String[] dataList3 = str.split(" ", 1);
String[] dataList4 = str.split("H");

System.out.println(Arrays.toString(dataList1));
System.out.println(Arrays.toString(dataList2));
System.out.println(Arrays.toString(dataList3));
System.out.println(Arrays.toString(dataList4));

--> [H, i, !, H, e, l, l, o, w, o, r, l, d]
--> [Hi!, Hello, world]
--> [Hi!, Hello world]
--> [H, i!, H, ello world]
```

위에서 str.split(” ”, 1); 이부분이 뜻하는 바는 공백으로 문자열을 자르는데, limit갯수만 큼만 자르라는 뜻 입니다.

또한 여기서 정규표현식을 사용하여 좀 더 세밀하게 자를 수 있습니다.

```java
        String str = "Hello123world456!";
        String[] dataList5 = str.split("\\d+"); // 숫자를 기준으로 자르기
        
        System.out.println(Arrays.toString(dataList5));
        
        --> [Hello, world, !]
```

위의 코드에서 \d+는 하나 이상의 숫자를 나타내는 정규 표현식 입니다.  

이를 이용해서 세 부분으로 잘랐습니다.

### substring(int start, int end)

```java
String str = "Hello world";
String dataList1 = str.substring(3);
String dataList2 = str.substring(3, 7);

System.out.println(Arrays.toString(dataList1));
System.out.println(Arrays.toString(dataList2));

--> 
llo world
lo w
```

참고로 String에는 각 문자마다 index값이 존재합니다.

substring은 int형의 매개변수를 2개 받는데, 첫번쨰 매개변수는 컷팅할 문자열의 시작 위치입니다. 

그리고 두번 쨰 매개 변수는 끝나는 지점을 알려주는 매개변수입니다.  

즉, int start ~ int end까지 자르겠다는 뜻 입니다.