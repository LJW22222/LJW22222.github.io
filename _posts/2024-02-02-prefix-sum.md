---
title: PrefixSum
date: 2024-02-02 19:31:00 +0800
categories: [Algorithm, Algorithm]
tags: [CS, Algorithm]
---
# 구간합
어떠한 N개의 숫자를 더할 때, 일일이 더하는 방법보다는, 구간합 방식을 활용하면 시간을 엄청나게 단축 시킬 수 있습니다.

만약에 구간합을 사용하지 않고, 합 배열을 사용한다면 합을 구하는 시간복잡도가 O(N)이 소요됩니다.

하지만 구간합을 사용한다면, O(1)으로 시간복잡도가 대폭 감소하게됩니다.

## 코드 예시
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(in.readLine());
        int A = Integer.parseInt(st.nextToken());
        int B = Integer.parseInt(st.nextToken());
        int sum;
        st = new StringTokenizer(in.readLine());
        int[] result = new int[A+1];
        for (int j = 1; j <= A; j++) {
            result[j] = result[j - 1] + Integer.parseInt(st.nextToken());
        }

        for (int i = 0; i < B; i++) {
            st = new StringTokenizer(in.readLine());
            int c = Integer.parseInt(st.nextToken());
            int l = Integer.parseInt(st.nextToken());
            sum = result[l] - result[c-1];
            System.out.println(sum);
        }
    }
}
```