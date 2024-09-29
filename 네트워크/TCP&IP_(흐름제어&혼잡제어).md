# TCP/IP (흐름제어/혼잡제어)

## TCP 통신이란?

 - 데이터의 송수신을 보장하는 신뢰성 있는 네트워크 프로토콜

## 신뢰할 수 있는 네트워크를 보장할 때의 문제점

 - Packet 손실

 - Packet 순서 

 - Network 혼잡

 - Receiver 부하

## 전송 과정

 1. Application Layer: 전송하고자 하는 Data를 Socket으로 전달

 2. Transport Layer: Data를 Segment로 감싼 후 Network Layer로 전달

 3. 여차저차 후 Receive node로 데이터가 전송됨
     - 각 node 별 데이터 저장 위치
        - Sender: send buffer
        - Receiver: receive buffer

 - Flow Control (흐름 제어)의 핵심: Receive buffer가 넘치지 않게 하는 것
 - Receiver 측에서는 RWND(Receivw Window)라는 자신의 남은 Buffer 용량을 전달함

## Flow Control (흐름 제어)

 - Receive의 처리 속도가 Sender보다 빠르면 문제가 없으나, Sender의 속도가 빠르면 문제 발생
 
 - Receive Buffer를 초과하는 데이터들은 손실될 수 있고, 이렇게 되면 불필요한 전송이 증가

 - Receiver의 상황에 따라 Sender가 데이터 전송량을 조절해야 함

### Stop and Wait

 - 매번 전송한 패킷에 대하여 수신 확인 응답을 받은 후 다음 패킷을 전송하는 방법

    ![alt text](images\1.png)

### Sliding Window

 - Receive Buffer의 크기만큼 Sender에서 데이터를 전송할 수 있도록 하여 동적으로 데이터 흐름을 조절하는 방법

 - 전송은 되었으나, ACK을 받지 못한 패킷의 숫자를 파악하고 재전송함

 - LastByteSent - LastByteAcked <= ReceivedWindowAdvertised
    - Sender가 보낸 데이터의 량은 Receive Buffer의 남은 크기를 넘을 수 없다는 식

 - Window에 포함되는 모든 Packet을 전송하고, ACK이 오는대로 Window를 옆으로 넘김으로서 (Sliding) 다음 패킷들을 전송

    ![alt text](images\2.png)


## Congestion Control (혼잡 제어)

 - Sender/Receiver가 아니라 Network Bandwidth로 인해 데이터가 오버플로우 되거나 손실되는 것을 방지하기 위해 전송 속도를 조절하는 방법

 - Flow Control: Sender - Receiver 사이의 전송 속도만 고려
 - Congestion Control: Network 상황까지 고려



### AIMD (Additive Increase Multicative Decrease)

 - 패킷을 하나씩 보내고, ACK이 올 때마다 Window 크기를 1씩 증가시키는 방법

 - 패킷 손실을 감지하면, Window Size를 절반으로 감소

    ![alt text](images\3.png)


### TCP Reno

![alt text](images\4.png)

### Slow Start

 - 네트워크 처리 용량을 미리 알 수 없기 때문에, 전송하는 데이터의 양을 천천히 늘려나가는 과정

 - Congestion Window (cwnd) = 1MSS (최대 세그먼트 크기, Max Segment Size)

 - ACK을 받을 떄마다 cwnd를 두 배씩 증가 (지수적 증가, cwnd *= 2)

 - cwnd의 크기가 Threshold에 도달하면, Congestion Avoidance 단계로 진입

### Congestion Avoidance

 - 네트워크에 혼잡을 발생시키지 않도록 데이터의 양을 천천히 증가시키는 과정

 - ACK을 받을 때마다 cwnd가 1씩 증가함 (cwnd += 1)

### Fast Transmit

 - 네트워크의 혼잡으로 인해 패킷이 손실되었다고 판단하면, 패킷을 재전송하는 과정

 - 패킷 손실 기준: 동일한 ACK 3번 수신 시

 - Timer 기반 재전송 방식보다 빠르게 패킷 손실 탐지 가능

### Fast Recovery

 - Fast Transmit이 발생하면, 네트워크가 혼잡한 상황이라고 가정하고 cwnd 크기를 줄여 원활한 상태로 만드는 과정

 - cwnd 크기를 절반으로 줄임

 - ACK을 받을 때마다 선형적으로 cwnd 크기를 증가 (cwnd += 1)

 - 혼잡 상태 판정 이후, 다시 Congestion Avoidance로 돌아간 상태

 - Timeout이 발생하면 cwnd size를 1로 조정하고 Slow Start 상태로 돌아간다


Reference
---
- https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html
- https://code-lab1.tistory.com/30