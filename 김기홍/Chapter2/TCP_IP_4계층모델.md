# TCP/IP 4계층 모델

[계층 구조와 주요 기능]
TCP/IP 4계층 모델은 인터넷 통신의 표준 프레임워크로, 데이터 전송 과정을 4개의 계층으로 추상화합니다.<br>
각 계층의 역할과 대표 프로토콜은 다음과 같습니다.
---
## 계층 구조
1. 애플리케이션 계층 (Application Layer)
    - 역할: 사용자와 직접 상호작용하는 서비스 제공 (HTTP, FTP, DNS 등)
    - PDU: 메시지(Message)
    - 핵심 포인트:
        - HTTP/HTTPS: 웹 통신의 기본 (상태 비저장성, REST API 동작 원리 이해 필수)
        - DNS: 도메인-IP 변환 메커니즘 (Recursive vs Iterative Query 차이)
        - 실무 활용: API 엔드포인트 설계, JWT 인증 구현 시 직접 사용
2. 전송 계층
    - 역할: 포트 기반 신뢰성 있는 데이터 전송 관리
    - PDU: 세그먼트(Segment, TCP) / 데이터그램(Datagram, UDP)
    - 핵심 포인트:
        - TCP 3-way handshake
    ```
   1. SYN → 2. SYN-ACK → 3. ACK (연결 수립)
   4. DATA → 5. ACK (실제 데이터 전송)[4][5]
    ```

3. 인터넷 계층 (Internet Layer)
    - 역할: IP 주소 기반 논리적 라우팅
    - PDU: 패킷(Packet)
    - 핵심 포인트:
        - IPv4 vs IPv6: 주소 고갈 문제 해결 (128bit 확장)
        - 라우팅 테이블: 목적지 IP 기반 최적 경로 선택
        - ICMP: Ping/Traceroute 진단 도구 기반 프로토콜

4. 링크 계층 (Link Layer)
    - 역할: MAC 주소를 이용한 물리적 데이터 전송
    - PDU: 프레임(Frame)
    - 핵심 포인트:
        - ARP: IP → MAC 주소 변환 프로토콜
        - 이더넷: 가장 널리 사용되는 LAN 기술 (CSMA/CD 충돌 방지)

---
🛠 캡슐화/역캡슐화 과정
데이터가 계층을 통과할 때 추가되는 정보:

애플리케이션: HTTP 헤더 추가

전송: TCP/UDP 헤더 (출발지/목적지 포트)

인터넷: IP 헤더 (출발지/목적지 IP)

링크: 이더넷 헤더 (출발지/목적지 MAC)

예시: 웹 요청 시
HTTP 메시지 → TCP 세그먼트 → IP 패킷 → 이더넷 프레임
---

## PDU
