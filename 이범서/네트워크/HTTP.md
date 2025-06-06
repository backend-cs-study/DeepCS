# HTTP

HTTP(HyperText Transfer Protocol)는 웹 서비스 통신에 사용되는 애플리케이션 계층 프로토콜입니다. 텍스트, 이미지, 음성, 영상 파일, JSON, XML 등 거의 모든 형태의 데이터를 전송할 수 있습니다.

| 특징             | HTTP/1.1 (TCP 기반)    | HTTP/2 (TCP 기반)           | HTTP/3 (UDP 기반)         |
| :--------------- | :--------------------- | :-------------------------- | :------------------------ |
| **연결 방식** | Keep-Alive             | 멀티플렉싱                  | 멀티플렉싱                |
| **HOL Blocking** | 파이프라이닝 시 발생   | 스트림별 해결               | 스트림별 해결             |
| **헤더 처리** | 비압축                 | HPACK 압축                  | QPACK 압축                |
| **보안** | HTTPS 필요             | HTTPS 필요                  | QUIC에 내장               |
| **서버 푸시** | 미지원                 | 지원                        | 지원                      |

---

## HTTP/1.0

HTTP/1.0은 **한 연결당 하나의 요청**만 처리하는 방식입니다. 클라이언트가 서버에 요청을 보내고 응답을 받으면 즉시 TCP 연결을 끊었습니다.

### 특징 및 한계점

* **한 연결당 단일 요청**: 매 요청마다 새로운 TCP 3-way 핸드셰이크가 필요했습니다.
* **RTT(Round Trip Time) 증가**: 잦은 연결 설정/해제로 인해 패킷 왕복 시간이 증가하며, 이는 서버 부담 증가 및 응답 시간 지연으로 이어지는 주요 한계였습니다.

### 한계점 해결을 위한 노력 (HTTP/1.0 시대)

* **이미지 스플리팅 (Image Splitting)**: 여러 이미지를 하나의 큰 이미지로 합쳐 다운로드하고, `background-image`의 `position` 속성을 이용해 원하는 이미지를 표시하는 기법입니다. HTTP 요청 수를 줄여 RTT 증가 문제를 완화했습니다.
* **코드 압축 (Code Compression)**: HTML, CSS, JavaScript 코드에서 개행 문자나 빈칸을 제거하여 파일 크기를 최소화했습니다.
* **이미지 Base64 인코딩 (Image Base64 Encoding)**: 이미지 파일을 64진법 문자열로 인코딩하여 HTML/CSS 코드 내에 직접 포함했습니다. 서버에 별도의 HTTP 요청을 할 필요가 없어졌지만, **Base64 인코딩으로 인해 파일 크기가 약 33% 정도 커지는 단점**이 있었습니다.

---

## HTTP/1.1

HTTP/1.1은 HTTP/1.0의 비효율성을 개선하기 위해 등장했습니다.

### 주요 개선 사항

* **Keep-Alive 옵션 도입**: 한 번 TCP 연결을 맺으면 여러 요청과 응답을 해당 연결을 통해 처리할 수 있게 되었습니다. 이는 매 요청마다 발생하는 TCP 3-way 핸드셰이크 오버헤드를 줄여 효율성을 크게 높였습니다.
* **파이프라이닝 (Pipelining)**: 응답을 기다리지 않고 여러 요청을 연속적으로 보낼 수 있으며, 서버는 요청 순서에 맞춰 응답을 보냅니다.
* **Host 헤더 도입**: 하나의 IP 주소에 여러 도메인을 호스팅할 수 있게 하여 가상 호스팅(Virtual Hosting)을 지원합니다.

### 한계점

* **HOL Blocking (Head Of Line Blocking)**: 파이프라이닝 사용 시, 먼저 보낸 요청 중 하나가 네트워크 지연 등으로 오래 걸리면, 그 뒤의 모든 요청들이 앞선 요청의 응답을 기다리며 멈추는 성능 저하 현상이 발생했습니다.
* **무거운 헤더 구조**: 헤더에 쿠키, 사용자 에이전트 등 중복된 메타데이터가 압축되지 않은 평문 형태로 매 요청마다 전송되어 불필요한 대역폭 낭비가 발생했습니다.

---

## HTTP/2

SPDY 프로토콜에서 파생된 HTTP/2는 HTTP/1.x보다 지연 시간을 줄이고 응답 시간을 빠르게 하는 데 초점을 맞춘 프로토콜입니다.

