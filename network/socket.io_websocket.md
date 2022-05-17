### **TCP/IP 소켓과** WebSocket**의 차이**

우선 **소켓**은 프로그램이 네트워크 상에서 **데이터를 송신과 수신을 하기 위한 연결부**를 의미한다. 
TCP/IP 소켓 통신과 웹소켓은 IP와 포트를 통해 통신하고 양방향 통신을 한다는 점이 같으나, 둘은 다른 개념이다.

**TCP/IP 소켓**은 어플리케이션 계층보다 하위의 계층들을 추상화한 것이고, **WebSocket**은 어플리케이션 계층에서 HTTP 프로토콜에서 헤더를 통해 구현한다.

- **TCP/IP 소켓 프로그래밍**
    - TCP/IP 계층에서 더 추상화 해서 만든 것이 Socket 계층
    - 사용자는 어플리케이션 계층의 프로토콜만 정하고, 하위 계층은 Socket을 사용하여 개발
- **WebSocket**
    - **HTTP 레이어에서 작동하는 소켓**
    - 웹 브라우저는 HTTP 프로토콜을 사용하므로 단방향 소통을 함 -> 단방향을 보완하기 위하여 WebSocket 사용
    - HTTP 프로토콜을 사용하면서도 실시간 소통을 하기 위한 프로토콜

## WebSocket

<aside>
💡 클라이언트와 서버의 양방향 소통을 위한 프로토콜

</aside>

- 탄생 배경
    - **HTTP 프로토콜**은 클라이언트에서 서버로의 단방향 통신이기에
    - 서버 측 업데이트를 클라이언트가 반영 못함
    - WebSocket 이전에는 Polling, Streaming 방식의 AJAX 코드로 양방향 통신 구현
    - 하지만 이 방법들은 브라우저마다 구현 방법이 달라 개발이 어려움
    - 기존의 단방향 HTTP 프로토콜과 호환되어 양방향 통신을 제공하기 위해 개발된 프로토콜 (HTML 5 이후)
- 구현
    - HTTP 헤더에 `Upgrade: websocket`을 통해 구현
    - 접속까지는 HTTP 프로토콜을 이용하고, 그 이후 통신은 자체적인 WebSocket 프로토콜로 통신
        
        ```
        GET / HTTP/1.1
        Host: localhost:8080
        Upgrade: websocket 
        Connection: Upgrade
        ```
        
- **특징**
    - **양방향 통신(Full-Duplex)** : 클라이언트와 서버가 서로 데이터를 주고 받을 수 있는 통신 방식
    - **실시간 네트워킹(Real Time-Networking)** : 채팅, 주식, 영상 스트리밍 등 연속된 데이터를 빠르게 보여주어야하거나 실시간 처리가 필요한 경우 사용함
    - **일반 Socket 통신**과 달리 HTTP 80 Port를 사용하므로 방화벽에 제약이 없음
    - http:// 대신 ws:// 로 시작하며 Streaming과 유사한 방식으로 푸쉬를 지원한다.

## Socket.io

<aside>
💡 클라이언트와 서버의 양방향 통신을 가능하게 해주는 모듈(라이브러리)

  


</aside>

- WebSocket, FlashSocket, AJAX Long Polling, AJAX Multi part Streaming, IFrame, JSONP Polling 등 다양한 방법을 하나의 API로 추상화함
- JavaScript를 이용하여 브라우저 종류에 상관없이 **실시간 웹**을 구현할 수 있음
- WebSocket을 지원하지 않는 브라우저(HTML 5 이전)에도 사용할 수 있다.
- 브라우저와 웹 서버의 종류와 버전을 파악하여 적합한 기술을 선택하여 실시간 웹을 구현할 수 있음.
