---
title: PriorityQueue
date: 2024-02-27 23:11:00 +0800
categories: [Java, PriorityQueue]
tags: [Stream, Stack, PriorityQueue]
---
# PriorityQueue ( 우선순위 큐 )

일반적인 큐는 먼저 저장된 데이터를 꺼내는 방식이지만, 이와 달리 우선순위 큐는 데이터를 우선순위에 따라 저장하고, 가장 높은 우선순위를 가진 데이터를 먼저 꺼내는 자료 구조입니다.  

우선순위 큐는 저장된 데이터가 순서대로 나오는 것이 아니라, 우선순위가 높은 데이터가 먼저 나오는 특징을 가지고 있습니다.  

만약에 우선순위가 같은 경우에는 선입선출 방식을 따르고 있습니다.  

우선순위 큐는 힙이나 리스 방식으로 구현이 되어 있습니다.  

우선순위 큐는 크게 오름차순, 내림차순, 사용자 지정 방식 등이 있습니다.  

## 오름차순

우선순위 큐의 오름차순은 Default값 입니다.  

아무것도 설정하지 않으면 기본적으로 오름차순으로 만들어 집니다. 

```java
public class AscendingPriorityQueueExample {
    public static void main(String[] args) {
        // 오름차순으로 정렬하는 우선순위 큐 생성
        PriorityQueue<Integer> ascendingQueue = new PriorityQueue<>();
    }
}
```

## 내림차순

우선순위 큐의 내림차순은  Collections.reverseOrder()라는 설정만 추가해주면 내림차순으로 설정이됩니다.  

```java
public class AscendingPriorityQueueExample {
    public static void main(String[] args) {
        // 내림차순으로 정렬하는 우선순위 큐 생성
        PriorityQueue<Integer> ascendingQueue = new PriorityQueue<>( Collections.reverseOrder());
    }
}
```

## 사용자 지정 방식

우선순위 큐의 사용자 지정방식은 Comparator인터페이스를 구현하여 사용하면 됩니다.

```java
static class sorted implements Comparable<sorted> {
        int z;

        public sorted(int z) {
            this.z = z;
        }

        @Override
        public int compareTo(sorted o) {
            if(this.z < o.z){
							return 1
						}else if(this.z > o.z){
							return -1
						}
						return 0;
        }
    }
```

위의 코드를 살펴보면 Comparator 인터페이스에 있는 compareTo 메서드를 구현하여 사용하고 있습니다.

compareTo 메서드를 보면

this.z < o.z를 만족하였을때 1을 반환하는데 이뜻은 첫 번째 인자(this.z)가 두 번째 인자보다 큼을 나타내는 걸 나타냅니다. 

this.z > o.z일 때는 첫번 째 인자가 두번 째 인자보다 작음을 나타냅니다. 

그리고 위의 2개의 조건이 해당하지 않으면 0을 반환하는데, 이는 서로 같음을 나타낸다는 뜻입니다.

결론적으로 this.z < o.z는 -1을 반환하여 오름차순, this.z > o.z는 1을 반환하여 내림차순, 마지막으로 this.z == o.z 는 같음이어서 아무일도 일어나지 않습니다.

