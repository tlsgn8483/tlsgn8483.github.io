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
last_modified_at: 2024-01-29
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


```
sudo docker exec -it keygen-db bash
```

psql 명령어와 함께 손에 익은 옵션을 기계적으로 붙이는중
```
psql -U postgres
```

## PSQL
기본적으로 psql 명령어는 옵션을 주지 않고 실행 하면 기본값인 root 사용자로 접속을 시도한다. 
또한, 서버 호스트는 기본 로컬 소켓, 포트는 기본 5432 등등 여러가지가 자동으로 설정된다.

그렇기 때문에 설정이 바뀌어야 할 경우에는 적절 한 값으로 옵션을 직접 부여해서 접속 해 주어야 한다.

root로 시도중

PSQL의 기본 사용법은 psql [OPTION]... [DBNAME [USERNAME]] 이며 다양한 옵션을 아래의 명령어로 손 쉽게 확인 할 수 있다.

```
bash
psql --help
```
옵션 목록은 아래와 같다.

### 일반 옵션
```
-c, --command=COMMAND 하나의 명령(SQL 또는 내부 명령)만 실행하고 끝냄
-d, --dbname=DBNAME 연결할 데이터베이스 이름(기본 값: "root")
-f, --file=FILENAME 파일 안에 지정한 명령을 실행하고 끝냄
-l, --list 사용 가능한 데이터베이스 목록을 표시하고 끝냄
-v, --set=, --variable=NAME=VALUE psql 변수 NAME을 VALUE로 설정 (예, -v ON_ERROR_STOP=1)
-V, --version 버전 정보를 보여주고 마침
```
### 입출력 옵션
```
-a, --echo-all 스크립트의 모든 입력 표시
-b, --echo-errors 실패한 명령들 출력
-e, --echo-queries 서버로 보낸 명령 표시
-E, --echo-hidden 내부 명령이 생성하는 쿼리 표시
-L, --log-file=FILENAME 세션 로그를 파일로 보냄
-n, --no-readline 확장된 명령행 편집 기능을 사용중지함(readline)
-o, --output=FILENAME 쿼리 결과를 파일(또는 파이프)로 보냄
-q, --quiet 자동 실행(메시지 없이 쿼리 결과만 표시)
-s, --single-step 단독 순차 모드(각 쿼리 확인)
-S, --single-line 한 줄 모드(줄 끝에서 SQL 명령이 종료됨)
```
### 출력 형식 옵션
```
-A, --no-align 정렬되지 않은 표 형태의 출력 모드
-F, --field-separator=STRING
```
**unaligned 출력용 필드 구분자 설정(기본 값: "|")**
```
-H, --html HTML 표 형태 출력 모드
-P, --pset=VAR[=ARG] 인쇄 옵션 VAR을 ARG로 설정(\pset 명령 참조)
-R, --record-separator=STRING
```
**unaligned 출력용 레코드 구분자 설정 (기본 값: 줄바꿈 문자)**
```
-t, --tuples-only 행만 인쇄
-T, --table-attr=TEXT HTML table 태그 속성 설정(예: width, border)
-x, --expanded 확장된 표 형태로 출력
-z, --field-separator-zero unaligned 출력용 필드 구분자를 0 바이트로 지정
-0, --record-separator-zero unaligned 출력용 레코드 구분자를 0 바이트로 지정
```
### 연결 옵션
```
-h, --host=HOSTNAME 데이터베이스 서버 호스트 또는 소켓 디렉터리 (기본값: "로컬 소켓")
-p, --port=PORT 데이터베이스 서버 포트(기본 값: "5432")
-U, --username=USERNAME 데이터베이스 사용자 이름(기본 값: "root")
-w, --no-password 암호 프롬프트 표시 안 함
-W, --password 암호 입력 프롬프트 보임(자동으로 처리함)
```
## 결론
`psql -U 유저명` 후에 공백을 하나 두고 혹은, `-d나 --dbname` 옵션으로 DB 이름을 지정해주면 원하는 결과를 얻을 수 있다.


```
psql -U username databasename
```
사실 위의 명령도 풀어 쓰면 아래와 같다.
```
psql --dbname=my_db_name --host=localhost --port=5432 --username=my_user_name --no-password
```
앞으로는 postgres 접속이 필요 할 때 시간 낭비를 더 이상 하지 않기 위해 정리 해보았다.


참조 : [qsql]([https://www.postgresql.org/docs/current/app-psql.html])
