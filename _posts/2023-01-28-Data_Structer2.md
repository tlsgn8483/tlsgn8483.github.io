---
title: "[Data_Structer] Array & Linked List"
excerpt: "Array & Linked List"

categories:
  - Data_Structer
tags:
  - [tag1, tag2]

permalink: /Data_Structer/Data_Structor2/

toc: true
toc_sticky: true

date: 2023-01-27
last_modified_at: 2023-01-27
---

## Array & Linked List
---
### Array


> Array는 연관된 data를 **메모리상에 연속적이며 순차적**으로 **미리 할당된 크기**만큼 저장하는 자료구조

> 💡 Array와 Linked List의 가장 큰 차이점은 메모리에 저장되는 방식과 이에 따른 operation의 연산 속도(time complexity)

![](https://velog.velcdn.com/images/tlsgn8483/post/c0e92997-d922-4da9-a5e6-3d8019db3937/image.png)

### Array의 특징

>
- 고정된 저장 공간(fixed-size)
- 순차적인 데이터 저장(order)

Array의 장점은 lookup과 append가 빠르다. 따라서 조회를 자주 해야되는 작업에서는 Array 자료구조 자주사용 

Array의 단점은 fixed-size 특성상 선언시에 Array의 크기를 미리 정해야 된다. 

이는 메모리 낭비나 추가적인 overhead가 발생할 수 있습니다.

### 시간복잡도

|  | Array |
| --- | --- |
| access | $O(1)$ |
| append | $O(1)$ |
| 마지막 원소delete | $O(1)$ |
| insertion | $O(n)$ |
| deletion | $O(n)$ |
| search | $O(n)$ |


### Q. Dynamic Array


- Array의 경우 size가 고정되었기 때문에 선언시에 설정한 size보다 많은 갯수의 data가 추가되면 저장 X. 
- Dynamic Array는 저장공간이 가득 차게 되면 resize를 하여 유동적으로 size를 조절하여 데이터를 저장하는 자료구조

>
1. resize를 하는 방식
2. 데이터 추가(append)할 때의 시간복잡도


### Dynamic Array

![](https://velog.velcdn.com/images/tlsgn8483/post/0a6cf601-46f1-415f-a2c4-b2e54a451b41/image.png)

- Dynamic Array는 size를 자동적으로 resizing을 하는 Array

- 기존에 고정된 size를 가진 Static Array의 한계점을 보완 

- Dynamic Array는 data를 계속 추가하다가 기존에 할당된 memory를 초과하게 되면, size를 늘린 배열을 선언 후, 그곳으로 모든 데이터를 옮김으로써 늘어난 크기의 size를 가진 배열

- resizing 을 하는 방법은 여러 가지가 있는데, 대표적으로 기존 Array size의 2배 size를 할당하는 doubling 방법이 있음. 

### Doubling

> 데이터를 추가(append $O(1)$) 하다가 메모리를 초과하게 되면 기존 배열의size보다 두배 큰 배열을 선언하고 데이터를 일일이 옮기는(n개의 데이터를 일일이 옮겨야 하므로 $O(n)$ ) 방법

![](https://velog.velcdn.com/images/tlsgn8483/post/caef766c-90c7-4ff1-9f64-06ede9b15272/image.png)


### 분할상환 시간복잡도 Amortized time complexity

>Dynamic array에 데이터를 추가할 때마다 $O(1)$의 시간이 걸리게 된다. → 추가를 하다가 미리 선언된 size를 넘어서는 순간에, resize → 이 때는 일일이 데이터를 모두 옮겨야 되기 때문에 이 때만큼은$O(n)$의 시간복잡도 

append의 총 과정을 살펴보면 데이터를 마지막 인덱스에 추가하는($O(1)$)작업이 대다수이고, size를 넘어설 때는 size를 두 배 늘리고 데이터를 일일이 옮기는 과정 (resize $O(n)$)이 아주 가끔 발생

append의 전체적인 시간복잡도는 $O(1)$


### Linked List


>Linked List는 Node라는 구조체로 이루어져 있는데, Node는 데이터 값과 다음 Node의 address를 저장. 
Linked List는 물리적인 메모리상에서는 비연속적으로 저장이 되지만 Linked list를 구성하는 각각의 Node가 next Node의 address를 가리킴으로써 논리적인 연속성을 가진 자료구조



💡 Linked List는 tree, graph등 다른 자료구조를 구현할 때 자주 쓰이는 기본 자료구조

- Linked list를 설명할 때에는 메모리상에서 불연속적으로 데이터가 저장되는 점과 Node의 next address를 통해 불연속적인 데이터를 연결하여 논리적 연속성을 보장

- 데이터가 추가 되는 시점에서 메모리를 할당하기 때문에 메모리를 좀 더 효율적으로 사용할 수 있다는 장점



### 물리적 메모리 관점에서 본 Linked list - 비연속적

### 논리적 연속성

> 각 Node들은 next address정보를 가지고 있기 때문에 논리적으로 연속성을 유지하면서 연결
Array의 경우,  연속성을 유지하기 위해 물리적 메모리 상에서 순차적으로 저장하는 방법을 사용하였고, Linked list에는 메모리에서 연속성을 유지하지 않아도 되기 때문에 메모리 사용이 좀 더 자유로운 대신, Next address를 추가적으로 저장해야 하기 때문에 데이터 하나당 차지하는 메모리가 더 커지게 된다.


(1) 그림에서 링크드리스트가 연속적으로 저장된것처럼 보이지만 실제 메모리상에서는 (2)와 같다

(1) 논리적 연속성

![](https://velog.velcdn.com/images/tlsgn8483/post/4d492d2f-81d5-4337-afdd-4e532825fc2c/image.png)

(2) 물리적 불연속성

![](https://velog.velcdn.com/images/tlsgn8483/post/0e7fb367-e438-464c-b249-8564fd0df5ac/image.png)

### 시간복잡도

>Array의 경우 중간에 데이터를 삽입/삭제하게 되면 해당 인덱스의 뒤에 있는 모든 원소들은 shift -> $O(n)$의 시간복잡도
Linked list를 물리적으로 옮길 필요없이 next address가 가리키는 주소값만 변경하면 되기 때문에 $O(1)$의 시간복잡도로 삽입/삭제가 가능

|  | Linked list |
| --- | --- |
| access | $O(n)$ |
| search | $O(n)$ |
| insertion | $O(1)$ |
| deletion | $O(1)$ |

![image](https://user-images.githubusercontent.com/61337570/215256219-522a7610-1baa-4eba-803c-a1ae1edb6bed.png)

![image](https://user-images.githubusercontent.com/61337570/215256238-201e61c3-d234-4eaa-8323-5371d772ffea.png)

---

---

### Q. ⭐ Array vs Linked list를 비교

>Array는 메모리 상에서 연속적으로 데이터를 저장하는 자료구조
Linked List는 메모리상에서는 연속적이지 않지만, 각각의 원소가 다음 원소의 메모리 주소값을 저장해 놓음으로써 논리적 연속성을 유지

각 operation의 시간복잡도가 다름. 

데이터 조회는 Array의 경우 $O(1)$, Linked list는 $O(n)$의 시간복잡도 가지며, 삽입/삭제는 Array $O(n)$, Linked list $O(1)$의 시간복잡도를 가진다. 

**💡추천**

얼마만큼의 데이터를 저장할지 미리 알고있고, 조회를 많이 한다면 Array를 사용

반면에 몇개의 데이터를 저장할 지 불확실하고 삽입 삭제가 잦다면 Linked list를 사용




💡  Array와 Linked List의 주된 차이점들은 메모리 구조의 차이! 

Array는 메모리상에서 연속적으로 데이터를 저장하고, Linked List는 불연속적으로 저장 

메모리 구조의 차이로 인해 operation구현방법이 다르고 시간복잡도가 달라진다. 또한 메모리 활용도에서도 차이가 있다.

상황에 따라 메모리를 효율적으로 사용할 수 있는 자료구조가 달라진다.



### 조회 (lookup)

 >Array는 메모리상에서 연속적으로 데이터를 저장하였기 때문에 저장된 데이터에 즉시 접근(random access $O(1)$)할 수 있다. 
 이와 반면 Linked List는 메모리 상에서 불연속적으로 데이터를 저장하기 때문에 순차 접근(Sequential Access)만 가능. 
 즉, 특정 index의 데이터를 조회하기 위해 $O(n)$의 시간복잡도

### 삽입/삭제 (insert/delete)

 Array의 경우 맨 마지막 원소를 추가/삭제하면 시간복잡도가 $O(1)$
 
 하지만 맨 마지막 원소가 아닌 중간에 있는 원소를 삽입/삭제하면 해당 원소보다 큰 인덱스의 원소들을 한 칸씩 shift 해줘야 하는 비용(cost)이 발생
 따라서, 이 경우에는 시간복잡도가 $O(n)$

 Linked List는 어느 원소를 추가/삭제 하더라도 node에서 다음주소를 가르키는 부분만 다른 주소 값으로 변경하면 되기 때문에 shift할 필요 없어 시간복잡도가 $O(1)$

 하지만 Linked list의 경우 추가/삭제를 하려는 index까지 도달하는데 $O(n)$의 시간이 걸리기 때문에, 실질적으로 Linked List도 추가/삭제 시에 $O(n)$의 시간복잡도 가진다. 

### memory

 Array의 주된 장점은 데이터 접근과 append가  빠르다는 것입니다. 하지만 메모리 낭비라는 단점, 배열은 선언시에 fixed size를 설정하여 메모리 할당
 
 즉, 데이터가 저장되어 있지 않더라도 메모리를 차지하고 있기 때문에 메모리 낭비가 발생.

 이와 반면 Linked List는 runtime중에서도 size를 늘리고 줄일 수 있다. 그래서 initial size를 고민할 필요 없고, 필요한 만큼 memorry allocation을 하여 메모리 낭비가 없다.
 
 
 참조 : [개발남노씨 자료구조](https://www.nossi.dev/interview/cs/dsa)
