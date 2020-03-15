---
title: LiveData Testing 어떻게 하니?
last_modified_at: '2020-03-15 15:40:00 +0300'
published : true
excerpt: Android에 존재하는 Livedata에 관한 테스트 방법입니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - Livedata
    - UnitTest
---

### 1.개요

Android내에서 UnitTest를 진행하다보면 Livedata에 들어있는 값을 테스트해야 하는 경우가 생깁니다. 이런 경우에 해결하는 방법과 팁을 안내해드리려고 합니다.

### 2. 예시

- TestLibrary는 Mockk를 사용하며, 다른 테스트를 사용하여도 동일한 방법으로 가능합니다.

```kotlin
@Test
fun `Livedata 테스트`() {

    val testLiveData = MutableLiveData<Boolean>(true)

    assertEquals(testLiveData.getOrAwaitValue(), true)
}
```

- LiveData에 관한 결과를 받으려면 옵저빙을 하고 있어야합니다. 이러한 역할을 getOrAwaitValue라는 함수를 이용하여 대신합니다. 구글 짱!

```kotlin
/* Copyright 2019 Google LLC.
   SPDX-License-Identifier: Apache-2.0 */
fun <T> LiveData<T>.getOrAwaitValue(
    time: Long = 2,
    timeUnit: TimeUnit = TimeUnit.SECONDS
): T {
    var data: T? = null
    val latch = CountDownLatch(1)
    val observer = object : Observer<T> {
        override fun onChanged(o: T?) {
            data = o
            latch.countDown()
            this@getOrAwaitValue.removeObserver(this)
        }
    }

    this.observeForever(observer)

    // Don't wait indefinitely if the LiveData is not set.
    if (!latch.await(time, timeUnit)) {
        throw TimeoutException("LiveData value was never set.")
    }

    @Suppress("UNCHECKED_CAST")
    return data as T
}
```

### 3. 결론

프로그래밍에서 점점 중요도가 올라가고 있는 테스트 입니다. 안드로이드는 context에 영향을 많이 받아 테스트하기가 까다롭기도 하지만, 항상 방법은 존재합니다.!


### 4. 참고 링크

- https://medium.com/androiddevelopers/unit-testing-livedata-and-other-common-observability-problems-bb477262eb04