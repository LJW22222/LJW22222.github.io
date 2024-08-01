---
title: PriorityQueue
date: 2024-03-01 19:40:00 +0800
categories: [Algorithm, Algorithm]
tags: [CS, Algorithm, PriorityQueue]
---
# 우선순위 큐를 이용한 알고리즘
먼저 앞서 우선순위 큐에 대해 간단하게 알아보고 가겠습니다.  

우선순위 큐는 일반적인 큐와 달리 데이터를 우선순위에 따라 저장하고, 가장 높은 우선순위를 가진 데이터를 먼저 꺼내는 자료 구조입니다.  

우선순위 큐는 저장된 데이터가 순서대로 나오는 것이 아니라, 우선순위가 높은 데이터가 먼저 나오는 특징을 가지고 있습니다.

이런 특징을 이용해서 다양한 알고리즘에 활용될 수 있는데 대표적으로 다익스트라 알고리즘, 최단경로 알로리즘 등 다양한 곳에서 활용됩니다. 

여기서는 절댓값 힙을 구하는 알고리즘을 작성해보겠습니다.

## 코드 예시 [ 백준 11286번 문제 ]
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {
    static PriorityQueue<sorted> qlist;
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(in.readLine());
        int a = Integer.parseInt(st.nextToken());
        qlist = new PriorityQueue<>();
        for (int i = 0; i < a; i++) {
            st = new StringTokenizer(in.readLine());
            int c = Integer.parseInt(st.nextToken());
            if (c == 0) {
                if (!qlist.isEmpty()) {
                    sorted poll = qlist.poll();
                    System.out.println(poll.z);
                }else{
                    System.out.println("0");
                }
            }else{
                qlist.add(new sorted(c));
            }
        }
    }

    static class sorted implements Comparable<sorted> {
        int z;

        public sorted(int z) {
            this.z = z;
        }

        @Override
        public int compareTo(sorted o) {
            if (Math.abs(this.z) < Math.abs(o.z)) {
                return -1;
            }else if(Math.abs(this.z) > Math.abs(o.z)){
                return 1;
            }else{
                if (this.z < o.z) {
                    return -1;
                }else if (this.z > o.z) {
                    return 1;
                }
            }
            return 0;
        }
    }
}
```

## 동작과정
```java
    static class sorted implements Comparable<sorted> {
        int z;

        public sorted(int z) {
            this.z = z;
        }

        @Override
        public int compareTo(sorted o) {
            if (Math.abs(this.z) < Math.abs(o.z)) {
                return -1;
            }else if(Math.abs(this.z) > Math.abs(o.z)){
                return 1;
            }else{
                if (this.z < o.z) {
                    return -1;
                }else if (this.z > o.z) {
                    return 1;
                }
            }
            return 0;
        }
    }
```
1. compareTo 메소드는 현재 객체와 비교 대상 객체를 받아와서 비교합니다.

2. 먼저 Math.abs() 메소드를 사용하여 현재 객체와 비교 대상 객체의 z 값을 절대값으로 변환하여 비교합니다.

3. 절대값이 다를 경우에는 절대값이 작은 객체를 먼저 오게 하기 위해 -1 또는 1을 반환하여 정렬합니다.

4. 절대값이 같을 경우에는 원래 값 자체를 비교하여 정렬합니다. 이 때, 양수일 경우에는 1, 음수일 경우에는 -1, 같을 경우에는 0을 반환하여 정렬합니다.
4. 절대값이 같을 경우에는 원래 값 자체를 비교하여 정렬합니다. 이 때, 양수일 경우에는 1, 음수일 경우에는 -1, 같을 경우에는 0을 반환하여 정렬합니다.