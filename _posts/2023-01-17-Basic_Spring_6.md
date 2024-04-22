---
title: "[Spring Framework] BeanFactory와 ApplicationContext"
excerpt: "BeanFactory와 ApplicationContext"

categories:
  - Spring
tags:
  - [tag1, tag2]

permalink: /Spring/Basic_Spring_6/

toc: true
toc_sticky: true

date: 2023-01-17
last_modified_at: 2023-01-17
---


## BeanFactory와 ApplicationContext

![](https://velog.velcdn.com/images/tlsgn8483/post/beca742f-ec90-4d4b-a937-0aca4e20e74e/image.png)

### BeanFactory
- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈을 관리하고 조회하는 역할을 담당한다.
- `getBean()`을 제공한다.
- 지금까지 사용한 대부분의 기능은 BeanFactor가 제공하는 기능이다.

### ApplicationContext
- BeanFactory 기능을 모두 상속받아서 제공한다.
- 빈을 관리하고, 검색하는 기능을 BeanFactory가 제공해주는데, 둘의 차이를 알아보자
- 애플리케이션을 개발할 때는 빈을 관리하고, 조회하는 기능을 물론이고, 수 많은 부가기능이 필요하다.
![](https://velog.velcdn.com/images/tlsgn8483/post/87ec4883-cf94-4d99-b422-e3a156c4a612/image.png)

- 메시지소스를 활용한 국제화 기능
  - 예를 들어, 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
- 환경변수
  - 로컬, 개발, 운영등을 구분해서 처리
- 애플리케이션 이벤트
  - 이벤트를 발행하고, 구독하는 모델을 편리하게 지원
- 편리한 리소스 조회
  - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회
  
## 정리
 - ApplicationContext는 BeanFactory의 기능을 상속받는다.
 - ApplicationContext는 빈 관리기능 + 편리한 부가 기능을 제공한다.
 - BeanFactory를 직접 사용할 일은 거의 없다. 부가기능이 포함된 ApplicationContext를 사용한다.
 - BeanFactory나 ApplicationContext를 스프링 컨테이너라 한다.
 
## 다양한 설정 형식 지원 - 자바 코드, XML
- 스프링 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있게 유연하게 설계되어 있다.
  - 자바코드, XML, Groovy
  ![](https://velog.velcdn.com/images/tlsgn8483/post/cd18340b-8b30-449b-815e-f4b28042dc3b/image.png)


### 애노테이션 기반 자바 코드 설정 사용
- `new AnnotationConfigApplicationContext(AppConfig.class)`
- `AnnotationConfigApplicationContext` 클래스를 사용하면서 자바 코드로된 설정 정보를 넘기면 된다.


### XML 설정 사용
- 최근에는 스프링 부트를 많이 사용하면서 XML기반의 설정은 잘 사용하지 않는다. 아직 많은 레거시
프로젝트 들이 XML로 되어 있고, 또 XML을 사용하면 컴파일 없이 빈 설정 정보를 변경할 수 있는 장점도 있으므로 한번쯤 배워두는 것도 괜찮다.
- `GenericXmlApplicationContext` 를 사용하면서 xml 설정 파일을 넘기면 된다.


### 스프링 빈 설정 메타 정보 - BeanDefinition
- 스프링은 어떻게 이런 다양한 설정 형식을 지원하는 것일까? 그 중심에는 `BeanDefinition` 이라는 추상화가 있다.
- 쉽게 이야기해서 역할과 구현을 개념적으로 나눈 것이다!
  - XML을 읽어서 BeanDefinition을 만들면 된다.
  - 자바 코드를 읽어서 BeanDefinition을 만들면 된다.
  - 스프링 컨테이너는 자바 코드인지, XML인지 몰라도 된다. 오직 BeanDefinition만 알면 된다.
- `BeanDefinition` 을 빈 설정 메타정보라 한다.
  - `@Bean` , `<bean>` 당 각각 하나씩 메타 정보가 생성된다.
- 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성한다.

## 정리
- `BeanDefinition`을 직접 생성해서 스프링 컨테이너에 등록할 수 도 있다. 하지만 실무에서 `BeanDefinition`을 직접 정의하거나 사용할 일은 거의 없다. 
- `BeanDefinition`에 대해서는 너무 깊이있게 이해하기 보다는, 스프링이 다양한 형태의 설정 정보를 `BeanDefinition`으로 추상화해서 사용하는 것 정도만 이해하면 된다.
- 가끔 스프링 코드나 스프링 관련 오픈 소스의 코드를 볼 때, `BeanDefinition` 이라는 것이 보일 때가 있다. 이때 이러한 `메커니즘`을 떠올리면 된다.


참조 : [Spring 핵심원리](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
