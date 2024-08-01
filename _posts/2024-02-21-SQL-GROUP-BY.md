---
title: SQL[GROUP BY]
date: 2024-02-21 22:31:00 +0800
categories: [DB, SQL]
tags: [SQL]
---
# GROUP BY
SQL의 GROUP BY는 특정 컬럼들을 그룹으로 묶어주는 함수입니다.  

이렇게 그룹화된 데이터는 그룹별로 집계 함수를 사용하여 결과를 도출할 수 있습니다.  

GROUP BY는 주로 집계함수와 함수와 함께 사용되며 데이터를 요약하거나 통계를 계산하는데 유용합니다.  

## SQL코드 예시 [ 프로그래머스 SQL 고득점 KIT 문제]
```
SELECT ANIMAL.ANIMAL_TYPE, COUNT(ANIMAL.ANIMAL_TYPE)
FROM ANIMAL_INS ANIMAL
GROUP BY ANIMAL.ANIMAL_TYPE
ORDER BY ANIMAL.ANIMAL_TYPE ASC
```
위의 SQL 코드를 보면, GROUP BY로 ANIMAL테이블의 ANIMAL_TYPE을 그룹화해서 묶은게 보입니다.  

그리고 나서 GROUP BY를 묶은 테이블을 COUNT()라는 집계 함수를 사용해서 결과를 도출한 부분이 보입니다.

즉, GROUP BY로 ANIMAL_TYPE별로 그룹을 지은다음, COUNT() 집계함수를 사용해서 TYPE별 갯수를 측정한겁니다.  

이렇게 GROUP BY는 지정한 컬럼을 그룹화 하여 집계 함수를 사용하여 결과를 도출할 수 있습니다.