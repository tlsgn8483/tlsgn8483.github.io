---
title: "[Spring Framework] Singleton Pattern & Singleton Container"
excerpt: "Singleton Pattern & Singleton Container"

categories:
  - Categories6
tags:
  - [tag1, tag2]

permalink: /categories6/Basic_Spring_7/

toc: true
toc_sticky: true

date: 2023-01-23
last_modified_at: 2023-01-23
---

## Singleton Pattern

### 기본적인 싱글톤 패턴
- 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴
- 객체 인스턴스를 2개 이상 생성하지 못하도록 막는다.
  - private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야 한다.
![](https://velog.velcdn.com/images/tlsgn8483/post/a1cd3042-1c67-4838-bcde-3f00aa10bbf3/image.png)

1. static 영역에 객체 instance를 미리 하나 생성해서 올려둔다.
2. 객체 인스턴스가 필요하면 오직 `getInstance()` 메서드를 통해서만 조회할 수 있다. 이 메서드를 호출하면 항상 같은 인스턴스를 반환한다.
3. 딱 1개의 객체 인스턴수만 존재해야 하므로, 생성자를 private로 막아서, 혹시라도 외부에서 new 키워드로 객체 인스턴스가 생성되는 것을 막는다. 

![](https://velog.velcdn.com/images/tlsgn8483/post/f031d518-2c08-4cb0-a11f-0a5005a83420/image.png)

- private로 new 키워드를 막아두었다.
- 호출할 때마다 같은 객체 인스턴스를 반환하는 것을 확인할 수 있다.

> 싱글톤 패턴의 문제점
- 싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.
- 의존관계상 클라이언트가 구체 클래스에 의존한다.(DIP를 위반한다.)
- 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
- 테스트하기 어렵다. 
- 내부 속성을 변경하거나 초기화하기 어렵다.
- private 생성자로 자식 클래스를 만들기 어렵다.
- 유연성이 떨어진다.

## 싱글톤 컨테이너

> 스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서, 객체 인스턴스를 싱글톤으로 관리한다.
스프링 빈이 바로 싱글톤으로 관리되는 빈이다.

### 싱글톤 컨테이너
- 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 싱글톤 객체를 생성하고 관리하는 기능을 `싱글톤 레지스트리`라 한다.
- 싱글톤의 이러한 기능 덕분에 싱글톤의 모든 단점을 해결하면서 싱글톤으로 유지할 수 있다.
  - 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다.
  - DIP, OCP, 테스트, private 생성자로부터 자유롭게 싱글톤을 사용할 수 있다.

```
@Test
@DisplayName("스프링 컨테이너와 싱글톤")
  void springContainer() {
      ApplicationContext ac = new
  AnnotationConfigApplicationContext(AppConfig.class);

//1. 조회: 호출할 때 마다 같은 객체를 반환
      MemberService memberService1 = ac.getBean("memberService",
  MemberService.class);

//2. 조회: 호출할 때 마다 같은 객체를 반환
       MemberService memberService2 = ac.getBean("memberService",
  MemberService.class);
//참조값이 같은 것을 확인
System.out.println("memberService1 = " + memberService1); System.out.println("memberService2 = " + memberService2);
      //memberService1 == memberService2
      assertThat(memberService1).isSameAs(memberService2);
  }
```

![](https://velog.velcdn.com/images/tlsgn8483/post/575b73d5-2927-4349-8ab6-e66cecdd9587/image.png)

- 스프링 컨테이너 덕분에 고객의 요청이 올 때마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 재사용할 수 있다.

### 싱글톤 방식의 주의점
- 싱글톤 패턴이든, 스프링 같은 싱글톤 컨테이너를 사용하던, 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다.
- 무상태(stateless)로 설계해야한다.
  - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
  - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
  - 가급적 읽기만 가능해야 한다.
  - 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
- 스프링 빈의 필드에 공유 값을 설정하면 큰 장애가 발생할 수 있다!

참조 : [Spring 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
