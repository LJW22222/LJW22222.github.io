---
title: Spring[Mock MVC]
date: 2024-03-03 19:40:00 +0800
categories: [Spring, SpringFrameWork]
tags: [Spring, Mock MVC]
---
# Mock MVC

Mock MVC는 주로 Spring MVC에서 C에 해당하는 컨트롤러를 테스트할 때 사용됩니다.  

이를 통해 서버를 실행시키지 않고 로컬에서 HTTP 요청 및 응답을 모의하여 컨트롤러 동작을 테스트 할 수 있습니다.  

서버를 실행시키지 않고 로컬에서 테스트를 하게 되면 테스트를 더 빠르고 격리된 환경에서 실행이 가능하며, 외부 의존성에 영향을 받지 않고 컨트롤러 동작을 확인할 수 있다는 장점이 존재합니다.  

Mock MVC는 스프링 MVC 애플리케이션을 테스트하기 위한 프레임워크입니다. Mock MVC는 다음과 같은 메서드들을 제공합니다1.

## 주요 메서드

### 1. mockMvc.perform(requestBuilder)

주어진 요청 빌더로부터 요청을 생성하고, Mock MVC에 디스패치합니다. 

이 메서드는 **`ResultActions`** 객체를 리턴합니다. 

**`requestBuilder`**는 **`MockMvcRequestBuilders`** 클래스의 정적 메서드를 사용하여 생성할 수 있습니다

### 1 - 1. 메서드 종류

- **`get()`**, **`post()`**, **`put()`**, **`delete()`**

[MockMvcRequestBuilders (Spring Framework 6.1.4 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/request/MockMvcRequestBuilders.html)

### 2. resultActions.andExpect(resultMatcher)

주어진 결과 매처로부터 기대하는 결과를 검증합니다. 

이 메서드는 **`ResultActions`** 객체를 리턴합니다. 

**`resultMatcher`**는 **`MockMvcResultMatchers`** 클래스의 정적 메서드를 사용하여 생성할 수 있습니다

### 2 - 1. 메서드 종류

- **`status()`**, **`view()`**, **`model()`**, **`content()`**, **`header()`**, **`cookie()`**

[MockMvcResultMatchers (Spring Framework 6.1.4 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/test/web/servlet/result/MockMvcResultMatchers.html)

### 3. resultActions.andDo(resultHandler)

주어진 결과 핸들러로부터 결과를 처리합니다. 

이 메서드는 **`ResultActions`** 객체를 리턴합니다. 

**`resultHandler`**는 **`MockMvcResultHandlers`** 클래스의 정적 메서드를 사용하여 생성할 수 있습니다

### 3 - 1 메서드 종류

- **`print()`**, **`log()`**, **`debug()`**

### 4. resultActions.andReturn()

**`MvcResult`** 객체를 리턴합니다. 

**`MvcResult`** 객체는 요청과 응답에 대한 정보를 담고 있습니다.

### 4 - 1 메서드 종류

- **`getRequest()`**, **`getResponse()`**, **`getHandler()`**, **`getModelAndView()`**

이 외에도 다양한 종류의 메서드들이 존재합니다.

## Mock Mvc 사용법

Mock Mvc는 test파일에서 사용합니다.  

### 코드 예시

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void TestUserController() throws Exception {
        //GET /user/1
        mockMvc.perform(MockMvcRequestBuilders.get("/user/{id}", 1)
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.id").value(1));
    }

    @Test
    public void testPostExample() throws Exception {
        //POST /user
        mockMvc.perform(MockMvcRequestBuilders.post("/user")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\": \"TestUser\"}"))
                .andExpect(MockMvcResultMatchers.status().isOk());
    }
}

```

### GET 방식

1. GET TEST 코드를 보면 먼저 HTTP GET 요청을 생성합니다. 
    - 엔드 포인트 /user/{id}로 생성합니다 ( 여기서는 1 )
2. 요청 컨텐츠의 타입을 json으로 설정합니다. ( contentType() )
3. 요청이 성공적으로 처리되고 HTTP 상태 코드가 200인지 확인합니다
4. 응답본문의 JSON 경로가 `$id`가 1의 값을 갖는지 확인합니다.

### POST 방식

1. 설정된 엔트포인트(/user)로 요청을합니다. 
2. HTTP POST 요청을 생성합니다.
3. 요청의 콘텐츠 타입을 JSON으로 설정합니다.
4. 요청 본문에 JSON 데이터를 설정하는데, 여기서는 `name` 속성이 “TestUser”인 JSON 객체를 전송합니다.
5. 요청이 성공적으로 처리되고 HTTP상태 코드가 200인지 확인합니다.

이처럼 GET 및 POST 요청을 보내고 그에 대한 응답을 확인하여 해당 컨트롤러가 제대로 동작하는지 확인할 수 있습니다.
