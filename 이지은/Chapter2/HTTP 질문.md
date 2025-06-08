## 💬 HTTP 관련 신입 백엔드 면접 예상 질문

### 1) HTTP/1.0
#### 💬 HTTP/1.0의 주요 특징과 한계점은 무엇인가요?
<details> <summary>✅ 모범 답안</summary>

특징:
* 연결당 단일 요청 처리 (1요청 → 1응답 → 연결 종료)
* 헤더 도입으로 메타데이터 전송 가능 (Content-Type, 상태 코드)
* GET, POST, HEAD 메서드 지원

한계:
* 매 요청마다 TCP 3-way 핸드셰이크 반복 → RTT 증가 
* 이미지/리소스 증가 시 성능 저하 심각 
* 해결 방안: 이미지 스플리팅, Base64 인코딩

</details>

<br>

### 2) HTTP/1.1
#### 💬 HTTP/1.1에서 도입된 Keep-Alive와 HOL Blocking 문제를 설명해주세요.
<details> <summary>✅ 모범 답안</summary>

* Keep-Alive: 하나의 TCP 연결로 여러 요청/응답 처리 → 핸드셰이크 오버헤드 감소
* HOL Blocking: 파이프라이닝 사용 시 선행 요청의 지연이 후속 요청에 미치는 문제

</details>

<br>

#### 💬 HTTP/1.1의 헤더 구조가 무겁다고 하는 이유는 무엇인가요?
<details> <summary>✅ 모범 답안</summary>

쿠키, 사용자 에이전트 등 중복된 메타데이터가 매 요청마다 전송
압축되지 않은 평문 헤더 → 대역폭 낭비 (예: Cookie: session-id=abcd... 1KB 이상)

👉 해결: HTTP/2의 HPACK 헤더 압축 도입

</details>

<br>

### 3) HTTP/2
#### 💬 HTTP/2의 멀티플렉싱이 HOL Blocking을 어떻게 해결하나요?
<details> <summary>✅ 모범 답안</summary>

* 멀티플렉싱:
단일 연결에서 여러 스트림(요청)을 병렬 처리
프레임 단위로 분할 → 독립적 전송 및 재조립

* HOL Blocking 해결:
스트림별 독립성 유지 (예: 스트림 A의 패킷 손실이 스트림 B에 영향 없음)
우선순위 설정으로 중요 리소스 먼저 처리

</details>

<br>

#### 💬 HTTP/2의 서버 푸시 기능은 어떤 장점이 있나요?
<details> <summary>✅ 모범 답안</summary>
동작: 클라이언트 요청 없이 서버가 CSS/JS 파일 등을 사전 전송

장점:
추가 요청 감소 → 페이지 로딩 시간 단축
캐시 활용도 증가 (예: style.css를 푸시 후 HTML에서 재사용)

주의점: 과도한 푸시는 오히려 성능 저하 유발

</details>

<br>

### 4) HTTPS
#### 💬 SSL/TLS 핸드셰이크 과정을 간략히 설명해주세요.
<details> <summary>✅ 모범 답안</summary>

Client Hello: 지원하는 암호화 스위트 전송
Server Hello: 선택한 암호화 스위트 + 인증서 전송
키 교환: 클라이언트가 Pre-Master Secret 생성 → 서버 공개키로 암호화 전송
세션 키 생성: 양측이 Pre-Master Secret으로 Master Secret → 세션 키 도출
암호화 통신 시작

</details>

<br>

#### 💬 HTTPS가 SEO에 미치는 영향을 설명해주세요.
<details> <summary>✅ 모범 답안</summary>
긍정적 영향:

구글 검색 순위 상승 요인 (보안 강조 정책)

사용자 신뢰도 증가 → 체류 시간 증가

최적화 방법:

캐노니컬 태그 설정 (<link rel="canonical">)

페이지 속도 개선 (Lighthouse 도구 활용)

사이트맵 제출 (Google Search Console)

</details>

<br>

### 5) HTTP/3
#### 💬 HTTP/3가 UDP를 사용하는 이유와 QUIC 프로토콜의 장점은 무엇인가요?
<details> <summary>✅ 모범 답안</summary>
* UDP 선택 이유:
TCP의 HOL Blocking 및 핸드셰이크 오버헤드 회피
사용자 정의 신뢰성 메커니즘 구현 가능 (QUIC 내재화)

* QUIC 장점:
0-RTT 핸드셰이크: 이전 연결 캐시 활용 → 지연 최소화
연결 마이그레이션: IP 변경 시 재연결 불필요 (예: Wi-Fi → LTE 전환)
멀티플렉싱 개선: 패킷 손실 시 스트림별 독립 복구

</details>

<br>

#### 💬 HTTP 버전별 핵심 차이점을 표로 정리해주세요.
<details> <summary>✅ 모범 답안</summary>

| 기능               | HTTP/1.0      | HTTP/1.1    | HTTP/2   | HTTP/3        |
|------------------|---------------|-------------|----------|---------------|
| **연결 방식**        | 비지속적          | Keep-Alive  | 멀티플렉싱    | QUIC (UDP 기반) |
| **HOL Blocking** | 발생            | 파이프라이닝 시 발생 | 스트림별 해결  | 스트림별 해결       |
| **헤더 처리**        | 비압축           | 비압축         | HPACK 압축 | QPACK 압축      |
| **보안**           | 미지원           | 미지원         | HTTPS 필요 | 내장 암호화 (QUIC) |
| **지연 시간**        | 높음 (반복 핸드셰이크) | 중간          | 낮음       | 극히 낮음 (0-RTT) |

</details>