---
layout: posts
title: "소켓 프로그래밍"
date: 2024-08-07 10:00:00 +0900
---

소켓 프로그래밍은 네트워크를 통해 다른 컴퓨터와 데이터를 주고받기 위한 기술입니다. 소켓은 네트워크를 통해 데이터를 주고받기 위해 사용되는 양 끝단을 의미합니다.
소켓 프로그래밍을 통해 클라이언트와 서버 간의 통신을 설정하고 데이터를 교환할 수 있습니다.

# 소켓의 기본 개념

- **소켓(Socket)**: 네트워크를 통해 통신하기 위한 엔드포인트.
- **IP 주소**: 네트워크 상에서의 컴퓨터 위치를 나타내는 주소.
- **포트(Port)**: 네트워크 상에서 특정 프로세스를 식별하기 위한 숫자.
- **프로토콜(Protocol)**: 네트워크 통신을 위한 규칙. (예: TCP, UDP)

# TCP

TCP(Transmission Control Protocol)는 애플리케이션 사이에서 안전하게 데이터를 통신하는 규약입니다.

데이터를 다양한 경로로 주고받으면 더 빠르게 통신할 수 있습니다. 고속도로가 막힐 때 일반 도로로 돌아가는 것과 비슷합니다. 하지만 데이터 통신 경로가 다양해지면
속도가 향상되는 대신 안정성이 떨어질 수 있습니다. 경로가 다양해지면 데이터 일부가 유실되거나 늦게 전달될 수 있기 때문입니다.

그래서 통신 속도를 높이면서도 안정성, 신뢰성을 보장할 수 있는 TCP가 탄생했습니다.

## 통신 과정

네트워크에서 데이터를 통째로 통신하는 경우는 거의 없습니다. 예를 들어 이미지를 전송할 때 여러 조각으로 나누어 각 조각을 따로 전달한 뒤 합칠 수 있습니다.
TCP 통신도 마찬가지로 과정은 다음과 같이 요약할 수 있습니다.

1. 데이터 스트림에서 받은 데이터를 일정 단위로 분할합니다.
2. 분할된 데이터 단위에 TCP 헤더를 붙여서 TCP 세그먼트를 생성합니다.
3. TCP 세그먼트를 IP 데이터그램으로 변환합니다. IP 데이터그램은 인터넷 통신에 사용되는 데이터 패킷입니다.
4. IP 데이터그램을 수신 애플리케이션에 보냅니다.

## TCP 세그먼트

TCP 세그먼트는 헤더와 데이터 필드가 나뉘어 있습니다. 주요 헤더를 자세히 살펴봅시다.

- Source Port

    데이터를 발송하는 애플리케이션의 포트 번호입니다.
    
- Destination Port
    
    데이터를 수신하는 애플리케이션의 포트 번호입니다.
    
- Sequence Number(SYN)

    TCP 통신 과정에서 데이터를 일정 단위로 분할하는데요. 분할된 데이터의 순서입니다.
    
- Acknowledgment Number(ACK)

    데이터를 수신하는 애플리케이션 입장에서, 다음으로 받고 싶은 TCP 세그먼트의 Sequence Number입니다. 통신 과정에서 분할된 데이터가 순서대로 전달되지
    않거나 각자 다른 경로로 통신될 수 있습니다. 위와 같은 TCP 헤더가 있어서 수신 애플리케이션에서 데이터를 안전하고 정확하게 원상복구할 수 있습니다.

```text
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |       |C|E|U|A|P|R|S|F|                               |
| Offset| Rsrvd |W|C|R|C|S|S|Y|I|            Window             |
|       |       |R|E|G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                           [Options]                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               :
:                             Data                              :
:                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## TCP Handshake

TCP 통신을 하려면 네트워크 연결 설정이 먼저 필요해요. 데이터를 발송하는 애플리케이션, 수신하는 애플리케이션 모두 준비가 됐다는 것을 보장하기 위해서 필요합니다.
데이터를 정확하게 전달하기 위해 세션을 만드는 과정이기도 하고요. TCP 연결을 생성할 때는 3-Way Handshake를 이용합니다.

1. SYN: 클라이언트가 서버에게 SYN을 전송해요. SYN은 임의 숫자 A입니다.
2. SYN-ACK: 서버가 SYN-ACK로 답장해요. SYN은 새로운 임의 숫자 B이고, ACK는 클라이언트로부터 전달받은 SYN의 1을 덧셈한 것입니다(A+1).
3. ACK: 클라이언트가 서버에게 ACK(B+1)를 전송합니다.

1-2번 과정은 클라이너트-서버 연결을 확인하고, 2-3번 과정은 반대로 서버-클라이언트 연결을 확인해요. Handshake로 안정적인 연결을 확인하면 데이터 통신을
시작할 수 있습니다.

# 참고자료

- [Wikipedia - Socket Programming](https://en.wikipedia.org/wiki/Socket_programming)
- [Toss payments - TCP](https://docs.tosspayments.com/resources/glossary/tcp#tcp-%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8)