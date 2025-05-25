### 1) TCP/IP 4계층 구조
#### 💬 TCP/IP 4계층 구조에 대해 설명해보세요.
*
  <details>
  <summary>✅ 모범 답안</summary>

    TCP/IP 4계층 모델은 인터넷 통신을 위한 네트워크 표준 모델로, 네트워크의 기능을 4개의 계층으로 나눕니다.
    - 1계층(링크/네트워크 접근 계층): 실제 데이터 전송, 물리적 연결, MAC 주소 사용. 대표 프로토콜: Ethernet.
    - 2계층(인터넷 계층): 네트워크 간 데이터 전송, 논리적 주소(IP) 지정, 라우팅. 대표 프로토콜: IP, ARP, ICMP.
    - 3계층(전송 계층): 송수신자 간 신뢰성, 흐름 제어, 오류 제어. 대표 프로토콜: TCP(신뢰성), UDP(속도).
    - 4계층(애플리케이션 계층): 사용자와 직접 소통, 웹·이메일 등 서비스 제공. 대표 프로토콜: HTTP, FTP, SMTP, DNS 등.
  </details>

<br>

#### 💬 각 계층에서 사용되는 대표 프로토콜과 역할을 예시와 함께 설명해주세요.
*
  <details>
  <summary>✅ 모범 답안</summary>

    - 애플리케이션 계층: HTTP(웹), FTP(파일 전송), SMTP(이메일), DNS(도메인-IP 변환), SSH(원격 접속)
    - 전송 계층: TCP(웹, 이메일 등 신뢰성 필요), UDP(스트리밍, 게임 등 속도 우선)
    - 인터넷 계층: IP(주소 지정, 라우팅), ARP(MAC-IP 변환), ICMP(네트워크 진단)
    - 링크 계층: Ethernet(유선 LAN), PPP(전화선 등), 실제 데이터 프레임 송수신
  </details>

<br>

#### 💬 TCP와 UDP의 차이점은 무엇인가요?
*
  <details>
  <summary>✅ 모범 답안</summary>

    - TCP: 연결 지향, 신뢰성 보장(순서, 재전송), 느리지만 정확. 예: 웹, 이메일, 파일 전송
    - UDP: 비연결형, 신뢰성 낮음(순서·재전송 없음), 빠름. 예: 스트리밍, 온라인 게임, VoIP
    - 기술적 차이: TCP는 3-way handshake로 연결, 4-way handshake로 해제. UDP는 연결/해제 과정 없음
    - 오버헤드: TCP가 더 큼
    - 사용 예시: TCP는 HTTP, UDP는 동영상 스트리밍
  </details>

<br>

#### 💬 TCP 3-way handshake와 4-way handshake 과정에 대해 설명해주세요.
*
  <details>
  <summary>✅ 모범 답안</summary>

    - 3-way handshake(연결):
        1. 클라이언트가 SYN 패킷 전송
        2. 서버가 SYN+ACK 패킷 응답
        3. 클라이언트가 ACK 패킷 전송 → 연결 성립
    - 4-way handshake(해제):
        1. 클라이언트가 FIN 전송
        2. 서버가 ACK 응답
        3. 서버가 FIN 전송
        4. 클라이언트가 ACK 응답 → 연결 해제
    - 의의: 신뢰성 있는 연결과 안전한 종료를 보장
  </details>

<br>

#### 💬 PDU(Protocol Data Unit)란 무엇이며, 각 계층에서 PDU의 명칭은 무엇인가요?
*
  <details>
  <summary>✅ 모범 답안</summary>
    - 애플리케이션 계층: 메시지(Data)
    - 전송 계층: 세그먼트(TCP), 데이터그램(UDP)
    - 인터넷 계층: 패킷(Packet)
    - 링크 계층: 프레임(Frame), 비트(Bit)
      PDU는 헤더(제어 정보)와 페이로드(실제 데이터)로 구성됩니다.
  </details>

<br>

#### 💬 유선 LAN과 무선 LAN의 차이점, 각각의 표준과 특징을 설명해주세요.
*
  <details>
  <summary>✅ 모범 답안</summary>

    - 유선 LAN:
        - 표준: IEEE 802.3(Ethernet)
        - 통신 방식: 전이중(Full Duplex), 충돌 제어(CSMA/CD, 현재는 거의 미사용)
        - 매체: UTP, STP, 광섬유
        - 특징: 안정성 높음, 설치 복잡
    - 무선 LAN:
        - 표준: IEEE 802.11(Wi-Fi)
        - 통신 방식: 반이중(Half Duplex), 충돌 회피(CSMA/CA)
        - 매체: 전파(2.4GHz, 5GHz)
        - 특징: 설치 용이, 환경에 민감, 이동성 우수
  </details>

<br>

#### 💬 네트워크 계층 간 데이터 송수신 과정(캡슐화/비캡슐화)이란 무엇인가요?
*
  <details>
  <summary>✅ 모범 답안</summary>

    - 캡슐화: 상위 계층 데이터에 하위 계층의 헤더를 붙여 전송 단위(PDU)를 만들어 내려보냄.  
      예: 애플리케이션 데이터 → TCP 세그먼트 → IP 패킷 → Ethernet 프레임
    - 비캡슐화: 수신 측에서 각 계층의 헤더를 제거하며 상위 계층으로 전달
    - 의의: 계층별로 독립적인 데이터 처리와 표준화, 문제 발생 시 원인 추적이 용이함
  </details>
