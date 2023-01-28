---
title: "[Data_Structer] Queue & Stack"
excerpt: "Queue & Stack"

categories:
  - Data_Structer
tags:
  - [tag1, tag2]

permalink: /Data_Structer/Data_Structor3/

toc: true
toc_sticky: true

date: 2023-01-28
last_modified_at: 2023-01-28
---


## Queue & Stack

---

### Q. Queue

>queue는 선입선출 FIFO(First In First Out)의 자료구조 
시간복잡도는 enqueue $O(1)$ , dequeue $O(1)$  

활용 예시는 `Cache구현`, `프로세스 관리`, `너비우선탐색(BFS)`


💡  기초적인 자료구조로 면접에서 간단한 질문으로 종종 나오는 문제. 
LIFO인 stack과 다르게 FIFO자료구조임을 기억 
또한 활용 예시들과 Circular Queue 자료구조를 잘 알아야한다. 

### FIFO

 >queue는 시간 순서상 먼저 집어 넣은 데이터가 먼저 나오는 선입선출 FIFO(First In First Out)형식으로 데이터를 저장하는 자료구조

![](https://velog.velcdn.com/images/tlsgn8483/post/9662a18f-e019-4a53-8cab-948ea3993c99/image.png)

### enqueue & dequeue

queue에서 데이터를 추가하는 것을 enqueue

queue에서 데이터를 추출 하는 것은 dequeue

enqueue의 경우 queue의 맨 뒤에 데이터를 추가하면 완료되기 때문에 시간복잡도는 O(1)

이와 비슷하게 dequeue의 경우 맨 앞의 데이터를 삭제하면 완료 되기 때문에 동일하게 O(1)의 시간복잡도를 가진다.

### 구현 방식

![](https://velog.velcdn.com/images/tlsgn8483/post/156f4967-4791-45d0-abea-793ec290afcd/image.png)

- Array-Based queue: enqueue와 dequeue 과정에서 남는 메모리가 생깁니다. 따라서 메모리의 낭비를 줄이기 위해 주로 Circular queue형식으로 구현

- List-Based: 재할당이나 메모리 낭비의 걱정이 없다.

### 원형큐(circular queue)

### 확장 & 활용

>queue의 개념에서 조금 확장한 자료구조들로는 양쪽에서 enqueue와 dequeue가 가능한 deque(double-ended queue)와 시간순서가 아닌 우선순위가 높은 순서로 dequeue할 수 있는 priority queue가 있음.

 활용 예시로는 하나의 자원을 공유하는 `프린터`나, `CPU task scheduling`, `Cache구현`, `너비우선탐색(BFS)`


### Stack

 >stack은 후입선출 LIFO(Last In First Out)의 자료구조. 
 시간복잡도는 push $O(1)$ , pop $O(1)$  
 
 활용 예시는 `후위 표기법 연산`, `괄호 유효성 검사`, `웹 브라우저 방문기록(뒤로 가기)`, `깊이우선탐색(DFS)`


>💡 stack이 굉장히 중요한 자료구조이지만, 면접에서는 stack을 단독으로 질문하는 경우는 많이 없다. 
따라서 Queue의 FIFO와 대조되는 LIFO라는 특성과, 활용예시 기억하자.


### LIFO

 >stack은 시간 순서상 가장 최근에 추가한 데이터가 가장 먼저 나오는 후입선출 LIFO(Last In First Out)형식으로 데이터를 저장하는 자료구조. 

![](https://velog.velcdn.com/images/tlsgn8483/post/1fd76e16-b3fd-42d3-af77-db7fd3fc2220/image.png)

### push & pop

 - stack에서 데이터를 추가하는 것을 push라고 하고 데이터를 추출 하는 것은 pop이라고 한다. 
 - push의 경우 stack의 맨 뒤에 데이터를 추가하면 완료되기 때문에 시간복잡도는 O(1)
 - 이와 동일하게 pop의 경우도 맨 뒤의 데이터를 삭제하면 완료 되기 때문에 O(1)의 시간복잡도를 가진다. 
 - push와 pop은 모두 stack의 top에 원소를 추가하거나 삭제하는 형식으로 구현

[3 Stack.mov](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4eca33df-d7f4-40be-bd67-60d5689485de/3_Stack.mov)

### 활용

 stack은 재귀적인 특징이 있어서 프로그램을 개발할 때 자주 쓰이는 자료구조 활용 예시로는 `call stack`, `후위 표기법 연산`, `괄호 유효성 검사`, `웹 브라우저 방문기록(뒤로 가기)`, `깊이우선탐색(DFS)` 

---

### Q. Stack 두 개를 이용하여 Queue를 구현


💡 stack 두 개를 사용하여 enqueue와 dequeue를 구현하는 것에 초점을 맞춰서 문제를 해결 가능

사실 이런 문제는 답이 다양하고, 답 자체가 중요하기 보단 답을 찾아가는 과정을 더 중요하게 보기 때문에, 풀이를 외우기보단 접근 방식을 주의깊게 생각하자



**[Answer]**

queue의 enqueue()를 구현할 때 첫 번째 stack을 사용하고, dequeue()를 구현할 때 두 번째 stack을 사용하면 queue를 구현할 수 있다.

편의상 enqueue()에 사용할 stack을 instack이라고 부르고 dequeue()에 사용할 stack을 outstack이라고 하자. 

두 개의 stack으로 queue를 구현하는 방법은 다음과 같다.

1. enqueue() ::  instack에 push()를 하여 데이터를 저장
2. dequeue() ::
    1. 만약 outstack이 비어 있다면 instack.pop() 을 하고 outstack.push()를 하여 instack에서 outstack으로 모든 데이터를 옮겨 넣는다. 이 결과로 가장 먼저 왔던 데이터는 outstack의 top에 위치하게 된다.
    
    2. outstack.pop()을 하면 가장먼저 왔던 데이터가 가정 먼저 추출된다.(FIFO)


**[코드 작성]**

```python
class Queue(object):
    def __init__(self):
        self.instack=[]
        self.outstack=[]

    def enqueue(self,element):
        self.instack.append(element)

    def dequeue(self):
        if not self.outstack:
            while self.instack:
                self.outstack.append(self.instack.pop())
        return self.outstack.pop()
```


### Queue 두 개를 이용하여 Stack을 구현


>💡 🍯queue 두 개를 사용하여 stack의 push와 pop를 구현하는 것에 초점을 맞춰서 문제를 해결하면 된다. 
구현 방법을 외우기보단 문제를 해결해 나가는 과정을 보여주는 것이 좋다.


**[Answer]**

편의상 push()에 사용할 queue는 q1이라고 부르고 pop()에 사용할 queue를 q2라고 하자. 

두 개의 queue로 stack을 구현하는 방법은 다음과 같다.

1. push() :: q1으로 enqueue()를 하여 데이터를 저장
2. pop() :: 
    1. q1에 저장된 데이터의 갯수가 1 이하로 남을 때까지 dequeue()를 한 후, 추출된 데이터를 q2에 enqueue()한다. 결과적으로 가장 최근에 들어온 데이터를 제외한 모든 데이터는 q2로 옮겨진다.
    
    2. q1에 남아 있는 하나의 데이터를 dequeue()해서 가장 최근에 저장된 데이터를 반환한다.(LIFO)
    
    3. 다음에 진행될 pop()을 위와 같은 알고리즘으로 진행하기 위해 q1과 q2의 이름을 swap한다.

**[코드 작성]**

```python
import queue

class Stack(object):
    def __init__(self):
        self.q1 = queue.Queue()
        self.q2 = queue.Queue()

    def push(self, element):
        self.q1.put(element)

    def pop(self):
        while self.q1.qsize() > 1:
            self.q2.put(self.q1.get())

        temp = self.q1
        self.q1 = self.q2
        self.q2 = temp

        return self.q2.get()
```


### Q. ⭐ Queue vs priority queue를 비교


>Queue 자료구조는 시간 순서상 먼저 집어 넣은 데이터가 먼저 나오는 **선입선출 FIFO(First In First Out)** 구조로 저장하는 형식. 
이와 다르게 우선순위큐(priority queue)는 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 추출된다. 

Queue의 operation 시간복잡도는 `enqueue` $O(1)$, `dequeue` $O(1)$이고, 

Priority queue는 `push` $O(logn)$ , `pop` $O(logn)$

**[** 🍯 **TIP]**
💡 우선순위큐를 잘 답하기 위해서는 구현 방법과 operation의 시간복잡도를 잘 설명할 수 있어야 한다.

우선순위큐를 구현하라고 하면 Heap을 구현. Heap 자료구조는 이진완전트리를 활용하는 것이고,대표적인 operation의 시간복잡도는  `push` $O(logn)$ , `pop` $O(logn)$

또한 tree가 그려져 있는 상태에서 최대힙, 최소힙의 삽입과 삭제시에 어떻게 node가 삭제되고 연결이 변경되는지의 과정을 그려서 설명가능해야 함. 


### Heap

Heap은 그 자체로 우선순위큐(priority queue)의 구현과 일치

Heap은 완전이진트리 구조입니다. Heap이 되기 위한 조건은 다음과 같다.

- 각 node에 저장된 값은 child node들에 저장된 값보다 작거나 같다(min heap)
    
    ⇒ root node에 저장된 값이 가장 작은 값이 된다.
    

![](https://velog.velcdn.com/images/tlsgn8483/post/04170ef1-5d56-4a4c-879b-cfd94384f35d/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/463431c9-d742-4842-88a8-2adf7802058e/image.png)


### Heap구현

트리는 보통 Linked list로 구현. 

하지만 Heap은 tree임에도 불구하고 array를 기반으로 구현. 그 이유는 새로운  node를 힙의 ‘마지막 위치’에 추가해야 하는데, 이 때 array기반으로 구현해야 이 과정이 수월해지기 때문이다.

- 구현의 편의를 위해 array의 0번 째 index는 사용하지 않습니다.
- 완전이진트리의 특성을 활용하여 array의 index만으로 부모 자식간의 관계를 정의.
    - $n$번 째 node의 left child node = $2n$
    - $n$번 째 node의 right child node = $2n+1$
    - $n$번 째 node의 parent node = $n/2$

![](https://velog.velcdn.com/images/tlsgn8483/post/af21d292-e991-4b9f-aef7-93132f3f130c/image.png)


### Heap push -  $O(logn)$

heap tree의 높이는 $logN$

`push()` 를 했을 때, swap하는 과정이 최대  $logN$번 반복되기 때문에 시간복잡도는  $O(logn)$


### Heap pop - $O(logn)$

`pop()`을 했을 때, swap하는 과정이 최대  $logN$번 반복되기 때문에 시간복잡도는  $O(logn)$

- 각 node에 저장된 값은 child node들에 저장된 값보다 크거나 같다(max heap)
    
    ⇒ root node에 저장된 값이 가장 큰 값이 된다.
    

---

## Hash table & BST(Binary Search Tree)

---

### ⭐⭐BST

**[핵심]**

이진탐색트리(Binary Search Tree; BST)는 정렬된 tree. 

어느 node를 선택하든 해당 node의 left subtree에는 그 node의 값보다 작은 값들을 지닌 node들로만 이루어져 있고, node의 right subtree에는 그 node의 값보다 큰 값들을 지닌 node들로만 이루어져 있는 binary tree

검색과 저장, 삭제의 시간복잡도는 모두 $O(logn)$이고, worst case는 한쪽으로 치우친 tree가 됐을 때 $O(n)$

**[** 🍯 **TIP]**

>💡 BST는 저장과 동시에 정렬을 하는 자료구조 
새로운 데이터를 저장할 때 일정한 규칙에 따라 저장. 

### BST

![BST.001.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/347642d6-e0ec-4311-bb7d-51d9a24ca849/BST.001.jpeg)

![BST.002.jpeg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4712956f-558e-4f64-b690-2beaa6fec06f/BST.002.jpeg)

### BST 조건

1. root node의 값보다 작은 값은 left subtree에, 큰 값은 right subtree에 있다.
2. subtree도 1번 조건을 만족한다.(Recursive)

![](https://velog.velcdn.com/images/tlsgn8483/post/66b215e5-e796-402c-9626-4f2795cfb8dd/image.png)
![](https://velog.velcdn.com/images/tlsgn8483/post/b6144994-a25c-4872-8f3a-21ffcbe17c80/image.png)

### `search`  $O(logn)$

![](https://velog.velcdn.com/images/tlsgn8483/post/b4d943bc-8d5d-423e-a44e-22fa0a84183a/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/36a93a65-3048-4b01-9ead-2f8550aa78a1/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/bef08a33-8877-46c0-8ff5-22b47f0b3bc2/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/d5857f38-7575-408d-993f-b2da92b55ea2/image.png)


### ⭐⭐ Hash table

>hash table은 효율적인 탐색(빠른 탐색)을 위한 자료구조로써 key-value쌍의 데이터를 입력는다. 
hash function $h$에 key값을 입력으로 넣어 얻은 해시값 $h(k)$를 위치로 지정하여 key- value 데이터 쌍을 저장. 저장, 삭제, 검색의 시간복잡도는 모두 $O(1)$

**[** 🍯 **TIP]**

 좋은 hash function의 조건. 
 
 좋은 hash function의 핵심적인 조건은 해시값이 고르게 분포되게 하는 것

### Direct-address Table

Direct-address Table(직접 주소화 테이블)이란, key 값으로 k를 갖는 원소는 index k에 저장하는 방식

```
key: 출석번호, value: 이름

(3, 노정호)
(5, 배준석)
(6, 정재헌)
(7, 남영욱)
```

![](https://velog.velcdn.com/images/tlsgn8483/post/2904127a-0a97-4918-b50d-19648e39330c/image.png)


직접 주소화 방법으로 통해 key-value 쌍의 데이터를 저장하고자 하면 많은 문제가 발생

- 불필요한 공간 낭비

```
key: 학번, value: 이름

(2022390, 노정호)
(2022392, 배준석)
(2022393, 정재헌)
(2022401, 남영욱)
```

![](https://velog.velcdn.com/images/tlsgn8483/post/7836a550-0ac4-4472-aab9-e222e668d9ed/image.png)


- key가 다양한 자료형을 담을 수 없게 됨

```
key: ID, value: 이름

(nossi8128, 노정호)
(js9876, 배준석)
(zebra001, 정재헌)
(nam1234, 남영욱)
```

![](https://velog.velcdn.com/images/tlsgn8483/post/a77d845e-e2b6-423b-8028-62625cb32a3e/image.png)

### Hash table

 (key, value) 데이터 쌍을 저장하기 위한 방법.
 
 
 hash table은 hash function $h$를 이용해서 ($key$, $value$)를 index: $h(k)$에 저장. 
 
 이 때, “키 $k$값을 갖는 원소가 위치 $h(k)$에 hash된다.” 또는 “$h(k)$는 키 $k$의 해시값이다”라고 표현. key는 무조건 존재해야 하며, 중복되는 key가 있어서는 안된다.

 한편, hash table을 구성하고 있는, (key, value)데이터를 저장할 수 있는 각각의 공간을 slot 또는 bucket이라 한다.

![](https://velog.velcdn.com/images/tlsgn8483/post/2dcdb3ce-52f2-4fc5-9c52-1468039b3b78/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/606157bb-bc23-46e8-82d8-0207afc383ed/image.png)


### Collision

collision이란 서로 다른 key의 해시값이 똑같을 때를 말한다. 

즉, 중복되는 key는 없지만 해시값은 중복될 수 있는데 이 때 collision이 발생했다고 한다. 

따라서 collision이 최대한 적게 나도록 hash function을 잘 설계해야하고, 어쩔 수 없이 collision이 발생하는 경우 seperate chaining 또는 open addressing등의 방법을 사용하여 해결해야 한다.

![](https://velog.velcdn.com/images/tlsgn8483/post/f877e919-1bb0-4e76-92e8-f5040a8e363d/image.png)


### 시간복잡도와 공간효율

**시간복잡도**는 저장, 삭제, 검색 모두 기본적으로 $O(1)$이지만, collision으로 인하여 최악의 경우  $O(n)$이 될 수 있다. 

**공간효율성**은 떨어진다. 데이터가 저장되기 전에 미리 저장공간(slot, bucket)을 확보해야 하기 때문. 

따라서 저장공간이 부족하거나 채워지지 않은 부분이 많은 경우가 생길 수 있습니다.


### ⭐⭐⭐⭐ Hash table에서 collision이 발생할 때 해결방법

**[핵심 답변]**

collision이 발생할 경우 대표적으로 2가지 방법으로 해결

첫 번째, open addressing 방식은 collision이 발생하면 미리 정한 규칙에 따라 hash table의 비어있는 slot을 찾는다. 

빈 slot을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나눈다.

두 번째, separete chaining 방식은 linked list를 이용한다. 만약에 collision이 발생하면 linked list에 노드(slot)를 추가하여 데이터를 저장한다.


💡 hashtable에서 collision이 발생했을 때, seperate chaining과 open addressing 두 가지 방식으로 해결을 한다. 

두 가지 방법이 어떤 메커니즘으로 작동하는지, 어떤 차이점이 있는지 이해

또한 seperate chaining의 경우 worst case의 시간복잡도에 대해서 설명할 줄 알아야 한다. 

삽입과 검색, 삭제의 시간복잡도는 $O(1)$이지만, worst case의 경우에 $O(n)$가 될 수 있다.


### Open addressing

 open addressing 방식은 collision이 발생하면 미리 정한 규칙에 따라 hash table의 비어있는 slot을 찾는다. 
 
 추가적인 메모리를 사용하지 않으므로 linked list 또는 tree자료구조를 통해 추가로 메모리 할당을 하는 separate chaining방식에 비해 메모리를 적게 사용한다.

 open addressing은 빈 slot을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나뉜다.

 Linear Probing(선형 조사법)& Quadratic Probing(이차 조사법) : 선형 조사법은 충돌이 발생한 해시값으로 부터 일정한 값만큼$(+1, +2, +3, ...)$ 건너 뛰어, 비어 있는 slot에 데이터를 저장한다. 
 
 이차 조사법은 제곱수($+1^2, +2^2, +3^2, ...$)로 건너 뛰어, 비어 있는 slot을 찾는다.
    
 충돌이 여러번 발생하면 여러번 건너 뛰어 빈 slot을 찾는다. 
 
 선형 조사법과 이차 조사법의 경우 충돌 횟수가 많아지면 특정 영역에 데이터가 집중적으로 몰리는 클러스터링(clustering)현상이 발생하는 단점 
 
 클러스터링 현상이 발생하면, 평균 탐색 시간이 증가하게 됩니다.
    
- Double Hashing(이중해시, 중복해시) : 이중 해싱은 open addressing 방식을 통해 collision을 해결할 때, probing하는 방식중에 하나

linear probing이나 quadratic probing을 통해 탐사할 때는 탐사이동폭이 같기 때문에 클러스터링 문제가 발생할 수 있다. 

클러스터링 문제가 발생하지 않도록 2개의 해시함수를 사용하는 방식을 이중 해싱이라고 한다. 하나는 최초의 해시값을 얻을 때 사용하고 또 다른 하나는 해시 충돌이 발생할 때 탐사 이동폭을 얻기 위해 사용한다.


### Separate chaining

Separate chaining 방식은 linked list(또는 Tree)를 이용하여 collision을 해결합니다. 충돌이 발생하면 linked list에 노드(slot)를 추가하여 데이터를 저장합니다.

- 삽입: 서로 다른 두 key가 같은 해시값을 갖게 되면 linked list에 node를 추가하여 (key, value) 데이터 쌍을 저장합니다. 삽입의 시간복잡도는 $O(1)$
- 검색: 기본적으로  $O(1)$의 시간복잡도 이지만 **최악의 경우** $O(n)$의 시간복잡도를 갖는다.
- 삭제:  삭제를 하기 위해선 검색을 먼저 해야하므로 검색의 시간복잡도와 동일

기본적으로 $O(1)$이지만 **최악의 경우** $O(n)$의 시간복잡도 가진다.

 worst case의 경우 n개의 모든 key가 동일한 해시값을 갖게 되면 길이 n의 linked list가 생성되게 된다. 이 때, 검색의 시간복잡도가 $O(n)$

![](https://velog.velcdn.com/images/tlsgn8483/post/6028def7-2352-4eea-8df7-d2cafae14968/image.png)

참고: seperate chaining은 기본적으로 linked list를 이용하여  데이터를 저장하지만, collision이 많이 발생하여 linked list의 길이가 길어지면 Binary Search Tree(BST)자료구조를 이용하여 데이터를 저장하기도 한다. 

BST를 사용함으로써 검색의 worst case 시간복잡도를 $O(n)$ 에서 $O(logn)$으로 낮출 수 있다.

참조 : [개발남노씨 자료구조](https://www.nossi.dev/interview/cs/dsa)
