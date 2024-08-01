---
title: Java[HttpServletRequest,HttpServletResponse]
date: 2023-03-24 23:29:00 +0800
categories: [Java, Servlet]
tags: [Java,Servlet]
---
# HttpServlet
## HttpServletRequest
서블릿에서 클라이언트의 HTTP 요청에 대한 정보를 담고 있는 객체입니다.<br/>

주로 데이터에 접근할 수 있도록 도와주는 역할을 하고 있습니다.<br/>

서블릿에서 HTTP 요청을 받아오면 헤더에 대한 정보를 확인할 수 있습니다.
### 예제 코드
```java
@WebServlet(name = "requestHeaderServlet", urlPatterns = "/information")
public class loginServlet extends HttpServlet {
 @Override
 protected void service(HttpServletRequest request, HttpServletResponse
response)
 throws ServletException, IOException {
    request.getMethod();
    //Get
    request.getProtocol();
    //HTTP/2.0
    request.getScheme();
    //http://localhost:8080/information
    request.getQueryString();
    //username=example
    response.getWriter().write("ok");
 }
}
```
- http://localhost:8080/information?username=example
    - request.getMethod(); -> HTTP의 요청메시지의 Method를 확인할 수 있습니다.
    - request.getProtocol(); -> HTTP의 요청메시지의 프로토콜을 확인할 수 있습니다.
    - request.getScheme(); -> HTTP의 요청메시지의 스키마를 확인할 수 있습니다.
    - request.getQueryString(); -> HTTP의 요청메시지의 쿼리를 확인할 수 있습니다.
이 외에도 다양한 기능들이 존재합니다.

### HTTP 요청 데이터
HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법은 주로 3가지가 있습니다.
1. GET - 쿼리 파라미터
    - 메세지 바디 없이 URL에 쿼리 파라미터에 데이터를 포함해서 전달하는 방법입니다.
        - /{url}?username=example
    - 주로 검색, 필터, 페이징에 많이 사용됩니다.
2. POST - HTML Form
    - 메세지 바디에 쿼리 파라미터 형식으로 전달하는 방법입니다
        - username=example
    - 주로 회원가입, 상품주문, HTML Form에 사용됩니다.
3. HTTP message body에 데이터를 직접 담아서 요청
    - HTTP API에서 주로 사용됩니다.
        - JSON, XML, TEXT

## HttpServletResponse
요청을 받고 이에 맞는 요청을 처리한 뒤, 다시 서블릿에서 클라이언트로 응답을 보낼때 데이터를 담당하는 객체입니다.<br/>

응답의 컨텐츠 타입과, 문자 인코딩 등 설정을하고, 클라이언트에게 전송할 데이터를 생성하여 보내는 역할을 담당하고 있습니다.<br/>
### HTTP 응답 데이터
1. 단순히 텍스트로 응답합니다.
    - writer.println("ok");
2. HTML 응답
    - 예시
    ```java
    @WebServlet(name = "responseHtmlServlet", urlPatterns = "/response-html")
        public class ResponseHtmlServlet extends HttpServlet {
        @Override
        protected void service(HttpServletRequest request, HttpServletResponse
        response)throws ServletException, IOException {
            //Content-Type: text/html;charset=utf-8
            response.setContentType("text/html");
            response.setCharacterEncoding("utf-8");
            PrintWriter writer = response.getWriter();
            writer.println("<html>");
            writer.println("<body>");
            writer.println(" <div>안녕?</div>");
            writer.println("</body>");
            writer.println("</html>");
        }
    }
    ```
    - 헤더를 만들고나서, 본분 바디를 작성합니다.
3. HTTP API - MessageBody JSON 응답
    - 예시
    ```java
    @WebServlet(name = "responseJsonServlet", urlPatterns = "/response-json")
        public class ResponseJsonServlet extends HttpServlet {
        private ObjectMapper objectMapper = new ObjectMapper();
        @Override
        protected void service(HttpServletRequest request, HttpServletResponse
        response)throws ServletException, IOException {
            //Content-Type: application/json
            response.setHeader("content-type", "application/json");
            response.setCharacterEncoding("utf-8");
            HelloData data = new HelloData();
            data.setUsername("kim");
            data.setAge(20);
            //{"username":"kim","age":20}
            String result = objectMapper.writeValueAsString(data);
            response.getWriter().write(result);
        }
    }
    ```
    - 헤더를 만들고나서, 보낼 데이터 객체를 만들고, 데이터를 담은뒤에 JSON 문자로 변환합니다.
    - 단 여기서는 content-type를 application/json으로 지정해야 합니다.
    
## 결론
HttpServletRequest는 서블릿에서 클라이언트의 HTTP 요청 정보를 다루는 객체로, 주로 데이터에 접근하는 역할을 합니다. 

HttpServletResponse는 서블릿에서 클라이언트로 응답을 보내는데 사용되며, 응답의 컨텐츠 타입과 문자 인코딩 설정 및 데이터 생성과 전송 역할을 수행합니다.


