# 8. MVC 패턴 (Model-View-Controller Pattern)

## 💡 개념
- MVC는 **Model-View-Controller**로 구성된 **애플리케이션 아키텍처 패턴**입니다.
- 사용자 인터페이스(UI)와 비즈니스 로직을 분리하여 **관심사의 분리(Separation of Concerns)**를 실현합니다.
- 유지보수성과 테스트 용이성을 크게 향상시킵니다.

---

## MVC 란 ?

| 구성요소 | 역할 |
|----------|------|
| **Model** | 비즈니스 로직, 데이터 처리 담당 (예: Entity, Service, Repository) |
| **View** | 사용자에게 보여지는 화면 (예: HTML, JSON, 템플릿 등) |
| **Controller** | 사용자의 요청을 받아 처리하고, 적절한 결과를 반환 (예: REST API의 진입점) |

---

## ☕ Spring MVC에서의 역할 매핑

| MVC 구성 | Spring 컴포넌트 |
|----------|----------------|
| Model    | `@Entity`, `@Service`, `@Repository`, DTO |
| View     | `Thymeleaf`, `Mustache`, 또는 `@RestController`의 JSON |
| Controller | `@Controller`, `@RestController` |

---

## 예제 코드 (Spring Boot 기반)

### 1. Controller

```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    @GetMapping("/{id}")
    public UserResponse getUser(@PathVariable Long id) {
        return userService.getUser(id);
    }
}
```

### 2. Service

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public UserResponse getUser(Long id) {
        User user = userRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("User not found"));
        return UserResponse.from(user);
    }
}
```

### 3. Repository

```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```

### 4. View (JSON 응답)

```json
{
  "id": 1,
  "name": "Seongjun",
  "email": "sj@example.com"
}
```

> Spring에서 `@RestController`를 사용하면 JSON이 View 역할을 수행합니다.

---

## 🌱 왜 MVC가 중요한가?

- 역할이 분리되어 유지보수가 쉬움
- 변경에 유연함 (ex. 화면 UI는 View만 수정)
- 테스트 용이성 (서비스 로직만 단위 테스트 가능)
- 확장성과 협업 효율 향상

---

### 장점
- **관심사의 분리(SoC)** 실현
- 확장성과 테스트 용이성 증가
- 역할 기반 협업이 용이함 (프론트 vs 백엔드 분리)

---

### 단점
- 구조가 복잡해질 수 있음
- 소규모 프로젝트에서는 오히려 과한 설계가 될 수도 있음

---

### 사용 예시
- Spring MVC 기반 웹 애플리케이션
- REST API 서버
- 템플릿 엔진을 사용하는 웹 서비스

---

## 면접 예상 질문
1. MVC 패턴의 각 구성 요소는 어떤 역할을 하나요?
2. Spring에서 Model, View, Controller는 어떻게 매핑되나요?
3. MVC 패턴을 사용하면 어떤 이점이 있나요?
4. `@RestController`와 `@Controller`의 차이는 무엇인가요?
5. MVC의 한계나 단점이 있다면 어떤 것이 있나요?
