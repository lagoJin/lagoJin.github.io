---
title: Kotlin Queue??
last_modified_at: '2020-01-15 15:40:00 +0300'
published : true
excerpt: Kotlin에서 list를 활용한 Queue에 대한 구현 방법입니다.
comments : true
toc : true
toc_sticky: true
tags :
    - Kotlin
    - queue
    - list
---

### 1.개요
kotlin에서 제공하는 list를 이용하여 queue를 이용하여 구현한 방법에 대해 소개하려고 합니다.

### 2. Queue란?
First In First Out의 형태를 가지며 입력은 enqueue, 출력은 dequeue이라고 부른다.

👉쉽게 말해 표를 끊기 위해 줄을 서는 모습을 상상하면 좋다. 맨 처음 줄을 선 사람은 맨 처음 결제할 수 밖에 없다.

---

### 3. Kotlin Queue 정의
```kotlin
    interface QueueImplement<Type> {

        fun enqueue(item: Type)
        fun dequeue(): Type?
        fun count(): Int
        fun peek(): Type?
        fun isEmpty(): Boolean

    }
```
※ Generic을 이용하여 사용자가 원하는 데이터 형을 넣을 수 있도록 구현

### 4. kotlin Queue 구현
```kotlin
    class Queue : QueueImplement<Int> {
        var list = mutableListOf<Int>()

        override fun enqueue(item: Int) {
            list.add(item)
        }

        override fun dequeue(): Int? {
            if (!isEmpty()) {
              return list.removeAt(0)
            }
            return null
        }

        override fun count(): Int {
            return list.size
        }

        override fun peek(): Int? {
            if (!isEmpty()) {
                return list[0]
            }
            return null
        }

        override fun isEmpty(): Boolean {
            return list.size == 0
        }
    }
```

### 5. 결론
kotlin에서 list를 이용하면 간단하게 Queue를 구현할 수 있다. 값이 존재하지 않을 때 필자는 null을 사용하여 값이 없을때를 판단 해주었으나, 다른 방법도 활용이 가능할 것이라 생각한다.
