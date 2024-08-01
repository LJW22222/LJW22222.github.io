---
title: Redis[이벤트 활성화]
date: 2024-02-03 22:23:00 +0800
categories: [CS, Redis]
tags: [CS, Redis]
---
# Redis 이벤트 활성화
먼저 설정 파일부터 활성화를 시켜줘야 합니다.

방법에는 직접 파일에 쓰는 방법도 있지만, 명령어로 편리하게 설정하는 방법이 있어, 여기서는 명령어로 다루겠습니다.

1. 쉘창(Ubuntu)에 명령어를 입력합니다.

```java
config set notity-keyspace-events Ex
```

1. 명령어 입력후, 잘 적용됬는지 확인합니다.

```java
config get notity-keyspace-events
//아래는 출력 결과입니다.
1) "notify-keyspace-events"
2) "xE"
```

위와 같이 나왔다면 설정은 끝났습니다.

## Spring 설정

Spring설정 코드는, 프로젝트하면서 설정했던거를 기반으로 말씀드리겠습니다.

### RedisConfiguration [ 전체 코드 ]

```java
import lombok.AllArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.Message;
import org.springframework.data.redis.connection.MessageListener;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.listener.PatternTopic;
import org.springframework.data.redis.listener.RedisMessageListenerContainer;
import org.springframework.data.redis.serializer.GenericToStringSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
@AllArgsConstructor
public class RedisConfiguration {

    @Bean
    RedisMessageListenerContainer redisMessageListenerContainer(RedisConnectionFactory connectionFactory) {
        RedisMessageListenerContainer container = new RedisMessageListenerContainer();
        container.setConnectionFactory(connectionFactory);
        container.addMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message, byte[] pattern) {
                String expiredKey = new String(message.getBody());
                System.out.println("Expired Key: " + expiredKey);
            }
        }, new PatternTopic("__keyevent@*__:expired"));
        return container;
    }

    @Bean
    public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, String> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(connectionFactory);
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericToStringSerializer<>(String.class));
        return redisTemplate;
    }
}
```

위의 설정 파일 살펴보겠습니다.

Bean으로 설정하기 위해 필요한 어노테이션 들을 설정합니다.

@Configuration

- 해당 클래스가 Spring의 구성을 정의하는 클래스임을 나타냅니다.

### RedisMessageListenerContainer

```java
RedisMessageListenerContainer redisMessageListenerContainer(RedisConnectionFactory connectionFactory) {
        RedisMessageListenerContainer container = new RedisMessageListenerContainer();
        container.setConnectionFactory(connectionFactory);
        container.addMessageListener(new MessageListener() {
            @Override
            public void onMessage(Message message, byte[] pattern) {
                String expiredKey = new String(message.getBody());
                System.out.println("Expired Key: " + expiredKey);
            }
        }, new PatternTopic("__keyevent@*__:expired"));
        return container;
    }
```

이부분은 RedisMessageListenerContainer 빈을 생성하는 역할을 합니다.

Redis의 이벤트를 처리하기 위한 컨테이너 이며, addMessageListener() 메서드를 사용하여 만료된 키에 대한 이벤트를 처리하는 리스너를 등록합니다.

RedisConnectionFactory

- Spring Data Redis에서 Redis 연견을 생성하고 관리하기 위한 인터페이스 입니다.
- 이 인터페이스를 구현한 구현체는 Redis서버와의 연결을 설정하고 관리하는 역할을 합니다.

RedisMessageListenerContainer

- Redis의 이벤트를 처리하는 컨테이너입니다.

.setConnectionFactory()

- Redis 연견 팩토리를 설정합니다.

.addMessageListener()

- Redis서버에서 특정 이벤트가 발생했을 경우, 해당 이벤트를 처리하기 위한 리스너를 등록하는 부분입니다.
- 첫번째 인자로 MessageListener를 구현한 객체가 전달되고, 이 객체는 이벤트를 수신하고 처리하는 역할을 합니다.
- 두번째 인자로는 이벤트의 패턴을 지정하는 PatterTopic 객체가 전달됩니다.

### RedisTemplate<T, T>

```java
    @Bean
    public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, String> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(connectionFactory);
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericToStringSerializer<>(String.class));
        return redisTemplate;
    }
```

Redis와 상호 작용하기 위한 RedisTemplate빈을 생성하는 역할을 합니다.

RedisTemplate는 데이터를 조작하기 위한 핵심 클래스로서, Redis 서버와의 통신을 추상화합니다.

setKeySerializer(), setValueSerializer() 메서드를 사용하여 키와 값을 직렬화하여 설정합니다.

.setConnectionFactory

- RedisMessageListenerContainer의 .setConnectionFactory부분과 마찬가지로 Reids Server의 연견을 설정하는 메서드 입니다.

.setKeySerializer

- 키 직렬화 방식을 설정하는 메서드입니다.
- Redis에는 키와 값이 모두 바이트 배열로 저장되기 때문에, 키를 문자열로 직렬화하여 Redis에 저장하고 조회할 수 있도록 설정이 필요하여 사용됩니다.

.setValueSerializer()

- setKetSerializer와 마찬가지로 이번에는 키가아닌 값을 직렬화 방식을 설정하는 메서드입니다.