### 주요 특징

* **멀티플렉싱 (Multiplexing)**:
    * 단일 TCP 연결 위에서 여러 개의 독립적인 **스트림**(요청과 응답의 흐름)을 동시에 송수신할 수 있습니다.
    * 데이터는 독립적인 **프레임**으로 분할되어 스트림을 통해 병렬적으로 전송된 후 다시 조립됩니다.
    * 특정 스트림의 패킷 손실이 발생해도 다른 스트림에는 영향을 주지 않습니다. 이로써 **HOL Blocking 문제를 효과적으로 해결**합니다.
* **헤더 압축 (Header Compression - HPACK)**:
    * **허프만 코딩 압축 알고리즘**을 사용하여 헤더를 압축합니다. 이전 요청의 헤더 정보를 참조하여 중복되는 헤더는 전송하지 않으므로, 데이터 전송량을 크게 줄입니다.
* **서버 푸시 (Server Push)**:
    * 클라이언트의 명시적인 요청 없이도 서버가 클라이언트가 필요할 것이라고 예상되는 리소스(예: CSS, JavaScript 파일, 이미지 등)를 미리 보내주는 기능입니다.
    * 클라이언트-서버 간의 추가적인 요청-응답 왕복(RTT)을 줄여 웹 페이지 로딩 시간을 크게 단축시킬 수 있습니다.
* **스트림 우선순위 지정 (Stream Prioritization)**:
    * 클라이언트가 요청에 우선순위를 부여할 수 있어, 서버는 중요도가 높은 리소스를 먼저 전송하여 사용자 경험을 개선합니다.

---

## HTTPS

HTTPS는 애플리케이션 계층과 전송 계층 사이에 **SSL/TLS 계층**을 추가하여 **신뢰 가능한 HTTP 요청**을 만듭니다. 즉, **통신 암호화**를 목적으로 합니다.

### SSL/TLS

* **SSL (Secure Socket Layer)**은 버전이 올라가면서 **TLS (Transport Layer Security)**로 이름이 변경되었습니다.
* 전송 계층에서 보안을 제공하는 프로토콜로, 통신 시 제3자가 메시지를 도청하거나 변조하는 것을 방어합니다.
* **보안 세션**을 기반으로 데이터를 암호화합니다. 보안 세션은 **인증 메커니즘, 키 교환 암호화 알고리즘, 해싱 알고리즘**을 통해 생성됩니다.
* **TLS 1.3**: 이전에 방문한 사이트 재방문 시, SSL/TLS 핸드셰이크를 건너뛸 수 있는 **0-RTT (Zero Round Trip Time)** 기능을 통해 초기 연결 설정 시간을 단축합니다.

### 보안 세션 구성 요소

1.  **인증 메커니즘**: CA(인증 기관)에서 발급한 **인증서**를 기반으로 공개키를 클라이언트에 제공하고 신뢰를 보장합니다. 클라이언트는 서버로부터 받은 인증서가 신뢰할 수 있는 CA에 의해 발급되었는지 확인하여 서버의 신원을 검증합니다.
2.  **키 교환 암호화 알고리즘**: 클라이언트와 서버가 암호화에 사용할 비밀 키를 안전하게 공유하는 방법입니다. 주로 **디피-헬만 방식 (Diffie-Hellman Key Exchange)**을 기반으로 하는 **`ECDHE` (Elliptic Curve Diffie-Hellman Ephemeral)** 또는 `DHE` (Diffie-Hellman Ephemeral)가 사용됩니다. 이를 통해 실제 데이터 암호화에 사용될 **세션 키(대칭 키)**를 안전하게 공유합니다.
3.  **해싱 알고리즘**: 데이터의 **무결성 검증**에 사용됩니다. SSL/TLS는 주로 **SHA-256** 및 **SHA-384** 알고리즘을 사용합니다.

### HTTPS와 SEO (검색 엔진 최적화)

* Google은 HTTPS를 사용하는 웹사이트에 검색 순위 가산점을 부여합니다.
* 사용자 신뢰도를 높여 웹사이트의 체류 시간 증가 등 간접적인 SEO 이점으로 이어집니다.
* **최적화 방법**: 캐노니컬 설정, 페이지 속도 개선, 사이트맵 관리 등을 함께 고려해야 합니다.

### HTTPS 구축 방법

