---
title: Kotlin Annotation 사용방법
last_modified_at: '2019-07-20 16:54:00 +0300'
published : true
excerpt: 소개해드릴것은 kotlin에서의 annotation 사용 방법 입니다.
comments : true
tags :
    - Kotlin
    - Annotation
---

코틀린에서도 자바와 동일하게 어노테이션(Annotation)을 정의하고 사용할 수 있으며, 그 방법에 대해 소개해드리려고 합니다.

1) Marker Annotaion
- 멤버 변수밖에 존재하지 않으며, 단순히 표식으로 사용되는 Annotation(어노테이션).
 - @AnnotationName

2) Single-Value Annotaion
- 단일 변수밖에 존재하지 않으며, 값을 명시하여 데이터 전달
- @AnnotationName(elementValue)

3) Full Annotaion
- 둘 이상의 변수를 가가지며, 데이터를 값=쌍 의 형태로 전달
- @AnnotationName(element=value, element=value, ...)


#### 1) 어노테이션 선언 방법

##### Java
```java
//어노테이션 선언
public @Interface People{
  String name();
}

//어노테이션 대입
@People(name = "HongGilDong")
class Student{

}
```
##### Kotlin
```Kotlin
//어노테이션 선언
annotation class People{
  val name: String
}
//어노테이션 대입
@People(name = "HongGilDong")
class Student{

}
```
#### 2) default 값 선언 방법
Java의 경우 명시적으로 default라는 키워드를 붙여줘야 한다.
Kotlin의 경우 기본 변수를 선언 하는것과 같은 방법으로 사용한다.

##### Kotlin
```java
//어노테이션 선언
public @Interface People{
  String name() default "HongGilDong"
}

//어노테이션 대입
@People
class Student{

}

```

##### Kotlin
```Kotlin
//어노테이션 선언
annotation class People{
  val name = "HongGilDong"
}

//어노테이션 대입
@People
class Student
```

</br>
Annotation(어노테이션)을 적절하게 사용하면 클래스에서 데이터 유효성 검사에 대한 부분을 진행 할 수 있게 되어, 코드의 양도 줄어들게 되고 문서화를 진행하여 추후 유지보수에 도움이 된다.
