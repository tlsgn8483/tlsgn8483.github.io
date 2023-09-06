---
title: "[Data_Structer] Hash Table"
excerpt: "Hash Table"

categories:
  - Data_Structer
tags:
  - [tag1, tag2]

permalink: /Data_Structer/Data_Structor4/

toc: true
toc_sticky: true

date: 2023-01-31
last_modified_at: 2023-01-31
---

## Hash table & BST(Binary Search Tree)

---

### ⭐⭐BST

**[핵심]**

이진탐색트리(Binary Search Tree; BST)는 정렬된 tree. 

어느 node를 선택하든 해당 node의 left subtree에는 그 node의 값보다 작은 값들을 지닌 node들로만 이루어져 있고, node의 right subtree에는 그 node의 값보다 큰 값들을 지닌 node들로만 이루어져 있는 binary tree

검색과 저장, 삭제의 시간복잡도는 모두 O(logn)이고, worst case는 한쪽으로 치우친 tree가 됐을 때 $O(n)$

**[** 🍯 **TIP]**

>💡 BST는 저장과 동시에 정렬을 하는 자료구조 
새로운 데이터를 저장할 때 일정한 규칙에 따라 저장. 

### BST

![image](https://user-images.githubusercontent.com/61337570/215256865-f165778a-1106-4a29-8622-e3aa9aadfcf1.png)

![image](https://user-images.githubusercontent.com/61337570/215256887-cc3d0143-0081-4fb3-b367-bd735671bf6c.png)



### BST 조건

1. root node의 값보다 작은 값은 left subtree에, 큰 값은 right subtree에 있다.
2. subtree도 1번 조건을 만족한다.(Recursive)

![image](https://user-images.githubusercontent.com/61337570/215256922-15d698c3-dcc1-4748-a4e8-882f5ee83bb7.png)

![image](https://user-images.githubusercontent.com/61337570/215256929-f24a9380-3204-4747-8d9c-e5d5c72a5ddb.png)

### `search`  O(logn)

![](https://velog.velcdn.com/images/tlsgn8483/post/b4d943bc-8d5d-423e-a44e-22fa0a84183a/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/36a93a65-3048-4b01-9ead-2f8550aa78a1/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/bef08a33-8877-46c0-8ff5-22b47f0b3bc2/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/d5857f38-7575-408d-993f-b2da92b55ea2/image.png)


### ⭐⭐ Hash table

>hash table은 효율적인 탐색(빠른 탐색)을 위한 자료구조로써 key-value쌍의 데이터를 입력. 
hash function $h$에 key값을 입력으로 넣어 얻은 해시값 h(k)를 위치로 지정하여 key- value 데이터 쌍을 저장. 저장, 삭제, 검색의 시간복잡도는 모두 $O(1)$

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
 
 
 hash table은 hash function h를 이용해서 (key, value)를 index: h(k)에 저장. 
 
 이 때, “키 k값을 갖는 원소가 위치 h(k)에 hash된다.” 또는 “h(k)는 키 k의 해시값이다”라고 표현. key는 무조건 존재해야 하며, 중복되는 key가 있어서는 안된다.

 한편, hash table을 구성하고 있는, (key, value)데이터를 저장할 수 있는 각각의 공간을 slot 또는 bucket이라 한다.

![](https://velog.velcdn.com/images/tlsgn8483/post/2dcdb3ce-52f2-4fc5-9c52-1468039b3b78/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/606157bb-bc23-46e8-82d8-0207afc383ed/image.png)


### Collision

collision이란 서로 다른 key의 해시값이 똑같을 때를 말한다. 

즉, 중복되는 key는 없지만 해시값은 중복될 수 있는데 이 때 collision이 발생했다고 한다. 

따라서 collision이 최대한 적게 나도록 hash function을 잘 설계해야하고, 어쩔 수 없이 collision이 발생하는 경우 seperate chaining 또는 open addressing등의 방법을 사용하여 해결해야 한다.

![](https://velog.velcdn.com/images/tlsgn8483/post/f877e919-1bb0-4e76-92e8-f5040a8e363d/image.png)


### 시간복잡도와 공간효율

**시간복잡도**는 저장, 삭제, 검색 모두 기본적으로 $O(1)$이지만, collision으로 인하여 최악의 경우  O(n)이 될 수 있다. 

**공간효율성**은 떨어진다. 데이터가 저장되기 전에 미리 저장공간(slot, bucket)을 확보해야 하기 때문. 

따라서 저장공간이 부족하거나 채워지지 않은 부분이 많은 경우가 생길 수 있다.


### ⭐⭐⭐⭐ Hash table에서 collision이 발생할 때 해결방법

**[핵심 답변]**

collision이 발생할 경우 대표적으로 2가지 방법으로 해결

첫 번째, open addressing 방식은 collision이 발생하면 미리 정한 규칙에 따라 hash table의 비어있는 slot을 찾는다. 

빈 slot을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나눈다.

두 번째, separete chaining 방식은 linked list를 이용한다. 만약에 collision이 발생하면 linked list에 노드(slot)를 추가하여 데이터를 저장한다.


💡 hashtable에서 collision이 발생했을 때, seperate chaining과 open addressing 두 가지 방식으로 해결을 한다. 

두 가지 방법이 어떤 메커니즘으로 작동하는지, 어떤 차이점이 있는지 이해

또한 seperate chaining의 경우 worst case의 시간복잡도에 대해서 설명할 줄 알아야 한다. 

삽입과 검색, 삭제의 시간복잡도는 O(1)이지만, worst case의 경우에 O(n)가 될 수 있다.


### Open addressing

 open addressing 방식은 collision이 발생하면 미리 정한 규칙에 따라 hash table의 비어있는 slot을 찾는다. 
 
 추가적인 메모리를 사용하지 않으므로 linked list 또는 tree자료구조를 통해 추가로 메모리 할당을 하는 separate chaining방식에 비해 메모리를 적게 사용한다.

 open addressing은 빈 slot을 찾는 방법에 따라 크게 Linear Probing, Quadratic Probing, Double Hashing으로 나뉜다.

 Linear Probing(선형 조사법)& Quadratic Probing(이차 조사법) : 선형 조사법은 충돌이 발생한 해시값으로 부터 일정한 값만큼$(+1, +2, +3, ...)$ 건너 뛰어, 비어 있는 slot에 데이터를 저장한다. 
 
 이차 조사법은 제곱수($+1^2, +2^2, +3^2, ...$)로 건너 뛰어, 비어 있는 slot을 찾는다.
    
 충돌이 여러번 발생하면 여러번 건너 뛰어 빈 slot을 찾는다. 
 
 선형 조사법과 이차 조사법의 경우 충돌 횟수가 많아지면 특정 영역에 데이터가 집중적으로 몰리는 클러스터링(clustering)현상이 발생하는 단점 
 
 클러스터링 현상이 발생하면, 평균 탐색 시간이 증가
    
- Double Hashing(이중해시, 중복해시) : 이중 해싱은 open addressing 방식을 통해 collision을 해결할 때, probing하는 방식중에 하나

linear probing이나 quadratic probing을 통해 탐사할 때는 탐사이동폭이 같기 때문에 클러스터링 문제가 발생할 수 있다. 

클러스터링 문제가 발생하지 않도록 2개의 해시함수를 사용하는 방식을 이중 해싱이라고 한다. 하나는 최초의 해시값을 얻을 때 사용하고 또 다른 하나는 해시 충돌이 발생할 때 탐사 이동폭을 얻기 위해 사용한다.


### Separate chaining

Separate chaining 방식은 linked list(또는 Tree)를 이용하여 collision을 해결합니다. 충돌이 발생하면 linked list에 노드(slot)를 추가하여 데이터를 저장

- 삽입: 서로 다른 두 key가 같은 해시값을 갖게 되면 linked list에 node를 추가하여 (key, value) 데이터 쌍을 저장. 삽입의 시간복잡도는 O(1)
- 검색: 기본적으로  $O(1)$의 시간복잡도 이지만 **최악의 경우** $O(n)$의 시간복잡도를 갖는다.
- 삭제:  삭제를 하기 위해선 검색을 먼저 해야하므로 검색의 시간복잡도와 동일

기본적으로 $O(1)$이지만 **최악의 경우** $O(n)$의 시간복잡도 가진다.

 worst case의 경우 n개의 모든 key가 동일한 해시값을 갖게 되면 길이 n의 linked list가 생성되게 된다. 이 때, 검색의 시간복잡도가 O(n)

![](https://velog.velcdn.com/images/tlsgn8483/post/6028def7-2352-4eea-8df7-d2cafae14968/image.png)

참고: seperate chaining은 기본적으로 linked list를 이용하여  데이터를 저장하지만, collision이 많이 발생하여 linked list의 길이가 길어지면 Binary Search Tree(BST)자료구조를 이용하여 데이터를 저장하기도 함. 

BST를 사용함으로써 검색의 worst case 시간복잡도를 O(n) 에서 O(logn)으로 낮출 수 있다.

참조 : [개발남노씨 자료구조](https://www.nossi.dev/interview/cs/dsa)
