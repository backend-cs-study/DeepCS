# 7. 노출 모듈 패턴 (Revealing Module Pattern)

## 💡 개념
- 노출 모듈 패턴은 **모듈 내부의 세부 구현은 숨기고, 외부에 필요한 기능만 선택적으로 공개**하는 설계 방식입니다.
- Java에서는 이 개념이 **접근 제어자(private, public)**, **레이어 분리(Service, Repository)**, **인터페이스 기반 설계**를 통해 자연스럽게 구현됩니다.

---

## ☕ Java에서의 적용 예시

### 예) 서비스 클래스의 내부 구현 은닉

```java
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // 외부에 공개되는 핵심 API
    public UserDto getUser(Long id) {
        User user = findUser(id);
        return UserDto.from(user);
    }

    // 외부에는 숨겨진 내부 전용 로직
    private User findUser(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("User not found"));
    }
}
```

- `getUser()`는 외부에 공개된 API
- `findUser()`는 내부에서만 사용하는 은닉된 구현
- 이런 식으로 **의도된 기능만 외부에 노출**하는 구조가 노출 모듈 패턴의 핵심입니다.

---

## 🌱 Spring에서의 적용 방식

### 1. 인터페이스 + 구현 클래스 분리

```java
public interface NotificationService {
    void send(String message);
}

@Service
class EmailNotificationService implements NotificationService {
    @Override
    public void send(String message) {
        // 내부 구현은 외부에 노출되지 않음
    }
}
```

- 클라이언트는 `NotificationService`만 알고, 구현체(`EmailNotificationService`)는 감춰져 있음
- 스프링 빈 주입 시에도 **인터페이스만 사용**하여 느슨한 결합 유지

---

### 2. 레이어 별 책임 분리

- Controller: 요청 수신, 응답 처리
- Service: 비즈니스 로직
- Repository: DB 접근

→ 각 레이어는 필요한 기능만 외부에 노출하고, 나머지는 `private`으로 은닉하여 모듈처럼 작동함

---

### 장점
- **의도한 기능만 외부에 노출** → 유지보수성, 보안성 증가
- 내부 구현 변경 시, 외부 코드에 영향 없음 → 유연한 구조
- 테스트 시에도 인터페이스 기반으로 Mock 객체 사용 가능

---

### 단점
- 설계 시 명확한 역할 분담이 필요함
- 과도한 분리 시 초기 진입장벽 증가

---

### 사용 예시
- `private` 메서드를 활용한 내부 로직 캡슐화
- `Service` 클래스에서 외부 공개용 `public` 메서드만 제공
- `@Component`, `@Service`로 등록된 구현체를 인터페이스로 주입

---

## 면접 예상 질문
1. Java에서 노출 모듈 패턴 개념은 어떤 방식으로 실현할 수 있나요?
2. 접근 제어자를 활용한 캡슐화는 어떤 이점이 있나요?
3. 인터페이스 기반 설계가 유지보수성과 테스트성에 어떤 영향을 주나요?
4. Spring에서 컴포넌트를 모듈처럼 사용하는 방법은?
5. 모듈화와 의도된 노출이 왜 중요한 설계 원칙인가요?
