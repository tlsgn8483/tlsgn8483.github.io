---
title: "[Spring Framework] 비즈니스 요구사항과 설계"
excerpt: "회원, 주문과 할인정첵 설계"

categories:
  - Categories6
tags:
  - [tag1, tag2]

permalink: /categories6/Basic_Spring_3/

toc: true
toc_sticky: true

date: 2023-01-13
last_modified_at: 2023-01-13
---

>비즈니스 요구사항과 설계 
- 회원
  - 회원을 가입하고 조회할 수 있다.
  - 회원은 일반과 VIP 두 가지 등급이 있다. 
  - 회원 데이터는 자체 DB를 구축할 수 있고, 외부 시스템과 연동 할 수 있다.
- 주문과 할인정책
  - 회원은 상품을 주문할 수 있다.
  - 회원 등급에 따라 할인정책을 적용할 수 있다.
  - 할인 정책은 모든 VIP는 1000원을 할인해주는 고정 금액 할인을 적용해달라.
  - 할인 정책은 변경 가능성이 높다. 회사에서 기본 할인 정책을 아직 적하지 못했으며, 오픈 직전까지 고민을 미루고 싶은 상황
  
 ## 회원 도메인 설계
 - 회원 도메인 협력 관계 
 ![](https://velog.velcdn.com/images/tlsgn8483/post/ae165afb-4d82-4259-93d4-2a19bd19dd14/image.png)

- 회원 클래스 다이어그램
![](https://velog.velcdn.com/images/tlsgn8483/post/8ea57474-bace-4d8a-aac9-d4f301823a20/image.png)

- 회원 객체 다이어그램
![](https://velog.velcdn.com/images/tlsgn8483/post/76388fc9-0264-4d1a-b16a-86b73fe08653/image.png)


- 주문 도메인 협력, 역할, 책임

![](https://velog.velcdn.com/images/tlsgn8483/post/94f3e649-9454-4d6f-8a04-b9f59873ca0b/image.png)

1. 주문 생성: 클라이언트는 주문 서비스에 주문 생성을 요청한다.
2. 회원 조회: 할인을 위해서는 회원 등급이 필요하다. 그래서 주문 서비스는 회원 저장소에서 회원을 조회한다.
3. 할인 적용 : 주문 서비스는 회원 등급에 따른 할인 여부를 할인 정책에 위임한다.
4. 주문 결과반환 : 주문 서비스는 할인 결과를 포함한 주문 결과를 반환한다. 

- 주문 도메인 전체
![](https://velog.velcdn.com/images/tlsgn8483/post/4e77f97f-5f83-4d3e-89fa-d5fed5946a9b/image.png)

- `역할`과 `구현`을 분리해서 자유롭게 구현 객체를 조립할 수 있게 설계했다. 덕분에 회원 저장소는 물론이고, 할인 정책도 유연하게 변경할 수 있다.

- 주문 도메인 클래스 다이어그램
![](https://velog.velcdn.com/images/tlsgn8483/post/ca905427-6c76-4b9a-aae0-93eee3d8cd51/image.png)

- 주문 도메인 객체 다이어그램
![](https://velog.velcdn.com/images/tlsgn8483/post/3528dade-9e07-47ef-b25a-325057f8734a/image.png)

- 회원을 메모리에서 조회하고, 정액 할인 정책(고정 금액)을 지원해도 주문 서비스를 변경하지 않아도 된다. 역할들의 협력 관계를 그대로 재사용 할 수 있음.

참조 : https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8
