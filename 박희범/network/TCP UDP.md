### TCP
    두 개의 호스트를 연결하고 데이터 스트림을 교환하게 해주는 프로토콜입니다.
    TCP는 데이터와 패킷이 안정적으로, 순서대로, 에러없이 교환할 것을 보장합니다.
    그러므로 TCP는 양방향 통신이 기본이며, 신뢰성있고 연결지향적입니다.
 - PDU : 세그먼트
 - 프로토콜 작동
    1. 연결 생성
        - 3-Way HandShake   
            상대방 컴퓨터와 사전에 세션을 수립하는 과정  
            SYN (C) -> SYN+ACK (S) -> ACK (C)
    2. 자료 전송 
    3. 연결 종료 
        - 4 way handshake   
            FIN (C) -> ACK (S) -> FIN (S) -> ACK (C)
    > C: client, S: server

- TCP의 소켓 통신

    ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99C5C63359FEB5DC06)

### UDP

    한 호스트에서 다른 호스트로 데이터를 보내는 프로토콜입니다.
    상호 연결하는 프로세스가 없습니다.
    데이터그램 손실이 일어나도 다시 전송되지 않습니다.
    데이터그램 순서를 지키지 않습니다.
    그러므로 TCP는 단방향 통신이지만 양방향 통신이 가능합니다. 단, 신뢰성은 보장하지 않습니다.
    
 - PDU : 데이터그램
 - 프로토콜 작동  
   Request -> Response -> Request -> Response...

 - UDP는 비신뢰성, 비연결 지향적이므로 TCP보다 빠릅니다. 때문에 실시간 서비스(온라인 게임, 스트리밍)에 자주 사용됩니다.
 
 - UDP의 소켓 통신
  
    ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9934293359FEB5EE38)

    UDP는 비연결지향적이기 때문에 listen()이 없습니다. 그러므로 리스닝 소켓, 연결 소켓과 같은 개념이 없습니다. 
### TCP와 UDP의 차이에 대해 설명해 주세요.
1. Checksum이 무엇인가요?
   네트워크 통신에서 데이터의 무결성을 확인하기 위한 오류 검출 메커니즘입니다.
   수신 측에서 전송할 때 보낸 어떤 값과 자신이 계산한 값이 일치하면 데이터가 올바르게 수신되었다고 나타내고, 다르면 데이터가 손상되었다고 판단하는 겁니다.  
   - 체크섬 과정
    1. 데이터를 16비트 단위로 나눔
    2. 16비트 데이터를 모두 합산
    3. 1의 보수로 변환
   전송할 때 변환된 값을 보내고 수신 측에선 위의 과정을 동일하게 수행하여 변환된 값과 계산 값이 같으면 데이터가 무결하다고 판단합니다.

2. TCP와 UDP 중 어느 프로토콜이 Checksum을 수행할까요?
   둘 다 사용합니다.  
   그러나 TCP 체크섬은 오류가 난 경우 재전송을 요청하지만, UDP는 재전송 매커니즘이 없습니다. 
3. 그렇다면, Checksum을 통해 오류를 정정할 수 있나요?
   TCP는 정정하고, UDP는 정정하지 않습니다.
