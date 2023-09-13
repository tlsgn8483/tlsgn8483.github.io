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

```
app.py 파일 상수설정 
host_addr = "0.0.0.0"
port_num = "5300"
app = Flask(__name__)
```

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

```
중요한 배포
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
