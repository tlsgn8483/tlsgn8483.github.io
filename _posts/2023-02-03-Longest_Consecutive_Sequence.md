---
title: "[Coding_Test] Longest_Consecutive_Sequence"
excerpt: "Longest_Consecutive_Sequence 문제풀이"

categories:
  - Coding_Test
tags:
  - [tag1, tag2]

permalink: /Coding_Test/Longest_Consecutive_Sequence/

toc: true
toc_sticky: true

date: 2023-02-03
last_modified_at: 2023-02-03
---

# Longest Consecutive Sequence
> 
Q. 정렬되어 있지 않은 정수형 배열 nums가 주어졌다. nums 원소를 가지고 만들 수 있는 가장 긴 연속된 수의 개수는 몇개인지 반환하시오

> 코딩테스트 적용방법!@ 
해쉬테이블 활용 => 메모리를 이용해서 시간복잡도를 줄일 때 사용
`key in {}` 가 핵심
**제약조건**
1. `0 <= nums.length <= 10^5``
2. `-10^9 <= nums[i] <= 10^9` 

![](https://velog.velcdn.com/images/tlsgn8483/post/f7bbe9c0-1aa4-41c2-82ae-553b77f664f6/image.png)

![](https://velog.velcdn.com/images/tlsgn8483/post/1a080f34-ddf5-4976-995f-f232264f5196/image.png)



**코드구현**
```

# 중요!~
def longestConsecutive(nums):
    longest = 0
    num_dict = {}
    for num in nums:
        num_dict[num] = True
    for num in num_dict:
        if num - 1 not in num_dict:
            cnt = 1
            target = num + 1
            while target in num_dict:
                target += 1
                cnt += 1
            longest = max(longest, cnt)
    return longest

print(longestConsecutive([6,7,100, 5, 4, 4]))
```

![](https://velog.velcdn.com/images/tlsgn8483/post/9f5e5008-79b5-41dc-83cf-04469dd232ca/image.png)


참조 : [개발남노씨 코딩테스트]([https://www.nossi.dev/interview/cs/dsa](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%85%EB%AC%B8-%ED%8C%8C%EC%9D%B4%EC%8D%AC/dashboard))
