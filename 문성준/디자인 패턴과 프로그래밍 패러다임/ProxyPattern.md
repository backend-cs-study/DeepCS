# 5. 프록시 패턴과 프록시 서버 (Proxy Pattern & Proxy Server)

## 💡 개념
- **프록시 패턴(Proxy Pattern)**은 **다른 객체에 대한 접근을 제어하기 위해 대리 객체를 사용하는 패턴**입니다.
- 실제 객체에 접근하기 전 중간 단계에서 로직을 추가하거나 접근 제한을 둘 수 있습니다.
- 보안, 로깅, 캐싱, 지연 로딩(lazy loading) 등에 자주 사용됩니다.

---

## ☕ Java에서의 프록시 패턴 구현

### 1. Subject 인터페이스

```java
public interface Image {
    void display();
}
```

### 2. RealSubject (실제 객체)

```java
public class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + fileName);
    }

    public void display() {
        System.out.println("Displaying " + fileName);
    }
}
```

### 3. Proxy (대리 객체)

```java
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName); // 지연 로딩
        }
        realImage.display();
    }
}
```

### 4. Client 코드

```java
public class Main {
    public static void main(String[] args) {
        Image image = new ProxyImage("photo.jpg");

        // 첫 호출: 로딩 + 출력
        image.display();

        // 두 번째 호출: 출력만
        image.display();
    }
}
```

---

## 🌱 Spring에서의 프록시 활용

Spring에서는 **AOP 기반의 Proxy 패턴**이 널리 활용됩니다.

### 예시: 트랜잭션 처리

```java
@Transactional
public void createUser(User user) {
    // 트랜잭션 시작 전, 후 로직은 Spring AOP가 프록시를 통해 처리
}
```

Spring 내부에서는 `@Transactional` 어노테이션이 붙은 메서드를 호출할 때, **프록시 객체**가 중간에 끼어들어 트랜잭션 시작/종료를 처리합니다.

또한 `@Async`, `@Cacheable`, `@Secured` 등도 모두 프록시 기반입니다.

---

### 프록시 종류

- **Static Proxy**: 직접 코드로 대리 클래스를 구현
- **Dynamic Proxy (JDK Proxy)**: 인터페이스 기반 프록시
- **CGLIB Proxy**: 클래스 기반 프록시 (인터페이스 없어도 가능)

---

## 프록시 서버 

- **프록시 서버**는 네트워크에서 클라이언트와 서버 사이에 위치하여 **요청을 중계**하는 서버입니다.
- 보안, 캐싱, 접근 제어, IP 우회 등 다양한 목적에 사용됩니다.

### 종류

| 유형          | 설명 |
|---------------|------|
| 정방향 프록시 (Forward Proxy) | 내부 사용자 → 외부 인터넷 접근 시 프록시 서버를 거침 |
| 역방향 프록시 (Reverse Proxy) | 외부 요청 → 내부 서버로 분산 처리 (ex. Nginx, Apache) |
| 투명 프록시 (Transparent Proxy) | 클라이언트 인지 없이 자동 중계 |

### 프록시 서버 활용 사례

- 로드 밸런싱
- 캐싱 처리
- 방화벽 및 접근 제어
- 실시간 로깅 및 모니터링
- CDN (Cloudflare, Akamai 등)

---

### 장점
- 접근 제어 및 인증 가능
- 부가 기능 추가 가능 (캐싱, 로깅, 트랜잭션 등)
- 실제 객체에 대한 간접 접근으로 보안 강화
- 성능 최적화 (지연 로딩 등)

---

### 단점
- 구현 복잡성 증가
- 실 객체와 프록시 객체 구분 어려울 수 있음
- AOP 기반 프록시는 **자기 호출(this) 문제** 발생 가능

---

### 사용 예시
- Spring AOP (`@Transactional`, `@Async`, `@Cacheable`)
- Hibernate Lazy Loading
- RPC 프록시 (gRPC Stub)
- 네트워크 프록시 서버 (Nginx, Squid)

---

## 면접 예상 질문
1. 프록시 패턴이란 무엇이며, 어떤 경우에 사용하나요?
2. 정적 프록시와 동적 프록시의 차이는 무엇인가요?
3. Spring에서 프록시 패턴은 어떤 식으로 사용되나요?
4. 프록시 패턴과 데코레이터 패턴의 차이점은 무엇인가요?
5. 프록시 서버의 역할과 종류에는 어떤 것이 있나요?
6. AOP 프록시에서 발생할 수 있는 한계점이나 주의할 점은 무엇인가요?
