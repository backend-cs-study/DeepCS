# 2. 팩토리 패턴 (Factory Pattern)

## 💡 개념
- 팩토리 패턴은 **객체 생성 로직을 별도의 클래스 또는 메서드로 분리**하여, 객체 생성 방식의 변경에도 클라이언트 코드를 수정하지 않도록 만드는 생성 패턴입니다.
- 즉, **객체 생성을 캡슐화**함으로써, 코드의 유연성과 확장성을 높입니다.

---

## 구조

```
Creator (Factory) → Product (Interface or Superclass) → ConcreteProduct (구현체)
```

---

##  ☕ Java 예제 코드

### Product (인터페이스)

```java
public interface Notification {
    void send(String message);
}
```

### Concrete Products

```java
public class EmailNotification implements Notification {
    public void send(String message) {
        System.out.println("Sending email: " + message);
    }
}

public class SmsNotification implements Notification {
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}
```

### Factory

```java
public class NotificationFactory {
    public static Notification create(String type) {
        switch (type.toLowerCase()) {
            case "email": return new EmailNotification();
            case "sms": return new SmsNotification();
            default: throw new IllegalArgumentException("Unknown type");
        }
    }
}
```

### Client

```java
public class NotificationService {
    public void notifyUser(String type, String message) {
        Notification notification = NotificationFactory.create(type);
        notification.send(message);
    }
}
```

---

## 🌱 Spring에서의 팩토리 활용

Spring에서는 다양한 구현체를 주입받아 유연하게 객체를 선택하는 방식으로 활용합니다.

```java
@Component
public class NotificationFactory {

    private final Map<String, Notification> map;

    public NotificationFactory(List<Notification> notificationList) {
        map = notificationList.stream()
            .collect(Collectors.toMap(
                n -> n.getClass().getSimpleName().toLowerCase(),
                Function.identity()
            ));
    }

    public Notification get(String type) {
        return map.get(type.toLowerCase());
    }
}
```

---

### 장점
- 객체 생성 책임 분리 → OCP 원칙 준수
- 구현 클래스 교체가 쉬움 → 유연한 설계
- 코드 재사용성 증가

---

### 단점
- 클래스 수 증가 (구현체 + 팩토리)
- 단순한 로직에는 과도한 설계가 될 수 있음

---

### 사용 예시
- Spring의 BeanFactory, ApplicationContext
- JDBC 드라이버 생성 (DriverManager.getConnection())
- 인터페이스 기반 전략 선택 로직 (ex. 결제 방식, 알림 방식)

---

## 면접 예상 질문
1. 팩토리 패턴이란 무엇이며, 왜 사용하는가요?
2. 팩토리 패턴은 어떤 객체지향 원칙을 만족시키나요?
3. Spring에서는 팩토리 패턴을 어떻게 활용하거나 대체하나요?
4. 간단한 팩토리 패턴 구현 예제를 설명해보세요.
5. 팩토리 패턴과 추상 팩토리 패턴의 차이점은 무엇인가요?
