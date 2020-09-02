---
title: Flow! Collect vs Single
last_modified_at: '2020-09-02 15:00:00 +0300'
published : true
excerpt: Flow에서 collect와 single에 관한 차이점을 설명하려고 합니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - Android
    - Flow
---

### 1. 개요

Kotlin 에서 지원하고 있는 [Flow](https://kotlinlang.org/docs/reference/coroutines/flow.html)에 대해 자세히 알아보고, data를 받을 수 있는 collect와 Single에 대해 비교해보려고 합니다.

### 2. 코드

- 구현

```kotlin
public suspend inline fun <T> Flow<T>.collect(crossinline action: suspend (value: T) -> Unit): Unit =
    collect(object : FlowCollector<T> {
        override suspend fun emit(value: T) = action(value)
    })
```

```kotlin
public suspend fun <T> Flow<T>.single(): T {
    var result: Any? = NULL
    collect { value ->
        if (result !== NULL) error("Expected only one element")
        result = value
    }

    if (result === NULL) throw NoSuchElementException("Expected at least one element")
    @Suppress("UNCHECKED_CAST")
    return result as T
}
```

- 사용방법

```kotlin
viewModelScope.launch {
  val data = repository.getSampleData().single()
  _data.value = data
}

viewModelScope.launch {
  repository.getSampleData().collect{ data ->
    _data.value = data
  }
}
```

### 3. 후기
- 처음 사용할 당시, collect를 사용하여 콜백형태의 코드를 작성하였으며 한 scope에서 collect를 내부적으로 다시 호출 하였을 때는 코드 가독성이 좋지않다. 잘 사용하고 싶어서 코루틴을 공부하면서, Flow에 있는 다양한 기능들이 강력하다고 생각하며, AAC 와 같이 사용하면 개발자가 고민할 부분을 덜어준다고 생각한다.

### 3. 정리
 - Kotlin Coroutine 사용하여 나아가고자 하는 방향은 콜백형태의 코드를 절차적으로 읽을 수 있는 형태를 권장하고 있습니다. 하나의 Scope에서 WithContext를 사용하게 되면 동기적으로 처리할 수도 있습니다. 코드 퀄리티는 다양한 방법으로 사용할 수 있는 Coroutine을 통해 상승시킬 수 있습니다.