1.  **직접 CA에서 인증서 구매**: `Let's Encrypt` 등에서 인증서를 발급받아 서버에 직접 설치합니다.
2.  **로드 밸런서 활용**: `AWS ELB`, `Nginx` 등 로드 밸런서에서 SSL/TLS를 Offloading 하여 서버의 부담을 줄입니다.
3.  **CDN 활용**: `Cloudflare`, `AWS CloudFront` 등의 CDN을 사용하여 사용자에게 더 빠르게 콘텐츠를 전송하며, CDN 단에서 HTTPS를 처리합니다.

---

## HTTP/3

HTTP/3는 **QUIC (Quick UDP Internet Connections)**이라는 새로운 전송 계층 프로토콜 위에서 **UDP 기반**으로 동작합니다.

### 주요 특징

* **UDP 기반**: TCP의 HOL Blocking 및 느린 연결 설정 시간을 극복하기 위해 UDP를 사용하며, QUIC 프로토콜이 TCP의 신뢰성, 혼잡 제어 등을 UDP 위에서 직접 구현합니다.
* **초기 연결 설정 지연 시간 감소**: QUIC는 TCP의 3-way 핸드셰이크와 TLS 핸드셰이크를 통합하여 첫 연결 설정 시 1-RTT(경우에 따라 0-RTT)만 소요되므로 연결 설정 시간이 단축됩니다.
* **향상된 멀티플렉싱 및 HOL Blocking 해결**: QUIC는 스트림별 독립적인 패킷 손실 복구를 지원하여 TCP보다 훨씬 효과적으로 HOL Blocking을 해결합니다.
* **연결 마이그레이션**: 클라이언트의 IP 주소나 포트 번호가 변경되더라도(예: Wi-Fi에서 LTE로 전환) 기존 연결이 끊어지지 않고 유지됩니다.
* **내장된 암호화**: QUIC 자체에 TLS 1.3 기반의 암호화 기능이 내장되어 있어, 별도로 HTTPS를 구성할 필요 없이 기본적으로 보안이 적용됩니다.

---

## 💬 HTTP 관련 신입 백엔드 면접 예상 질문

### HTTP/1.0
* HTTP/1.0의 주요 특징과 한계점은 무엇인가요?
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/1.0은 **한 연결당 하나의 요청만 처리**하는 방식입니다. 이로 인해 웹 페이지의 리소스 하나하나마다 새로운 TCP 3-way 핸드셰이크가 반복되어 **RTT(Round Trip Time)가 증가**하고 서버 부담이 커지는 한계가 있었습니다. 이를 해결하기 위해 이미지 스플리팅, 코드 압축, Base64 인코딩 같은 방법들이 사용되었습니다.
    </details>

### HTTP/1.1
* HTTP/1.1에서 도입된 Keep-Alive와 HOL Blocking 문제를 설명해주세요.
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/1.1에서는 **Keep-Alive** 기능을 통해 한 번 맺은 TCP 연결로 여러 요청과 응답을 처리하여 핸드셰이크 오버헤드를 줄였습니다. 하지만 **HOL Blocking (Head Of Line Blocking)** 문제가 존재합니다. 파이프라이닝 사용 시, 먼저 보낸 요청 중 하나가 지연되면 그 뒤의 모든 요청들이 앞선 요청의 응답을 기다리며 멈추게 되어 전체 응답 시간을 지연시킵니다.
    </details>

* HTTP/1.1의 헤더 구조가 무겁다고 하는 이유는 무엇인가요?
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/1.1의 헤더는 쿠키나 사용자 에이전트 같은 **메타데이터가 압축되지 않은 평문 형태로 매 요청마다 중복 전송**되기 때문에 무겁다고 평가됩니다. 이는 불필요하게 많은 대역폭을 소비하여 네트워크 효율성을 떨어뜨렸습니다.
    </details>

### HTTP/2
* HTTP/2의 멀티플렉싱이 HOL Blocking을 어떻게 해결하나요?
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/2의 **멀티플렉싱**은 단일 TCP 연결 위에서 여러 개의 독립적인 **스트림**을 동시에 송수신함으로써 HOL Blocking을 해결합니다. 각 요청과 응답은 프레임 단위로 쪼개져서 병렬적으로 전송되므로, 한 스트림의 지연이 다른 스트림에 영향을 주지 않아 병목 현상이 발생하지 않습니다.
    </details>

