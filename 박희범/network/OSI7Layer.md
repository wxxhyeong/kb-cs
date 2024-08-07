## OSI 7계층에 대해 설명해 주세요.
네트워크 통신에서 데이터가 어떻게 전달되는지를 이해하기 위한 개념적 모델   
각 계층마다 명확한 인터페이스 정의를 포함   
계층은 상대방의 계층과 특정 기능을 약속   
이처럼 표준 규약을 정하고 준수하여 **상호 운영성**과 **호환성**을 보장  
![test](https://ngwoon.github.io/assets/images/post/Network/OSI%207%EA%B3%84%EC%B8%B5/osi-layers.png)

- 7) 응용 계층   
   네트워크 서비스 접근   
- 6) 표현 계층   
   데이터 형식 일관성 및 보안 보장   
- 5) 세션 계층   
   통신 세션 관리   
- 4) 전송 계층   
   신뢰성 있는 데이터 전송   
- 3) 네트워크 계층   
   패킷의 올바른 경로 보장   
- 2) 데이터 링크 계층   
   신뢰성 있는 프레임 전송     
- 1) 물리 계층   
   정확한 비트 전송   

### Transport Layer와, Network Layer의 차이에 대해 설명해 주세요.
   전송 계층은 호스트간 서로 네트워킹할 때 약속되는 규약들을 정의합니다.    
   네트워크 계층은 패킷을 목적지까지 전송하는 것을 목적으로 합니다. 주로 패킷 라우팅과 관련된 기능을 포함합니다.   
   전송 계층의 TCP 프로토콜은 초기설정(Handshake)과 흐름 제어를 사용하여 세그먼트에 대한 신뢰성을 보장합니다.  
   네트워크 계층은 RIP, OSPF, BGP와 같은 라우팅 프로토콜을 사용하고, 라우팅 프로토콜은 라우팅 알고리즘을 사용하여, 목적지까지의 최적의 경로를 결정합니다.   

### L3 Switch와 Router의 차이에 대해 설명해 주세요.
실제론 별 차이가 없습니다. 다만 개념적인 차이가 있습니다.   
L3 Switch의 네트워크 구조는 subnet 간 연결 혹은 campus LAN의 VLAN 의 연결 구조를 지닙니다.   
Router는 네트워크 간 연결 구조를 가집니다.   
L3 Switch는 하드웨어 기반 라우팅을 수행하고 Router는 소프트웨어 기반 라우팅을 수행합니다.    
그래서 L3 Switch가 더 빠릅니다.   
L3 Switch는 NAT 기능을 지원하지 않습니다. 또한 WAN 인터페이스가 없습니다.   
    
### 각 Layer는 패킷을 어떻게 명칭하나요? 예를 들어, Transport Layer의 경우 Segment라 부릅니다.
응용계층은 메시지, 전송 계층은 세그먼트, 네트워크 계층은 패킷, 데이터 링크 계층은 프레임, 물리 계층은 비트라 부릅니다.
### 각각의 Header의 Packing Order에 대해 설명해 주세요.
OSI 모델에서 패킷이 상위 계층에서 하위 계층으로 내려갈 때, header + payload에 캡슐화가 일어나고, 자신의 계층에 맞는 header를 캡슐화한 데이터 앞부분에 생성합니다.
패킷이 하위 계층에서 상위 계층으로 올라갈 때 헤더를 사용하고, 사용한 헤더를 버리고 페이로드를 상위 계층으로 보냅니다.
> 전송 계층 예시 : 전송계층에서 헤더의 포트번호를 통해 올바른 응용 프로그램을 식별하고 페이로드를 전달합니다. 계층이 이동되면 전송 계층의 헤더를 버립니다.
### ARP에 대해 설명해 주세요.
`데이터링크 계층에 위치한 프로토콜`입니다.     
ARP는 IP 주소를 MAC 주소로 변환해주는 역할을 합니다.
수행 방식 :
   1. 단말 A는 단말 B에게 데이터를 보내기 위해, B의 MAC 주소가 필요합니다. 단말 A는 IP 주소를 알고 있지만 MAC 주소를 모르기 때문에 ARP를 사용하여 MAC 주소를 알아내야 합니다.
   2. 단말 A는 ARP Request 패킷을 네트워크에 브로드캐스트합니다. 이 요청 패킷에는 단말 B의 IP 주소가 포함되어 있습니다.
   3. 단말 B는 자신의 MAC 주소를 단말 A로 보냅니다.(단말 A의 MAC주소로 유니캐스트)
   4. 단말 A는 단말 B로부터 받은 MAC 주소를 자신의 ARP 캐시에 저장합니다. ARP 캐시는 동일한 IP 주소에 대한 미래의 요청을 빠르게 처리하기 위해 사용됩니다.

> 이더넷 : 유선 LAN에서 사용되는 네트워크 기술, LAN 케이블로 물리적인 네트워크를 구축

> 와이파이 : 무선 LAN에서 사용되는 네트워크 기술, 전파를 통해 데이터를 전송
1. 호스트 A와 B가 LAN 환경에서 통신   
호스트 A와 호스트 B가 같은 로컬 네트워크(LAN)에 있을 때, 서로 직접 통신할 수 있습니다.
1. 호스트 A와 B가 WAN 환경애서 통신   
호스트 A와 호스트 B가 서로 다른 네트워크(WAN)에 있을 때, 라우터가 중간에서 통신을 중계합니다.