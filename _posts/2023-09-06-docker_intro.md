---
title: "[Docker] DockerFile Intro "
excerpt: "도커파일 개념, 작성방법/문법 예시"

categories:
  - Docker
tags:
  - [tag1, tag2]

permalink: /Docker/dockerfile_intro/

toc: true
toc_sticky: true

date: 2023-09-06
last_modified_at: 2023-09-06
---

## 🦥 도커파일(Dockerfile) 이란?

> 도커파일은 docker 에서 이미지를 생성하기 위한 용도로 작성하는 파일이다. 만들 이미지에 대한 정보를 기술해 둔 템플릿(template) 이라고 보면 된다.

도커 이미지를 만들 때 
**docker build [옵션] [작성한 dockerfile 경로]** 
위와 같이 명령어를 입력하면 작성한 도커파일의 내용을 기반으로 이미지 빌드가 시작된다.

### 도커파일 예시

FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py

도커파일은 위와 같이 명령어들이 나열된 형태로 작성한다.

그리고 각 line 의 맨 앞은 대문자로 이루어진 `지시어(Instruction)` 로 시작한다.

### 도커파일 Instructions 종류(타입)

1) FROM


도커파일에서 FROM 은 베이스 이미지(Base image)를 지정하는 지시어다.

도커 이미지를 만들 때 완전 무(無)의 상태로 만드는 게 아니라,

어느 정도 기본적인 구성 요소들이 갖추어진 상태의 이미지를 토대로 만드는 게 보통인데

이를 베이스 이미지라고도 한다.

FROM 에서 베이스 이미지와 태그를 지정하면 registry 에서 해당 이미지를 땡겨오게(pull) 된다.

**FROM ubuntu:18.04**

위와 같은 line 이 실행되면 도커는 registry 에서 linux ubuntu 18.04 버전 이미지를 다운로드하여

새로 만들 이미지의 기초가 되도록 구성한다.


2) RUN

 

RUN 은 말 그대로 command 를 실행(run)하여 새 이미지에 포함시키는 역할을 한다.

컨테이너에 꼭 필요한 소프트웨어나 라이브러리가 있을 수 있다.

그리고 특정 위치에 파일이나 디렉토리가 존재해야 할 수도 있다. (ex. 로그 파일이 저장될 폴더)

그런 경우,

RUN 뒤에 소프트웨어/라이브러리 설치 명령어, 또는 파일/디렉토리 생성 명령어를 작성하는 것이다.

이렇게 하면 명령어가 실행된 후의 변경사항이 새 이미지에 반영된다.

#shell form
RUN <command>

#exec form
RUN ["<executable>", "<param 1>", "<param 2>"]

command 는 보통 위와 같이 shell form, exec form 의 형태로 작성이 가능하다.


#shell form
RUN /bin/bash -c 'echo hello'

#exec form
RUN ["/bin/bash", "-c", "echo hello"]


shell form 이 우리가 직접 터미널에 입력하는 명령어의 형태와 비슷하다.
exec form 은 명령어 내의 각 단어를 따옴표와 bracket 으로 감싸주어야 해서 살짝 번거롭다.
(하지만 exec form 이 명령어 인식이 조금 더 잘된다는 얘기를 들은 것 같기도..)

3) CMD

CMD 는 컨테이너가 시작될 때 실행할 커맨드를 지정하는 지시어이다.

#shell form
CMD <command>

#exec form
CMD ["<executable", "<param>"]

CMD 도 커맨드를 shell 형태 또는 exec 형태로 작성할 수 있다.

언뜻 보면 RUN 과 똑같아 보이지만
RUN 은 이미지를 빌드할 때 실행되는 것이고
CMD 는 이미 만들어진(빌드 완료된) 이미지로부터 도커 컨테이너를 시작할 때 실행되는 것이다.

FROM ubuntu
CMD ["/usr/bin/wc","--help"]

이렇게 컨테이너가 올라올 때 실행되어야 할 명령어를 위와 같이 작성하면 된다.


CMD 는 한 도커파일 내에 CMD 가 여러번 나올 경우 `맨 마지막 줄의 CMD 명령만 유효`하다.
 
참고로 command 는 shell form 과 exec form 두 가지 방식으로 작성 가능하다고 했는데,

shell form 은 환경변수 인식이 가능하고 exec form 은 불가능하다.

CMD [ "echo", "$HOME" ]
위와 같은 경우는 '$HOME' 이라는 단어가 그대로 나타나게 되고

CMD echo $HOME
위와 같은 경우는 $HOME 환경변수로 지정된 값(ex. toramko)이 나오게 된다.

(다음에 이어질 ENTRYPOINT 에서도 동일하다)

4) ENTRYPOINT

 

ENTRYPOINT 역시 컨테이너 시작 시 실행될 command 를 지정한다.

그리고 마찬가지로 shell, exec 형태의 command 가 가능하다.

# shell form
ENTRYPOINT command param1 param2

# exec form
ENTRYPOINT ["executable", "param1", "param2"]
 

CMD 와 거의 동일하지만

컨테이너 실행 시 param 값을 대체할 수 없다는 점에서 다르다. (CMD 는 대체 가능)

 

다음과 같이 도커파일이 작성되었다고 가정해보자.

FROM ubuntu
ENTRYPOINT ["/bin/echo", "Hello"]
CMD ["world"]
CMD 가 ENTRYPOINT 의 additional param 이 되는 형태이다.

 

