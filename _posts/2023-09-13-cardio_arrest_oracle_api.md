---
title: "[Flask] Cardio_arrest_Test_API"
excerpt: "심정지예측 AI 인터페이스 REST API 툴제작"

categories:
  - Flask
tags:
  - [tag1, tag2]

permalink: /Flask/Cardio_arrest_Test_API/

toc: true
toc_sticky: true

date: 2023-09-13
last_modified_at: 2023-09-13
---
![Alt Text](https://tenor.com/ko/view/cat-laptop-typing-shitposting-internet-gif-5142304.gif)

> AI 심정지 예측 인터페이스 프로그램 API 테스트를 위해 테스트 API Tool 프로그램을 Flask로 제작하기로 하였다.
방법은 간단하다. POST로 요청하였을 때, AI 프로그램 형식에 맞는 파라미터를 조회해오고(조회할때 데이터는 오라클 DB를 이용하였다.), 그 조회된 정보를 다시 AI 프로그램에 POST나 GET으로 요청하면, 인터페이스 연동이 된다.

# API 보내는 방식 설계  
```
http 요청시 규격은 아래와 같다. 
환자 List 조회
- http://192.168.56.99:5300/api/oracle_vital_signals
- Type : Post
- parameter
* stdt : 조회시작시간
* eddt : 조회종료시간
 - ex ) 

{ "stdt" : "20230301170700", "eddt" : "20230323172659
```

# 동작하는 IP 및 app 기본 설정 설계
```
app.py 파일 상수설정 
host_addr = "0.0.0.0"
port_num = "5300"
app = Flask(__name__)
```


# JSON REST API 동작 설계 
POST로 요청을때 작동하도록 API 동작
각각의 json 파일로 DB 계정접속정보 쿼리문, 스케줄러 동작여부 등 만들어두었다.  

```
@app.route("/api/oracle_vital_signals", methods=['POST'])
def oracle_vital_signals():
# 파라미터 데이터는 json 형식의 데이터로 받는다. 
    param = request.get_json()

    try :
        with open("json_data/settings.json", "r") as db_info_json:
            # LOG.info(db_info_json)
            # oracle DB 계정 접속정보를 받는다. 
            db_info = json.load(db_info_json)
        if db_info["db_kind"] == "oracle":
            db_common = dbOracle()
            api_obj = Scheduler(db_common)
            api_obj.DBprocess_oracle(db_info)
            
    except Exception as e:
        print(e)
    # oracle DB 데이터를 가져오는 쿼리문을 가져온다. 
    with open("json_data/lists.json", "r") as select_json:
        select_info = json.load(select_json)
    # 가져올 데이터의 시작점과 끝점을 가져온다. 
    stdt = param['stdt']
    eddt = param['eddt']

    # 조건찾기
    list_val = [stdt, eddt]

    query = select_info["query"] % tuple(list_val)

    rows = db_oracle.dbOracle.excuteSelect(api_obj.db_oracle, query)
    try:
        selectList = json.loads(rows)
        return selectList

    except Exception as e:
        print(e)
    finally:
        db_oracle.dbOracle.dbClose(api_obj.db_oracle)
```

# 심정지 AI 프로그램 입력 포맷에 맞춘 오라클 DB 더미데이터 생성 
```
#사전에 랜덤 데이터 프로그램을 통한 더미 데이터 생성 
1,P-100000,안유진,95,19270512,F,20221025,Ingrown toenail,구관_1층,구관_1층_1병동,심장내과,20221025063207,93,68,121,20,36.5,00,99,3,3,
2,P-100001,Denise Blake,32,19900327,M,20221025,Overactive thyroid,신관_10층,신관_10층_2병동,혈액내과,20221025115834,140,93,84,24,36.1,56,99,1,2,
3,P-100000,안유진,95,19270512,F,20221025,Ingrown toenail,구관_1층,구관_1층_1병동,심장내과,20221025131729,105,79,141,21,36.3,00,98,2,4,
4,P-100002,하혜린,34,19880707,F,20221025,Bowel cancer,신관_10층,신관_10층_2병동,노년내과,20221025171804,104,84,128,29,36.3,81,98,2,4,
5,P-100001,Denise Blake,32,19900327,M,20221025,Overactive thyroid,신관_10층,신관_10층_2병동,혈액내과,20221025181752,141,85,87,23,36.1,56,99,1,2,
6,P-100003,박라윤,47,19750915,F,20221025,Catarrh,신관_7층,신관_7층_2병동,유방외과,20221025192540,134,63,75,24,37.1,98,96,1,2,
7,P-100000,안유진,95,19270512,F,20221025,Ingrown toenail,구관_1층,구관_1층_1병동,심장내과,20221025214728,110,75,139,17,37.3,00,98,2,2,
8,P-100002,하혜린,34,19880707,F,20221025,Bowel cancer,신관_10층,신관_10층_2병동,노년내과,20221026002228,104,84,128,29,36.3,81,98,2,4,
9,P-100005,홍동우,2,20191208,M,20221025,Meningitis,신관_5층,신관_5층_2병동,치과보철과,20221026004706,185,59,98,22,36.8,63,99,1,3,
10,P-100004,한우혁,71,19510412,M,20221025,Retinoblastoma,신관_9층,신관_9층_2병동,치과보철과,20221026012542,133,32,60,18,36.4,45,99,1,0,
11,P-100003,박라윤,47,19750915,F,20221025,Catarrh,신관_7층,신관_7층_2병동,유방외과,20221026012823,124,58,100,32,36.4,98,98,1,3,
12,P-100001,Denise Blake,32,19900327,M,20221025,Overactive thyroid,신관_10층,신관_10층_2병동,혈액내과,20221026020149,135,91,90,24,36.0,56,99,1,3,
13,P-100007,최승재,85,19361218,M,20221025,Heart failure,신관_6층,신관_6층_2병동,수부외과,20221026031851,199,106,153,6,35.8,60,99,3,5,
14,P-100006,김주영,69,19530402,M,20221025,Nosebleed,구관_6층,구관_6층_2병동,류마티스내과,20221026034541,161,83,78,17,37.0,69,97,1,0,
15,P-100000,안유진,95,19270512,F,20221025,Ingrown toenail,구관_1층,구관_1층_1병동,심장내과,20221026051422,110,73,122,15,36.9,00,98,2,2,
16,P-100002,하혜린,34,19880707,F,20221025,Bowel cancer,신관_10층,신관_10층_2병동,노년내과,20221026060032,104,84,128,29,36.3,81,98,2,4,
17,P-100008,홍지현,86,19360711,F,20221026,Brain tumours,신관_6층,신관_6층_1병동,방사선종양학과,20221026064514,140,107,83,22,36.2,23,99,1,2,
18,P-100003,박라윤,47,19750915,F,20221025,Catarrh,신관_7층,신관_7층_2병동,유방외과,20221026074450,122,65,96,22,36.7,98,97,1,3,
19,P-100005,홍동우,2,20191208,M,20221025,Meningitis,신관_5층,신관_5층_2병동,치과보철과,20221026075855,189,62,99,32,36.8,63,97,1,3,
20,P-100001,Denise Blake,32,19900327,M,20221025,Overactive thyroid,신관_10층,신관_10층_2병동,혈액내과,20221026081212,136,85,82,23,36.0,56,99,1,3,
21,P-100004,한우혁,71,19510412,M,20221025,Retinoblastoma,신관_9층,신관_9층_2병동,치과보철과,20221026083516,147,50,30,33,36.7,45,95,3,6,
22,P-10000C,조채원,58,19640525,F,20221026,Cellulitis,구관_7층,구관_7층_1병동,치과,20221026084230,178,119,74,22,36.1,01,97,1,2,
...
```

# Docker Container를 통한 배포설계 

```
FROM python:3.8
# 모든 소스는 app 폴더에 복사해주고, 
COPY . /app
# 작업폴더는 app폴더로 지정 
WORKDIR /app
# 오라클 압축파일 복사
COPY instantclient_21_6.zip ./app

RUN apt-get clean && apt-get update && apt-get install -y locales
# 오라클 압축파일 풀기 
RUN apt-get install -y unzip libaio1 && \
    unzip instantclient_21_6.zip && \
    mv instantclient_21_6/ /usr/lib/ && \
    rm instantclient_21_6.zip &&\
    ln /usr/lib/instantclient_21_6/libclntsh.so.21.1 /usr/lib/libclntsh.so && \
    ln /usr/lib/instantclient_21_6/libocci.so.21.1 /usr/lib/libocci.so && \
    ln /usr/lib/instantclient_21_6/libociei.so /usr/lib/libociei.so && \
    ln /usr/lib/instantclient_21_6/libnnz21.so /usr/lib/libnnz21.so

# 사용했던 라이브러리 복사
COPY requirements.txt /app/requirements.txt

RUN pip install --upgrade pip

RUN pip install -r requirements.txt

# 오라클의 환경변수를 동기화
ENV ORACLE_BASE /usr/lib/instantclient_21_6
ENV LD_LIBRARY_PATH /usr/lib/instantclient_21_6
ENV TNS_ADMIN /usr/lib/instantclient_21_6
ENV ORACLE_HOME /usr/lib/instantclient_21_6

CMD ["python", "app.py"]
```



간단하였지만, Docker에 대한 지식, 서버준비, AI 심정지 예측프로그램에 대한 파라미터 포맷에 따른 맞추기 등 다양한 부분에 대한 지식들을 알아야 만들수 있었다. 
