---
title: "[Redis] Redis 특수명령어"
excerpt: "Redis 특수명령어"

categories:
  - Redis
tags:
  - [tag1, tag2]

permalink: /Redis/Redis_intro2/

toc: true
toc_sticky: true

date: 2024-04-19
last_modified_at: 2024-04-19
---

> Redis 특수명령어

데이터 만료
- Expiration 데이터를 특정시간 이후에 만료 시키는 기능
- TTL(Time To Live) 데이터가 유효한 시간(초 단위)
- 특징 데이터 조회 요청시에 만료된 데이터는 조회되지 않음
- 데이터가 만료되자마자 삭제하지 않고, 만료로 표시했다가 백그라운드에서 주기적으로 삭제

```
명령어 
$ SET greeting hello
$ EXPIRE greeting 10
$ TTL greeting
$ GET greeting
$ SETEX greeting 10 hello
```

SET NX/XX
- NX : 해당 Key가 존재하지 않는 경우에만 SET
- XX : 해당 Key가 이미 존재하는 경우에만 SET
- Null Reply :  SET이 동작하지 않은 경우 (nil) 응답

```
명령어 
$ SET greeting hello NX
$ SET greeting hello XX
```

Pub/Sub

<img width="761" alt="스크린샷 2024-04-19 오후 2 41 11" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/dc9d865d-044b-4bdc-844b-0803b1d59998">

- Pub/Sub : Publisher와 Subscriber가 서로 알지 못해도 통신이 가능하도록 decoupling 된 패턴 Publisher는 Subscriber에게 직접 메시지를 보내지 않고, Channel에 Publish Subscriber는 관심이 있는 Channel을 필요에 따라 Subscribe하며 메시지 수신

- vs. Stream : 메시지가 보관되는 Stream과 달리 Pub/Sub은 Subscribe 하지 않을 때 발행된 메시지 수신 불가

Pipeline

<img width="645" alt="image" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/1f31d525-1449-48e8-a524-4514598c540d">

- Pipelining : 다수의 commands를 한 번에 요청하여 네트워크 성능을 향상 시키는 기술 Round-Trip Times 최소화, 대부분의 클라이언트 라이브러리에서 지원
- Round-Trip Times : Request / Response 모델에서 발생하는 네트워크 지연 시간

Transaction

- Transaction : 다수의 명령을 하나의 트랜잭션으로 처리 -> 원자성(Atomicity) 보장, 중간에 에러가 발생하면 모든 작업 Rollback 하나의 트랜잭션이 처리되는 동안 다른 클라이언트의 요청이 중간에 끼어들 수 없음
- Atomicity vs Pipeline 기술 : All or Nothing / 모든 작업이 적용되거나 하나도 적용되지 않거나, Pipeline은 네트워크 퍼포먼스 향상을 위해 여러개의 명령어를 한 번에 요청, Transcation은 작업의 원자성을 보장하기 위해 다수의 명령을 하나처럼 처리하는 Pipeline과 Transaction을 동시에 사용 가능

```
명령어
$ MULTI
$ INCR foo
$ DISCARD
$ EXEC
```

One-Time Password
One-Time Password 인증을 위해 사용되는 임시 비밀번호(e.g. 6자리 랜덤 숫자)

<img width="949" alt="스크린샷 2024-04-19 오후 2 46 21" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/3b489a48-afb2-4372-988b-9765a049611a">

Distributed Lock
Distributed Lock 분산 환경의 다수의 프로세스에서 동일한 자원에 접근할 때, 동시성 문제 해결

<img width="927" alt="스크린샷 2024-04-19 오후 2 46 55" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/1f82bad5-fad1-4ab6-9ab4-2ca3d0eb00f9">

Rate Limiter
- Rate Limiter : 시스템 안정성/보안을 위해 요청의 수를 제한하는 기술 IP-Based, User-Based, Application-Based, etc.
- Fixed-window Rate Limiting : 고정된 시간(e.g. 1분) 안에 요청 수를 제한하는 방법

<img width="979" alt="image" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/bd8e50c7-259b-4364-8d71-e0f1628a9388">

SNS Activity Feed
- Activity Feed : 사용자 또는 시스템과 관련된 활동이나 업데이트를 시간순으로 정렬하여 보여주는 기능
- Fan-Out : 단일 데이터를 한 소스에서 여러 목적지로 동시에 전달하는 메시징 패턴

<img width="1124" alt="image" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/f498ef6d-ce9c-4502-ac32-9fbf75630a4d">

