---
title: Kotlin Stack이란?
last_modified_at: '2019-11-23 20:35:00 +0300'
published : true
excerpt: 소개해드릴것은 kotlin에서 Stack구현 방법 및 사용 방법 입니다.
comments : true
tags :
    - Kotlin
    - Stack
---

### 스택이란 ?

List In First Out의 형태를 가지며 입력은 push, 출력은 pop, peek는 top이라고 부른다.

👉 쉽게 말해 책을 쌓는 모습을 상상하면 좋다. 맨 처음 쌓은 책을 맨 마지막에 뺄 수 밖에 없다.

- 스택 정의
    1. 힙 영역 메모리에서 일반적인 데이터를 저장하는 스택
    2. 스택 영역 메모리에서 프로그램의 각 분기점에 변수 같은 정보를 저장하기 위한 스택

    ![](/assets/images/2019/11/stack/1.png)*출처 : https://visualgo.net/en/list?slide=4*
---

Kotlin Stack Implement

    interface StackImplement<Type>{

        fun count() : Int
        fun pop() : Type?
        fun peek() : Type?
        fun push(item : Type)
        fun isEmpty() : Boolean
    }

🔨Generic을 이용하여 사용자가 원하는 타입에 맞게 구현할 수 있도록 interface 제작

Kotlin Stack

    class Stack : StackImplement<Int>{

        val list = mutableListOf<Int>()

        override fun count(): Int {
            return list.size
        }

        override fun pop(): Int? {
            if(!isEmpty()){
              return list.removeAt(list.size-1)
            }
            return null
        }

        override fun peek(): Int? {
            if(!isEmpty()){
                val item = list[list.size-1]
                return item
            }
            return null
        }

        override fun push(item: Int) {
            list.add(item)
        }

        override fun isEmpty(): Boolean {
            return list.size == 0
        }
    }
