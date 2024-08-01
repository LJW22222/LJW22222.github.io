---
title: Spring[Socket]
date: 2024-01-09 23:30:00 +0800
categories: [Spring, SpringFrameWork]
tags: [Spring, Socket]
---
# Socket
## HTTP 통신
일반적인 HTTP 통신은 클라이언트 - 서버 간의 통신을 얘기하는데, HTTP 통신은 클라이언트가 서버로 요청을 보내고, 다시 서버가 클라이언트로 응답을 보내는데 이거를 HTTP 통신이라고 합니다.<br/>
또한 이런 특성을 단방향 통신이라고 하는데, 이런 특성 때문에  양방향 통신이 불가능합니다.<br/>
클라이언트가 서버로 통신을 하면 지속적으로 통신을 하는게 아닌, 한번 통신후 종료되고, 다시 통신 연결을 해야하기 때문에 자원 낭비가 발생 합니다.<br/>

## Socket
이런 단점을 해결하기 위해 등장한 기술이 Socket 통신입니다. <br/>
Socket통신은 HTTP 통신과 다르게, 한번 연결을 하고 나면 다시 연결을 해주지 않아도 됩니다.<br/>
또한 양방향 통신이기 때문에 서로 실시간으로 통신이 가능하다는 특성이 있습니다.<br/>
이로 인해 낭비되는 자원 소모가 적다는 점이 장점입니다.<br/>
주로 실시간 웹 어플리케이션, 채팅, 온라인 게임 등과 같은 빠른 응답이 필요한 상황이나 실시간으로 통신을 해야하는 상황 에서 효과적으로 쓰일 수 있습니다.<br/>

### 연결 과정
1. HandShake
    1. 클라이언트가 서버에게 웹 소켓 연결을 요청합니다.
    2. 클라이언트와 서버 간에 HandShake가 이루어집니다.
    3. 클라이언트가 HTTP 요청을 서버에 보내고, 서버는 이에 응답하여 연결을 수락합니다.
        - 1,2,2 → HTTP → Socket 업그레이드 과정
2. 웹 소켓 연결
    1. 클라이언트와 서버 간에 웹 소켓 연결이 설정됩니다.
3. 통신 유지
    1. 클라이언트와 서버가 명시적으로 종료하지 않는 한 유지됩니다.
    2. 이를 통해 양방향 통신이 가능합니다.
4. 종료
    1. 클라이언트나 서버 중 하나가 연결을 종료하고자 하면, 종료하기 위한 close handshake가 이루어집니다.

### 예제 코드
- **ChatHandler.java**
    ```java
    @Slf4j
    @Component
    @RequiredArgsConstructor
    public class ChatHandler extends TextWebSocketHandler {
    
        private final ChatService chatService;
        private final ObjectMapper objectMapper;
    
        //소켓 연결 확인
        @Override
        public void afterConnectionEstablished(WebSocketSession session) throws Exception {
            log.info("{} 연결됨", session.getId());
    
        }
        //소켓 통신 시 메세지의 전송을 다룸
        @Override
        public void handleTextMessage(WebSocketSession session, TextMessage message) {
            // ...
        }
        private  void sendToEachSocket(Set<WebSocketSession> sessions, TextMessage message){
            // ...
        }
        //소켓 종료 확인
        @Override
        public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
            // ...
        }
    }
    ```
- ChatHandler.java 파일은 Spring에서 WebSocket 연결, 메시지 처리, 소켓 연결 확인 및 종료를 다루는 핸들러 클래스로, TextWebSocketHandler를 상속받아 WebSocket에 대한 이벤트를 처리합니다.<br/>

- **WebSocketConfig.java**
    ```java
    @Configuration
    @EnableWebSocket
    @RequiredArgsConstructor
    public class WebSocketConfig implements WebSocketConfigurer {
    
        private final ChatHandler chatHandler;
    
        @Override
        public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
            registry.addHandler(chatHandler, "/ws/chat").setAllowedOrigins("*");
        }
    }
    ```
    WebSocketConfig.java파일은 Spring에서 WebSocket을 설정하고, "/ws/chat" 경로로 들어오는 WebSocket 요청을 처리하는 핸들러로 ChatHandler를 등록하는 클래스입니다.<br/>
    또한, CORS(Cross-Origin Resource Sharing) 문제를 해결하기 위해 모든 origin으로부터의 WebSocket 연결을 허용하도록 설정되어 있습니다.<br/>



## 결론
소켓 연결확인, 통신시 메세지의 전송을 다루는 부분, 소켓 종료 확인 등, 여러가지 기능을 지원하고 있습니다.<br/>
많은 기능들이 있습니다. 자세한 내용은 <br/>
[스프링 socket 공식문서](https://docs.spring.io/spring-framework/docs/4.3.x/spring-framework-reference/html/websocket.html)에서 확인 가능합니다.

Handler파일과 config파일이 핵심 파일이며, Spring MVC 패턴을 적용하여 Service,Controller 등을 접목하여 다양한 기능을 개발할 수 있습니다.

