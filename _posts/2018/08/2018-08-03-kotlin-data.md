---
title: Kotlin만의 data class
last_modified_at: '2018-08-03 15:32:00 +0300'
published : true
excerpt: 코틀린 data class 대해 설명드리겠습니다.
comments : true
tags: [Kotlin,Java,data,class]
toc : true
toc_sticky: true
---

### 1. 개요
자바에서의 Pojo와 코틀린에서 Pojo 생성 방법입니다.


### 2. Java
```kotlin
class Person {

  private String name;
  private int age;
  private String address;

  public Person(String name, int age, String address){
    this.name = name;
    this.age= age;
    this.address = address;
  }
```

getter/setter는 기본에  + hashCode와 Equals를 추가로 정의해줘야하는 경우가 생깁니다


하지만, 코틀린에서는 사용자의 편의성을 위해 class앞에 data를 붙여주면 됩니다

### 3. Kotlin
```kotlin
data class Person(
  private val name:String,
  private val age:Int,
  private val address:String)
```

이렇게만 선언해주시면 내부적으로 처리를 해줍니다!

정말 간단하죠!? 사용자의 편의성을 위해 생각을 많이 한것 같습니다~

Java에서도 비슷한 기능을 제공하는 [Lombok](https://projectlombok.org)도 존재합니다!
