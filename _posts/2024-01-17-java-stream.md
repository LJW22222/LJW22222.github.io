---
title: Java[Stream]
date: 2024-01-17 21:42:00 +0800
categories: [Java, Stream]
tags: [Stream, Java]
---
# Stream
Java의 `Stream`은 데이터를 처리하는데 사용되는 연속된 요소의 시퀀스를 나타내는 기능입니다.

이는 컬렉션, 배열, 파일 등과 같은 데이터 소스에서 간편하게 연속된 요소를 처리할 수 있는 방법을 제공합니다.

비유적으로 설명하면, `Stream`은 마치 택배 박스에서 일일이 물건을 꺼내는 대신에 박스를 열어두고 필요한 조건에 따라 선택하여 사용하는 것과 유사합니다.

하지만 무작정 Stream을 쓰는 것 보다는 현재 상황에 맞게 쓰면 됩니다. 데이터 양이 많지 않거나, 간단한 로직이면 반복문을 쓰는게 더 나을 수 있습니다.

하지만 대용량 데이터이거나, 로직이 복잡하면 Stream을 쓰는게 더 좋습니다. Stream을 쓰면 가독성이 더 좋아지고, 성능이 더 좋아지는 이점이 있습니다.

### 예시 코드

```jsx
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class Stream {

    public static void main(String[] args) throws IOException {
        int less_data = 10000;
        int more_date = 1000000;
        int number = 0;
        while (number < 3) {
            System.out.println("less_data_Yes_Stream ::: " + Yes_Stream(less_data));
            System.out.println("less_data_No_Stream ::: " + No_Stream(less_data));
            System.out.println("------------------------------------------------");
            System.out.println("more_date_Yes_Stream ::: " + Yes_Stream(more_date));
            System.out.println("more_date_No_Stream ::: " + No_Stream(more_date));
            number++;
        }
    }

    public static long Yes_Stream(int in_put_data) {
        // 데이터 초기화
        List<Integer> data = IntStream.rangeClosed(1, in_put_data)
                .boxed()
                .collect(Collectors.toList());

        // Stream을 사용한 처리
        long startTime = System.nanoTime();
        List<Integer> result = data.stream()
                .filter(n -> n % 2 == 0)
                .map(n -> n * n)
                .collect(Collectors.toList());
        long end = System.currentTimeMillis();

        long endTime = System.nanoTime();
        long duration = (endTime - startTime) / 1_000_000; // 나노초를 밀리초로 변환

        return duration;
    }

    public static long No_Stream(int in_put_data) {
        List<Integer> numbers = new ArrayList<>();
        for (int i = 1; i <= in_put_data; i++) {
            numbers.add(i);
        }

        long startTime = System.nanoTime();

        List<Integer> result = new ArrayList<>();
        for (int number : numbers) {
            if (number % 2 == 0) {
                result.add(number * number);
            }
        }

        long endTime = System.nanoTime();
        long duration = (endTime - startTime) / 1_000_000; // 나노초를 밀리초로 변환
        return duration;
    }
}
```

데이터를 많이 받아올때 의 기준은 약 10,000,000개의 데이터를 받아오고, 이중에서 짝수만 골라내서 제곱하여 List에 다시 추가하는 로직을 8번정도 반복하여 평균적으로 걸리는 시간을 추출하는 로직을 담고 있습니다.

로직의 처리시간을 측정 했을 때 다음과 같았습니다.

```java
More Data_Average Stream Time: 193 milliseconds
More Data_Average For Loop Time: 229 milliseconds
Less Data_Average Stream Time: 31 milliseconds
Less Data_Average For Loop Time: 1 milliseconds

```

데이터가 많은 경우에는 Stream을 쓰는 경우가 좀 더 빨랐습니다.

하지만 데이터가 500,000인 경우에는, 확실히 For문을 쓰는 경우가 훨씬 더 빠르다는 사실을 알 수 있습니다.

## 결론
 Java의 Stream은 데이터를 처리하는데 사용되는 연속된 요소의 시퀀스를 나타내는 기능으로, 컬렉션, 배열, 파일 등과 같은 데이터 소스에서 간편하게 연속된 요소를 처리할 수 있도록 도와줍니다.  
 
 Stream을 사용하면 데이터 처리를 위한 로직을 더 간결하고 가독성 있게 작성할 수 있으며, 대용량 데이터나 복잡한 로직의 경우 성능도 향상될 수 있습니다.  
 
 하지만 데이터 양이 적거나 간단한 로직의 경우에는 반복문을 사용하는 것이 더 효율적일 수 있습니다.