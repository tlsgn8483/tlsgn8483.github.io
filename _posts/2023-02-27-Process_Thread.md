---
title: "[Coding_Test] Process & Thread"
excerpt: "프로세스와 쓰레드에 대한 설명"

categories:
  - operating_system
tags:
  - [tag1, tag2]

permalink: /operating_system/Process_Thread/

toc: true
toc_sticky: true

date: 2023-02-27
last_modified_at: 2023-02-27
---


# Process & Thread

## process란?
> 🍯 실행파일(program)이 memory에 적재되어 CPU를 할당받아 실행되는 것을 process라 한다. 

![](https://velog.velcdn.com/images/tlsgn8483/post/1b95d7e1-8bd7-44cd-b4ce-cea851a36635/image.png)

## Memory에 적재
- memory는 CPU가 직접 접근할 수 있는 컴퓨터 내부의 기억장치.
- program이 CPU에서 실행되려면 해당 내용이 memory에 적재된 상태여야만 함
- 프로세스에 할당되는 memory 공간은 Code, Data, Stack, Heap 4개의 영역으로 이루어져 있으며, 각 process마다 독립적으로 할당

![](https://velog.velcdn.com/images/tlsgn8483/post/07befd7f-f830-4049-a2fc-2ac32b7585dc/image.png)

| Code 영역 | 실행한 프로그램의 코드가 저장되는 메모리 영역 |
| --- | --- |
| Data 영역 | 프로그램의 전역 변수와 static 변수가 저장되는 메모리 영역 |
| Heap 영역 | 프로그래머가 직접 공간을 할당(malloc)/해제(free) 하는 메모리 영역 |
| Stack 영역 | 함수 호출 시 생성되는 지역 변수와 매개 변수가 저장되는 임시 메모리 영역 |

## CPU의 연산과 PC register

- 프로그램의 코드를 토대로 CPU가 실제로 연산을 해야 실행된다고 볼 수 있음
- 코드 읽는 것을 정하는 것은, CPU 내부에 있는 Program Counter Register에 저장되어 있음.
- PC Register에 다음에 실행될 코드 주소값이 저장되어 있으며, 순차적으로 PC Register가 가리키게 되고, 해당 명령어들을 읽어와 CPU가 연산을 하게 되면, 프로세스가 실행 되는 것. 

## Multi process란?
- Multi process란 2개 이상의 process가 동시에 실행되는 것을 말합니다. 동시에라는 말은 동시성(concurrency)과 병렬성(parallelism) 두 가지를 의미한다.
- 동시성은 CPU core가 1개일 때, 여러 process를 짧은 시간동안 번갈아 가면서 연산을 하게 되는 시분할 시스템(time sharing system)으로 실행되는 것
- 병렬성은 CPU core가 여러개일 때, 각각의 core가 각각의 process를 연산함으로써 process가 동시에 실행되는 것

예시 
> EX) 노트북 여러개의 CPU Core = 여러 프로세스가 동시에 처리되는 것을 병렬성이라 함.
동시성 = 한개의 CPU core에서 mulit process가 작동되는 원리

## 동시성(Concurrency) vs 병렬성(Parallelism)
| 동시성 | 병렬성 |
| --- | --- |
| Single core | Multi core |
| 동시에 실행되는 것 같아 보인다. | 실제로 동시에 여러 작업이 처리 된다. |

## Single core의 동시성
### Multi process
- M2개 이상의 process가 동시에 실행, 이 때 process들은 CPU와 메모리를 공유한다.
- Memory 경우, 여러 Process들이 각자의 메모리 영역을 차지하여 동시에 적재
- 하나의 CPU는 매순간 하나의 Process만 연산할 수 있으나, CPU 처리속도가 빨라서, 수 ms 이내의 짧은 시간동안 여러 프로세스들이 CPU에서 번갈아 실행되기 때문에, 사용자 입장에서는 여러 프로그램이 동시에 실행되는 것처럼 보임.
- CPU의 작업시간을 여러 프로세스들이 조금씩 나누어 쓰는 시스템을 시분할 시스템이라 불림.

### 메모리 관리
- 여러 프로세스가 동시에 메모리에 적재된 경우, 서로 다른 프로세스의 영역을 침범하지 않도록 각 프로세스가 자신의 메모리영역에만 접근하도록 운영체제가 관리.
![](https://velog.velcdn.com/images/tlsgn8483/post/5f5cb281-a13c-47e7-88ac-ac6c89dc6385/image.png)

### CPU의 연산과 PC register
- CPU는 PC(Program Counter) register가 가리키고 있는 명령어를 읽어, 연산을 진행한다.
- PC Register에는 다음에 실행될 명령어의 주소값이 저장되어 있음.

> `멀티프로세스 시스템`에서 Process1 진행되고 있을때는 process1 code 영역을 PC Register가 가리키다가, Process2가 진행되면 Process2의 code영역을 가리키게 된다. CPU는 PC Register가 가리키는 곳에 따라 Process를 변경해가면서, 명령어를 읽어들이고, 연산을 하게 된다.

### Context
- 시분할 시스템에서는 한 프로세스가 매우 짧은 시간동안 CPU를 점유하면서, 일정부분의 명령을 수행하고, 다른 프로세스에게 넘긴다. 차례가 되면 다시 CPU를 점유하여, 명령을 수행
- 이전에서 어디까지 명령을 수행하고, Register에는 어떤 값이 저장되어 있는지에 대한 정보가 필요
- 프로세스가 현재 어떤 상태로 수행되고 있는지에 대한 정보가 `Context`
- Context정보들은 PCB(Process Control Block)에 저장

### PCB(Process Control Block)
- 운영체제가 프로세스를 표현한 자료구조
- 프로세스의 중요한 정보를 포함(보호된 메모리 영역안에 저장)
- 일부 운영체제 에서는 커널 스택에 위치(보호받으면서, 접근하기 쉬움)

PCB는 아래와 같은 정보가 포함되어 있음

| PCB |  |
| --- | --- |
| Process State | new, running, waiting, halted 등의 state가 있다. |
| Process Number | 해당 process의 number |
| Program counter(PC) | 해당 process가 다음에 실행할 명령어의 주소를 가리킨다 |
| Registers | 컴퓨터 구조에 따라 다양한 수와 유형을 가진 register 값들 |
| Memory limits | base register, limit register, page table 또는 segment table 등 |

![](https://velog.velcdn.com/images/tlsgn8483/post/8da8c15e-6bd1-4c09-b608-db68360789e1/image.png)

### Context switch
- Context switch란 한 프로세스에서 다른 프로세스로 CPU 제어권을 넘겨주는 것
- 이전의 프로세스의 상태를 PCB에 저장하여 보관하고 새로운 프로세스의 PCB를 읽어서 보관된 상태를 복구하는 작업이 이루어진다.
![](https://velog.velcdn.com/images/tlsgn8483/post/283fcb92-1de0-49db-b732-c5a4d519dc8f/image.png)


## ⭐⭐ Multi thread란?
- thread는 한 process 내에서 실행되는 동작(기능 function)의 단위
-  각 thread는 속해있는 process의 Stack 메모리를 제외한 나머지 memory 영역을 공유할 수 있다
- Multi thread란 하나의 process가 동시에 여러개의 일을 수행할수 있도록 해주는 것
- 하나의 process에서(실행이 된 하나의 program에서) 여러 작업을 병렬로 처리하기 위해 multi thread를 사용한다.
- multi thread에서는 한 process 내에 여러 개의 thread가 있고, 각 thread들은 Stack 메모리를 제외한 나머지 영역(Code, Data, Heap) 영역을 공유한다.

> Thread는 Process내에서 독립적인 기능을 수행한다. 독립적으로 함수를 호출함을 의미하고, Stack memory가 각자 필요함. Thread가 무엇인지 이해하고, multi proess와 어떤점이 다른지를 생각해보기!
또한, 독립적인 stack memory와 PC Register가 필요하다는 점 기억!

### Thread와 multi thread
- thread는 process 내에서 독립적인 기능을 수행
- 각 thread는 독립적인 기능을 수행한다는 것은 독립적으로 함수를 호출함을 의미

### Stack memory &  PC register
- thread가 함수를 호출하기 위해서는 인자 전달, Return Address 저장, 함수 내 지역변수 저장 등을 위한 독립적인 stack memory 공간을 필요
- thread는 process로부터 Stack memory 영역은 독립적으로 할당받고, Code, Data, Heap 영역은 공유하는 형태
- multi thread에서는 각각의 thread마다 PC register를 가지고 있어야 한다. 
(한 process 내에서도 thread끼리 context switch가 일어나게 되는데, PC register에 code address가 저장되어 있어야 이어서 실행을 할 수 있기 때문)

## ⭐⭐ multi process와 multi thread를 비교
- multi thread는 multi process보다 적은 메모리 공간을 차지하고 `Context Switching`이 빠르다.
- multi process는 multi thread보다 많은 메모리공간과 CPU 시간을 차지
- multi thread는 동기화 문제와 하나의 thread 장애로 전체 thread가 종료될 위험
- multi process는 하나의 process가 죽더라도 다른 process에 영향을 주지 않아 안정성이 높음

>두 방법은 동시에 여러 작업을 수행한다는 측면에서 유사한 면이 있음.
적용할 시스템에 따라 두 방법의 장단점을 고려하여 적합한 방식을 선택
메모리 구분이 필요할때 = Multi Process가 유리
Context swichin 빈번, 데이터 공유 빈번, 자원 효율적 활용일 경우 = Multi Thread

### Multi process & Multi thread
- multi process대신 multi thread로 구현할 경우, 메모리 공간과 시스템 자원 소모가 줄어든다.
(하지만 멀티 스레드를 사용할 때에는, 스레드간 자원을 공유하기 때문에, 동기화 문제가 발생할 수 있기 때문에, 프로그램 설계가 필요)
- 프로세스간 통신(IPC)보다 Thread간 통신 비용이 적기 때문에 오버헤드가 적음.

|  | 메모리 사용 / CPU 시간 | Context switching | 안정성 |
| --- | --- | --- | --- |
| multi process | 많은 메모리 공간 / CPU 시간 차지 | 느림 | 높음 |
| multi thread | 적은 메모리 공간 / CPU 시간 차지 | 빠름 | 낮음 |

![](https://velog.velcdn.com/images/tlsgn8483/post/db60934a-56cd-456d-b855-c4569d9d8888/image.png)

참조 : [개발남노씨 운영체제]([[https://www.nossi.dev/interview/cs/dsa](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%85%EB%AC%B8-%ED%8C%8C%EC%9D%B4%EC%8D%AC/dashboard)](https://www.nossi.dev/interview/cs/os))
