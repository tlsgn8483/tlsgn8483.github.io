---
title: "[Coding_Test] Basic_Coding_Rule"
excerpt: "Basic_Coding_Rule"

categories:
  - Coding_Test
tags:
  - [tag1, tag2]

permalink: /Coding_Test/Basic_Coding_Rule/

toc: true
toc_sticky: true

date: 2023-02-03
last_modified_at: 2023-02-03
---


# 1. 알고리즘 algorithm

>📍 알고리즘이란
- 어떠한 문제를 해결하기 위해 정해진 일련의 절차나 방법; 문제를 해결하는 방법
- 한 문제를 해결하는 방법은 한 가지만 있는게 아니라 무수히 많을 수 있다.
- 자주 쓰이는 문제 해결 방법(알고리즘)은 패턴화; BFS, DFS, DP, 다익스트라 등등
- 각 상황에 적합한 알고리즘을 선택할 수 있어야 한다.

# 2. 알고리즘 평가기준

📍 좋은 알고리즘이란
>
- 시간 복잡도
- 공간 복잡도
- 구현 복잡도


1. 시간 복잡도
    
     동일한 코드로 작성되어도 실행 환경에 따라서 매번 실행시간이 달라질 수 있다. 하지만 걸리는 시간의 경향성은 계산할 수 있습니다. 앞으로 배워볼 시간복잡도(Big-O)를 통해서 알고리즘마다 실행시간 경향성을 살펴보자
    
2. 공간 복잡도 (메모리)
    
     메모리는 한정적이기 때문에 최대한 적은 용량의 메모리를 사용하는 것이 좋다. 하지만 컴퓨터의 메모리 용량이 커짐에 따라서 메모리를 크게 신경쓰지 않고 알고리즘을 작성하는 경우도 있다. 공간복잡도를 통해 특정 알고리즘이 얼마만큼의 메모리를 차지하게 될 지를 나타낸다. 
    
3. 구현 복잡도
    
     꽤 괜찮은 알고리즘을 떠올렸더라도 구현이 너무 복잡하고 알아보기 힘들다면 다시 생각해볼 필요가 있다. 개발자는 대부분의 경우 협업을 해야하고, 구현이 너무 복잡해지면 다른사람은 물론 미래의 나도 알아볼 수 없게 된다.
     
>
 시간과 공간은 보통 trade-off 관계! 실행시간을 줄이려면 메모리를 더 사용해야 하고, 메모리 사용량을 줄이려면 실행시간이 늘어나게 된다. 


# 3. 시간복잡도 Time Complexity

## Runtime (실행 시간)

 시간복잡도에 대해서 공부하기 전에, 실행시간(runtime)에 대해 알고 있어야 한다. 우리는 코드를 한 줄 한 줄 작성하여 프로그램을 완성해야한다. 그렇게 완성된 프로그램을 실행시키면 컴퓨터(CPU)는 한 줄 한 줄 코드를 처리하게 된다. 
 
 모든 코드를 처리하면 우리가 원하던 값을 얻을 수 있게 되고 프로그램은 끝나게 된다. 이렇게 프로그램이 시작되고 모든 코드를 실행하는데 걸리는 시간을 runtime(실행시간)이라고 한다.
 
 
 **실행시간에 영향을 주는 요소는 크게 두 가지**
>
1. 한 줄의 코드당 걸리는 시간
2. 프로그램 내 실행되는 코드의 갯수

![](https://velog.velcdn.com/images/tlsgn8483/post/3dd880fb-ebe6-421f-a02c-90d4cbf4443f/image.png)


>💡 $runtime(n) = C1 + C2 + n \times C3 = An + B$
입력된 $n$의 값에 따라 실행시간이 변한다!


## Big-O

>
💡 input $n$ 의 크기가 커지면 작은 차수의 항들이 runtime에 미치는 영향이 미미하다 
⇒ **작은차수 무시, 계수 무시**

Big-O를 적용하기 위해서는 최고차항이 무엇인지 판별을 해야되는데, 기본적인 것들은 외우고 계시면 편하다
>
$n! > 2^n > n^3 > n^2 > nlogn > n > logn > 1$

![](https://velog.velcdn.com/images/tlsgn8483/post/60f1f263-f871-43b7-88d3-42a8db100d1b/image.png)

```
simple statements
O(1)

a = 5
a += 1
```

```
single loop
O(n)

for i in range(n):
		print("hi")
```


```
nested loop
O(n^2)


for i in range(n):
		for j in range(n):
				print("hi")
```

```
두 번 재호출하는 재귀함수 ([recursion])
O(2^n)

int fibo(int n)
{
    if (n == 1)
        return 0;
    else if(n == 2)
        return 1;
    else
        return fibo(n - 1) + fibo(n-2);
}
```

```
탐색해야되는 데이터가 절반식 줄어드는 함수
ex) 이진탐색 알고리즘
O(logn)

int binarySearch(int data[], int n, int target)
{
    int start = 0;
    int end = n;
    int mid;
    while (end >= start)
    {
        mid = (end + start) / 2;
        if (data[mid] == target)
            return 1;
        else if (data[mid] > target)
            end = mid - 1;
        else
            start = mid + 1;
    }
    return 0;
}
```

## worst-case

Big-O notation을 이용하여 시간복잡도를 구할 때에는 최악의 상황(worst-case)를 고려해야 한다.


```
# O(n)
if condition: 
		for i in range(n):
		pass

# O(n^2)
else :
		for i in range(n):
				for j in range(n):
						pass
```

위의 코드에서 보통의 경우 if문이 실행되고, 굉장히 가끔 else문이 실행된다고 가정 그렇다면 최악의 경우(else문이 실행되는 상황)의 시간복잡도는 O(n^2)이기 때문에 위의 코드의 시간복잡도는 O(n^2)이다. 

# 코딩테스트를 위한 시간복잡도

## 외우면 편한 것들

1. `sort()`  O(nlogn)$
    
    몇몇 문제는 정렬을 하면 쉽게 해결할 수 있다. 하지만 우리는 정렬을 직접 구현하지 않을 것이고, 각 언어에서 정의되어 있는  O(nlogn) 의 `sort()` 함수를 사용하자.
    
2. Hashtable 구축 : O(n) // Hashtable 검색 : O(1)  
    
     Hashtable의 강력함은 검색의 시간복잡도가 O(1) 라는 점, 사실 hashtable은 공간(메모리)를 사용함으로써 시간을 절약하는 방법중에 하나다
    
     보통 문제에서 주어지는 크기 $n$의 데이터(배열의 형태)에 하나 하나 접근하여 hashtable을 구축해야 한다. 이 과정에서 O(n)의 시간복잡도가 걸린다.
    
3. Binary Search ⇒  O(logn) 
    
     정렬된 배열에서 특정 숫자를 찾는 알고리즘인 Binary search는 코딩테스트 문제에서도 종종 나온다. 반복문 또는 재귀 함수가 재호출 될 때마다 탐색해야할 데이터의 크기가 절반씩 줄어들기 때문에 시간복잡도는 O(logn)이 된다.
    
    만약 정렬이 안된 배열이 주어졌다면, 정렬 $O(nlogn)$을 먼저 해주어야 한다.
    
4. Heap (priority queue) 
    - 길이가 n인 배열을 heap으로 만들 때 ⇒ O(nlogn)
    - push() ⇒ O(logn)
    - pop() ⇒  O(logn)


참조 : [개발남노씨 자료구조](https://www.nossi.dev/interview/cs/dsa)
