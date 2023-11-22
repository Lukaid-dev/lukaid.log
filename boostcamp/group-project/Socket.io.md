# Socket.io

## WebSocket vs HTTP

그룹 플젝에서 채팅기능을 기획했고, 이를 WebSocket을 이용해 구현하기로 했다. HTTP는 많이 사용해봐서 잘 알고있지만(잘 알고있다고 말하기 무섭긴하지만...), WebSocket은 이해도가 많이 떨어진다. WebSocket과 HTTP의 차이점을 간단히 말하자면 다음과 같다.

HTTP는 stateless한 text기반의 protocol이다. 간단하게, 클라이언트가 서버로 요청으로 보내면 서버는 응답을 해주고 끝! 하지만 WebSocket은 연결을 유지하고 양방향 통신이 가능한 이벤트 기반의 binary protocol이다. 클라와 서버가 연결되면 계속해서 데이터를 주고 받을 수 있다. 그러니까 서버에서 클라이언트로의 정보 전달을 지속할 수 있게 된다. WebSocket이라고 해서 포트를 따로 사용하지는 않고, 80이나 443을 사용한다고 한다. 중요한 차이점은 여기까지인 것 같다.

그럼 이제 우리가 사용하기로 한 [Socket.io](https://socket.io/docs/v4/)에 대해 알아보자.

## Socket.io

요새는 풍문으로 들은 기술의 도입 이전에 공식문서를 읽어 보는 것이 자연스러워졌다. 공식문서에서는 Socket.io를 다음과 같이 설명한다.

> Socket.IO is a library that enables low-latency, bidirectional and event-based communication between a client and a server.
>
> Socket.IO는 클라이언트와 서버 간의 저지연, 양방향, 이벤트 기반 통신을 가능하게 하는 라이브러리입니다.

여기까지만해도, "아 WebSocket"의 node용 구현체구나~~" 라고 생각했다. 그런데,

> Socket.IO is NOT a WebSocket implementation.
>
> Socket.IO는 웹소켓의 구현이 아닙니다.

엥? 구현체가 아니라고 한다.

> Although Socket.IO indeed uses WebSocket for transport when possible, it adds additional metadata to each packet. That is why a WebSocket client will not be able to successfully connect to a Socket.IO server, and a Socket.IO client will not be able to connect to a plain WebSocket server either.
>
> Socket.IO는 실제로 가능한 경우는 데이터 전송에 WebSocket을 사용하지만, 각 패킷에 추가 메타데이터를 추가합니다. 그렇기 때문에 웹소켓 클라이언트는 Socket.IO 서버에 성공적으로 연결할 수 없고, Socket.IO 클라이언트도 일반 웹소켓 서버에 연결할 수 없습니다.

그러니까, Socket.io는 클라이언트와 서버 간의 저지연, 양방향, 이벤트 기반 통신을 가능하게 하기 위해 WebSocket도 물론 기본으로 사용하지만 만약 브라우저에서 WebSocket을 지원하지 않는 경우 폴링(polling) 및 기타 전송 방법을 사용하여 실시간 통신을 지원한다. 따라서 클라이언트와 서버 양단에 모두 Socket.io의 라이브러리를 설치해야한다.

### Features: 일반 웹소켓에 비해 Socket.IO가 추가적으로 제공하는 기술들

**HTTP long-polling fallback** : WebSocket 연결이 설정되지 않을 경우, 연결을 HTTP long-polling으로 전환한다.

**Automatic reconnection** : 서버와 클라이언트 간의 웹소켓 연결이 특정 조건에서 끊길 경우, 다시 연결해주는 기능을 포함한다. (heartbeat mechanism등을 이용한다는데 자세히 알 필요는 없을 것 같다.)

**Packet buffering** : 클라이언트가 연결이 끊길 경우, 패킷들은 자동으로 버퍼링되며 재연결 시에 전송된다.

**Namespace and Room** : 네임스페이스와 룸을 통해 클라이언트를 그룹화하고, 특정 그룹 간에만 통신할 수 있도록 한다. 이는 채팅 애플리케이션에서 특정 채널에만 메시지를 보내는 등의 용도로 활용된다.

## 그래서 어떻게 쓰는데?
