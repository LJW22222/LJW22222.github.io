---
title: Spring[JUnit5]
date: 2024-03-03 15:40:00 +0800
categories: [Spring, SpringFrameWork]
tags: [Spring, JUnit5]
---
# JUnit5

JUnit은 자바에서 사용되는 단위 테스트 프레임워크 입니다.   

Spring boot 2.2.0 이전에는 JUnit4가 기본으로 설정이 되어 있었지만, Spring boot 2.2.0버전 이후부터는 JUnit5가 기본으로 설정이 됩니다.  

JUnit5는 JUnit Platform + JUnit Jupiter + JUnit Vintage로 구성되어 있습니다.  

## JUnit Platform

- JVM에서 동작하는 테스트 프레임워크입니다.
- 이 플랫폼 위에서 동작하는 테스트 프레임워크를 개발하기위한 TestEngine API를 정의합니다.

## JUnit Jupiter

- JUnit5에서 제공하는 TestEngine API의 구현체입니다.
- 파라미터화된 테스트, 중첩 테스트, 동적 테스트 등 다양한 스타일과 패러다임을 제공하며, 모듈러 아키텍처로 인해 확장성과 커스터마이징이 용이합니다.

## JUnit Vintage

- JUnit3, JUnit4에서 제공하는 TestEngine API의 구현체입니다.
- JUnit5를 사용해도 이전 버전의 테스트를 계속 사용할 수 있게 도와줍니다.

## JUnit5 어노테이션

### @Test

- 테스트 Method임을 정의합니다.

### @ParameterizedTest

- 파라미터화된 테스트 메서드를 정의합니다.
- 즉, 매개변수를 받는 테스트를 작성합니다.

### @RepeatedTest

- 테스트 메서드를 반복적으로 실행하는 어노테이션입니다.

### @TestFactory

- @Test로 선언된 정적 테스트가 아닌 동적으로 테스트를 사용합니다.

### @TestInstance

- 테스트 클래스의 생명주기를 결정합니다.

### @TestTamplate

- 공급자에 의해 여러 번 호출될 수 있도록 설계된 테스트 케이스 플랫폼임을 나타냅니다.

### @TestMethodOrder

- 테스트 메소드 실행 순서를 구성에 사용합니다.

### @DisplayName

테스트 클래스, 메소드의 사용자 정의 이름을 선언시 사용합니다.

### @BeforeEach

- 각 테스트 메소드 실행 전에 실행되는 메소드를 정의합니다.

### @AfterEach

- 각 테스트 메소드 실행 후에 실행되는 메소드를 정의합니다.

### @BeforeAll

- 모든 테스트 메소드 실행 전에 실행되는 메소드를 정의합니다.
- static으로 선언해야 합니다.

### @AfterAll

- 모든 테스트 메소드 실행 후에 실행되는 메소드를 정의합니다.
- static으로 선언해야 합니다.

### @Nested

- 클래스를 정적이 아닌 중첩 테스트 클래스임을 나타냅니다.

### @Tag

- 클래스 또는 메소드 레벨에서 태그를 선언할 때 사용합니다.

### @Disabled

- 이 클래스나 테스트를 사용하지 않습니다.

### @Timeout

- 테스트 실행 시간을 선언 후 초과하면 실패하도록 설정합니다.

### @ExtendWith

- 확장을 선언적으로 등록할 때 사용합니다.

### @RegisterExtension

- 필드를 통해 프로그래밍 방식으로 확장을 등록할 때 사용합니다.

### @TempDir

- 필드 주입 또는 매개변수 주입을 통해 임시 디렉토리를 제공하는데 사용합니다.

## JUnit5 Assertions 메소드

