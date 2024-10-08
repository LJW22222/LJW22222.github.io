---
title: NetWork[PipeLine_connection]
date: 2024-05-08 15:34:00 +0800
categories: [CS, Network]
tags: [HTTP]
---
# 파이프라인 커넥션(Pipelined Connections)
![PipeLine_connection](/assets/img/cs/NetWork/Pipe-Line.png){: width="550" height="300" }

파이프라인 커넥션은 HTTP/1.1에서 도입된 기술로, 클라이언트가 하나의 TCP 연결을 통해 여러 요청을 순차적으로 보내면서도 각 요청에 대한 응답을 기다리지 않고 다음 요청을 계속해서 보낼 수 있게 해줍니다. 이를 통해 네트워크의 대기 시간을 줄여 성능을 향상시킬 수 있습니다.

### 동작 원리
파이프라인 커넥션은 다음과 같은 방식으로 작동합니다:

1. **지속 커넥션 확인**: 클라이언트는 서버가 지속 커넥션(persistent connection)을 지원하는지 확인해야 합니다. 지속 커넥션이란 동일한 TCP 연결을 통해 여러 요청과 응답을 주고받을 수 있는 기능을 말합니다.
2. **요청 순서 유지**: 클라이언트는 여러 개의 HTTP 요청을 순차적으로 보냅니다. 서버는 이 요청들을 받은 순서대로 처리하고 응답합니다. 응답 역시 요청한 순서대로 도착해야 합니다.
3. **커넥션 복구 준비**: 클라이언트는 네트워크 문제가 발생하여 연결이 끊어질 경우, 끊어진 시점 이후의 요청을 다시 보낼 준비가 되어 있어야 합니다.
4. **멱등성 요청만 사용**: 파이프라인 커넥션은 멱등성(Idempotent) 요청에만 사용해야 합니다. 즉, 같은 요청을 여러 번 보내더라도 서버 상태에 변화를 주지 않는 요청(GET, HEAD 등)에만 사용해야 합니다. 비멱등성 요청(POST, PUT 등)은 파이프라인 커넥션에 사용하면 안 됩니다.

### 제약 사항
파이프라인 커넥션을 사용할 때 주의해야 할 몇 가지 제약 사항이 있습니다:

1. **지속 커넥션 확인 전 사용 금지**: 클라이언트는 서버가 지속 커넥션을 지원하는지 확인하기 전에는 파이프라인 커넥션을 시작해서는 안 됩니다.
2. **요청 순서 보장**: 서버는 요청을 받은 순서대로 처리하고 응답을 보내야 합니다. 이는 클라이언트가 보낸 요청 순서와 응답 순서가 일치해야 함을 의미합니다.
3. **연결 복구**: 클라이언트는 네트워크 연결이 중간에 끊어지더라도 다시 요청을 보낼 준비가 되어 있어야 합니다. 이는 특히 네트워크 상태가 불안정한 환경에서 중요합니다.
4. **비멱등성 요청 금지**: POST와 같은 비멱등성 요청은 파이프라인 커넥션을 통해 보내면 안 됩니다. 이는 같은 요청이 여러 번 보내질 경우 서버 상태가 예측 불가능하게 변할 수 있기 때문입니다.

### 장점
- **네트워크 대기 시간 감소**: 여러 요청을 연속으로 보내면서 응답을 기다리지 않기 때문에 왕복 시간(RTT, Round-Trip Time)을 절약할 수 있습니다.
- **성능 향상**: 대기 시간이 긴 네트워크 환경에서 특히 성능이 개선됩니다.

### 단점
- **구현 복잡성**: 클라이언트와 서버 모두 파이프라인 커넥션을 제대로 구현하기 위해서는 추가적인 복잡성이 필요합니다.
- **지원 제한**: 모든 서버와 클라이언트가 파이프라인 커넥션을 지원하지는 않습니다. 일부 프록시 서버나 방화벽은 이를 지원하지 않을 수 있습니다.

### 결론
파이프라인 커넥션은 HTTP/1.1의 유용한 기능 중 하나로, 네트워크 성능을 향상시키는 데 도움을 줄 수 있습니다. 하지만, 이를 올바르게 사용하기 위해서는 여러 가지 제약 사항과 주의사항을 잘 이해하고 준수해야 합니다. 특히, 멱등성 요청만을 사용하고 네트워크 연결의 안정성을 유지하는 것이 중요합니다.