Shopping Cart
- Shopping Cart : 사용자가 구매를 원하는 상품을 임시로 모아두는 가상의 공간
- 특징 수시로 변경이 발생할 수 있고, 실제 구매로 이어지지 않을 수도 있다
  
<img width="894" alt="스크린샷 2024-04-19 오후 2 49 18" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/af966f2c-62f4-4433-9897-04e42baae73b">

Login Session
- Login Session :동시 로그인 제한 제한
- 사용자의 로그인 상태를 유지하기 위한 기술
- 로그인시 세션의 개수를 제한하여, 동시에 로그인 가능한 디바이스 개수

<img width="1100" alt="스크린샷 2024-04-19 오후 2 50 05" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/0493ab80-aaaf-42ce-9430-2e5fff78b82c">

Sliding Window Rate Limiter
- Sliding Window Rate Limiter vs. Fixed Window : 시간에 따라 Window를 이동시켜 동적으로 요청수를 조절하는 기술
- Fixed Window는 window 시간마다 허용량이 초기화 되지만, Sliding Window는 시간이 경과함에 따라 window가 같이 움직인다.

<img width="1102" alt="스크린샷 2024-04-19 오후 2 51 17" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/cdfcb6c5-1450-448c-8671-7c7ff5a3f831">

Geofencing
- Geofencing : 위치를 활용하여 지도 상의 가상의 경계 또는 지리적 영역을 정의하는 기술
  
```
명령어
$ GEOADD gang-nam:burgers 127.025705 37.501272 five-guys 127.025699 37.502775 shake-shack 127.028747 37.498668 mc-donalds 127.027531 37.498847 burger-king
$ GEORADIUS gang-nam:burgers 127.027583 37.497928 0.5 km
```

<img width="394" alt="스크린샷 2024-04-19 오후 2 52 20" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/60013210-dae7-41f3-9661-bdfdaf564c6a">

Online Status
- Online Status : 사용자의 현재 상태를 표시하는 기능

특징
실시간성을 완벽히 보장하지는 않는다. 수시로 변경되는 값이다.

<img width="841" alt="스크린샷 2024-04-19 오후 2 53 16" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/3ed77c91-7438-4e0c-8ed2-f40d525f5001">

Visitors Count
- Visitors Count Approximation :  방문자 수(또는 특정 횟수)를 대략적으로 추정하는 경우 정확한 횟수를 셀 필요 없이 대략적인 어림치만 알고자 하는 경우

```
명령어
$ PFADD today:users user:1:1693494070 user:1:1693494071 user:2:1693494071
$ PFCOUNT today:users
```

<img width="490" alt="스크린샷 2024-04-19 오후 2 54 37" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/22fb9573-8643-4af5-897e-bd7cb9f15567">

Unique Events
- Unique Events : 동일 요청이 중복으로 처리되지 않기 위해 빠르게 해당 item이 중복인지 확인하는 방법

<img width="1087" alt="스크린샷 2024-04-19 오후 2 55 23" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/b922a679-a695-4a57-91e3-4b391fa5bd55">

> Redis 사용시 주의사항

O(N) 명령어 주의사항  : O(N) 명령어 대부분의 명령어는 O(1) 시간복잡도를 갖지만, 일부 명령어의 경우 O(N)
- Redis는 Single Thread로 명령어를 순차적으로 수행하기 때문에, 오래 걸리는 O(N) 명령어 수행시, 전체적인 어플리케이션 성능 저하

```
예시 KEYS : 지정된 패턴과 일치하는 모든 키 key 조회, Production 환경에서 절대 사용 금지 -> SCAN 명령어로 대체
SMEMBERS : Set의 모든 member 반환(N = Set Cardinality)
HGETALL :  Hash의 모든 field 반환(N = Size of Hash) 
SORT :  List, Set, ZSet의 item 정렬하여 반환
```
Thundering Herd Problem
- Thundering Herd 병렬 요청이 공유 자원에 대해서 접근할 때, 급격한 과부하가 발생하는 문제

<img width="897" alt="스크린샷 2024-04-19 오후 2 57 32" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/c60ddb99-fd18-448e-98bb-583a62fc5427">

Stale Cache Invalidation
- Cache Invalidation : 캐시의 유효성이 손실되었거나 변경되었을 때, 캐시를 변경하거나 삭제하는 기술
- Two Hard Things : “There are only two hard things in Computer Science: cache invalidation and naming things”

<img width="747" alt="스크린샷 2024-04-19 오후 2 58 22" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/7eaa3f50-e78d-4c52-ab07-2de512de57ba">
 
참조 : [Redis]([https://inf.run/BQH4z])
