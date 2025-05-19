# 9. MVP 패턴 (Model-View-Presenter Pattern)

## 💡 개념
- MVP는 **Model-View-Presenter**로 구성된 아키텍처 패턴입니다.
- MVC 패턴에서 **Controller 대신 Presenter가 View와 Model 사이를 중재**하며, **View의 로직을 Presenter로 분리**한다는 특징이 있습니다.
- View는 Presenter에게 모든 사용자 이벤트 처리를 위임하며, Presenter는 비즈니스 로직과 화면 갱신을 모두 담당합니다.

---

## MVP란 ?

| 구성요소 | 역할 |
|----------|------|
| **Model** | 데이터 및 비즈니스 로직 담당 |
| **View** | UI 담당, Presenter로부터 지시를 받아 동작 (자기 로직 없음) |
| **Presenter** | View와 Model 사이의 중재자, 모든 흐름을 제어 |

---

## 어떤 구조로 되어 있길래 ?

### 1. View 인터페이스

```java
public interface UserView {
    void showUser(UserDto user);
    void showError(String message);
}
```

### 2. Presenter

```java
public class UserPresenter {

    private final UserService userService;
    private final UserView view;

    public UserPresenter(UserService userService, UserView view) {
        this.userService = userService;
        this.view = view;
    }

    public void loadUser(Long id) {
        try {
            UserDto user = userService.getUserById(id);
            view.showUser(user);
        } catch (Exception e) {
            view.showError("사용자 조회 실패");
        }
    }
}
```

### 3. Model (Service 계층)

```java
@Service
public class UserService {
    public UserDto getUserById(Long id) {
        // DB 조회 후 가공
        return new UserDto(id, "Seongjun");
    }
}
```

### 4. View 구현체 (콘솔 or 화면 담당)

```java
public class ConsoleUserView implements UserView {

    public void showUser(UserDto user) {
        System.out.println("User: " + user.getName());
    }

    public void showError(String message) {
        System.err.println("Error: " + message);
    }
}
```

---

## 🌱 Spring에서의 활용 가능성

Spring MVC는 기본적으로 **MVC 패턴 기반**이지만, 다음과 같은 경우에 **MVP적 사고방식**을 적용할 수 있음:

- **REST 컨트롤러를 View처럼 취급**하고, 그 외 모든 흐름을 Service(Presenter)가 책임지도록 구성
- Controller는 데이터 전달만 하고, 비즈니스 로직 흐름 제어는 Service에 집중

```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/users")
public class UserController {

    private final UserPresenter presenter;

    @GetMapping("/{id}")
    public ResponseEntity<?> getUser(@PathVariable Long id) {
        return presenter.loadUser(id);
    }
}
```

---

### MVC와의 차이점

| 항목 | MVC | MVP |
|------|-----|-----|
| 이벤트 처리 | Controller 또는 View | View → Presenter |
| 로직 분리 | Controller에 집중됨 | Presenter가 UI 로직까지 포함 |
| View 역할 | 데이터 표시 + 일부 로직 | UI 표시만 담당 (로직 없음) |
| 테스트 용이성 | 상대적으로 낮음 | Presenter 단독 테스트 쉬움 |

---

### 장점
- View 로직까지 분리되어 테스트 용이
- UI와 로직의 완전한 분리 → 유지보수성 향상
- Presenter 단독으로 단위 테스트 가능

---

### 단점
- 클래스 수 증가
- MVP를 명확하게 적용하기엔 Java + Spring에선 과한 경우도 있음

---

### 사용 예시
- Android Java (MVP 구조가 널리 사용됨)
- 복잡한 UI 상태 관리가 필요한 데스크탑 앱
- Controller를 최대한 얇게 유지하고, Presenter(Service)에 로직 집중시키는 Spring 구조

---

## 면접 예상 질문
1. MVP 패턴이란 무엇이며, MVC와 어떤 차이가 있나요?
2. Presenter의 책임은 무엇인가요?
3. Java/Spring 환경에서 MVP 패턴을 어떻게 적용할 수 있나요?
4. MVP 구조의 장점은 무엇이고, 단점은 어떤 것이 있나요?
5. View가 로직을 가지지 않는 이유는 무엇인가요?
