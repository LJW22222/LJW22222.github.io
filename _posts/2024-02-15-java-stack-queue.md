---
title: Stack And Queue
date: 2024-02-17 23:31:00 +0800
categories: [Java, Stream]
tags: [Stream, Stack, Queue]
---
# Stack,Queue
## Stack
스택(Stack)은 후입선출(LIFO) 방식을 사용하는 데이터 구조입니다. 

이는 가장 최근에 추가된 항목이 가장 먼저 추가되거나  제거되는 방식입니다.

 스택은 일반적으로 한 쪽 끝에서만 데이터의 추가와 제거가 이루어집니다. 

이것은 스택의 "막혀있는" 한 쪽 끝을 보통 "맨 위" 또는 "탑"이라고 하며, 다른 쪽은 "열려있는" 쪽으로 여겨집니다.

 따라서 스택에 데이터를 추가할 때는 이 "열려있는" 쪽에 추가되고, 데이터를 제거할 때는 "막혀있는" 쪽에서 제거됩니다.

### Stack 용어

- top : 삽입과 삭제가 일어나는 위치를 말합니다.
- push : top 위치에 새로운 데이터를 삽입하는 연산입니다.
- pop : top 위치에 현재 있는 데이터를 삭제하고 확인하는 연산입니다.
- peek : top 위치에 현재 있는 데이터를 단순 확인하는 연산입니다.

### Stack 사용법

```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<String> stack = new Stack<>();

        // 요소 추가
        stack.push("첫 번째");
        stack.push("두 번째");
        stack.push("세 번째");

        // 요소 출력 및 제거
        while (!stack.isEmpty()) {
            System.out.println(stack.pop()); // 후입선출로 출력됨
        }
    }
}

```

## Queue

큐(Queue) 선입선출(FIFO) 방식을 사용하는 데이터 구조입니다.

이는 마지막으로 추가된 항목이 가장 먼저 추가되거나 제거되는 방식입니다.

큐는 한쪽에서 데이터의 추가가 이뤄지고, 다른 한쪽에서는 데이터의 삭제가 이루어집니다.

큐는 데이터를 추가하는 쪽을 rear라고 하고, 데이터를 삭제하는 쪽을 front라고 합니다.

따라서 큐는 데이터를 추가할때는 rear쪽에 추가가되고, 제거할 때는 front쪽에서 제거가 됩니다.

### Queue 용어

- rear : 큐에서 가장 끝 데이터를 가리키는 영역입니다.
- front : 큐에서 가장 앞의 데이터를 가리키는 영역입니다.
- add : rear 부분에 새 데이터를 삽입하는 연산입니다.
- poll : front부분에 있는 데이터를 삭제하고 확인하는 연산입니다.
- peek : 큐의 front에 있는 데이터를 확인할 때 사용되는 연산입니다.

### Queue 사용법

```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
    public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();

        // 요소 추가
        queue.offer("첫 번째");
        queue.offer("두 번째");
        queue.offer("세 번째");

        // 요소 출력 및 제거
        while (!queue.isEmpty()) {
            System.out.println(queue.poll()); // 선입선출로 출력됨
        }
    }
}

```

## 결론

스택은 주로 DFS(깊이 우선 탐색)알고리즘에 주로 사용됩니다. 

또한 백트래킹 종류의 코딩 테스트에 효과적 입니다.

이런 이유는 후입선출이 개념 자체가 재귀 함수 알고리즘 원리와 똑같기 때문입니다.

큐는 주로 BFS(너비 우선 탐색)알고리즘에서 주로 사용됩니다.