---
title: "[Coding_Test] Design_Browser_History"
excerpt: "Design_Browser_History 문제풀이"

categories:
  - Coding_Test
tags:
  - [tag1, tag2]

permalink: /Coding_Test/Design_Browser_History/

toc: true
toc_sticky: true

date: 2023-02-03
last_modified_at: 2023-02-03
---

# Design Browser History
> 
Q. 인터넷 브라우저에서 방문기록과 동일한 작동을 하는 BrowserHistory class를 구현할 것이다. 구현할 브라우저는 Homepage에서 시작하고, 다른 url에 방문할 수 있다. 또, "뒤로가기"와 "앞으로 가기"가 작동하도록 구현해라

- BrowserHistory(string homepage)를 호출하면 브라우저는 homepage에서 시작이 된다.
- void visit(string url)을 호출하면 현재 앞에 있는 페이지의 기록은 다 삭제가 되고, url로 방문을 한다.
- string back(int streps)를 호출하면 steps수 만큼 "뒤로 가기"를 한다. "뒤로 가기"를 할 수 있는 page 개수가 x이고, step > x라면 x번 만큼만 "뒤로가기"를 한다. "뒤로 가기"가 완료되면 현재 url을 return 한다.
- string forword(int steps)을 호출하면 "앞으로 가기"를 한다. "앞으로 가기"를 할 수 있는 page 개수가 x이고, step > x라면 x 번 만큼만 "앞으로 가기"를 한다. "앞으로 가기"가 완료되면 현재 url을 return한다.

**제약조건**
`1 <= homepage.length <=20`
`1 <= url.length <= 20`
`1 <= step <= 100`
`hompage와 url은 '.'를 포함한 lower case 영문자로 되어있다.`
`visit, back 그리고 forword는 최대 5000번의 호출이 있을 수 있다.

![](https://velog.velcdn.com/images/tlsgn8483/post/8b981ed5-ad17-47f4-bc9e-dd66dac890a2/image.png)

코드구현
```
class ListNode(object):
    def __init__(self, val = 0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev

class BrowserHistory(object):
    def __init__(self, homepage):
        self.head = self.current = ListNode(val=homepage)

    def visit(self, url):
        self.current.next = ListNode(val=url, prev=self.current)
        self.current = self.current.next
        return None
    def back(self, steps):
        while steps > 0 and self.current.prev != None:
            steps -= 1
            self.current = self.current.prev
        return self.current.val
    def forward(self, steps):
        while steps >0 and self.current.next != None:
            steps -= 1
            self.current = self.current.next
        return self.current.val


browswerHistory = BrowserHistory("leetcode.com")
browswerHistory.visit("google.com")
browswerHistory.visit("facebook.com")
browswerHistory.visit("youtube.com")
browswerHistory.back(1)
browswerHistory.back(1)
browswerHistory.forward(1)
browswerHistory.visit("linkdin,com")
browswerHistory.forward(2)
browswerHistory.back(2)
browswerHistory.back(7)
```

![](https://velog.velcdn.com/images/tlsgn8483/post/fbc472a2-9f89-4beb-be18-64d5ac639537/image.png)





참조 : [개발남노씨 코딩테스트]([https://www.nossi.dev/interview/cs/dsa](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%85%EB%AC%B8-%ED%8C%8C%EC%9D%B4%EC%8D%AC/dashboard))
