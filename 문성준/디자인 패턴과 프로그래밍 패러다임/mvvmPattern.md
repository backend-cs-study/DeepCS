# 10. MVVM 패턴 (Model-View-ViewModel Pattern)

## 💡 개념
- MVVM은 **Model-View-ViewModel**로 구성된 아키텍처 패턴입니다.
- ViewModel이 View를 직접 조작하지 않고, **바인딩(binding)**을 통해 UI를 자동으로 갱신하는 구조입니다.
- 주로 **데스크탑/모바일 앱 개발**에 사용되며, **양방향 데이터 바인딩**이 핵심입니다.

---

## MVVM 이란 ?

| 구성요소 | 역할 |
|----------|------|
| **Model** | 데이터 및 비즈니스 로직 처리 (Java의 Service/Entity 등) |
| **View** | 사용자 UI (HTML, 화면 등) |
| **ViewModel** | View에 보여줄 데이터를 가공하여 제공, View와 바인딩 |

---

## 핵심 개념
### 바인딩

- View와 ViewModel 사이에서 데이터가 **자동으로 동기화**
- ViewModel이 상태를 바꾸면 View가 자동 갱신됨
- 주로 프론트엔드/모바일 프레임워크 (Vue.js, Angular, WPF 등)에서 사용

---

## ☕ Java/Spring 개발자의 관점에서 MVVM를 이해 한다면 ?

### Spring에서 직접적인 MVVM은 사용되지 않지만

- **REST API + 프론트엔드 프레임워크** 조합에서 ViewModel 역할은 프론트엔드에서 담당
- Spring은 주로 **MVC 또는 MVP 기반 서버 아키텍처**를 사용함

#### 예시 구성 (MVVM-ish 구조)

| 역할 | 담당 위치 |
|------|-----------|
| Model | Spring의 `@Entity`, `Service` |
| View | React / Vue / Thymeleaf |
| ViewModel | React의 상태 관리 / Controller가 JSON 응답 생성 |

```java
@RestController
public class ProductController {

    @GetMapping("/products/{id}")
    public ProductViewModel getProduct(@PathVariable Long id) {
        // Service → DTO → ViewModel 역할
        return productService.getProductViewModel(id);
    }
}
```

---

## ViewModel은 무엇을 하나?

- UI에 필요한 형태로 데이터를 가공해서 전달
- 불필요한 정보는 제거하고, UI 중심의 데이터 구조를 만든다
- Spring에서는 **DTO나 Response 객체**가 ViewModel 역할을 수행하기도 함

```java
public class ProductViewModel {
    private String name;
    private String priceText;
    private boolean soldOut;

    public ProductViewModel(Product product) {
        this.name = product.getName();
        this.priceText = product.getPrice() + "원";
        this.soldOut = product.getStock() == 0;
    }
}
```

---

### MVC / MVP / MVVM 차이점 요약

| 패턴 | View와의 연결 | 중간 제어자 | View의 책임 | 테스트 용이성 |
|------|----------------|--------------|-------------|----------------|
| MVC  | Controller가 직접 제어 | Controller | 일부 로직 포함 | 보통 |
| MVP  | View → Presenter 호출 | Presenter | 로직 없음 | 매우 좋음 |
| MVVM | 바인딩 기반 자동 갱신 | ViewModel | 로직 없음 | 프레임워크 의존 |

---

### 장점
- View와 로직 분리 → UI 변경에 강함
- 양방향 바인딩으로 화면 동기화가 쉬움
- 테스트 가능한 구조

---

### 단점
- Spring에서는 직접 구현이 어렵고, 프론트 기술에 의존
- 바인딩 처리의 복잡성 → 성능 이슈
- 백엔드 개발에선 활용도 낮음

---

### 사용 예시
- 프론트엔드 프레임워크 (Vue, Angular, React+MobX)
- Android 앱 개발 (Jetpack Compose, DataBinding)
- 데스크탑 애플리케이션 (JavaFX, WPF)

---

## 면접 예상 질문
1. MVVM 패턴이란 무엇이며, 어떤 구조로 동작하나요?
2. MVC, MVP, MVVM의 차이점은 무엇인가요?
3. Spring에서는 MVVM 패턴을 어떻게 해석할 수 있나요?
4. ViewModel은 어떤 책임을 가지며, 어떤 객체가 해당 역할을 대신하나요?
5. 양방향 바인딩의 장단점은 무엇인가요?
