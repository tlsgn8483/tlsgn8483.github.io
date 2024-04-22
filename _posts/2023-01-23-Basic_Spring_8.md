---
title: "[Spring Framework] Component Scan"
excerpt: "Component Scan"

categories:
  - Spring
tags:
  - [tag1, tag2]

permalink: /Spring/Basic_Spring_8/

toc: true
toc_sticky: true

date: 2023-01-23
last_modified_at: 2023-01-23
---

### Component Scan 

> 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공
의존관계도 자동으로 주입하는 `@Autowired`라는 기능도 제공

```
package hello.core;
  import org.springframework.context.annotation.ComponentScan;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.context.annotation.FilterType;
  import static org.springframework.context.annotation.ComponentScan.*;
  
  @Configuration
  @ComponentScan(
  excludeFilters = @Filter(type = FilterType.ANNOTATION, classes =
    Configuration.class))
    public class AutoAppConfig {
}
```
- 컴포턴트 스캔을 사용하려면 먼저 `@ComponentScan`을 설정 정보에 붙여주면 된다.
- 기존의 AppConfig와 다르게 @Bean으로 등록한 클래스가 하나도 없다.

> 컴포넌트 스캔은 이름 그대로 `@Component`애노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다. `@Component`를 붙여주자.

- 예시

```
   @Component
   public class MemberServiceImpl implements MemberService {
      
      private final MemberRepository memberRepository;
      
      @Autowired
      public MemberServiceImpl(MemberRepository memberRepository) {
          this.memberRepository = memberRepository;
      }
}
```

- Appconfig에서는 `@Bean`으로 직접 설정 정보를 작성했고, 의존관계도 직접 명시했다. 현재는 이런 설정 정보 자체가 없기 때문에 의존관계 주입도 클래스 안에서 해결해야 한다.
- `@Autowired`는 의존관계를 자동으로 주입해준다. 

```
@Component
public class OrderServiceImpl implements OrderService {
      private final MemberRepository memberRepository;
      private final DiscountPolicy discountPolicy;

@Autowired
public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
  discountPolicy) {
          this.memberRepository = memberRepository;
          this.discountPolicy = discountPolicy;
      }

```
- `@Autowired`를 사용하면 생성자에서 여러 의존관계도 한번에 주입받을 수 있다. 

```
  import hello.core.AutoAppConfig;
  import hello.core.member.MemberService;
  import org.junit.jupiter.api.Test;
  import org.springframework.context.ApplicationContext;
  import
  org.springframework.context.annotation.AnnotationConfigApplicationContext;
  import static org.assertj.core.api.Assertions.*;
  
  public class AutoAppConfigTest {
      
      @Test
      void basicScan() {
      ApplicationContext ac = new
  AnnotationConfigApplicationContext(AutoAppConfig.class);
          
          MemberService memberService = ac.getBean(MemberService.class);
          assertThat(memberService).isInstanceOf(MemberService.class);
      
      }
}

```
- `AnnotationConfigApplicationContext`를 사용하는 것은 기존과 동일하다.
- 설정 정보로 `AutoConfig`클래스를 넘겨준다.

결과 
```
ClassPathBeanDefinitionScanner - Identified candidate component class:
  .. RateDiscountPolicy.class
  .. MemberServiceImpl.class
  .. MemoryMemberRepository.class
  .. OrderServiceImpl.class
```

![](https://velog.velcdn.com/images/tlsgn8483/post/25e9fbc6-1aaf-4b82-807a-2b72dc9d3b68/image.png)


- `@ComponentScan` 은 `@Component` 가 붙은 모든 클래스를 스프링 빈으로 등록한다. 
- 이때 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용한다. 
  - 빈 이름 기본 전략: MemberServiceImpl 클래스 memberServiceImpl
  - 빈 이름 직접 지정: 만약 스프링 빈의 이름을 직접 지정하고 싶으면    `@Component("memberService2")` 이런식으로 이름을 부여하면 된다.

![](https://velog.velcdn.com/images/tlsgn8483/post/8046bb71-cdd4-451e-b8ca-d79caf52c3c9/image.png)

- 생성자에 `@Autowired`를 지정하면, 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다. 
- 이때 기본 조회 전략은 타입이 같은 빈을 찾아서 주입한다.
  - `getBean(MemberRepository.class)` 와 동일하다고 이해하면 된다.

참조 : [Spring 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
