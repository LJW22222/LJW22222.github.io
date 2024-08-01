---
title: SQL[Window Funtion]
date: 2024-02-20 23:31:00 +0800
categories: [SQL, MYSQL]
tags: [CS, SQL]
---
# Window Funtion
윈도우 함수란, 특정 창 또는 윈도우 내의 행에 대한 계산을 수행하는 함수입니다.  

일반적인 집계함수와는 다르게 윈도우 함수는 결과 집함의 각 행에 대해 계산을 수행하고, 결과에는 원본 데이터가 포함됩니다.  

즉, 집계함수는 원본 데이터에 특정 연산을 수행하고 결과를 도출합니다.  

참고로 집계함수는 특정 열의 값들을 하나로 합치거나 통계적인 정보를 계산하는 함수입니다. ( EX : SUM(), AVG(), COUNT(), MIN(), MAX() 등)

하지만 이 과정에서 원본 데이터는 사라지고 결과 데이터로 대체되어 원본데이터가 사라진다는 점이 존재합니다.  

하지만 윈도우 함수는 각 행에 대해 결과를 계산하고, 결과 집합에는 각 행에 대한 원본 데이터가 포함됩니다.  

즉, 원본데이터가 보존이되고, 결과 데이터도 같이 도출한다는 뜻 입니다.

이 점이 집계함수와 윈도우 함수의 큰 차이점 입니다.  

이제 사용법과 윈도우 함수의 종류에 대해 알아보겠습니다.  

## 사용법
```
SELECT employee_id, first_name, last_name, department_id, salary
FROM (
    SELECT employee_id, first_name, last_name, department_id, salary,
           ROW_NUMBER() OVER(PARTITION BY department_id ORDER BY salary DESC) AS rank
    FROM employees
) ranked_employees
WHERE rank = 1;

```
```
SELECT employee_id, first_name, last_name, department_id, salary,
       ROW_NUMBER() OVER(PARTITION BY department_id ORDER BY salary DESC) AS rank
FROM employees;
```
윈도우 함수는 SELECT, ORDER BY등 다양한 절에서 사용이 가능합니다.  

기본적인 사용법은 먼저  
***" 윈도우 함수 + OVER() + AS "컬럼이름 자유롭게"***   
형식으로 사용됩니다.  

참고로 위의 예시에서 OVER뒤에 붙은 PARITITION은 데이터를 그룹화 하는데 사용됩니다.  

즉, 위의 예시를 경우로 들면 employment 테이블의 데이터를 department_id 열의 값으로 그룹화를 합니다.  

따라서 각 부서별로 데이터가 별도의 윈도우로 처리되며, ROW_NUMBER() 함수는 각 부서별로 직원의 급여를 내림차순으로 순위를 매깁니다.  

## Window Funtion 종류
### ROW_NUMBER()
결과 집합에서 각 행에 순차적인 정수 값을 할당합니다.  

### RANK()
값의 순위를 할당하고, 값이 같으면 동일한 순서를 부여하고 그 다음 순위는 건너뜁니다.  

### DENES_RANK()
값의 순위를 할당하고, 값이 같으면 동일한 순위를 부여하지만, 다음 순위는 이전 순위보다 1이 큽니다.  

### NTILE(n)
결과 집합을 n개의 동일한 크기와 그룹으로 나누고, 각 행에 대해 해당하는 그룹 번호를 할당합니다.  

### SUM()
지정된 열의 값에 대한 합계를 계산합니다.

### AVG()
지정된 열의 값에 대한 평균을 계산합니다.

### MIN()
지정된 열의 최소값을 반환합니다.  

### MAX()
지정된 열의 최대값을 반환합니다.  

### LEAD()
현재 행 다음의 특정 행의 값을 반환합니다.  

### FIRST_VALUE()
지정된 열의 첫 번째 값을 반환합니다.

### LAST_VALUE()
지정된 열의 마지막 값을 반환합니다.
