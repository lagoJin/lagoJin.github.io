---
title: Android Databinding이란??
last_modified_at: '2019-11-30 15:06:00 +0300'
published : true
excerpt: 소개해드릴것은 AAC에서 제공하는 dataBinding과 viewbinding에 대해 비교하려고 합니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - databinding
    - viewbinding
---

### 1. 뷰 바인딩이란 ?
---
- xml에 내에 존재하는 id값을 가지고 있는 뷰들을 클래스로 묶어서 사용할 수 있게 해주는 것
- 데이터바인딩보다 적은 비용으로 뷰와 바인딩 시켜준다.
- 유효하지 않은 view생성으로 인한 Null에 대한 위험이 없다.
- 뷰 타입에 대한 예외가 발생할 위험이 없다.

> 2019-11-30 기준 3.6카나리 이상에서만 사용가능

 사용방법
```kotlin
    android {
            ...
            viewBinding {
                enabled = true
            }
        }
```

### 2. 데이터 바인딩이란?
---
- 데티어바인딩은 뷰 바인딩의 하위에 속하는 존재이다.
- 옵저버 패턴을 기반으로 하고 있으며, 바인딩할 xml을 layout 태그로 래핑하면 됩니다.
- 뷰 바인딩에 비해 비용이 더 들어가서 빌드가 느려지나 신경쓸 정도는 아니다.
- data와 xml을 연결시켜 뷰와 데이터의 의존도를 낮춘다.

사용방법
```kotlin
    android {
            ...
            dataBinding {
                enabled = true
            }
        }
```
어댑터 사용방법

```kotlin
    @BindingAdapter("app:goneUnless")
        fun goneUnless(view: View, visible: Boolean) {
            view.visibility = if (visible) View.VISIBLE else View.GONE
        }
```
- 어댑터를 사용자가 구현함으로써 뷰에 존재하는 코드들을 줄일 수 있다.
