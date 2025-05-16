# 6. 이터레이터 패턴 (Iterator Pattern)

## 💡 개념
- 이터레이터 패턴은 **컬렉션 내부 구조를 노출하지 않고도 요소를 순차적으로 접근**할 수 있도록 하는 디자인 패턴입니다.
- 반복자를 통해 일관된 방식으로 다양한 자료구조를 순회할 수 있게 합니다.

---

## ☕Java에서의 이터레이터 패턴 구현

Java의 `Iterator` 인터페이스가 이 패턴을 직접 구현한 대표 사례입니다.

### 1. Aggregate (집합체) 인터페이스

```java
public interface MyCollection<T> {
    MyIterator<T> iterator();
}
```

### 2. Iterator 인터페이스

```java
public interface MyIterator<T> {
    boolean hasNext();
    T next();
}
```

### 3. Concrete Collection

```java
public class BookCollection implements MyCollection<String> {
    private String[] books;

    public BookCollection(String[] books) {
        this.books = books;
    }

    public MyIterator<String> iterator() {
        return new BookIterator(books);
    }
}
```

### 4. Concrete Iterator

```java
public class BookIterator implements MyIterator<String> {
    private String[] books;
    private int position = 0;

    public BookIterator(String[] books) {
        this.books = books;
    }

    public boolean hasNext() {
        return position < books.length;
    }

    public String next() {
        return books[position++];
    }
}
```

### 5. 사용 예시

```java
public class Main {
    public static void main(String[] args) {
        String[] books = {"Effective Java", "Clean Code", "Spring in Action"};
        BookCollection collection = new BookCollection(books);

        MyIterator<String> iterator = collection.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

---

## 🌱 Java Collection API에서의 이터레이터

Java의 대부분의 컬렉션(List, Set 등)은 `Iterable`을 구현하고, `Iterator`를 제공합니다.

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Iterator<String> it = names.iterator();

while (it.hasNext()) {
    System.out.println(it.next());
}
```

또는 향상된 for문 (`for-each`)도 내부적으로 Iterator를 사용합니다.

```java
for (String name : names) {
    System.out.println(name);
}
```

---

### 장점
- 내부 구조를 숨기고, 일관된 방식으로 접근 가능
- 다양한 컬렉션에 대해 동일한 방식의 순회 가능
- SRP(단일 책임 원칙)에 부합 (순회 로직 분리)

---

### 단점
- 컬렉션이 변경되는 경우(ConcurrentModification) 예외 발생 가능
- 간단한 순회에도 클래스를 많이 작성해야 할 수 있음

---

### 사용 예시
- Java의 `Iterator`, `ListIterator`, `Enumeration`
- 향상된 for문 (`for-each`)
- 커스텀 반복 로직 분리

---

## 면접 예상 질문
1. 이터레이터 패턴이란 무엇이며, 어떤 상황에서 사용하나요?
2. 자바의 `Iterator`는 어떤 인터페이스를 제공하나요?
3. `for-each`문은 내부적으로 어떻게 동작하나요?
4. 이터레이터 패턴을 사용하면 어떤 객체지향 원칙을 지킬 수 있나요?
5. `ConcurrentModificationException`은 어떤 경우에 발생하나요?
