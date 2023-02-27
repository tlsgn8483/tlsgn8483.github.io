---
title: "[operating_system] Multi_Process"
excerpt: "멀티 프로세스환경에서 프로세스간 데이터 연동 및 동기화 문제"

categories:
  - operating_system
tags:
  - [tag1, tag2]

permalink: /operating_system/Multi_Process/

toc: true
toc_sticky: true

date: 2023-02-27
last_modified_at: 2023-02-27
---

# ⭐⭐ multi process환경에서 process간에 데이터를 어떻게 주고 받을까
- 원칙적으로 process는 독립적인 주소 공간을 갖기 때문에, 다른 process의 주소 공간을 참조할 수 없다
- 하지만 경우에 따라 운영체제는 process 간의 자원 접근을 위한 매커니즘인 프로세스 간 통신(IPC, Inter Process Communication)를 제공
- 프로세스 간 통신(IPC) 방법으로는 파이프, 파일, 소켓, 공유메모리 등을 이용한 방법이 있음


>[면접 🍯 TIP]
IPC는 면접에서도 굉장히 자주 나오는 질문
multi thread와 다르게 process끼리는 데이터 공유를 하고 있지 않는다.
따라서 데이터를 주고 받기 위해서 IPC기법을 사용한다.
IPC는 크게 공유메모리 방식과 메시지 전달방식으로 나뉘는데 이 둘의 장단점과 차이점을 중심으로 알아보기! 

## IPC (Inter-Process Communication)
- process는 각자 자신만의 독립적인 주소공간을 가지는데, 다른 process가 이 주소공간을 참조하는 것은 허용하지 않음.
( 그렇기 때문에 다른 process와 데이터를 주고받을 수 없음)
- 이를 해결하고자 운영체제는 IPC기법을 통해 process들 간에 통신을 가능하게 해줌
-  process간 통신(IPC)에는 기본적으로 `공유메모리(shared memory)`와 `메시지 전달(message passing)`의 두 가지 모델

### 공유메모리(shared memory)
- 공유 메모리 방식에서는 process들이 주소 공간의 일부를 공유
- 공유한 메모리 영역에 읽기/쓰기를 통해서 통신을 수행
- process가 공유 메모리 할당을 kernel에 요청하면 kernel은 해당 process에 메모리 공간을 할당
- 공유 메모리 영역이 구축된 이후에는 모든 접근이 일반적인 메모리 접근으로 취급되기 때문에 더이상 kernel의 도움없이도 각 process들이 해당 메모리 영역에 접근할 수 있음.
- 따라서 커널의 관여 없이 데이터를 통신할 수 있기 때문에 `IPC속도가 빠르다는 장점`
- 공유 메모리 방식은 process간의 통신을 수월하게 만들지만 동시에 같은 메모리 위치에 접근하게 되면 일관성 문제가 발생할 수 있음
- 커널이 관여하지 않기 때문에 process들 끼리 직접 공유 메모리 접근에 대한 동기화 문제를 책임져야 함.

