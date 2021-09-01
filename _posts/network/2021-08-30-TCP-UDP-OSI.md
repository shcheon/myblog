---
title: "OSI 7계층 및 TCP와 UDP의 차이"
last_modified_at: 2021-08-30
categories:
  - Network
tags:
  - OSI 7Layers
  - TCP
  - UDP
  - 3-ways handshake
  - 4-ways handshake
---

- **OSI 7 Layers**

  - 네트워크에서 통신이 일어나는 과정을 7단계로 분류한 계층

  - 7 계층 (Application Layer) : 사용자와 직접 상호작용하는 응용 프로그램들이 포함된 계층

  - 6 계층 (Presentation Layer) : 데이터의 형식을 정의하는 계층

  - 5 계층 (Session Layer) : 컴퓨터끼리 통신하기 위해 세션을 만드는 계층

  - 4계층 (Transport Layer) : 최종 수신 프로세서로 데이터의 전송을 담당하는 계층 (UDP, TCP 이용)

  - 3 계층 (Network Layer) : 패킷을 목적지까지 가장 빠른 길로 전송을 담당하는 계층 (IP주소로 전송)

  - 2 계층 (Datalink Layer) : 데이터의 물리적인 전송과 에러 검출, 흐름제어를 담당하는 계층 (MAC주소로 전송)

  - 1 계층 (Physical Layer) : 데이터를 전기신호로 바꾸어주는 계층

    

- **TCP/IP**

  - TCP 프로토콜과 IP 프로토콜을 합쳐서 부르는 용어

  - IP는 네트워크에서 패킷을 교환할 때,  목적지까지 빠르게 전송하기 위한 프로토콜이며, TCP는 목적지까지 데이터 손실 없이 무사히 도착하였는지, 순서가 올바른지, 네트워크의 혼잡도 고려하는 역할을 하는 프로토콜 임. TCP와 IP를 같이 자주 쓰이므로, TCP/IP로 묶어서 말함

    

- **TCP와 UDP의 차이점**

  - TCP는 가상회선을 만들어 목적지까지 전송할 논리적인 연결을 만들고, 각 패킷은 가상 식별 번호가 부여되어 패킷들이 순서대로 도착하고, 신뢰성이 높 . 통신은 1:1로만 가능하며 UDP에 비해 속도가 느리다는 단점이 있음

  - UDP는 패킷에 목적지 주소만 이용하여 전송되므로, 동일 패킷이라 하더라도 목적지까지 도착하는 네트워크 경로는 다를 수 있음.  통신은 1:1 1:N N:N이 가능하며, 연결과정이 없으므로, TCP보다 빠르지만 데이터 신뢰성을 보장하지 못한다는 단점이 있음

  - TCP의 혼잡제어, 흐름제어, 순서 제어 등이 가능한 이유는 헤더에 정보가 포함되어 있기 때문임. UDP 헤더는 목적지 주소이외에 없다고 봐도 무방.

    

- **TCP 의 3-ways handshake**

  - 세션 만들때 사용됨
  - Client -> Server : TCP SYN 전송
  - Client <- Server : TCP SYN ACK 전송
  - Client -> Server : TCP ACK 전송

  

- **TCP 의 4-ways handshake** 

  - 세션 종료할 때 사용됨

  - Client -> Server : FIN 전송

  - Client <- Server : ACK 전송

  - Client <- Server : FIN 전송

  - Client -> Server : ACK 전송
