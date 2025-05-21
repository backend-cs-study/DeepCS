# 1. 싱글톤 패턴 (Singleton Pattern)

## 💡 개념
- 싱글톤 패턴은 애플리케이션 내에서 **클래스의 인스턴스를 하나만 생성**하도록 보장하는 디자인 패턴입니다.
- 주로 **공유 자원**(ex. 설정 클래스, DB 커넥션 등)에 대해 **전역 접근**이 필요할 때 사용됩니다.

---

##  ☕ Java에서의 싱글톤 구현 방식

### 1. 고전적인 싱글톤
```java
public class ClassicSingleton {
    private static ClassicSingleton instance;

    private ClassicSingleton() {}

    public static ClassicSingleton getInstance() {
        if (instance == null) {
            instance = new ClassicSingleton();
        }
        return instance;
    }
}
```


### 2. Thread-safe 방식

```JAVA
public class SynchronizedSingleton {
private static SynchronizedSingleton instance;

    private SynchronizedSingleton() {}

    public static synchronized SynchronizedSingleton getInstance() {
        if (instance == null) {
            instance = new SynchronizedSingleton();
        }
        return instance;
    }
}
```

### 3. 이른 초기화(Eager Initialization)

```JAVA
public class EagerSingleton {
private static final EagerSingleton instance = new EagerSingleton();

    private EagerSingleton() {}

    public static EagerSingleton getInstance() {
        return instance;
    }
}
```

### 4. Bill Pugh 방식 (내부 정적 클래스)
```JAVA
public class BillPughSingleton {
   private BillPughSingleton() {}

   private static class SingletonHelper {
   private static final BillPughSingleton INSTANCE = new BillPughSingleton();
   }

   public static BillPughSingleton getInstance() {
   return SingletonHelper.INSTANCE;
   }
}
```

---
## 🌱 Spring Framework와 Singleton
Spring에서는 기본적으로 모든 Bean을 Singleton Scope로 관리합니다.
```JAVA
@Component
public class MyService {
    public void doSomething() {
        System.out.println("Service executed");
    }
}
```
```JAVA
@Service
@RequiredArgsConstructor
public class UserService {
    private final MyService myService;
}
```
MyService는 하나의 인스턴스로 생성되어 여러 클래스에서 주입되어도 동일한 객체가 사용됩니다.

---

### 장점
인스턴스가 하나이므로 메모리 절약

전역 상태 관리에 유리

인스턴스 생성 비용 절감

---

###  단점
테스트 어려움 (Mocking 등)

상태 공유로 인한 멀티스레드 동시성 문제

OOP 원칙 위반 가능 (SRP 위배 등)

---

### 사용 예시
Spring의 ApplicationContext

DB Connection Pool

LogManager

설정 클래스 (@Configuration)

---

## 면접 예상 질문
1. 싱글톤 패턴이란 무엇이며, 언제 사용하나요?
2. Java에서 싱글톤을 안전하게 구현하는 방법은 무엇인가요?
3. Spring의 Bean Scope와 싱글톤의 관계는 무엇인가요?
4. 싱글톤 객체가 멀티스레드 환경에서 발생할 수 있는 문제점은 무엇이며, 해결 방법은?
5. 싱글톤이 테스트 환경에서 불리할 수 있는 이유는 무엇인가요?
