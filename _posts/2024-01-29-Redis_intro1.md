---
title: "[Redis] Redis 활용방법"
excerpt: "Redis를 알아보고, 활용해보기"

categories:
  - Redis
tags:
  - [tag1, tag2]

permalink: /Redis/Redis_intro1/

toc: true
toc_sticky: true

date: 2024-01-29
last_modified_at: 2024-04-19
---

## 서론
> Redis(Remodt Dictionary Server)

특징
- In Memory : 모든 데이터를 RAM에 저장 (백업 / 스냅샷 제외)
- Single Threaded : 단일 Thread에서 모든 task 처리
- Cluster Mode : 다중 노드에서 데이터를 분산 저장하여 안정성 & 고가용성 제공
- Persistence : RDB(Redis Database) + AOF(Append only file) 통해 영속성 옵션 제공
- PUB/SUB : Pub/Sub 패턴을 지원하여 손쉬운 어플리케이션 개발 

장점
- 높은성능 : 모든 데이터를 메모리에 저장하기 때문에 매우 빠른 읽기/쓰기 속도 보장
- Data Type 지원 : Redis에서 지원하는 Data Type을 잘 활용하여, 다양한 기능 구현
- Client Library : Python, Java, JavaScript 등 다양한 언어로 작성된 클라이언트 라이브러리 지원 
- 다양한 사례 / 강한 커뮤니티 : Redis를 활용하여 비슷한 문제를 해결한 사례가 많고, 커뮤니티 지원받기 쉬움

Redis 사용사례
- Caching : 임시 비밀번호(One-Time Password), 로그인 세션(Session)
- Rate Limiter : Fixed-Window / Sliding-Window Rate Limiter(비율 계산기)
- Message Broker : 메시지 큐(Message Queue)
- 실시간 분석 / 계산 : 순위표, 반경 탐색, 방문자 수 계산
- 실시간 채팅 : PUB/SUB 패턴


Redis 알아보기
Persistence(영속성)
- Redis는 주로 캐시로 사용되지만 데이터 영속성을 위한 옵션 제공 SSD와 같은 영구적인 저장 장치에 데이터 저장

RDB(Redis Database)
- Point-in-time Snapshot -> 재난 복구(Disaster Recovery) 또는 복제에 주로 사용 일부 데이터 유실의 위험이 있고, 스냅샷 생성 중 클라이언트 요청 지연 발생

AOF(Append Only File)
- Redis에 적용되는 Write 작업을 모두 log로 저장
- 데이터 유실의 위험이 적지만, 재난 복구시 Write 작업을 다시 적용하기 때문에 RDB 보다 느림
(RDB + AOF 함께 사용하는 옵션 제공)

캐싱(Caching)
- 데이터를 빠르게 읽고 처리하기 위해 임시로 저장하는 기술
- 계산된 값을 임시로 저장해두고, 동일한 계산 / 요청 발생시 다시 계산하지 않고 저장된 값 바로 사용
- 캐시(Cache) = 임시 저장소

사용 사례
- CPU 캐시 CPU와 RAM의 속도 차이로 발생하는 지연을 줄이기 위해 L1, L2, L3 캐시 사용
- 웹 브라우저 캐싱웹 브라우저가 웹 페이지 데이터를 로컬 저장소에 저장하여 해당 페이지 재방문시 사용
- DNS 캐싱 이전에 조회한 도메인 이름과 해당하는 IP 주소를 저장하여 재요청시 사용
- 데이터베이스 캐싱 데이터베이스 조회나 계산 결과를 저장하여 재요청시 사용, CDN 원본 서버의 컨텐츠를 PoP 서버에 저장하여 사용자와 가까운 서버에서 요청처리
- 어플리케이션 캐싱 어플리케이션에서 데이터나 계산 결과를 캐싱하여 반복적 작업

<img width="1111" alt="스크린샷 2024-04-19 오후 2 11 06" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/397bc1bb-49d0-44ef-b99f-d6dd76e2f224">

<img width="1076" alt="스크린샷 2024-04-19 오후 2 12 09" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/4790d3bd-40c8-4c8e-a9bc-c9efe68798b0">

> Redis 설치 / 실행

Redis 설치
MacOS https://redis.io/docs/getting-started/installation/install-redis-on-mac-os/ 

Windows https://redis.io/docs/getting-started/installation/install-redis-on-windows/

Redis 실행
```
$ redis-cli
$ ping
```
데이터 저장/조회/삭제
```
저장
 $ SET lecture inflearn-redis
조회
 $ GET lecture
삭제
 $ DEL lecture
```

> 데이터 타입 알아보기
- Strings 문자열, 숫자, serialized object(JSON string) 등 저장 명령어
```
$ SET lecture inflearn-redis
$ MSET price 100 language ko $ MGET lecture price language
$ INCR price
$ INCRBY price 10
$ SET ‘{“lecture”: “inflearn-redis”, “language”: “en”}’ $ SET inflearn-redis:ko:price 200
```

Lists

<img width="581" alt="스크린샷 2024-04-19 오후 2 16 15" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/bf74b176-14c1-416f-b840-229289a9620b">

Lists String을 Linked List로 저장 -> push / pop에 최적화 O(1) Queue(FIFO) / Stack(FILO) 구현에 사용
```
$ LPUSH queue job1 job2 job3 $ RPOP queue
$ LPUSH stack job1 job2 job3 $ LPOP stack
$ LPUSH queue job1 job2 job3 $ LRANGE queue -2 -1
$ LTRIM queue 0 0
```

