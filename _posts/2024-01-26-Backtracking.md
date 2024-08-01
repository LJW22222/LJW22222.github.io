---
title: Backtracking
date: 2024-01-26 23:13:00 +0800
categories: [Algorithm, Algorithm]
tags: [Backtracking, Java, Algorithm]
---
# 백트래킹
## 백트래킹 알고리즘이란

조건이 만족할 때까지 모든 가능한 경우의 수를 탐색하고, 조건이 만족하지 않으면 이전 단계로 되돌아가며 다른 경우를 탐색하는 알고리즘 기법입니다.

대표적으로 문제의 해를 찾는데 사용되고, 일반적으로 깊이 우선 탐색(DFS)와 함께 사용됩니다.

## **DFS란**

그래프를 탐색하는 데 사용되는 알고리즘 중 하나입니다.

그래프의 한 정점에서 시작하여 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색합니다.

즉, 한 분기를 끝까지 탐색하고 나서야 다음 분기로 넘어갑니다.

이런 방식으로 DFS는 더 이상 탐색할 수 없을 때 까지 분기를 탐색합니다.

보통 스택이나 재귀 함수를 사용하여 구현합니다.

## 예시 [ N과 M ]

1부터 N까지의 자연수 중에서 중복 없이 M개를 고른 수열을 모두 구하는 프로그램입니다.

입력

```
3 1
```

출력
```
1
2
3
```

### 코드예시

```java
import java.util.Scanner;

public class NAndMProblem {
    static int[] selected;
    static boolean[] visited;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int N = scanner.nextInt();
        int M = scanner.nextInt();
        
        selected = new int[M];
        visited = new boolean[N];

        solve(N, M, 0);
    }

    static void solve(int N, int M, int depth) {
        if (depth == M) {
            // 수열이 완성된 경우 출력
            for (int num : selected) {
                System.out.print(num + " ");
            }
            System.out.println();
            return;
        }

        for (int i = 0; i < N; i++) {
            if (!visited[i]) {
                visited[i] = true;
                selected[depth] = i + 1;
                solve(N, M, depth + 1);
                visited[i] = false;
            }
        }
    }
}
```

여기서는 재귀함수와 백트래킹 알고리즘을 결합하여 사용하고 있습니다.

먼저 solve라는 재귀함수를 만들고, 이 함수는 현재까지 선택된 숫자의 개수가 M과 같아지면 수열을 출력하고 종료합니다.

반대로, 그렇지 않으면 가능한 모든 숫자를 선택하고, 선택한 숫자에 대해 다음 단계의 solve 메서드를 호출합니다.

현재 선택한 숫자는 selected라는 배열에 저장되고, 해당 숫자가 이미 선택되었는지를 나타내는 visited배열을 사용하여 중복 선택을 방지합니다.