4. TCP가 신뢰성을 보장하는 방법에 대해 설명해 주세요.
    - 에러 검출
      Checksum을 사용합니다.

    - 순서 보장   
      세그먼트에 번호를 부여하여 나중에 순서대로 조립될 수 있도록 보장합니다. 만약 손실된 경우 세그먼트가 재전송될 때까지 기다립니다(TCP HOLB)

    - 흐름 제어
      - 목적 : 송신 측과 수신 측 사이의 데이터 전송 속도를 조절하여 수신 측이 데이터의 과부하를 겪지 않도록 하는 것
        1. Stop And Wait   
        송신 측은 수신 측에서 잘 받았다는 응답이 올때까지 전송을 기다립니다. 송신자는 과도하게 기다릴수밖에 없어 비효율적입니다. 때문에 슬라이딩 윈도우를 일반적으로 사용합니다.
        2. Sliding Window   
        여러 데이터 패킷을 동시에 전송합니다.(최대 윈도우 사이즈 만큼) 윈도우는 송신측 윈도우, 수신측 윈도우가 존재합니다. 수신측은 자신의 윈도우 크기를 TCP 헤더에 넣어 송신측에게 보내면, 송신측은 수신측의 윈도우 사이즈를 고려하여 자신의 윈도우 사이즈를 조절합니다. 이는 수신측의 오버플로우를 방지하도록 합니다.

     - 혼잡 제어
        - 목적 : 네트워크 내부의 혼잡(congestion)을 관리하여 전체 네트워크의 성능을 유지하는 것
        > 패킷이 네트워크 내부에 들어서면 수많은 라우터들을 경유하여 목적지로 향합니다. 라우터들에게도 패킷을 처리할 수 있는 한계가 있습니다. 송신측은 라우터에서 처리하지 못한 패킷을 손실 데이터로 판단하고 데이터를 재전송하고 이는 네트워크를 더 혼잡하게 합니다.
        1. Slow Start   
          윈도우의 크기를 지수적으로 증가하다가 혼잡이 감지되면 위도우 사이즈를 1로 줄입니다.
        2. AIMD(Additive Increase & Multicative Decrease)   
          윈도우 크기를 선형적(1 세그먼트 사이즈) 증가하다가 혼잡이 감지되면 윈도우 사이즈를 반만큼 줄입니다.
        3. Fast Recovery   
          혼잡 상태가 되면 윈도우 크기를 1로 줄이지않고 반으로 줄입니다. 이후 AIMD처럼 작동합니다.
    
         혼잡 제어는 이 방식을 혼합 사용하여 상황에 따라 동작 방식을 바꿔가며 수행합니다.
5. TCP의 혼잡 제어 처리 방법에 대해 설명해 주세요.
6. 왜 HTTP는 TCP를 사용하나요?
   Tim Berners-Lee는 연구자들이 서로 간에 정보를 쉽게 공유할 수 있는 시스템을 만들기 위해 WWW를 제안했습니다. 또한 Tim Berners-Lee는 최초의 웹 브라우저와 웹 서버를 개발하여, HTML 문서를 HTTP를 통해 전송하는 시스템을 구현했습니다.   
   HTML과 관련된 데이터에 손실이 생기면 위의 의도와 맞지 않게 작동할 것입니다. 때문에 HTTP는 TCP를 사용합니다.
7. 그렇다면, 왜 HTTP/3 에서는 UDP를 사용하나요? 위에서 언급한 UDP의 문제가 해결되었나요?
   HTTP/3.0은 TCP의 latency(3-way handshake, 혼잡 제어 등)와 HOLB를 극복하기 위해 UDP기반의 QUIC을 사용합니다.    
   QUIC은 TCP와 같이 재전송, 흐름 제어, 혼잡 제어 메커니즘을 구현했습니다. 다만 TCP와 달리 하나의 연결에 여러 스트림이 존재하기 때문에 QUIC에 맞게 재구현되었습니다. 
8. 그런데, 브라우저는 어떤 서버가 TCP를 쓰는지 UDP를 쓰는지 어떻게 알 수 있나요?  
   1. **HTTP Alternative Services**   
  브라우저 초기 연결 설정때 TCP(HTTP/1.1, HTTP/2.0)를 사용하여 연결합니다.
  서버가 대체할 수 있는 다른 HTTP 프로토콜이 있다면 HTTP Header에 `Alt-Svc: h3=":443"; ma=86400 `를 포함하여 보냅니다. 이를 통해 클라이언트는 서버가 TCP, UDP를 사용하는지 알 수 있습니다.
9.  본인이 새로운 통신 프로토콜을 TCP나 UDP를 사용해서 구현한다고 하면, 어떤 기준으로 프로토콜을 선택하시겠어요?   
    통신하는 대상의 통신량, 접속의 규모, 빠른 통신의 중요도에 따라 선택할 거 같습니다. 어플리케이션의 요구사항에 따라 통신량 많거나, 접속 규모가 크거나, 통신이 빨라야 한다면 TCP가 가진 근본적인 문제점을 극복하기 위해 UDP를 사용하여 통신 프로토콜을 구현할 것입니다.  