---
title: SlidingWindow
date: 2024-02-13 21:31:00 +0800
categories: [Algorithm, Algorithm]
tags: [CS, Algorithm]
---
# SlidingWindow
## 슬라이딩 윈도우 알고리즘

고정 크기의 윈도우가 배열이나 연속적인 데이터 구조를 따라 이동하면서 문제를 해결하는 알고리즘 기법입니다.  

이 알고리즘은 배열 또는 연속적인 데이터 구조에서 연속된 부분집합(윈도우)을 처리할 때 유용하게 사용되는 알고리즘 기법입니다.  

주로 부분집합의 합, 최대값, 최소값 등을 구하는 문제에 적용됩니다.

또한 슬라이딩 윈도우 알고리즘은, 2개의 포인터로 범위를 지정한 다음, 범위를 유지한 채로 이동하며 문제를 해결하는 방식이어서 투 포인터 알고리즘과 방식이 매우 비슷하고 원리도 간단합니다.

## 주요 아이디어

1. 초기에 윈도우를 설정하고, 윈도우의 크기를 고정합니다.
2. 윈도우를 한번에 한 칸씩 이동시키며 새로운 요소를 추가하고 이전 요소를 제거합니다.
3. 새로운 요소를 추가한 후에는 윈도우 내의 데이터를 사용하여 필요한 계산을 수행합니다.
4. 윈도우를 끝까지 이동시키면서 필요한 계산을 반복하고 결과값을 얻습니다.

## 특징

데이터를 한번만 처리하면서 원하는 결과를 얻을 수 있습니다.  

이로 인해 효율적인 시간 복잡도를 가질 수 있습니다.  

슬라이딩 윈도우 알고리즘은 연속적인 데이터 구조를 처리하는 많은 문제에 적용될 수 있으며, 특히 데이터의 크기가 크거나, 최적화가 필요한 경우 유용하게 사용됩니다.

## 종류

- 고정 크기의 슬라이딩 윈도우
- 가변 크기의 슬라이딩 윈도우
- 최소값이나 최대값을 구하는 슬라이딩 윈도우
- 부분집합의 합을 구하는 슬라이딩 윈도우

## 장점

1. **시간 복잡도 개선**: 슬라이딩 윈도우 알고리즘은 데이터를 한 번만 처리하면서 원하는 결과를 얻을 수 있습니다. 따라서 데이터를 다시 처리하지 않아도 되므로 시간 복잡도를 개선할 수 있습니다.
2. **메모리 효율성**: 슬라이딩 윈도우 알고리즘은 고정된 크기의 윈도우를 사용하여 문제를 해결하므로 필요한 메모리의 양이 고정되어 있습니다. 이는 메모리 효율성을 높이는 데 도움이 됩니다.
3. **쉬운 구현**: 슬라이딩 윈도우 알고리즘은 간단한 아이디어를 기반으로 하고 있으며, 주어진 문제에 대한 직관적인 해결 방법을 제공합니다. 이로 인해 상대적으로 쉽게 구현할 수 있습니다.

## 활용 상황

1. **부분집합의 합**: 주어진 배열에서 특정 합계를 갖는 부분집합을 찾는 문제에 슬라이딩 윈도우 알고리즘을 적용할 수 있습니다.
2. **최대값 또는 최소값 구하기**: 배열이나 리스트에서 연속된 부분집합 중에서 최대값 또는 최소값을 찾는 문제에 슬라이딩 윈도우 알고리즘을 사용할 수 있습니다.
3. **특정 개수 이상의 문자열 패턴 찾기**: 문자열에서 특정 문자 또는 문자열 패턴이 특정 개수 이상 등장하는 부분문자열을 찾는 문제에 슬라이딩 윈도우 알고리즘을 적용할 수 있습니다.
4. **특정 조건을 만족하는 부분집합 찾기**: 배열 또는 리스트에서 특정 조건을 만족하는 연속된 부분집합을 찾는 문제에 슬라이딩 윈도우 알고리즘을 사용할 수 있습니다.
5. **데이터의 윈도우 기반 분석**: 데이터 스트림에서 일정한 윈도우를 유지하면서 통계 또는 분석을 수행하는 데 슬라이딩 윈도우 알고리즘을 활용할 수 있습니다.

## 코드 예제 [ 백준 12891번 문제 ]
```java
package Silver;

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
        String[] dataList = new String[A];
        int[] numberList = new int[4];
        int[] check = new int[4];
        String cc = in.readLine();
        int result = 0;
        for (int i = 0; i < A; i++) {
            String tt = String.valueOf(cc.charAt(i));
            dataList[i] = tt;
        }
        st = new StringTokenizer(in.readLine());
        for (int z = 0; z < 4; z++) {
            numberList[z] = Integer.parseInt(st.nextToken());
        }

        for (int i = 0; i < B; i++) {
            switch (dataList[i]) {
                case "A":
                    check[0]++;
                    break;
                case "C":
                    check[1]++;
                    break;
                case "G":
                    check[2]++;
                    break;
                case "T":
                    check[3]++;
                    break;
            }
        }

        if (numberList[0] <= check[0] &&
                numberList[1] <= check[1] &&
                numberList[2] <= check[2] &&
                numberList[3] <= check[3]) {
            result++;
        }

        for (int c = B; c < A; c++) {
            switch (dataList[c - B]) {
                case "A":
                    check[0]--;
                    break;
                case "C":
                    check[1]--;
                    break;
                case "G":
                    check[2]--;
                    break;
                case "T":
                    check[3]--;
                    break;
            }
            switch (dataList[c]) {
                case "A":
                    check[0]++;
                    break;
                case "C":
                    check[1]++;
                    break;
                case "G":
                    check[2]++;
                    break;
                case "T":
                    check[3]++;
                    break;
            }
            if (numberList[0] <= check[0] &&
                    numberList[1] <= check[1] &&
                    numberList[2] <= check[2] &&
                    numberList[3] <= check[3]) {
                result++;
            }
        }
        System.out.println(result);
    }
}
```