| Method | JUnit | 설명 |
| --- | --- | --- |
| assertEquals(expected, actual) | JUnit4 ~ | 예상값과 실제값이 동일한지 확인합니다. |
| assertArrayEquals(expected, actual) | JUnit4 ~ | 예상 배열과 실제 배열이 동일한지 확인합니다. |
| assertNull(object) | JUnit4~ | 객체가 null인지 확인합니다. |
| assertNotNull(object) | JUnit4~ | 객체가 null이 아닌지 확인합니다. |
| assertSame(expected, actual) | JUnit4~ | 예상값과 실제값이 다른 객체인지 확인합니다. |
| assertNotSame(expected, actual) | JUnit4~ | 예상값과 실제값이 다른 객체인지 확인합니다. |
| assertTrue(condition) | JUnit4~ | 조건이 참인지 확인합니다. |
| assertFalse(condition) | JUnit4~ | 조건이 거짓인지 확인합니다. |
| assertAll((Executable... executables) | JUnit5 | 모든 단언문(assertion)이 성공하는지 확인합니다. |
| assertIterableEquals(Iterable<?> expected, Iterable<?> actual) | JUnit5 | 예상 가능한 순서로 반복 가능한 항목이 동일한지 확인합니다. |
| assertLinesMatch((List<String> expectedLines, List<String> actualLines) | JUnit5 | 두 문자열 목록이 일치하는지 확인합니다. |
| assertNotEquals(Object unexpected, Object actual) | JUnit5 | 예상값과 실제값이 동일하지 않은지 확인합니다. |
| assertThrows(Class<T> expectedType, Executable executable) | JUnit5 | 지정된 예외가 발생하는지 확인합니다. |
| assertTimeout(Duration timeout, Executable executable) | JUnit5 | 주어진 시간 내에 작업이 완료되는지 확인합니다. |
| assertTimeoutPreemptively(Duration timeout, Executable executable) | JUnit5 | 주어진 시간 내에 작업이 완료되는지 확인하며, 작업이 중단될 수 있습니다. |

## BDD
BDD는 TDD를 근간으로 파생된 개발 방법입니다.  

TDD에서 한발 더 나아가 테스트 케이스 자체가 요구사항이 되도록 하는 개발 방법입니다.  

TDD에 대해서 잘 모르신다면 [TDD](/_posts/2024-01-12-spring-TDD.md)게시글을 참고하시면 됩니다.

### BDD 기본 패턴
- Feature : 테스트에 대상의 기능/책임을 명시합니다.
- Scenario : 테스트 목적에 대한 상황을 설명합니다. 
- Given : 시나리오 진행에 필요한 값을 설정합니다.
- When : 시나리오를 진행하는데 필요한 조건을 명시합니다.
- Then : 시나리오를 완료했을 때 보장해야 하는 결과를 명시합니다.


## JUnit 기본 사용법
JUnit은 Spring Porject의 파일 목록중, test 파일에서 Test 클래스를 만들어서 사용하시면 됩니다. 

아래의 테스트 클래스는 BDD의 기본패턴을 적용하였습니다.

```java
public class CalculatorService {
    public int add(int a, int b) {
        return a + b;
    }
}

```

예를 들어서 덧셈을 해주는 Calculator 클래스가 있다고 가정하면 테스파일에서는 아래와 같이 사용할 수 있습니다.

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

@SpringBootTest
public class CalculatorTest {

    @Autowired
    private CalculatorService calculatorService;

    @Test
    void givenTwoNumbers_whenAdd_thenSumReturned() {
        // Given
        int a = 2;
        int b = 3;

        // When
        int result = calculatorService.add(a, b);

        // Then
        assertEquals(5, result, "2와 3의 합은 5가 되어야 합니다");
    }
}

```
calculatorService라는 클래스를 빈으로 주입 받아서 testAdd에서 호출하여 사용할 수 있습니다.

## 결론
JUnit5를 활용하여 많은 어노테이션과 메서드를 사용하여 Service 레이어의 단위 테스트를 진행할 수 있습니다.  

BDD패턴인 Given - When - Then 사용하여 좀 더 명확한 테스트를 할 수 있습니다.  

이를 통해 테스트 케이스의 가독성과 유지보수를 높일 수 있다는 장점이 있습니다.  

