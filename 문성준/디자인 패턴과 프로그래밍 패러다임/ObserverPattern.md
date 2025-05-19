# 4. 옵저버 패턴 (Observer Pattern)

## 💡 개념
- 옵저버 패턴은 **객체의 상태 변화**를  관찰자(옵저버)들에게 자동으로 통지하는 디자인 패턴입니다.
- 하나의 객체(Subject)의 상태가 변경되면, 이와 연결된 여러 옵저버(Observer)들에게 알림이 전달됩니다.
- **이벤트 기반 프로그래밍**에 자주 사용됩니다.

---

##  ☕ Java에서의 옵저버 패턴 구현 방식

### 1. Subject (Publisher)

```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers(String message);
}
```

### 2. Concrete Subject

```java
public class NewsPublisher implements Subject {
    private List<Observer> observers = new ArrayList<>();

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void publishNews(String news) {
        System.out.println("News: " + news);
        notifyObservers(news);
    }
}
```

### 3. Observer 인터페이스

```java
public interface Observer {
    void update(String message);
}
```

### 4. Concrete Observers

```java
public class EmailSubscriber implements Observer {
    public void update(String message) {
        System.out.println("Email received: " + message);
    }
}

public class SmsSubscriber implements Observer {
    public void update(String message) {
        System.out.println("SMS received: " + message);
    }
}
```

### 5. 사용 예시

```java
public class Main {
    public static void main(String[] args) {
        NewsPublisher publisher = new NewsPublisher();

        Observer email = new EmailSubscriber();
        Observer sms = new SmsSubscriber();

        publisher.registerObserver(email);
        publisher.registerObserver(sms);

        publisher.publishNews("New Java update released!");
    }
}
```

---

## 🌱 Spring에서의 옵저버 패턴

Spring에서는 **ApplicationEventPublisher + @EventListener** 조합으로 옵저버 패턴을 구현할 수 있습니다.

### 이벤트 객체

```java
public class UserRegisteredEvent extends ApplicationEvent {
    private final String email;

    public UserRegisteredEvent(Object source, String email) {
        super(source);
        this.email = email;
    }

    public String getEmail() {
        return email;
    }
}
```

### 이벤트 발행

```java
@Component
public class UserService {

    private final ApplicationEventPublisher publisher;

    public UserService(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void registerUser(String email) {
        // 유저 등록 로직...
        publisher.publishEvent(new UserRegisteredEvent(this, email));
    }
}
```

### 이벤트 수신

```java
@Component
public class EmailService {

    @EventListener
    public void handleUserRegistered(UserRegisteredEvent event) {
        System.out.println("Sending welcome email to: " + event.getEmail());
    }
}
```

---

### 장점
- 객체 간 결합도 감소 (Subject ↔ Observer 간 느슨한 연결)
- 이벤트 기반 확장에 용이
- 여러 Observer에게 손쉽게 알림 전달 가능

---

### 단점
- 복잡한 관계일수록 디버깅 어려움
- 순환 호출 주의 필요
- 옵저버가 많을 경우 성능 이슈 가능

---

### 사용 예시
- UI 이벤트 처리 (버튼 클릭 등)
- Spring의 이벤트 기반 구조 (ApplicationEvent)
- 채팅, 푸시 알림, 뉴스피드 구독 시스템

---

## 면접 예상 질문
1. 옵저버 패턴이란 무엇이며, 언제 사용하나요?
2. 옵저버 패턴에서 Subject와 Observer는 어떤 관계를 가지나요?
3. 옵저버 패턴을 사용하면 어떤 객체지향 설계 원칙을 만족할 수 있나요?
4. Spring에서 옵저버 패턴은 어떻게 구현되나요?
5. 옵저버 패턴의 단점과 해결 방안은 무엇인가요?
