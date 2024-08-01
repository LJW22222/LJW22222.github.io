---
title: JPQL[서브쿼리]
date: 2023-07-08 21:45:00 +0800
categories: [Spring, JPQL]
tags: [Spring, JPQL]
---
# JPQL 서브쿼리
서브쿼리는 JPQL 쿼리안에 다른 JPQL 쿼리가 포함한 것을 의미합니다.  
이를 통해 복잡합 쿼리를 단순화하거나 특정 조건에 따라 데이터를 필터링 할 수 있습니다.  

## 서브쿼리 예시
```SQL
SELECT e
FROM Employee e
WHERE e.salary > (SELECT AVG(e2.salary) FROM Employee e2 WHERE e2.department = e.department)

```
위의 서브쿼리는 같은 부서에 속하는 직원들의 평군 급여를 계산하는
WHERE 절이 해당됩니다.  

서브쿼리는 WHERE절 말고도 여러 종류가 존재합니다.  
- [NOT] EXISTS (subquery)
    - 서브 쿼리의 결과가 존재하면 참입니다.
- [NOT] IN ( subquery )
    - 서브 쿼리의 결과중 하나라도 같은 것이 있으면 참 입니다.

## 서브쿼리의 한계
- JPA는 WHERE, HAVING 절에서만 서브 쿼리를 사용 할 수 있습니다.
- SELECT 절도 가능합니다.
- 하지만, FROM 절의 서브쿼리는 불가능합니다.

## JPQL 기본 함수
- CONCAT
    - 문자열을 연결하여 하나의 문자열로 만듭니다.
- SUBSTRING
    - 문자열의 일부분을 반환합니다.
- TRIM
    - 문자열의 앞뒤에 있는 공백을 제거합니다.
- LOWER, UPPER
    - 문자열을 각각 소문자 또는 대문자로 변환합니다.
- LENGTH
    - 문자열의 길이를 반환합니다.
- LOCATE
    - 하나의 문자열이 다른 문자열 내에서 처음으로 등장하는 위치를 반환합니다.
- ABS
    - 절댓값을 반환합니다.
- SQRT
    - 제곱근을 반환합니다.
- MOD
    - 나머지를 반환합니다.
- SIZE
    - 컬렉션의 크기를 반환합니다. (주로 JPQL에서 사용됨)
- INDEX
    - 컬렉션의 인덱스를 반환합니다. (주로 JPQL에서 사용됨)