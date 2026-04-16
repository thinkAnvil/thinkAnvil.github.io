---
title: "Bigdecimal, 자동매매의 화폐 변수타입"
date: 2026-04-16 22:28:00 +0900
categories: [Java]
tags: [java, variable]
---
[[Java_변수의 타입]]

## BigDecimal 정리

자바에서 소수 계산을 다루다 보면 반드시 마주치는 것이 `BigDecimal`이다.  
특히 금융, 정산, 자동매매 시스템에서는 선택이 아니라 **필수 요소**이다.

---

## 1. BigDecimal이란 무엇인가

`BigDecimal`은 **정확한 소수 연산을 위한 클래스**이다.  
패키지는 `java.math.BigDecimal`이다.

일반적인 `double`, `float`는 내부적으로 **이진수 기반 부동소수점**을 사용하기 때문에  
사람이 기대하는 값과 다르게 계산되는 문제가 존재한다.

```java
double a = 0.1;
double b = 0.2;

System.out.println(a + b); // 0.30000000000000004
```

이런 오차는 설계상 피할 수 없는 문제이다.

---

## 2. BigDecimal을 사용하는 이유

### 정확한 계산이 필요할 때

```java
BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");

System.out.println(a.add(b)); // 0.3
```

오차 없이 정확하게 계산된다.

👉 자동매매 시스템에서는 **핵심 도메인 객체 전부 BigDecimal 사용**이 기본 전략이다.

---

## 3. 생성 방법 (가장 중요한 포인트)

### 잘못된 방식

```java
new BigDecimal(0.1);
```

이미 `double`에서 오차가 발생했기 때문에 그대로 전파된다.

---

### 올바른 방식

```java
new BigDecimal("0.1");
```

또는

```java
BigDecimal.valueOf(0.1);
```

실무에서는 문자열 생성이 가장 안전하다.

---

## 4. 연산 방법 (연산자 사용 불가)

BigDecimal은 `+`, `-`, `*`, `/` 연산자를 사용할 수 없다.  
모든 연산은 메서드로 수행한다.

```java
BigDecimal a = new BigDecimal("10");
BigDecimal b = new BigDecimal("3");
```

### 덧셈

```java
a.add(b);
```

### 뺄셈

```java
a.subtract(b);
```

### 곱셈

```java
a.multiply(b);
```

### 나눗셈 (주의)

```java
a.divide(b); // 예외 발생 가능
```

나눗셈은 반드시 반올림 정책을 지정해야 한다.

```java
a.divide(b, 2, RoundingMode.HALF_UP); // 3.33
```

---

## 5. 반올림 정책 (RoundingMode)

대표적으로 많이 사용하는 방식은 다음과 같다.

- `HALF_UP` → 일반적인 반올림 (4사5입)

- `DOWN` → 버림

- `UP` → 무조건 올림


```java
a.divide(b, 2, RoundingMode.HALF_UP);
```

---

## 6. 비교 방법 (실무에서 자주 터지는 문제)

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("1.00");
```

```java
a.equals(b); // false
```

이유는 **소수점 자리수(scale)**까지 비교하기 때문이다.

---

### ✅ 올바른 비교 방식

```java
a.compareTo(b) == 0 // true
```

- 0 → 같다

- 1 → 크다

- -1 → 작다


---

## 7. 불변 객체 (Immutable)

BigDecimal은 값을 변경하지 않는다.

```java
BigDecimal a = new BigDecimal("10");
a.add(new BigDecimal("5"));

System.out.println(a); // 10
```

반드시 결과를 다시 할당해야 한다.

```java
a = a.add(new BigDecimal("5"));
```

---

## 8. 실전 예시 (자동매매 기준)

```java
BigDecimal price = new BigDecimal("50000");
BigDecimal quantity = new BigDecimal("0.001");

BigDecimal total = price.multiply(quantity);
```

이런 계산은 반드시 BigDecimal을 사용해야 한다.

---

## 9. 성능 vs 정확도

|타입|특징|
|---|---|
|double|빠르지만 부정확|
|BigDecimal|느리지만 정확|

👉 결론은 명확하다.

- 일반 로직 → double

- 금액/정산 → BigDecimal


---

## 10. 정리

- BigDecimal은 **정확한 소수 계산을 위한 클래스이다**

- 반드시 **문자열 기반으로 생성해야 한다**

- 연산은 **메서드로 수행한다**

- 나눗셈은 **반올림 지정 필수이다**

- 비교는 `compareTo`를 사용해야 한다

- 불변 객체이므로 **재할당이 필요하다**


---

## 핵심 한 줄 정리

👉 돈이 들어가는 순간 BigDecimal을 사용해야 한다
