---
title: "Java에서의 변수타입(Variable Type)"
date: 2026-03-17 20:30:00 +0900
categories: [Java]
tags: [java]
---


자바의 변수 타입은 크게 **기본형(Primitive Type)**과 **참조형(Reference Type)**으로 나뉘며, 이들은 각각의 메모리 구조와 사용 방법이 다르다. 아래는 자바의 변수 타입을 세세하게 정리한 내용이다.

---

# 1. 자바 변수 타입 분류

```
[ 변수 타입 ]
├── 기본형 (Primitive Type)
└── 참조형 (Reference Type)
```

---

# 2. 기본형(Primitive Type)

## 총 8가지 타입

|그룹|타입|크기|기본값|설명|
|---|---|---|---|---|
|정수형|`byte`|1 byte|0|-128 ~ 127|
||`short`|2 byte|0|-32,768 ~ 32,767|
||`int`|4 byte|0|기본 정수 타입|
||`long`|8 byte|0L|끝에 `L` 붙임|
|실수형|`float`|4 byte|0.0f|끝에 `f` 붙임|
||`double`|8 byte|0.0|기본 실수 타입|
|문자형|`char`|2 byte|'\u0000'|유니코드 문자 (e.g. `'A'`, `'가'`)|
|논리형|`boolean`|1 bit*|false|true / false|

> 🔹 `boolean`은 JVM 내부적으로는 1 byte로 처리함.  
> 🔹 기본형은 Stack 메모리에 저장됨 (값 그 자체를 저장).

---

# 3. 참조형(Reference Type)

## 객체나 배열을 가리키는 타입

|종류|예시|설명|
|---|---|---|
|클래스 타입|`String`, `Scanner`, `ArrayList`, 사용자 정의 클래스 등|객체의 주소를 저장|
|배열 타입|`int[]`, `String[]`, `char[]` 등|배열은 객체로 취급됨|
|인터페이스 타입|`Runnable`, `Comparable` 등|구현체를 참조함|
|열거 타입|`enum`|상수 그룹 표현|

> 🔹 참조형은 Stack에 **참조 주소(레퍼런스)**를 저장하고, 실제 객체는 Heap 메모리에 저장됨.  
> 🔹 `null`은 참조형 변수의 초기값으로 사용될 수 있음 (아무 객체도 참조하지 않음).

---

# 4. 타입별 예시 코드

```java
// 기본형
int age = 25;
double height = 175.5;
char grade = 'A';
boolean isPass = true;

// 참조형
String name = "홍길동";
int[] scores = {90, 80, 70};
Scanner sc = new Scanner(System.in);
```

---

# 5. 변수의 크기 순서 및 선택 기준

## 기본형 크기 정리 (오름차순)

```
boolean (1bit)
byte (1byte)
char, short (2byte)
int, float (4byte)
long, double (8byte)
```

# 선택 기준

- 정수는 `int`, 실수는 `double`이 기본
- 메모리 아낄 땐 `byte`, `short`, `float` 등 사용 가능
- 정밀한 계산 필요 시 `double` or `BigDecimal`
- 논리값만 다룰 땐 `boolean`

---

# 6. 자바 변수의 선언 및 초기화 규칙

```java
타입 변수명;            // 선언만
타입 변수명 = 값;       // 선언 + 초기화

// 여러 개 선언
int x = 10, y = 20, z = 30;
```

---

# 7. 형 변환 (Type Casting)

## 자동 형 변환 (작은 타입 → 큰 타입)

```java
int a = 100;
long b = a; // OK
```

## 강제 형 변환 (큰 타입 → 작은 타입)

```java
double d = 3.14;
int i = (int) d; // 소수점 잘림
```

---

# 8. Wrapper 클래스 (기본형 → 객체형)

|기본형|Wrapper 클래스|
|---|---|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|char|Character|
|boolean|Boolean|

