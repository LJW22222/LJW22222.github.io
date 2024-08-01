---
title: Spring[MVC_Pattern-1]
date: 2023-03-30 22:15:00 +0800
categories: [Spring, Spring-MVC-1]
tags: [Spring,Spring-MVC]
---
# Spring MVC
Spring MVC는 일반적인 MVC의 문제점을 해결하기 위해 발전된 패턴입니다.<br/>

여기서는 FrontController라고 불리는 DispatcherServlet이 스프링 MVC의 핵심입니다.<br/>

DispatcherServlet는 클라이언트의 요청을 받아서 핸들러에게 전달하고, 결과를 받아서 뷰로 전달하여 응답을 생성하는 역할을 담당하고 있습니다.<br/>

DispatcherServlet의 핵심인 diDispatch() 코드를 보도록 하겠습니다.<br/>

## Spring MVC 동작/구조 
```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    ModelAndView mv = null;
    // 1. 핸들러 조회
    mappedHandler = getHandler(processedRequest);
    if (mappedHandler == null) {
        noHandlerFound(processedRequest, response);
        return;
    }
    // 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
    // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
    processDispatchResult(processedRequest, response, mappedHandler, mv, 
    dispatchException);
}
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {
    // 뷰 렌더링 호출
    render(mv, request, response);
}
protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
    View view;
    String viewName = mv.getViewName();
    // 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
    view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
    // 8. 뷰 렌더링
    view.render(mv.getModelInternal(), request, response);
}
```

## Spring MVC 구조/동작 순서
![spring-design-mvc-pattern-png](/assets/img/spring/spring-design-mvc-pattern.png){: width="700" height="600" }
- 클라이언트로 부터 HTTP 요청이 들어옵니다.
1. 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러 ( 컨트롤러 )를 조회합니다.

2. 핸들러를 실행할 수 있는 핸들러 어댑터를 조회합니다.

3. 핸들러 어댑터를 실행합니다.

4. 핸들러 어댑터가 실제 핸들러를 실행합니다.

5. 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환합니다.

6. View Resolver를 찾고 실행합니다.

7. View Resolver는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환합니다.

8. View를 통해서 View를 렌더링 합니다.

## Spring MVC 사용
지금까지는 어노테이션을 사용하기 전에 과거에 사용했던 방식입니다.<br/>

하지만 최근에는 많은 어노테이션을 지원하여, 보다 편리하게 사용할 수 있습니다.<br/>
```java
@Controller
@RequestMapping("/springmvc/v3/members")
public class SpringMemberControllerV3 {
 private MemberRepository memberRepository = MemberRepository.getInstance();
 @GetMapping("/new-form")
 public String newForm() {
    return "new-form";
 }
}
```
@Controller 어노테이션과 @RequestMapping 어노테이션을 사용하여 엔드포인트를 매핑하고, 중복되는 엔드포인트들을 상위 클래스에 배치하여 코드를 더 구조적으로 관리하고 있습니다.<br/>

또한, MemberRepository를 final로 선언하여 필드 값이 변경되지 않도록 보장하고 있습니다.<br/>



## 결론
Spring MVC는 DispatcherServlet을 통해 클라이언트의 HTTP 요청을 처리하고, 핸들러에게 전달하여 실행하며, 결과를 받아 뷰로 전달하여 응답을 생성하는 패턴입니다.

이를 위해 핸들러 매핑, 핸들러 어댑터, 뷰 리졸버 등의 구조를 활용하며, 최근에는 어노테이션을 사용한 개발이 흔히 적용되고 있습니다.<br/>



