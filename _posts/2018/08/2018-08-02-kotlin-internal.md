---
title: Kotlin 접근지정자 Internal
last_modified_at: '2018-08-02 22:02:00 +0300'
excerpt: 코틀린 접근 지정자 Internal에 대해 설명드리겠습니다.
comments : true
tags: [Kotlin,Java,Internal]
toc: true
toc_sticky: true
---

### 1. 코틀린 접근 지정자
Java와는 다르게 코틀린에는 Internal이라는 접근 지정자가 존재합니다. Internal접근 지정자와 Java와의 차이점을 이야기해보려고 합니다.

### 2. Java
```java
public int a = 1
protected int b = 2
private int c = 3
int d = 4 (default / 패키지 단위)
```

default의 경우 따로 명시를 해주지 않아도 됩니다.

여기서 default의 경우 패키지 단위라는 것에 대해 의문점을 가지는 분들이 계실겁니다.! 저는 이 부분을 집고 넘어가려고 합니다

패키지 단위라는 것은 같은 패키지내에 있으면 어디에서든 접근이 가능하다는 것이죠

A라는 클래스에 변수 a와 B라는 클래스에 변수 c가 default로 선언이 되어있으면, 서로 접근에 제한이 없다는 것입니다.

deafult는 패키지 이름을 보고 접근이 가능한지 아닌지를 판단하기 때문에, 보안의 위험을 나타낼 수도 있습니다


### 3. Kotlin
```kotlin
val a = 1 (default public)
protected val b = 2
private val c = 3
internal val d = 4
```

코틀린에서 자바의 단점을 보완하기 위해 internal 접근 제한자를 제공합니다

자바에서 제한하는 패키지 단위와는 달리 동일한 모듈 내에 있는 클래스들의 접근을 제한합니다.

코틀린에서 제한하는 모듈의 범위는

IntelliJ IDEA 모듈
Maven / Gradle 프로젝트
하나의 Ant 태스크 내에서 함께 컴파일되는 파일들

범위를 제한함으로써, 보안을 높이고, 자바의 단점을 보완한 코틀린입니다.