컨테이너를 실행할 때 param 지정 없이 일반적으로 실행할 경우 그 결과는 다음과 같다.

$ docker run -it --rm --name test

Hello world
 

만약 컨테이너를 실행할 때 param 을 지정하는 경우('toramko')는

$ docker run -it --rm --name test toramko

Hello toramko
 

위와 같이 나오게 된다.

ENTRYPOINT 의 param 인 'Hello' 가 아닌 CMD 의 param 인 'world' 가 대체된다.

 

그래서 보통 CMD 는 ENTRYPOINT 와 같이 쓰이면서

default 값을 갖는 param 역할을 하는 경우가 많다.

 

 

아, 그리고 ENTRYPOINT 도

한 도커파일 내에 ENTRYPOINT 가 여러번 나올 경우 맨 마지막 줄의 명령만 유효하다.

 


5) LABEL

key-value 형식으로 작성된 메타데이터를 이미지에 추가한다.

LABEL <key>=<value> <key>=<value> <key>=<value> ...
위와 같이 key-value pair 를 작성하면 된다.

 

LABEL "toramkey"="toramval"
LABEL version="1.0"
LABEL description="label's values can span \
multiple lines."
도커파일에 위와 같이 지정해두고 이미지를 생성한 뒤,

이미지 내에 있는 label 데이터를 확인할 때는 다음과 같이 명령어를 입력한다.

 **$ docker image inspect --format='' myimage**
 

 

6) ENV

 

ENV 는 환경변수(Environment Variables) 를 설정하는 지시어이다.

key-value 형식으로 환경변수명과 그 값을 지정한다.

ENV <key>=<value> <key>=<value> <key>=<value> ...
 

LABEL 과 사용법이 같다.

다만 LABEL 은 메타데이터를 설정하는 것이고, ENV 는 환경변수를 설정한다는 것!

 

환경변수 값에 " (따옴표) 와 같은 인용문자를 넣으려면 \ (백슬래시) 를 붙여주어야 한다. (escape 처리)

 

ENV NAME="Toram" ANIMAL_TYPE=Rabbit\ And\ Squirrel \
    AGE=2
 

 

7) EXPOSE

 

EXPOSE 지시어는 컨테이너가 실행될 때 컨테이너로 들어오는 트래픽을

특정 포트(port) 로 받아들일 수 있도록(listen) 지정하는 역할을 한다.


EXPOSE <포트>/<프로토콜>
위와 같은 형식이며, 프로토콜을 따로 지정하지 않는 경우 기본적으로 TCP 로 인식된다.

EXPOSE 80/udp
UDP 프로토콜의 80 포트는 위와 같이 작성할 수 있다.

 
 

8) COPY

Host 내에 있는 파일 또는 디렉토리를 컨테이너의 파일시스템으로 복사.

다음과 같은 형식으로 <src> 에서 <dest> 로 파일/디렉토리를 복사한다.

COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
--chown 옵션으로 파일과 디렉토리에 대한 소유 권한을 지정할 수 있다.

 

COPY --chown=toram:toram *.txt /toramko/
 

 

9) ADD

 

ADD 역시 파일 또는 디렉토리를 컨테이너로 복사한다.

ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
COPY 와 거의 동일하나,
ADD 는 Host 내에 있는 파일 외에도

경로(URL)를 지정하여 remote 파일/디렉토리를 복사해올 수 있다는 점이 다르다.

 

**ADD http://example.com/foobar /toramko/**
 

 

10) USER


컨테이너 안에서 명령을 실행할 유저명, 유저그룹을 설정한다.

기본적으로 컨테이너는 root 계정으로 실행되는데 이를 사용자계정 등으로 변경하기 위함이다.

USER <user>[:<group>]
USER <UID>[:<GID>]

위와 같이 user 명, group 명으로 지정할 수도 있고 uid, gid 로 지정할 수도 있다.

 

도커파일 내에서 USER 가 선언되면

그 이후의 명령들(RUN, CMD, ENTRYPOINT, COPY 등)에 적용된다.

 

**toram:toram**

 

11) WORKDIR


작업 디렉토리(Working Directory) 를 설정한다.

**WORKDIR /path/to/workdir**

리눅스의 cd 명령어와 유사하나,

이동하려는 디렉토리가 존재하지 않을 경우 해당 디렉토리를 생성하여 이동한다.


**WORKDIR /home/toramko**
 

12) VOLUME

 
VOLUME 은 컨테이너 내의 특정 디렉토리를 컨테이너 외부 경로에 마운트(mount) 시켜주는 지시자이다.

컨테이너 내부 데이터는 보통 컨테이너가 삭제되면 날아가버린다.

이를 막기 위해, 즉 컨테이너가 삭제되어도 데이터가 보존될 수 있도록

컨테이너 내부 경로를 외부로 연결시켜주는 것이 마운트 작업이다.

 
연결된 외부 경로에 데이터가 쌓이면서 영구 보존이 가능해진다.

**VOLUME ["/data"]**

생각보다 사용법은 매우 간단하다.

VOLUME 지시자는 컨테이너 내의 디렉토리만 지정하면 된다.

host 의 어떤 폴더에 마운트할지 정할 수는 없어 보임!

 

**/var/lib/docker/volumes/{volume명}**

위 경로에 기본적으로 마운트된다고 한다.
 


위의 다양한 지시자들을 용법에 맞게 잘 활용해서

컨테이너를 간편하게 실행하여 배포 프로세스를 효율화할 수 있는 이미지를 만들어내자 :)

 