* HTTP/2의 서버 푸시 기능은 어떤 장점이 있나요?
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/2의 **서버 푸시** 기능은 클라이언트의 요청 없이도 서버가 미리 필요할 것이라고 예상되는 리소스(CSS, JS 등)를 클라이언트에게 푸시해서 보내주는 기능입니다. 이 기능의 가장 큰 장점은 클라이언트-서버 간의 **추가적인 요청-응답 왕복(RTT)을 줄여 웹 페이지 로딩 시간을 크게 단축**시킬 수 있다는 점입니다.
    </details>

### HTTPS
* SSL/TLS 핸드셰이크 과정을 간략히 설명해주세요.
    <details>
    <summary>✅ 모범 답안</summary>
    SSL/TLS 핸드셰이크는 클라이언트와 서버가 암호화된 통신을 시작하기 위해 필요한 암호화 방식, 키 교환, 인증 등을 협상하는 과정입니다. 주요 단계는 **Client Hello**, **Server Hello (인증서 전송 포함)**, **인증서 검증**, **키 교환 (Pre-Master Secret 암호화 전송)**, **세션 키 생성**, 그리고 **암호화 통신 시작**으로 이루어집니다. 이 과정을 통해 양측은 안전하게 대칭키를 공유하고 데이터를 암호화합니다.
    </details>

* HTTPS가 SEO에 미치는 영향을 설명해주세요.
    <details>
    <summary>✅ 모범 답안</summary>
    HTTPS는 SEO(검색 엔진 최적화)에 **긍정적인 영향**을 미칩니다. 구글은 사용자 보안을 강조하며 HTTPS를 사용하는 웹사이트에 검색 순위 가산점을 부여합니다. 또한, HTTPS는 사용자 신뢰도를 높여 웹사이트의 체류 시간을 증가시키는 등 간접적인 SEO 이점도 제공합니다.
    </details>

### HTTP/3
* HTTP/3가 UDP를 사용하는 이유와 QUIC 프로토콜의 장점은 무엇인가요?
    <details>
    <summary>✅ 모범 답안</summary>
    HTTP/3가 UDP를 사용하는 이유는 **TCP의 한계점, 특히 HOL Blocking과 느린 연결 설정 시간을 극복하기 위해서**입니다. UDP 기반의 **QUIC 프로토콜**은 다음과 같은 장점을 제공합니다: 0-RTT/1-RTT 연결 설정으로 초기 연결 지연을 최소화하고, 스트림별 독립적인 패킷 손실 복구를 통해 HOL Blocking을 효과적으로 해결하며, 클라이언트의 IP 주소 변경 시에도 연결을 유지하는 **연결 마이그레이션** 기능을 제공합니다. 또한, QUIC 자체에 TLS 1.3 기반의 암호화가 내장되어 있어 보안이 기본적으로 적용됩니다.
    </details>

---

## 💡 추가 심화 내용

* **웹 소켓 (WebSocket)**
    * HTTP는 기본적으로 클라이언트의 요청에 서버가 응답하는 **단방향(요청-응답)** 통신 모델을 따릅니다. 웹 소켓은 한 번 연결이 수립되면 클라이언트와 서버 간에 **지속적인 양방향 통신 채널**을 제공합니다. 실시간 채팅, 온라인 게임, 알림 서비스 등 실시간성이 중요한 서비스에 활용됩니다.

* **RESTful API**
    * **Representational State Transfer (REST)** 아키텍처 스타일을 따르는 API 디자인 원칙입니다.
    * 모든 것을 **자원(Resource)**으로 간주하고, URL(URI)로 식별합니다.
    * 자원에 대한 **행위(Verb)**는 HTTP 메서드(GET, POST, PUT, DELETE, PATCH 등)로 표현합니다.
    * **무상태(Stateless)**: 각 요청은 필요한 모든 정보를 포함하며, 서버는 클라이언트의 이전 요청 상태를 저장하지 않습니다. 대부분의 현대 웹 서비스 백엔드 API 설계에 활용됩니다.

* **Idempotent (멱등성)**
    * 연산을 여러 번 수행해도 결과가 동일한 속성을 의미합니다.
    * **GET, HEAD, PUT, DELETE** 메서드는 멱등성을 가집니다. (`PUT` 요청을 여러 번 보내도 최종 자원의 상태는 동일합니다.)
    * **POST** 메서드는 멱등성을 가지지 않습니다. (여러 번 보내면 새로운 자원이 계속 생성될 수 있습니다.)
    * 네트워크 오류 등으로 요청이 재전송될 때, 멱등성이 보장되면 의도치 않은 부작용(예: 중복 결제)을 방지할 수 있습니다.