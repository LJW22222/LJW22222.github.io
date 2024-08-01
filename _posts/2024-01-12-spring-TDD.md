---
title: Spring[TDD]
date: 2024-01-12 20:30:00 +0800
categories: [Spring, SpringFrameWork]
tags: [Spring, TDD]
---
# TDD
테스트 주요 개발의 약자입니다.

주로 소프트웨어를 개발하는 방법론 중 하나로, 코드를 작성하기 전에 테스트를 작성하고, 그 테스트를 통과시키는 코드를 작성하는 것을 말합니다. 이를 통해 코드의 품질을 향상 시키고 유지 보수를 용이하게 만들 수 있습니다.

TDD를 사용한 예시 코드를 보겠습니다.
예시 코드는 Socket을 기반으로한 테스트 코드 예제입니다.  

### 코드예시
```java
@Autowired
    RoomRepository roomRepository;

    @Autowired
    UserRepository userRepository;

    @Autowired
    RoomsRepository roomsRepository;

    //사용자를 생성하고, 채팅방을 생성
    //사용자와 채팅방의 연결 테스트
    @Test
    void createRoom() {

        //사용자 생성
        UserEntity user1 = new UserEntity(1L, "user1");
        UserEntity user2 = new UserEntity(2L, "user2");

        //채팅방 생성
        ChatRoom chattingroom = new ChatRoom(1L, "2", "테스트용");

        //사용자와 채팅방 연결
        ChatRooms chatrooms = new ChatRooms(1L, user2, chattingroom);

        //----------------------------------------검증 시작--------------//

        // Not Null 검증
        assertThat(user1.getId()).isNotNull();
        assertThat(user2.getId()).isNotNull();
        assertThat(chattingroom.getId()).isNotNull();

        // 사용자 검증
        assertThat(user1.getUsername()).isEqualTo("user1");
        assertThat(user2.getUsername()).isEqualTo("user2");

        // 채팅방 검증
        assertThat(chattingroom.getChatroomID()).isEqualTo("2");
        assertThat(chattingroom.getRoomName()).isEqualTo("테스트용");

        // 채팅방, 사용자 연결 검증
        assertThat(chatrooms.getId()).isNotNull();
        assertThat(chatrooms.getUser().getId()).isEqualTo(user2.getId()); // 사용자가 제대로 들어있는지
        assertThat(chatrooms.getChatRoom().getId()).isEqualTo(chattingroom.getId());
    }
```

위의 코드를 보면 Repository를 불러와서 @Autowired를 사용해 자동으로 주입(DI)해주고,

사용자가와 채팅방, 채팅방이 사용자와 연결되어있다는 상활을 가정하고 있습니다.

검증 코드는, 사용자의 데이터가 비어있지 않은지, 사용자와 채팅방이 제대로 생성이 되어있는지, 사용자와 채팅방이 서로 연결이 되어 있는지를 검증하고 있습니다.

### TDD를 왜 사용해야 하는지
먼저 TDD를 사용하지 않고 개발을 이어나가면, 코드에 버그가 생겼을 경우, 발견하고 수정하는데 어려움이 생길 수 있습니다.

또한, 코드의 동작이 확실하지 않은 경우이기 때문에 안정성이 감소하고, 이로 인해 예상하지 못한 문제가 생길 수 있습니다.

하지만 TDD를 사용함으로써, 코드가 정상적으로 동작 하는지 지속적으로 검토하고 진행하기 때문에 코드의 신뢰성을 높일 수 있습니다.

또한 변경 사항에 대한 빠른 피드백을 얻어 개발 속도를 향상 시킬 수 있다는 장점이 존재합니다.

### TDD 작성 방법
1. 테스트 케이스 작성
    - 먼저, 새로운 기능이나, 수정하려는 기능에 대한 테스트 케이스를 작성합니다.
2. 테스트 실행
    - 작성된 테스트 케이스를 기반으로 실행하여 테스트의 동작 여부를 확인합니다.
    - 이 시점은, 실패가 많이 나올 수 있습니다.
3. 코드 작성
    - 테스트를 통과시키기 위한 최소한의 코드를 작성합니다.
    - 테스트를 통과시킬 정도의 코드를 작성하고, 복잡한 구현이나 세부 사항은 신경 쓰지 않아도 됩니다.
4. 테스트 실행 및 통과 확인
    - 작성한 코드가 테스트를 통과하는지 확인하기 위해 다시 테스트를 실행합니다.
5. 리펙토링
    - 테스트가 통과한 후에 코드를 리펙토링 하여 가독성을 높이거나 불필요한 부분을 개선합니다.
6. 반복
    - 다음 기능 또는 수정하려는 부분에 대한 새로운 테스트 케이스를 작성하고, 1~5번 단계를 반복합니다.

## 결론
위의 글을 통해 TDD(Test-Driven Development)가 코드의 품질을 향상시키고 유지보수를 용이하게 만드는 방법론임을 알 수 있습니다.  

TDD를 사용하면 코드 변경 시 발생할 수 있는 버그를 미리 예방할 수 있고, 코드의 동작을 확실히 검증하여 안정성을 높일 수 있습니다.  

또한, 빠른 피드백을 통해 개발 속도를 향상시킬 수 있습니다.  

따라서 TDD는 개발 과정에서 중요한 역할을 하며, 코드를 작성하기 전에 테스트를 작성하는 습관을 기르는 것이 좋습니다.







