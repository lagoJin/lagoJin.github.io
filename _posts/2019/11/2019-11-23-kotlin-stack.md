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
        fun pop() : Type
        fun peek() : Type
        fun push(item : Type)
        fun isEmpty() : Boolean
    }

🔨Generic을 이용하여 사용자가 원하는 타입에 맞게 구현할 수 있도록 interface 제작

Kotlin Stack

    class Stack<E> : StackImplement<E> {

        val list = mutableListOf<E>()

        override fun size(): Int {
            return list.size
        }

        override fun pop(): E {
            return list.removeAt(list.size - 1)
        }

        override fun peek(): E {
            val item = list[list.size - 1]
            return item
        }

        override fun push(item: E) {
            list.add(item)
        }

        override fun isEmpty(): Boolean {
            return list.size == 0
        }



### 문제풀이 스택 -> [쇠막대기](https://programmers.co.kr/learn/courses/30/lessons/42585)

- 쇠막대기는 자신보다 긴 쇠막대기 위에만 놓일 수 있습니다.
- 쇠막대기를 다른 쇠막대기 위에 놓는 경우 완전히 포함회도록 놓되, 끝점은 겹치지 않도록 놓습니다.
- 각 쇠막대기를 자르는 레이저는 적어도 하나는 존재합니다.
- 레이저는 어떤 쇠막대기의 양 끝점과도 겹치지 않습니다.

여기서 주의해서 봐야할 점은 레이저가 지나가면서 짜른 막대에 대한 갯수 또한 포함해야 한다는 것입니다.

()을 -로 변경하여 반복문을 돌면서 만난 순간 stack에 저장되어있는 갯수를 answer에 저장

    fun solution(arrangement: String): Int {

            var answer: Int = 0
            val stack = Stack<Char>()
            val str = arrangement.replace("()", "-")

            str.forEach {
                when (it) {
                    '-' -> {
                        answer += stack.size()
                    }
                    '(' -> {
                        stack.push(it)
                    }
                    ')' -> {
                        stack.pop()
                        answer += 1
                    }
                }
            }
            return answer
    }