Sets

<img width="497" alt="스크린샷 2024-04-19 오후 2 17 22" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/4da7f785-d2ab-4e3c-a83b-71bc4e74791c">

Sets Unique string을 저장하는 정렬되지 않은 집합
Set Operation 사용 가능(e.g. intersection, union, difference)
```
명령어 
$ SADD user:1:fruits apple banana orange orange $ SMEMBERS user:1:fruits
$ SCARD user:1:fruits
$ SISMEMBER user:1:fruits banana
$ SADD user:2:fruits apple lemon
$ SINTER user:1:fruits user:2:fruits $ SDIFF user:1:fruits user:2:fruits
$ SUNION user:1:fruits user:2:fruits
```

Hashes
Hashes field-value 구조를 갖는 데이터 타입
다양한 속성을 갖는 객체의 데이터를 저장할 때 유용

```
명령어 
$ HSET lecture name inflearn-redis price 100 language ko 
$ HGET lecture name
$ HMGET lecture price language invalid 
$ HINCRBY lecture price 10
```

Sorted Sets

<img width="482" alt="스크린샷 2024-04-19 오후 2 19 04" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/71ebc6e7-6430-4183-b43b-daa4b47a8a9c">

ZSets Unique string을 연관된 score를 통해 정렬된 집합(Set의 기능 + 추가로 score 속성 저장) 내부적으로 Skip List + Hash Table로 이루어져 있고, score 값에 따라 정렬 유지
score가 동일하면 lexicographically(사전 편찬 순) 정렬

```
명령어 
$ ZADD points 10 TeamA 10 TeamB 50 TeamC 
$ ZRANGE points 0 -1
$ ZRANGE points 0 -1 REV WITHSCORES 
$ ZRANK points TeamA
```

Streams

<img width="573" alt="image" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/1398800d-5cda-4243-8bcb-8d86621a0cb7">

append-only log에 consumer groups과 같은 기능을 더한 자료 구조

추가기능
unique id를 통해 하나의 entry를 읽을 때, O(1) 시간 복잡도
Consumer Group을 통해 분산 시스템에서 다수의 consumer가 event 처리 

```
명령어
$ XADD events * action like user_id 1 product_id 1
$ XADD events * action like user_id 2 product_id 1
$ XRANGE events - +
$ XDEL events ID
```

Geospatials

<img width="495" alt="스크린샷 2024-04-19 오후 2 26 28" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/895a2b97-3744-436c-a7a2-b2692051e9bd">

Geospatial Indexes 좌표를 저장하고, 검색하는 데이터 타입 거리 계산, 범위 탐색 등 지원

```
명령어
$ GEOADD seoul:station
126.923917 37.556944 hong-dae 127.027583 37.497928 gang-nam
$ GEODIST seoul:station hong-dae gang-nam KM
```

Bitmaps

<img width="357" alt="스크린샷 2024-04-19 오후 2 34 03" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/39530acd-6cc0-417e-9cd5-2fce5aa0db5e">

Bitmaps 실제 데이터 타입은 아니고, String에 binary operation을 적용한 것 최대 42억개 binary 데이터 표현 = 2^32(4,294,967,296)

```
명령어 
$ SETBIT user:log-in:23-01-01 123 1 $ SETBIT user:log-in:23-01-01 456 1 $ SETBIT user:log-in:23-01-02 123 1
$ BITCOUNT user:log-in:23-01-01
$ BITOP AND result
user:log-in:23-01-01 user:log-in:23-01-02
$ GETBIT result 123
```

HyperLogLog

<img width="464" alt="스크린샷 2024-04-19 오후 2 22 29" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/995bba41-f0ce-49b7-93db-f2c179c3178e">

HyperLogLog 집합의 cardinality를 추정할 수 있는 확률형 자료구조
정확성을 일부 포기하는 대신 저장공간을 효율적으로 사용(평균 에러 0.81%)
Vs. SET 실제 값을 저장하지 않기 때문에 매우 적은 메모리 사용

```
명령어
$ PFADD fruits apple orange grape kiwi 
$ PFCOUNT fruits
```

BloomFilter

<img width="489" alt="스크린샷 2024-04-19 오후 2 23 59" src="https://github.com/tlsgn8483/tlsgn8483.github.io/assets/61337570/940c1ba1-753f-446d-ba54-161b354c87a5">

BloomFilter element가 집합 안에 포함되었는지 확인할 수 있는 확률형 자료 구조 (=membership test)
정확성을 일부 포기하는 대신 저장공간을 효율적으로 사용
false positive element가 집합에 실제로 포함되지 않은데 포함되었다고 잘못 예측하는 경우
vs. Set 실제 값을 저장하지 않기 때문에 매우 적은 메모리 사용

```
명령어
$ BF.MADD fruits apple orange 
$ BF.EXISTS fruits apple
$ BF.EXISTS fruits grape
```
 
참조 : [Redis]([https://www.inflearn.com/course/%EC%8B%A4%EC%A0%84-redis-%ED%99%9C%EC%9A%A9?inst=324c9cb6&utm_source=instructor&utm_medium=referral&utm_campaign=inflearn_%ED%8A%B8%EB%9E%98%ED%94%BD_promotion-link])
