### 3-Way Handshake
 - 3-Way Handshake란?
TCP는 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 three-way handshake를 사용합니다.
![3wayhandshake](https://github.com/user-attachments/assets/1ba06c42-5e1e-4699-9c15-af4ee2e21f1b)

- 사전 조건
  1. 클라이언트 포트 할당
    클라이언트는 active open합니다. 
   > active open : 클라이언트가 능동적으로 서버에 연결 요청을 보내는 방식입니다.
  > 동적 포트: 클라이언트는 운영 체제에 의해 임시로 할당된 임의의 고유 포트를 사용합니다. 이 포트는 일반적으로 49152에서 65535 사이의 범위에 속하며, 이 범위를 동적 포트라 부릅니다.
  1. 서버 소켓 listening 상태
   해당 소켓이 listen()을 호출하면 passive open하고 TCB를 생성합니다.
   > passive open : 서버가 수동적으로 연결 요청을 대기하고, 클라이언트의 연결 요청에 응답하는 방식입니다.
  > TCB : 연결을 식별하기 위한 정보, 송수신되는 데이터를 가리키는 버퍼, 승인처리와 관련된 데이터 등이 저장됩니다.
- 동작 방식
  1. SYN 요청(연결 요청) : 클라이언트가 서버에 연결 요청을 보낼 때, 클라이언트는 랜덤한 초기 시퀀스 번호(ISN: Initial Sequence Number)를 생성합니다.
  클라이언트는 이 시퀀스 번호를 SYN 패킷에 담아 서버에 전송합니다.
  2. SYN + ACK 응답(연결 요청 수락 및 확인) : 
   서버는 클라이언트의 SYN 패킷을 수신하고, 자신의 초기 시퀀스 번호를 생성합니다.
    서버는 클라이언트의 시퀀스 번호에 1을 더한 값을 ACK 번호로 설정하고, 자신의 시퀀스 번호와 함께 SYN-ACK 패킷을 클라이언트에 전송합니다.
  3. ACK : 클라이언트는 서버의 SYN+ACK 패킷을 수신한 후, 서버의 연결 요청을 확인하는 ACK 패킷을 서버로 보냅니다.
  이 패킷은 서버의 초기 시퀀스 번호에 1을 더한 값을 ACK 번호로 설정합니다. 이는 서버가 보낸 연결 요청을 수락했음을 확인하는 역할을 합니다.
  
  4. 이후 : 
      - ESTABLISHED : 시퀀스 번호가 양방향으로 동기화되어 연결이 **설정**된 상태가 됩니다.   
   시퀀스 번호가 양방향으로 동기화되었다는 것은 서버와 클라이언트 둘 다 자신이 생성한 ISN 번호의 1 증가한 값을 ACK을 통해 수신받았다는 뜻입니다. 이는 올바르게 수신되었다는 증거이며, 데이터 통신을 시작합니다.
      - 소켓의 connection() 실행  
        이후 서버 측에서 연결 소켓, 연결 소켓의 TCB가 생성됩니다.
  
  > TCP 세그먼트 종류 :
     - SYN 세그먼트:
    연결 설정을 위한 초기 메시지입니다. 시퀀스 번호가 포함되며, SYN 플래그가 설정됩니다.
    클라이언트가 서버에 연결을 요청할 때 사용됩니다.

    - SYN-ACK 세그먼트:
    연결 요청에 대한 응답 메시지입니다. 서버가 클라이언트의 SYN 메시지에 응답할 때 사용됩니다.
    SYN과 ACK 플래그가 모두 설정됩니다.

    - ACK 세그먼트:
    데이터 수신을 확인하는 메시지입니다. ACK 플래그가 설정됩니다.
    수신 측은 이 메시지를 통해 송신 측에 데이터가 성공적으로 수신되었음을 알립니다.

    - FIN 세그먼트:
    연결 종료를 요청하는 메시지입니다. FIN 플래그가 설정됩니다.
    송신 측은 이 메시지를 통해 연결을 종료하려고 한다는 의사를 밝힙니다.

    - RST 세그먼트:
    연결을 강제로 재설정하는 메시지입니다. RST 플래그가 설정됩니다.
    비정상적인 상황이나 오류가 발생했을 때 사용됩니다.

    - PSH 세그먼트:
    데이터를 즉시 전달하도록 요청하는 메시지입니다. PSH 플래그가 설정됩니다.
    일반적으로 대화형 응용 프로그램에서 사용됩니다.


#### 질문  
- ACK, SYN 같은 정보는 어떻게 전달하는 것 일까요?   
    TCP 메시지의 flags 부분에 저장되어 1 bit로 표현됩니다.
    > TCP 제어 플래그(control flag) :
      URG: 긴급 포인터 필드가 유효함을 나타냅니다.
      ACK: 확인 응답 번호 필드가 유효함을 나타냅니다.
      PSH: 즉시 푸시할 데이터를 나타냅니다.
      RST: 연결을 재설정함을 나타냅니다.
      SYN: 연결 설정을 나타냅니다.
      FIN: 연결 종료를 나타냅니다.
- 2-Way Handshaking 를 하지않는 이유에 대해 설명해 주세요.   
  ISN 값의 1 증가한 값을 서버가 받지 못한다면 클라이언트가 자신의 메시지를 잘 받았는지 알 수 없을 것입니다.

- 두 호스트가 동시에 연결을 시도하면, 연결이 가능한가요? 가능하다면 어떻게 통신 연결을 수행하나요?   
  네 연결이 가능합니다. 이 과정을 동시 연결 시도(Simultaneous Open 또는 simultaneous active open on both sides) 라고 부릅니다. syn + syn > syn+ack + syn+ack 패킷을 주고 받습니다. 이 과정으로 자신이 보낸 ISN 번호 + 1 값을 수신받고 연결이 수립됩니다.(established) 
  - passive open으로는 안되나요?    
  연결을 시도한다는 말은 active open으로 연결한다는 뜻을 내포합니다. 즉, 둘 다 passive open으로 한다는 것은 tcp가 2개 열린다는 것으로 simulataneous open이 아닙니다.
- SYN Flooding 에 대해 설명해 주세요.   
  SYN flooding은 TCP 3-Way Handshake의 특징을 사용한 DDOS 공격입니다.
  1. 공격자는 공격 대상 서버로 다량의 SYN을 보냅니다.
  2. 서버는 받은 SYN 만큼 SYN+ACK을 전송하고 ACK을 기다립니다.
  3. 공격자는 더 많은 SYN을 보내 서버의 가용 포트를 소모시킵니다. 
  4. 서버는 사용 가능한 포트가 없게 되어 서비스 불가능 상태에 빠집니다.
 이처럼 공격자는 SYN 패킷을 반복적으로 전송하여 서버 컴퓨터에 있는 사용 가능한 모든 포트를 사용하게 하여, 정상적인 트래픽에 느리게 응답하거나 전혀 응답하지 못하게 할 수 있습니다.

- 위 질문과 모순될 수 있지만, 3-Way Handshake의 속도 문제 때문에 이동 수를 줄이는 0-RTT 기법을 많이 적용하고 있습니다. 어떤 방식으로 가능한 걸까요?
  
     ![](https://www.keycdn.com/img/blog/0-rtt-vs-1-rtt-md.webp)
     
  TLS 1.2 이전 방식은 TCP Handshake > TLS Handshake > 데이터 전송의 과정을 순차적으로 수행했습니다.(2-RTT)   
  TLS1.3에서는 initial handshake 하는 과정에서만 TLS Handshake를 수행하여 세션을 설정합니다.  
  세션이 설정되면, 서버는 클라이언트에게 ticket을 발행해줍니다.
  클라이언트는 재연결을 시도할 때 ticket이 유효하면, 초기 연결 설정을 수행하지 않고 ticket과 함께 데이터를 암호화하여 보냅니다.(0-RTT)
  ticket이 유효하지 않다면, 다시 세션을 설정합니다.(0-RTT)