![](https://velog.velcdn.com/images/tlsgn8483/post/fdaeb49a-3a2e-4579-9f14-1aaa4b9334e9/image.png)

### 메시지 전달(message passing)
- 메시지 전달 방법은 통상 system call을 사용하여 구현
- kernel을 통해 send(message)와 receive(message)라는 두 가지 연산을 제공받음
- process1이 kernel로 message를 보내면 kernel이 process2에게 message를 보내주는 방식으로 동작방식
- 메모리 공유보다는 속도가 느리지만, 충돌을 회피할 필요가 없기 때문에 적은양의 데이터를 교환하는 데 유용하며, 구현하기가 쉽다는 장점이 있음
- 대표적인 예시로 pipe, socket, message queue가 있음.

![](https://velog.velcdn.com/images/tlsgn8483/post/c04703aa-ce75-4140-b840-0ebeba757606/image.png)

##  ⭐ Multi process/thread 환경에서 동기화 문제 해결방법
- 동기화문제를 해결하기 위해 mutex, semaphore 기법사용
- Mutex란 1개의 스레드만이 공유 자원에 접근할 수 있도록 하여, 경쟁 상황(race condition)를 방지하는 기법
  - 공유자원을 점유하는 thread가 lock을 걸면, 다른 thread는 unlock 상태가 될 때까지 해당 자원에 접근할 수 없음
- Semaphore란 S개의 thread만이 공유 자원에 접근할 수 있도록 제어하는 동기화 기법
  - Semaphore 기법에서는 정수형 변수 S(세마포) 값을 가용한 자원의 수로 초기화
  - 자원에 접근할 때는 S-- 연산을 수행하여 세마포 값을 감소시키고 자원을 방출할 때는 S++ 연산을 수행하여 세마포 값을 증가시킴
  - 이 때 세마포 값이 0이 되면 모든 자원이 사용 중임을 의미하고, 이후 자원을 사용하려는 프로세스는 세마포 값이 0보다 커질 때까지 block이 된다.

[면접 🍯 TIP]
> mutex와 semaphore는 면접 질문에서 자주 나오는 용어
이를 설명하기 위해서는 동기화 문제가 무엇이고 왜 발생하는지에 대해서 설명필요. multi process/thread  환경에서는 서로 다른 thread가 메모리 영역을 공유하기 때문에 여러 thread가 동일한 자원에 동시에 접근하여 엉뚱한 값을 읽거나 수정하게 되는 동기화 문제가 발생할 수 있음.
atomic operation과 경쟁상황 살펴보기! 

 
### 동기화 문제
- 동기화 문제란 서로 다른 thread가 메모리 영역을 공유하기 때문에 여러 thread가 동일한 자원에 동시에 접근하여 엉뚱한 값을 읽거나 수정하는 문제

`count++`를 CPU 입장에서 분해해보면 3개의 atomic operations으로 나뉩니다.

1. `count` 변수의 값을 가져온다.
2. `count` 변수의 값을 1 증가시킨다.
3. 변경된 `count` 값을 저장한다.

CPU는 atomic operation을 연산하게 됩니다. 따라서 `count++`을 하기 위해 3번의 연산

시분할 시스템으로 작동하는 multi process/multi thread 시스템에서, 두 개의 thread가 동일한 데이터인 count에 동시에 접근을 하여 조작을 하는 상황을 가정

> thread1에서도 count++를 하고, thread2에서도 count++를 한다면 그 실행 결과가 접근이 발생한 순서에 따라 달라질 수 있음. 이를 경쟁상황(race condition)

- 둘 이상의 thread가 동일한 자원에 접근하여 조작하고, 그 실행 결과가 접근이 발생한 순서에 따라 달라지는 경쟁상황에 의해서 동기화 문제가 발생가능
- 경쟁 상황으로부터 보호하기 위해, 우리는 한 순간에 하나의 process/thread만 해당 자원에 접근하고 조작할 수 있도록 보장해야함. 
- process/thread들이 동기화되도록 할 필요가 있음

![](https://velog.velcdn.com/images/tlsgn8483/post/7b888d8c-e243-41ab-a420-5b0a848217bf/image.png)

### 임계영역(critical section)
- 둘 이상의 process/thread가 동시에 동일한 자원에 접근하도록 하는 프로그램 코드 부분을 의미
- 중요한 특징중 하나는, 한 process/thread가 자신의 임계구역에서 수행하는 동안에는 다른 process/thread들은 그들의 임계구역에 들어갈 수 없어야 한다는 사실
- 임계영역 내의 코드는 원자적으로(atomically) 실행이 되어야 함
- 원자적으로 실행 되기 위해서 각각의 process/thread는 자신의 임계구역으로 진입하려면 진입 허가를 요청해야 함
- entry section이라고 하고, 진입이 허가되면 임계영역을 실행할 수 있음
- 임계영역이 끝나고 나면 exit section으로 퇴출을 하게 된다.
- 임계영역의 원자성을 보장하여 process/thread들이 동기화되도록 할 수 있음

동기화 방법은 대표적으로 Mutex와 Semaphore가 있다.

![](https://velog.velcdn.com/images/tlsgn8483/post/f78a9b80-53b6-4bff-8d9d-ecae08a1a14b/image.png)

### Mutex
- 공유자원에 접근할 수 있는 process/thread의 수를 1개로 제한
- 임계영역을 보호하고, 경쟁상황을 방지하기 위해 mutex lock을 사용
- process/thread는 임계영역에 들어가기 전에 반드시 lock을 획득해야 하고, 임계구역을 빠져나올 때 lock을 반환해야 함
- acquire()함수가 lock을 획득하고 release()함수가 lock을 반환

```
acquire() // entry section

// critical section

release() // exit section
```

busy waiting은 다른 process/thread가 생산적으로 사용할 수 있는 CPU를 낭비한다는 단점

![](https://velog.velcdn.com/images/tlsgn8483/post/4d2d9b85-dc98-4283-a50b-623756eae952/image.png)

### Semaphore

- mutex와 가장 큰 차이점은 공유 자원에 접근할 수 있는 process/thread의 개수가 2개 이상이 될 수 있다는 것
- semaphore 변수 S(세마포)에 동시에 접근 가능한 process/thread의 갯수를 저장
- S가 0보다 크면 임계영역으로 들어갈 수 있고, 임계영역에 들어가면 S값을 1 감소시킴
- S값이 0이 되면 다른 process/thread는 임계영역으로 접근할 수 없음
- 임계영역에서의 작업이 끝나고 임계영역에서 exit하면서 S값을 1 증가시킴


```cpp
wait(S) // entry section

// critical section

signal(S) // exit section
```
semaphore값이 0,1만 가질 수 있는 경우 binary semaphore라고 하는데, 이는 mutex랑 거의 유사하게 작동합

![](https://velog.velcdn.com/images/tlsgn8483/post/56547b28-1f4d-4a49-a81f-cd38a485864b/image.png)

### 교착상태(Deadlock)란?
> 둘 이상의 thread가 각기 다른 thread가 점유하고 있는 자원을 서로 기다릴 때, 무한 대기에 빠지는 상황

deadlock이 발생하는 조건은 상호 배제(mutual exclusion), 점유 대기(hold-and-wait), 비선점(no preemption), 순환 대기(circular wait)입니다. 이 4가지 조건이 동시에 성립할 때 발생.

deadlock 문제를 해결하는 방법에는 무시, 예방, 회피, 탐지-회복의 4가지 방법

#### Deadlock 예시
![](https://velog.velcdn.com/images/tlsgn8483/post/1c227aae-7b03-4764-b233-a6c05fae95ee/image.png)

#### Deadlock 발생 조건

deadlock은 다음 4가지 조건이 동시에 성립할 때 발생가능

1. **상호 배제**(mutual exclusion)
    - 동시에 한 thread만 자원을 점유할 수 있는 상황
    - 다른 thread가 자원을 사용하려면 자원이 방출될 때까지 기다려야 함
2. **점유 대기**(hold-and-wait)
    - thread가 자원을 보유한 상태에서 다른 thread가 보유한 자원을 추가로 기다리는 상황
3. **비선점**(no preemption)
    - 다른 thread가 사용 중인 자원을 강제로 선점할 수 없는 상황
    - 자원을 점유하고 있는 thread에 의해서만 자원이 방출
4. **순환 대기**(circular wait)
    - 대기 중인 thread들이 순환 형태로 자원을 대기하고 있는 상황
    
####   Deadlock 해결 방법

deadlock 문제를 해결하는 방법에는 
(1) 무시, (2) 예방, (3) 회피, (4) 탐지-회복의 4가지 방법

| 기법 | 설명 | 비고 |
| --- | --- | --- |
| 무시 | deadlock 발생 확률이 낮은 시스템에서 아무런 조치도 취하지 않고 deadlock을 무시하는 방법 | - 무시 기법은 시스템 성능 저하가 없다는 큰 장점 - 현대 시스템에서는 deadlock이 잘 발생하지 않고, 해결 비용이 크기 때문에 무시 방법이 많이 사용 |
| 예방 | 교착 상태의 4가지 발생 조건중 하나가 성립하지 않게 하는 방법 | - 순환 대기 조건이 성립하지 않도록 하는 것이 현실적으로 가능한 예방 기법, 자원 사용의 효율성이 떨어지고 비용이 큼 |
| 회피 | thread가 앞으로 자원을 어떻게 요청할지에 대한 정보를 통해 순환 대기 상태가 발생하지 않도록 자원을 할당하는 방법 | - 자원 할당 그래프 알고리즘, 은행원 알고리즘 등을 사용하여 자원을 할당하여 deadlock을 회피 |
| 탐지-회복 | 시스템 검사를 통해 deadlock 발생을 탐지하고, 이를 회복시키는 방법 | - 자원 사용의 효율성이 떨어지고 비용이 큼 |



참조 : [개발남노씨 운영체제]([[https://www.nossi.dev/interview/cs/dsa](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%85%EB%AC%B8-%ED%8C%8C%EC%9D%B4%EC%8D%AC/dashboard)](https://www.nossi.dev/interview/cs/os))
