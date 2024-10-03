# 목차
1. [Transport Layer](#Transport-Layer)
2. [TCP (Transmission Control Protocol)](#tcp-transmission-control-protocol)
    - [TCP 특징](#tcp-특징)
    - [3-Way Handshake와 4-Way Handshake](#3-way-handshake와-4-way-handshake)
3. [TCP의 3-Way Handshake](#tcps-3-way-handshake)
    - [3-way handshake의 기본 매커니즘](#3-way-handshake의-기본-매커니즘)
    - [작동 방식](#작동-방식)
4. [TCP의 4-Way Handshake](#tcps-4-way-handshake)
    - [Termination의 종류](#termination의-종류)
    - [작동방식 (Abrupt)](#작동방식-abrupt)
    - [작동방식 (Graceful)](#작동방식-graceful)

---

## Transport Layer

OSI 7계층에서 Transport Layer에는 양 끝단(End to end)의 사용자들이 신뢰성 있는 데이터를 주고 받을 수 있도록 하여, 
상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다. 

이렇듯 Transport Layer는 Application 프로세스들 간의 논리적인 통신을 제공한다.

![images_averycode_post_01f46d6e-a577-41e7-b99f-31d67a512ce0_image](https://github.com/user-attachments/assets/124d203e-4477-4a77-9981-3d19e6ddd69f)

- 전송 프로토콜 중 잘 알려진 것이 바로 TCP와 UDP이다.
- Transport Layer의 패킷을 “세그먼트”라고 한다.

<br></br>
## TCP (Transmission Control Protocol)

> 인터넷 상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜

- TCP는 애플리케이션에게 **신뢰성 있는 연결 지향성** 서비스를 제공한다. 
일반적으로 TCP와 IP는 함께 사용되며 **IP는 배달을, TCP는 패킷의 추적 및 관리**를 하게 된다.
- 연결형 서비스로 전송에 대한 신뢰성을 보장하기 때문에 hanshaking을 수행하고, 데이터의 흐름제어와 혼잡제어를 수행한다. 
하지만 이러한 기능으로 인해 TCP의 속도는 느린 편이다.

### **TCP 특징**

- 3-way handshaking 과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제
- 흐름 제어 및 혼잡 제어
- 높은 신뢰성을 보장
- 속도가 느림 (UDP에 비해 느림)

![2](https://github.com/user-attachments/assets/bff2f5fb-10f0-4005-88bc-ea99a8cc5b1e)

### 3-Way Handshake와 4-Way Handshake

> 3-Way Handshake는 TCP의 접속, 4-Way Handshake는 TCP의 접속 해제 과정이다.

- 포트(PORT) 상태 정보
    - `CLOSED`: 포트가 닫힌 상태
    - `LISTEN`: 포트가 열린 상태로 연결 요청 대기 중
    - `SYN_RCV`: SYNC 요청을 받고 상대방의 응답을 기다리는 중
    - `ESTABLISHED`: 포트 연결 상태
- 플래그 정보
    - TCP Header에는 CONTROL BIT(플래그 비트, 6bit)가 존재하며, 각각의 bit는 "URG-ACK-PSH-RST-SYN-FIN"의 의미를 가진다.
        - 즉, 해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.
    - SYN(Synchronize Sequence Number) / 000010
        - 연결 설정
        - Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
    - ACK(Acknowledgement) / 010000
        - 응답 확인
        - 패킷을 받았다는 것을 의미한다.
        - Acknowledgement Number 필드가 유효한지를 나타낸다.
        - 양단 프로세스가 쉬지 않고 데이터를 전송한다고 가정하면 최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
    - FIN(Finish) / 000001
        - 연결 해제
        - 세션 연결을 종료 시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

<br></br>
## TCP의 3-Way Handshake

- TCP 통신을 이용하여 데이터를 전송하기 위해 네트워크 **연결을 설정(Connection Establish)** 하는 과정
- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달을 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.
- 즉, TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

### 3-way handshake의 기본 매커니즘

> TCP 통신은 PAR(Positive Acknowledgement with Re-transmission)을 통해 신뢰적인 통신을 제공한다.

receiver가 데이터 유닛이 손상된 것을 확인하면(Error Detection에 사용되는 transport layer의 `checksum`을 활용) 해당 세그먼트를 없앤다. 
그러면 sender는 positive ACK가 오지 않은 데이터 유닛을 다시 보내야 한다.

(즉, ACK를 받을 때까지 데이터 유닛을 재전송 하게 된다.)

![3](https://github.com/user-attachments/assets/e0d26606-bd1a-4c28-ba3f-f42570367003)

⇒ 이 과정에서 클라이언트와 서버 사이에서 3개의 세그먼트가 교환 되는 것을 확인할 수 있다. 이것이 바로 3-way handshake의 기본 매커니즘이다.

### 작동 방식

- Client는 Server와 연결하기 위해 3-way handshake를 통해 연결 요청을 하게 된다.
- 우리가 일반적으로 생각하는 Client와 Server는 모두 서로 연결 요청을 먼저 할 수 있기 때문에, 연결 요청을 먼저 시도한 요청자를 Client로, 연결 요청을 받은 수신자를 Server쪽으로 가정한다.
- **SYN(Synchronization)**: 연결 요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄
- **ACK(Acknowledgement)**: 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송
    - 동기화 요청에 대한 답변: `Client의 Sequence Number+  1`을 하여 ACK로 돌려준다.
    
![5](https://github.com/user-attachments/assets/108461e3-3acc-478d-956d-b17c64b47aaf)

1. **Step 1 (SYN)**
    
    **클라이언트는 서버와 커넥션을 연결하기 위해 SYN을 보낸다. (seq : x)**
    
    - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
    - PORT 상태
        - Client : `CLOSEDSYN_SENT` 로 변함
        - Server : `LISTEN`
2. **Step 2 (SYN + ACK)**
    
    **서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (seq : y, ACK : x + 1)**
    
    - 접속 요청을 받은 Server가 요청을 수락했으며, 접속 요청 프로세스인 Client도 포트를 열어달라는 메세지를 전송 (SYN-ACK signal bits set)
    - ACK Number필드를 `Sequence Number + 1`로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 세그먼트 전송 (seq = y, ACK = x+1, SYN, ACK)
    - PORT 상태
        - Client : `CLOSED`
        - Server : `SYN_RCV`
3. **Step 3 (ACK)**
    
    **클라이언트는 서버의 응답은 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄**
    
    - 마지막으로 접속 요청 프로세스 Client가 수락 확인을 보내 연결을 맺음 (ACK)
    - 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
    - PORT 상태
        - Client : `ESTABLISED`
        - Server : `SYN_RCV` ⇒ ACK ⇒ `ESTABLISED`

<br></br>
### TCP의 4-Way Handshake

> 4-Way Handshake은 연결을 해제(Connecntion Termination)하는 과정이다. 이때, FIN 플래그를 이용한다.

## Termination의 종류

TCP는 대부분의 connection-oriented 프로토콜과 같은 두 가지 연결 해제 방식이 있다.

1. **Graceful connection release(정상적인 연결 해제**)
    
    정상적인 연결 해제에서는 양쪽이 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 있다.
    
2. **Abrupt connection release(갑작스런 연결 해제**)
    1. 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
    2. 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

### 작동방식 (Abrupt)

> RST(TCP reset) 세그먼트가 전송되면 갑작스러운 연결 해제가 수행된다.

RST 세그먼트는 다음과 같은 경우에 전송된다.

- 존재하지 않는 TCP 연결에 대해 SYN 세그먼트가 수신된 경우
- 열린 커넥션에서 잘못된 Header를 가진 세그먼트가 수신된 경우, 해당 커넥션을 닫기 위해 RST 세그먼트를 보내서 공격을 방지
- 기존 TCP 연결을 종료해야 하는 경우
    - 연결을 지원하는 리소스 부족할 때
    - 원격 호스트에 연결할 수 없고 응답이 중지되었을 때

### 작동방식 (Graceful)

연결 종료 요청 역시, 일반적으로 Client와 Server는 모두 연결 해제 요청을 먼저 할 수 있기 때문에, 요청을 먼저 시도한 요청자를 Client(host A)로, 요청을 받은 수신자를 Server(host B)로 가정한다.

> Half-Close 기법 사용

아래 그림에서 처음 보내는 종료 요청인 **FIN** 패킷에 실질적으로 ACK가 포함되어 있는 것을 알 수 있는데, 이는 **Half-Close 기법**을 사용하기 때문이다.
즉, 연결을 종료하려고 할 때 완전히 종료하지 않고 반만 종료한다.

Half-Close 기법을 사용하면 종료 요청자가 처음 보내는 FIN 패킷에 승인 번호를 함께 담아서 보내게 되는데, 
이때 승인 번호의 의미는 **_"일단 연결은 종료할 건데 귀는 열어둘게. 이 승인 번호까지 처리했으니까 더 보낼 거 있으면 보내"_**라는 뜻이다.

이후 수신자가 남은 데이터를 모두 보내고 나면 다시 요청자에게 **FIN** 패킷을 보냄으로써 모든 데이터가 처리되었다는 신호를 보낸다.
따라서 요청자는 그때 나머지 반을 닫아 좀 더 안전하게 연결을 종료할 수 있게 된다.

![6](https://github.com/user-attachments/assets/b11457d6-580a-4da0-a15f-f787e177ac02)

1. **STEP1 (Client → Server : FIN(+ACK))**
    - 서버와 클라이언트가 연결된 상태에서 클라이언트가 close()를 호출하여 접속을 끊는다.
    - 이때, 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
        - 이때 FIN 패킷에는 실질적으로 ACK도 포함되어있다.
2. **STEP2 (Server → Client : ACK)**
    - 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보내고 자신의 통신이 끝날 때까지 기다린다. (⇒ `TIME_WAIT` 상태)
        - Server(수신자)는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
    - 서버는 클라이언트에게 응답을 보내고 **`CLOSE_WAIT` 상태**에 들어간다. 그리고 **아직 남은 데이터가 있다면 마저 전송을 마친 후에 close()를 호출한다.**
    - 클라이언트에서는 서버에서 ACK를 받은 후에 서버가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다리게 된다.
3. **STEP3 (Server → Client : FIN)**
    - 데이터를 모두 보냈다면, 서버는 연결이 종료에 합의한다는 의미로 FIN 패킷을 클라이언트에게 보낸 후에, 승인 번호를 보내줄 때까지 기다리는 `LAST_ACK` 상태로 들어간다.
4. **STEP4 (Client → Server : ACK)**
    - 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다.
    - 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT`을 통해 기다린다. (실질적인 종료 과정 `CLOSED`에 들어가게 된다.)
        - 이때 `TIME_WAIT` 상태는 **의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지**
        - 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 `CLOSED`로 들어감
    - 서버는 ACK를 받은 이후 소켓을 닫는다. (`CLOSED`)
    - `TIME_WAIT` 시간이 끝나면 클라이언트도 연결을 닫는다. (`CLOSED`)
  
---
### ❓ 질문
1. TCP의 3-Way Handshake 과정에 대해 설명해 주세요.
2. TCP의 연결 설정 과정과 연결 종료 과정이 단계가 차이나는 이유는 무엇인가요?
3. 만약 Server에서 FIN 플래그를 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황이 발생하면 어떻게 되나요?
