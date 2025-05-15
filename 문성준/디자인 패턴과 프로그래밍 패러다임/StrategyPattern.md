# 3. 전략 패턴 (Strategy Pattern)

## 💡 개념
- 전략 패턴은 **행동(알고리즘)들을 캡슐화하고**, 이들을 **상호 교체 가능**하게 만들어, 런타임에 알고리즘을 선택할 수 있도록 하는 디자인 패턴입니다.
- `if-else` 또는 `switch` 문으로 분기되는 코드 로직을 객체 지향적으로 대체할 수 있습니다.

---

## ☕Java에서의 전략 패턴 구현 방식

### 1. Strategy 인터페이스

```java
public interface PaymentStrategy {
    void pay(int amount);
}
```

### 2. Concrete Strategy

```java
public class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println(amount + " paid with credit card.");
    }
}

public class KakaoPayPayment implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println(amount + " paid with KakaoPay.");
    }
}
```

### 3. Context 클래스

```java
public class PaymentService {
    private PaymentStrategy strategy;

    public PaymentService(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void process(int amount) {
        strategy.pay(amount);
    }
}
```

### 4. 실행 예시 (클라이언트)

```java
public class Main {
    public static void main(String[] args) {
        PaymentService service1 = new PaymentService(new CreditCardPayment());
        service1.process(5000);

        PaymentService service2 = new PaymentService(new KakaoPayPayment());
        service2.process(12000);
    }
}
```

---

## 🌱 Spring에서의 전략 패턴 활용

Spring에서는 구현체들을 Bean으로 등록하고, `Map<String, Bean>` 또는 `List<Bean>`으로 주입받아 동적으로 전략을 선택할 수 있습니다.

```java
@Component
public class PaymentStrategyFactory {

    private final Map<String, PaymentStrategy> strategyMap;

    public PaymentStrategyFactory(List<PaymentStrategy> strategies) {
        this.strategyMap = strategies.stream()
                .collect(Collectors.toMap(
                    s -> s.getClass().getSimpleName().toLowerCase(),
                    Function.identity()
                ));
    }

    public PaymentStrategy get(String type) {
        return strategyMap.get(type.toLowerCase());
    }
}
```

---

### 장점
- 조건문 제거 → 코드 가독성 향상
- OCP(개방-폐쇄 원칙) 만족
- 알고리즘 교체가 쉬움 → 유연한 구조

---

### 단점
- 클래스 수 증가
- 전략 선택 로직이 복잡해질 수 있음

---

### 사용 예시
- 결제 방식 선택 (카드, 포인트, 간편결제 등)
- 인증 방식 선택 (비밀번호, OAuth, SSO)
- 할인 정책, 정렬 정책 등

---

## 면접 예상 질문
1. 전략 패턴이란 무엇이며, 어떤 상황에서 사용하나요?
2. 전략 패턴과 팩토리 패턴의 차이점은 무엇인가요?
3. 전략 패턴을 사용하면 어떤 객체지향 설계 원칙을 지킬 수 있나요?
4. Spring에서는 전략 패턴을 어떤 방식으로 활용하나요?
5. 전략 패턴을 사용하지 않으면 어떤 문제가 발생할 수 있나요?
