---
title: Spring JPA[AOP]
date: 2023-06-17 22:31:00 +0800
categories: [Spring, JPA]
tags: [Spring, JPA, AOP]
---

# AOP 용어 정리
AOP에 대해 알아보기전에 주로 사용하는 용어에 대해 간단하게 알아보고 가겠습니다.
## Aspect
- 관심사를 모듈화한 것입니다.

## Advice
- Aspect에서 특정 지점에서 실행되어야 하는 코드 블록입니다.
- Before, Afrer, Around 등 다양한 종류의 조언이 있으며, 각각 다른 시점에 실행됩니다.

## Join Point
- Adivce가 실행되는 특정 시점을 가리킵니다.
- 메소드 호출, 객체 생성 등과 같은 프로그램 실행 중 특정 지점들이 결합점이 될 수 있습니다.

## Pointcut
- 결함점의 집합으로, 어떤 Join Point에서 Advice를 실행할지를 정의합니다.

## Weaving
- Aspect와 일반 프로그래밍 코드를 합치는 프로세스를 나타냅니다.
- 컴파일 타임, 로드 타임, 런타임 등 다양한 시점에서 이루어질 수 있습니다.

## AspectJ
- Java에서 AOP를 구현하기 위한 대표적인 언어 및 도구 중 하나입니다.
- AspectJ 언어는 Java언어를 기반으로 하며, AOP 개념을 자바에 적용할 수 있도록 해줍니다.
