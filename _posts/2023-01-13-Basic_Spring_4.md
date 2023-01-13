---
title: "[Spring Framework] 비즈니스 요구사항과 설계2"
excerpt: "회원, 주문과 할인정첵 설계2"

categories:
  - Categories6
tags:
  - [tag1, tag2]

permalink: /categories6/Basic_Spring_4/

toc: true
toc_sticky: true

date: 2023-01-13
last_modified_at: 2023-01-13
---

# 관심사의 분리

애플리케이션을 하나의 공연이라 생각해보자. 각각의 인터페이스를 배역(배우 역할)이라 생각하자. 그런데! 실제 배역 맞는 배우를 선택하는 것은 누가 하는가?

로미오와 줄리엣 공연을 하면 로미오 역할을 누가 할지 줄리엣 역할을 누가 할지는 배우들이 정하는게 아니다. 이전 코드는 마치 로미오 역할(인터페이스)을 하는 레오나르도 디카프리오(구현체, 배우)가 줄리엣 역할(인터페이스)을 하는 여자 주인공(구현체, 배우)을 직접 초빙하는 것과 같다. 디카프리오는 공연도 해야하고 동시에 여자 주인공도 공연에 직접 초빙해야 하는 다양한 책임을 가지고 있다.

## 관심사를 분리하자
`배우`는 본인의 역할인 배역을 수행하는 것에만 집중해야 한다.
디카프리오는 어떤 여자 주인공이 선택되더라도 똑같이 공연을 할 수 있어야 한다.
공연을 구성하고, 담당 배우를 섭외하고, 역할에 맞는 배우를 지정하는 책임을 담당하는 별도의 `공연 기획자`가 나올시점이다.
공연 기획자를 만들고, 배우와 공연 기획자의 책임을 확실히 분리하자.

### AppConfig 등장
- 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, 구현 객체를 생성하고, 연결하는 책임을 가지는 별도의 설정 클래스를 만들자.

- Appconfig
![](https://velog.velcdn.com/images/tlsgn8483/post/4e20d7b2-535a-4b52-b390-e7f83fb3438b/image.png)

- AppConfig는 애플리케이션의 실제 동작에 필요한 `구현 객체를 생성`한다.
  - MemberServiceImpl
  - MemoryMemberRepository
  - OrderServiceImpl
  - FixDiscountPolicy
- AppConfig는 생성한 객체 인스턴스의 참조(레퍼런스)를 `생성자를 통해서 주입(연결)`해준다.
  - MemberServiceImpl => MemoryMemberRepository
  - OrderServiceImpl => MemoryMemberRepository , FixDiscountPolicy

### AppConfig 실행

- 사용클래스 - MemberApp
![](https://velog.velcdn.com/images/tlsgn8483/post/cbd69b56-dc1e-4055-a0dd-6912ef72986d/image.png)


- 사용클래스 - OrderApp 
![](https://velog.velcdn.com/images/tlsgn8483/post/3f6dfdb7-b069-42bb-bd2e-b96e0241258b/image.png)


## 정리
- App Config를 통해서 관심사를 확실하게 분리했다.
- 배역, 배우를 생각해보자
- AppConfig는 공연 기획자다.
- AppConfig는 구체 클래스를 선택한다. 배역에 맞는 담당 배우를 선택한다. 애플리케이션이 어떻게 동작해야할지 전체 구성을 책임진다.
- 이제 각 배우들은 담당 기능을 실행하는 책임만 지면 된다. 


## AppConfig Refactoring
- 현재 AppConfig를 보면 `중복`이 있고, `역할`에 따른 `구현`이 잘 안보인다. 

- 기대하는 그림

![](https://velog.velcdn.com/images/tlsgn8483/post/ff8b4660-cd51-45a2-a673-3ec07a13ff36/image.png)

- 리팩터링 전
![](https://velog.velcdn.com/images/tlsgn8483/post/d0397a55-7517-4df1-a5bf-4b8ea2280829/image.png)

- 리팩터링 후
![](https://velog.velcdn.com/images/tlsgn8483/post/4b2934aa-c892-488b-8a33-01ff33e1f49b/image.png)

- `new MemoryMemberRepository()` 이 부분이 중복 제거되었다. 이제 `MemoryMemberRepository` 를 다른 구현체로 변경할 때 한 부분만 변경하면 된다.


### 새로운 구조와 할인 정책 적용
- AppConfig의 등장으로 애플리케이션이 크게 사용 영역과, 객체를 생성하고 구성하는 영역으로 분리되었다.
![](https://velog.velcdn.com/images/tlsgn8483/post/83f74608-f670-4e3b-8ec8-814e0c72df6e/image.png)

- 할인 정책의 변경 시, 
![](https://velog.velcdn.com/images/tlsgn8483/post/7528ad2d-443c-4b04-8102-a05461558bab/image.png)

- FixDiscountPolicy => RateDiscountPolicy 로 변경해도 구성 영역만 영향을 받고, 사용 영역은 전혀 영향을 받지 않는다.

```

public class AppConfig {
public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
}
    public OrderService orderService() {
        return new OrderServiceImpl(
                memberRepository(),
                discountPolicy());
}
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
}
    public DiscountPolicy discountPolicy() {
          return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

- `AppConfig` 에서 할인 정책 역할을 담당하는 구현을 FixDiscountPolicy => RateDiscountPolicy 객체로 변경했다.
- 이제 할인 정책을 변경해도, 애플리케이션의 구성 역할을 담당하는 AppConfig만 변경하면 된다. 클라이언트 코드인 OrderServiceImpl 를 포함해서 사용 영역의 어떤 코드도 변경할 필요가 없다.
- 구성 영역은 당연히 변경된다. 구성 역할을 담당하는 AppConfig를 애플리케이션이라는 공연의 기획자로 생각하자. 공연 기획자는 공연 참여자인 구현 객체들을 모두 알아야 한다.


## 관심사의 분리
- 애플리케이션을 하나의 공연으로 생각
- 기존에는 클라이언트가 의존하는 서버 구현 객체를 직접 생성하고, 실행함
- 비유를 하면 기존에는 남자 주인공 배우가 공연도 하고, 동시에 여자 주인공도 직접 초빙하는 다양한 책임을 가지고 있음
- 공연을 구성하고, 담당 배우를 섭외하고, 지정하는 책임을 담당하는 별도의 `공연 기획자`가 나올 시점
공연 기획자인 AppConfig가 등장
- AppConfig는 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, `구현 객체를 생성`하고, `연결`하는 책임
- 이제부터 클라이언트 객체는 자신의 역할을 실행하는 것만 집중, 권한이 줄어듬(책임이 명확해짐)

## AppConfig 리팩터링
- 구성 정보에서 역할과 구현을 명확하게 분리 역할이 잘 드러남
- 중복 제거


## 새로운 구조와 할인 정책 적용
- 정액 할인 정책 =>  정률% 할인 정책으로 변경
- AppConfig의 등장으로 애플리케이션이 크게 `사용영역`과, 객체를 생성하고 `구성(Configuration)`하는 영역으로 분리
- 할인 정책을 변경해도 AppConfig가 있는 구성 영역만 변경하면 됨, 사용 영역은 변경할 필요가 없음. 물론 클라이언트 코드인 주문 서비스 코드도 변경하지 않음

참조 : [Spring 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
