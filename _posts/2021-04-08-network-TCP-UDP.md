---
layout: post
title:  "TCP와 UDP"
subtitle: "TCP와 UDP"
categories: cs
tags: network
comments: true

---

데이터는 0과 1로 이루어진 숫자이고, 컴퓨터는 이것을 꺼짐 / 켜짐 으로 나타낼수있다.

즉 데이터는 0과 1로 이루어진 아주 긴 전기신호라고 볼수있다.

프로토콜은 발생한 데이터가 상대방 컴퓨터/서버 로 전달하기 위해 만든 표준화된 절차라고 할수있다.

데이터를 보내는쪽에서는 데이터를 안전하고 정확하고 신속하게 규격화 하는 포장방법이 필요하고

받는 쪽에서는 데이터를 안전하고 정확하게 해석 하는 방법이 필요하다.

컴퓨터 간 데이터를 주고받을 때 에러가 발생하지 않도록 알맞게 나누어 전송하고, 이를 수신하여 다시 기존에 정보로 변환하는 과정, 어떤 모델이 약속되어 있는지 알아보자.

포로토콜중 대표적으로 TCP / UDP가 존재한다.

### UDP

UDP는 비연결형 프로토콜입니다. 즉, 연결을 위해 할당되는 논리적인 경로가 없는데, 

그렇기 때문에 각각의 패킷은 다른 경로로 전송되고, 각각의 패킷은 독립적인 관계를 지니게 되는데 이렇게 데이터를 서로

다른 경로로 독립적으로 처리하게 되고, 이러한 프로토콜을 UDP라고 합니다. 

정보를 주고 받을 때 정보를 보내거나 받는다는 신호절차를 거치지 않는다.

UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다. 신뢰성이 낮다

TCP보다 속도가 빠르다.

속도가 빠르며 네트워크 부하가 적다는 장점이 있지만 신뢰성있는 데이터의 전송을 보장하지는 못합니다. 그렇기 때문에

신뢰성보다는 연속성이 중요한 서비스 예를 들면 실시간 서비스(streaming)에 자주 사용됩니다.

UDP에서 DNS(domain name space: 네트워크에서 도메인이나 호스트이름을 숫자로된 ip주소로 해석해주는 서비스)를 사용함.

하지만 가끔 Tcp에서도 쓰는 경우가 있는데 


### TCP (Transmission Control Protocal)

인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜을 뜻합니다.

TCP는 IP와 함께 사용하는데, IP가 데이터 배달을 처리하면 TCP는 패킷을 추적하고 관리합니다.

TCP 서비스는 송신자와 수신자 모두가 소켓이라고 부르는 종단점을 생성함으로써 이루어지며 연결형 서비스로 가상 회선 방식을 제공한다.

3-way handshaking과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제한다.

흐름 제어 및 혼잡 제어. 높은 신뢰성을 보장한다.
- 흐름제어 : 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것입니다. 예를 들어 송신하는 곳에서 감당이 안되게 데이터를 빠르게 많이 보내면 수신자에서 문제가 발생하기 때문입니다.
- 혼잡제어 : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것입니다. 만약 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는 것을 막습니다.



TCP header에는 code bit(flag bit)가 존재한다. 이부분은 6bit로 이루어져있음.

urg-ack-psh-rst-syn-fin 순서로 되어있으며 해당위치의 비트가 1일때 어떤 내용을 담고있는 패킷인지 나타내게 된다.

syn = synchronize sequence number // ack = acknowledgement

3-way handshaking

1. 클라이언트가 서버에 접속을 요청하는 syn 패킷을 보냄.
2. 서버는 syn패킷을 받고 클라이언트 요청을 받겟다고 클라이언트에게 ack syn 패킷을 보냄
3. 클라이언트는 ack와 syn 패킷을 받고 ack를 서버로 보내 연결하겠다고 서버에게 알림.

4-way handshaking

1. 클라이언트가 연결을 종료하겟다는 fin 플래그를 서버에 보냄
2. 서버는 클라이언트 fin요청을 받고 확인메세지로 ack를 보냄
   1. 데이터를 모두 보낼때까지 time_out 상태가 됨
3. 데이터를 모두 보내고 통신이 끝나면 클라이언트에게 fin플래그를 보냄
4. 클라이언트는 fin 플래그를 받앗다는 메세지를 서버에 보냄
5. 클라이언트의 ack 메세지를 받은 서버는 소켓연결을 닫음

이렇게 확인하는 절차를 거치기떄문에 UDP보다 속도가 느림.

서버와 클라이언트는 1대1로 연결이됨.

정상적인 패킷을 받지 못하면 다시 재요청을 하고, 

Q) 패킷(Packet)이란?

인터넷 내에서 데이터를 보내기 위한 경로배정(라우팅)을 효율적으로 하기 위해서 데이터를 여러 개의 조각들로 나누어 전송을 하는데 이때, 이 조각을 패킷이라고 합니다.

Q) TCP는 패킷을 어떻게 추적 및 관리하나?

위에서 데이터는 패킷단위로 나누어 같은 목적지(IP계층)으로 전송된다고 설명하였습니다. 예를 들어 한줄로 서야하는 A,B,C라는 사람(패킷)들이 서울(발신지)에서 출발하여 부산(수신지)으로 간다고 합시다. 그런데 A,B,C가 순차적으로 가는 상황에서 B가 길을 잘못 들어서 분실되었다고 합시다. 하지만 목적지에서는 A,B,C가 모두 필요한지 모르고 A,C만 보고 다 왔다고 착각할 수 있습니다. 그렇기 때문에 A,,B,C라는 패킷에 1,2,3이라는 번호를 부여하여 패킷의 분실 확인과 같은 처리를 하여 목적지에서 재조립을 합니다. 이런 방식으로 TCP는 패킷을 추적하며, 나누어 보내진 데이터를 받고 조립을 할 수 있습니다.
