# 2. TCP/IP 4계층 모델

##  개요

- 네트워크 통신을 계층별로 나눈 **표준 참조 모델**
- 각 계층은 **역할 분리**, **모듈화**, **책임 분담**을 위함
- 실제 인터넷 통신은 **TCP/IP 4계층 구조**에 따라 이루어짐

---

##  TCP/IP 4계층 구조

| 계층 | 주요 프로토콜 | 설명 |
|------|----------------|------|
| **4. 응용 계층** (Application) | HTTP, HTTPS, FTP, SMTP, DNS | 사용자 애플리케이션이 사용하는 계층 |
| **3. 전송 계층** (Transport) | TCP, UDP | 신뢰성 있는 데이터 전송(TCP) 또는 빠른 전송(UDP) |
| **2. 인터넷 계층** (Internet) | IP, ICMP, ARP | 목적지까지 패킷 전달, 라우팅 |
| **1. 네트워크 인터페이스 계층** (Network Interface / Link) | Ethernet, Wi-Fi | 물리적 연결, MAC 주소 기반 통신 |

---


![KakaoTalk_20250519_102908512](https://github.com/user-attachments/assets/90663820-d533-4f69-a802-5bb021c5ddbf)

## OSI 각 계층 상세 설명

| OSI 7계층             | TCP/IP 4계층              | 설명 |
|------------------------|----------------------------|------|
| 7. 응용 계층 (Application)   |                            | 사용자와 가장 가까운 계층 |
| 6. 표현 계층 (Presentation) | → 응용 계층               | 데이터 인코딩, 암호화 |
| 5. 세션 계층 (Session)       |                            | 연결 관리, 세션 유지 |
| 4. 전송 계층 (Transport)     | 전송 계층 (TCP/UDP)       | 종단 간 연결, 포트 기반 통신 |
| 3. 네트워크 계층 (Network)   | 인터넷 계층 (IP)          | 라우팅, IP 주소 기반 전송 |
| 2. 데이터 링크 계층 (Data Link) |                            | MAC 주소, 프레임 전달 |
| 1. 물리 계층 (Physical)       | 네트워크 인터페이스 계층 | 전기 신호, 케이블, 물리적 전송 매체 |

## 4 계층 상세 설명

### 1. 네트워크 인터페이스 계층 (Link Layer)
- 1+2 = (물리 + 데이터 링크)
- 실제 장비 간의 **물리적 데이터 전송**
- LAN, 스위치, MAC 주소 등을 사용
- 하드웨어적 통신 + 프레임 단위 전송

### 2. 인터넷 계층 (IP Layer)
- 3계층
- **패킷의 목적지 IP 주소**에 따라 라우팅
- 주요 프로토콜: IP(IPv4, IPv6), ICMP(핑), ARP(MAC ↔ IP 매핑)
- 네트워크 간 데이터 전송 책임

### 3. 전송 계층 (Transport Layer)
- 4계층
- **종단 간(end-to-end) 연결 및 신뢰성 확보**
- TCP: 연결지향, 신뢰성 보장, 순서 보장
- UDP: 비연결, 빠르지만 신뢰성 없음 (ex. 실시간 스트리밍)

### 4. 응용 계층 (Application Layer)
- 5~7계
- 사용자 서비스 제공: HTTP, FTP, DNS 등
- 개발자가 가장 자주 접하는 계층

---

##  TCP vs UDP 비교

| 항목 | TCP | UDP |
|------|-----|-----|
| 연결 방식 | 연결 지향 (3-way handshake) | 비연결 지향 |
| 신뢰성 | 있음 (재전송, 순서 보장) | 없음 |
| 속도 | 느림 (신뢰성 위해 오버헤드 존재) | 빠름 (신속 전송 위주) |
| 예시 | HTTP, HTTPS, FTP | DNS, 스트리밍, 게임, VoIP |

* 3 way handshake : 신뢰성을 확보할 때 사용

---

##  캡슐화 구조 예시

> HTTP 요청을 전송하면…

- HTTP (응용 계층)
  ↓
- TCP/UDP (전송 계층)
  ↓
- IP (인터넷 계층)
  ↓
- Ethernet 프레임 (네트워크 인터페이스 계층)

→ **각 계층마다 헤더를 덧붙이며 하위 계층으로 내려감 (Encapsulation)**

---

##  백엔드 개발자 입장에서 중요한 계층 
(물리, 데이터, 네트워크) 

- **응용 계층**: HTTP 통신, REST API, 쿠키/세션
- **전송 계층**: TCP 소켓, 포트 번호, 커넥션 관리
- **인터넷 계층**: IP 주소, 서브넷, 라우팅 개념 이해

---

## 면접 예상 질문

1. TCP/IP 4계층의 각 계층 역할을 설명해주세요.
2. TCP와 UDP의 차이점은 무엇인가요?
3. HTTP는 어떤 계층에서 동작하나요?
4. 캡슐화와 역캡슐화는 무엇인가요?
5. 백엔드 개발자가 TCP/IP 계층을 이해해야 하는 이유는 무엇인가요?
