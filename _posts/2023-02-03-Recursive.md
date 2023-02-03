---
title: "[Data_Structer] 재귀함수(Recursice Function)"
excerpt: "재귀함수(Recursice Function)"

categories:
  - Coding_Test
tags:
  - [tag1, tag2]

permalink: /Coding_Test/Recursive/

toc: true
toc_sticky: true

date: 2023-02-03
last_modified_at: 2023-02-03
---


# 재귀함수(Recursive Function)

>
재귀함수란 자신을 정의할 때, 자기 자신을 재참조하는 함수를 뜻한다.

![](https://velog.velcdn.com/images/tlsgn8483/post/66cedd98-7814-41d5-884e-1a3ce7f3478f/image.png)

## 구성요소 2가지

1. recurrence relation => `점화식`
f(n)을 f(n-1),f(n-2), .. ,f(2),f(1)의 관계식으로 표현하는 것을 recurrence relation이라 말한다.
- Problem과 subproblem(s) 관계식을 말한다.

ex) Factorial: f(n) = n * f(n-1)
ex) Fibonacci sequence : f(n) = f(n-1) + f(n-2)
ex) pascal's Triangle : f(n,m) = f(n-1, m-1) + f(n-1,m)

2. Base Case
- 더 이상 재귀호출을 하지 않아도 계산값을 반환할 수 있는 상황(조건)을 말한다.
- 모든 입력이 최종적으로 Base Case를 이용하여 문제를 해결할 수 있어야 한다.
- BaseCase가 무조건 있어야 재귀함수의 무한루프를 방지할 수 있다.

## 시간복잡도
>
재귀함수 전체 시간복잡도 = 재귀 함수 호출 수 x (재귀 함수 하나당)시간복잡도
Execution Tree: 재귀함수 실행 흐름을 나타내는 데 사용되는 트리

O(n) n에 비례한 호출
`recurrence relation` : f(n) = f(n-1) + n
O(2^n)에 비례한 호출
`recurrence relation`: f(n) = f(n-1) + f(n-2)

O(log2n) : `Binary Search`



참조 : [개발남노씨 자료구조](https://www.nossi.dev/interview/cs/dsa)
