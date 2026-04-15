---
title: "getter의 3가지 방식(getter, @Getter, record)"
date: 2026-04-15 22:28:00 +0900
categories: [Java]
tags: [java, spring]
---
## getter 방식 3가지 비교

같은 `User(name, age)` 기준으로 비교한다.

---

## 1. 일반 getter (순수 자바)

```java id="basic-getter"
public class User {

    private final String name;
    private final int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getter 직접 작성
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

### 특징

* 모든 코드를 직접 작성해야 한다
* 가장 명시적이다
* 라이브러리 의존 없음

---

## 2. @Getter (Lombok)

```java id="lombok-getter"
import lombok.Getter;

@Getter
public class User {

    private final String name;
    private final int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 특징

* getter 자동 생성
* 코드 줄어듦
* **Lombok 의존**

---

## 3. record

```java id="record-user"
public record User(String name, int age) {}
```

### 특징

* 생성자, getter, equals, hashCode, toString 자동 생성
* getter 이름이 다름 → `name()`, `age()`
* 완전 불변 객체

---

## 4. 사용 코드 비교

### 일반 / @Getter

```java id="use-class"
User user = new User("kim", 20);

user.getName();
user.getAge();
```

---

### record

```java id="use-record"
User user = new User("kim", 20);

user.name();
user.age();
```

---

## 5. 핵심 차이 요약

| 구분              | 일반 getter | @Getter   | record |
| --------------- | --------- | --------- | ------ |
| 코드량             | 많음        | 중간        | 최소     |
| getter 이름       | getX()    | getX()    | x()    |
| 불변성             | 직접 관리     | 직접 관리     | 기본 강제  |
| equals/hashCode | 직접 구현     | 직접 구현     | 자동     |
| 의존성             | 없음        | Lombok 필요 | 없음     |
| 의도 표현           | 약함        | 약함        | 매우 강함  |

---

## 6. 설계 관점 정리

* 일반 getter → 가장 기본, 명확한 방식이다
* @Getter → 반복 코드 줄이는 편의 도구이다
* record → “데이터 전용 객체”라는 설계 선언이다

---

## 최종 한 줄 정리

**record는 단순 getter 자동화가 아니라, 객체의 성격 자체를 “불변 데이터”로 제한하는 구조이다.**
