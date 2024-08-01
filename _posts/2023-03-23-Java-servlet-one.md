---
title: Java[Servlet]
date: 2023-03-23 21:29:00 +0800
categories: [Java, Servlet]
tags: [Java,Servlet]
---
# Servlet and JSP
## JSP?
Servlet을 배우기전에 JSP에 대해 간단하게 알아보고 가겠습니다.

JSP는 JavaEE에서 사용되는 서버 측 웹 개발 기술입니다.

주로 동적인 컨텐츠를 생성하고, 클라이언트의 요청을 응답하는데 활용되고 있습니다.

간단하게 HTML파일 안에 Java코드를 포함하여 웹 페이지를 생성한다고 생각하시면 됩니다.

## Servlet?
Java언어를 기반으로 작성된 애플리케이션 구성 요소 중 하나입니다.<br/>

서블릿은 동적 웹 페이지를 만들거나, 기타 HTTP 요청을 처리하기 위한 자바 클래스입니다.<br/>

또한 서블릿은 Java EE, Jakatra EE 스펙의 일부이며, 웹 어플리케이션 서버에서 실행됩니다.<br/>

예를 들어, 클라이언트가 특정 URL를 요청하면, **서블릿**을 담고 있는 **서블릿 컨테이너**가 URL에 맞는 서블릿을 찾아 실행하고, 서블릿은 Java코드를 통해 요청을 처리하여 동적인 컨텐츠를 생성한 후, 클라이언트에게 응답을 보내줍니다.

### 동작과정
![spring-basic-servlet-png](/assets/img/spring/spring-basic-servlet.png){: width="700" height="600" }
1. 클라이언트가 WEB 서버로 요청을 보냅니다.

2. 웹 서버는 해당 요청을 받아서 서블릿 컨테이너로 전송합니다.

3. 서블릿 컨테이너는 매핑 정보를 web.xml이나 어노테이션에서 확인합니다. 

4. 해당 서블릿을 인스턴스화하고 init() 메서드를 호출하여 초기화합니다.

5. 클라이언트의 요청이 들어오면 service() 메서드에서 해당 요청을 처리합니다.
    - 이때 해당 요청을 처리할 때, 요청의 종류에 맞게 실행하여 처리합니다.

6. 요청의 처리 과정에서 필요한 HttpServletRequest, HttpServletResponse 객체를 생성합니다.

7. 생성된 응답 객체를 웹 서버를 통해 클라이언트에 전송합니다.

8. 서블릿 컨테이너가 종료되거나 웹 애플리케이션이 다시 로드될 때 destroy() 메서드를 호출하여 서블릿을 소멸시킵니다.

### 서블릿 등록 예제 코드
- 어노테이션으로 서블릿을 등록하는 예제 코드입니다.
```java
@WebServlet(name = "Servlet", urlPatterns = "/example")
public class Servlet extends HttpServlet {
 @Override
 protected void service(HttpServletRequest request, HttpServletResponse
response)
 throws ServletException, IOException {
 String username = request.getParameter("username");
 response.setContentType("text/plain");
 response.setCharacterEncoding("utf-8");
 response.getWriter().write("hello " + username);
 }
}
```
- HttpServlet을 상속받아, service를 오버라이드 하여 사용하고 있는 예제 코드입니다.<br/>

## 결론
JavaEE에서 사용되는 서버 측 웹 개발 기술인 JSP는 주로 동적인 웹 페이지를 생성하고 클라이언트의 요청에 응답하는데 활용되며, Servlet은 Java 언어로 작성된 애플리케이션 구성 요소로 동적인 웹 페이지를 만들거나 HTTP 요청을 처리하기 위한 클래스이며, 두 기술은 함께 사용되어 웹 애플리케이션을 개발합니다.<br/>

서블릿은 클라이언트의 요청을 받아 처리하는 동작 과정을 거치며, 서블릿 코드는 어노테이션을 통해 등록될 수 있습니다.<br/>

다음 포스터 에서는 **HttpServletRequest**, **HttpServletResponse** 에 대해 다뤄보겠습니다.<br/>


