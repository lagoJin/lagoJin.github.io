---
title: Kotlin만의 data class
date: '2018-08-03 15:32:00 +0300'
published : true
categories : [Software]
excerpt: 코틀린 data class 대해 설명드리겠습니다.
comments : true
tags: [Kotlin,Java,data,class]
---

자바에서 Pojo를 만들게 되면

### Java
~~~
class Person {

  private String name;
  private int age;
  private String address;

  public Person(String name, int age, String address){
    this.name = name;
    this.age= age;
    this.address = address;
  }
~~~

getter/setter는 기본에  + hashCode와 Equals를 추가로 정의해줘야하는 경우가 생깁니다


하지만, 코틀린에서는 사용자의 편의성을 위해 class앞에 data를 붙여주면 됩니다

### Kotlin
~~~
data class Person(
  private val name:String,
  private val age:Int,
  private val address:String)
~~~

이렇게만 선언해주시면 내부적으로 처리를 해줍니다!

정말 간단하죠!? 사용자의 편의성을 위해 생각을 많이 한것 같습니다~

Java에서도 비슷한 기능을 제공하는 [Lombok](https://projectlombok.org)도 존재합니다!
