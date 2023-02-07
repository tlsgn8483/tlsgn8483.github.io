---
title: "[Flask] Flask 전반적인 내용 및 Restful API 구현"
excerpt: "Flask 전반적인 내용 및 Restful API 구현"

categories:
  - Categories3
tags:
  - [tag1, tag2]

permalink: /categories3/Flask_Restful_1/

toc: true
toc_sticky: true

date: 2023-01-02
last_modified_at: 2023-01-02
---

### Flask 특징
- 마이크로 프레임워크 기반
- 웹 개발 최소 기능 제공, Restful 요청 처리, 유니코드 기반, 필요한부분 확장가능

Flask 와 Rest API

정적 페이지 리턴(HTML)
- 복잡한 URI를 함수로 쉽게 연결하는 방법을 제공

### HTTP(Hypertext Transfer Protocol)
- Server/Client 모델로 Request/Response 사용
  - Client에서 요청(Request)을 보내면, Server에서 응답(Response)을 준다.


<center><img src="https://www.fun-coding.org/00_Images/web_http.png" height=350 /></center>

> 프로토콜 (protocol): 컴퓨터간 통신을 하기 위한 규칙 

### HTTP(Hypertext Transfer Protocol) Request/Response

- Request
<center><img src="https://www.fun-coding.org/00_Images/http_request.png" /></center>


### HTTP(Hypertext Transfer Protocol) Request/Response

- Response
<center><img src="https://www.fun-coding.org/00_Images/http_response.png" /></center>


### REST
- REST(REpresentational State Transfer)
  - 자원(resource)의 표현(representation)에 의한 상태 전달
  - HTTP URI를 통해 자원을 명시하고, HTTP Method를 통해 자원에 대한 CRUD Operation 적용
    - CRUD Operation와 HTTP Method
      - Create: 생성 (POST)
      - Read: 조회 (GET)
      - Update: 수정 (PUT)
      - Delete: 삭제 (DELETE)

### REST API
- REST 기반으로 서비스 API를 구현한 것
- 마이크로 서비스, OpenAPI(누구나 사용하도록 공개된 API) 등에서 많이 사용됨

### flask 로 REST API 구현 방법
- 특정한 URI를 요청하면 JSON 형식으로 데이터를 반환하도록 만들면 됨
- 즉, 웹주소(URI) 요청에 대한 응답(Response)를 JSON 형식으로 작성
- Flask에서는 dict(사전) 데이터를 응답 데이터로 만들고, 이를 jsonify() 메서드를 활용해서 JSON 응답 데이터로 만들 수 있음

### 일반 Flask 구현 

예시

```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():                           
    return "<h1>Hello World!</h1>"

@app.route("/hello")
def hello_flask():
    return "<h1>Hello Flash!</h1>"

@app.route("/first")
def hello_first():
    return "<h3>Hello First</h3>"

if __name__ == "__main__":              
    app.run(host="0.0.0.0", port="8080")
```

#### 복잡한 라우팅: 데이터 전달하기
```
from flask import Flask

app = Flask(__name__)
@app.route("/")
def hello():                           
    return "<h1>Hello World!</h1>"

@app.route("/profile/<username>")
def get_profile(username):
    return "profile: " + username

@app.route("/first/<username>")
def get_first(username):
    return "<h3>Hello " + username + "!</h3>"

if __name__ == "__main__":              
    app.run(host="0.0.0.0", port="8080")
```

URI를 변수로 사용, 변수에 데이터 타입도 줄 수 있음.
- 데이터 타입이 없으면 문자열로 인식
- int 이외, float도 데이터 타입으로 줄 수 있음.

Flask로 Rest API 구현
```
from flask import Flask, jsonify
app = Flask(__name__)
```
* data를 사전 데이터로 만들고, 이를 jsonify() 메서드에 넣어서 return 해주면 됨
```
@app.route('/json_test')
def hello_json():
    data = {'name' : '김미연', 'family' : 'kim'}
    return jsonify(data)

@app.route('/server_info')
def server_json():
    data = { 'server_name' : '0.0.0.0', 'server_port' : '8080' }
    return jsonify(data)
```

Rest API 요청시 파라미터/파라미터 값 넣기
- HTTP 의 요청 방식 중, 가장 많이 사용되는 방식이 GET 방식
  - GET 방식에서는 URI 상에 파라미터와 파라미터 값을 넣을 수 있음
    - 규칙: URL?파라미터1=파라미터1값&파라미터2=파라미터2값 
    - URL 이후 첫 파라미터 이름 전에 ? 를 표시하고, 추가 파라미터가 있을 시에는 & 표시를 해야 함
    
### 폴더 구조를 다음과 생성하고, login.html 생성

```
/login_test.py
/templates
    /login.html
```

```
html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <form action="/login" method="get">
      <p>Enter Name:</p>
      <p><input type="text" name="user_name" /></p>
      <p><input type="submit" value="submit" /></p>
    </form>
  </body>
</html>  
```

Flask로 정적 웹페이지 로드 

### flask 로 정적 웹페이지 로드하기 
> 프론트엔드 페이지도 flask 로 보여줄 수 있음

- flask render_template(HTML파일명): HTML 파일 전송하기
  - HTML파일은 flask 가 실행되는 하위 폴더인 templates 폴더 안에 위치해야 함
  
```
@app.route('/html_test')
def hello_html():
    # html file은 templates 폴더에 위치해야 함
    return render_template('login.html')


### login_test.py 파일 업데이트
- flask 의 render_template 함수 추가
- @app.route('html_test') 추가
```

```
from flask import Flask, jsonify, request, render_template
app = Flask(__name__)


@app.route('/login')
def login():
    username = request.args.get('user_name')
    if username == 'dave':
        return_data = {'auth': 'success'}
    else:
        return_data = {'auth': 'failed'}
    return jsonify(return_data)


@app.route('/html_test')
def hello_html():
    # html file은 templates 폴더에 위치해야 함
    return render_template('login.html')


if __name__ == '__main__':
    app.run(host="0.0.0.0", port="8080")
```
### bootstrap 과 static_url_path
- flask_bootstrap 폴더

> 수시로 소스가 달라지므로, 현재 제공해드린 소스를 기반으로 소스가 어떻게 구성되었는지만 설명
### flask 로 정적 페이지 로드시, 경로를 찾지 못함
- 가장 합리적인 방법은 static_url_path 를 flask 객체 생성시 선언해주는 것임

```
from flask import Flask, jsonify, request, render_template
app = Flask(__name__, static_url_path='/static')
```

- html 상의 경로 변경
```
html
<link rel="canonical" href="https://getbootstrap.com/docs/4.5/examples/sign-in/" />
<link href="/static/dist/css/bootstrap.min.css" rel="stylesheet" />
```

###  REST API 구현해보기
 - 특정한 URI를 요청하면 JSON 형식으로 파라미터값을 가져오고, JSON 형식으로 데이터를 반환하도록 만들면 됨
 - flask에서는 dict(사전) 데이터를 응답 데이터로 만들고, 이를 jsonify() 메서드를 활용해서 JSON 응답 데이터로 만들 수 있음

### REST API method 정의하기
- flask API 정의시, methods 에 지원하는 request method 를 작성하면 됨
  - 각 요청 메서드마다 요청 메서드에 함께 오는 파라미터값을 추출하는 방식이 다름 
    - GET/PUT/DELETE 는 동일, POST 만 다름
- API 리턴값은 flask 의 jsonify() 함수를 사용해서, JSON 형식으로 리턴값을 넣어서 보내면 됨

```
python
@app.route("/test", methods=['GET', 'POST', 'PUT', 'DELETE'])
def test():
    if request.method == 'POST':
        print ('POST')
        data = request.get_json()
        print (data['email'])
    if request.method == 'GET':
        print ('GET')
        user = request.args.get('email')
        print (user)
    if request.method == 'PUT':
        print ('PUT')
        user = request.args.get('email')
        print (user)
    if request.method == 'DELETE':
        print ('DELETE')
        user = request.args.get('email')
        print (user)

    return jsonify(
        {'status': 'success'}
    )
```

### axios (frontend) 에서 각 요청 메서드와 파라미터값 전달하기

- vue template 에서 각 버튼 클릭시, 각 버튼에 연결된 함수를 호출하는 코드

```
html
    <button type="submit" class="btn btn-secondary" v-on:click="test_get">GET TEST</button>
    <button type="submit" class="btn btn-secondary" v-on:click="test_post">POST TEST</button>
    <button type="submit" class="btn btn-secondary" v-on:click="test_put">PUT TEST</button>
    <button type="submit" class="btn btn-secondary" v-on:click="test_delete">DELETE TEST</button>
```

- vue 에서 axios 로 HTTP를 요청하는 코드
  - HTTP 요청 메서드는 method: 에 넣으면 됨
  - GET, PUT, DELETE 는 params 에 데이터를 JSON 형식으로 넣으면 됨
  - POST 는 data 에 데이터를 JSON 형식으로 넣으면 됨

- flask 의 jsonify() 함수를 사용해서, JSON 형식으로 리턴된 리턴값은 response.data 에 담겨져 있으므로, 해당 데이터를 JSON 데이터를 꺼내듯이 추출하면 됨

```
javascript
          test_get: () => {
            axios("http://localhost:5000/test", {
              method: "get",
              params: {
                email: "test@test.com",
              }
            })
              .then((response) => {
                console.log(response.data['status']);
              })
              .catch((error) => {
                console.log(error);
              });
          },
          test_post: () => {
            axios("http://localhost:5000/test", {
              method: "post",
              data: {
                email: "test@test.com",
              }
            })
              .then((response) => {
                console.log(response.data["status"]);
              })
              .catch((error) => {
                console.log(error);
              });
          },
          test_put: () => {
            axios("http://localhost:5000/test", {
              method: "put",
              params: {
                email: "test@test.com",
              }
            })
              .then((response) => {
                console.log(response.data["status"]);
              })
              .catch((error) => {
                console.log(error);
              });
          },
          test_delete: () => {
            axios("http://localhost:5000/test", {
              method: "delete",
              params: {
                email: "test@test.com",
              }
            })
              .then((response) => {
                console.log(response.data["status"]);
              })
              .catch((error) => {
                console.log(error);
              });
          }
```
### HTTP 요청 메서드 (request method)
- 클라이언트(client)가 서버(server)에 HTTP 요청시 요청 목적을 알리는 표시
- 크게 GET, POST, PUT, DELETE 방식이 있으며, 이 중에서 GET 과 POST 방식을 많이 사용함
- 요청 메서드에 따라 요청 데이터를 전달하는 방식에 차이가 있음

### 주요 Request Method in HTML
- HTML 에서는 GET 과 POST 만 지원함

- GET: 정보 읽기(SELECT)
  - 전달이 필요한 파라미터들은 URL을 통해 전달


- POST: 정보 입력하기(INSERT)
  - 전달이 필요한 파라미터들은 HTTP body에 포함되어 전달되므로, 사용자는 직접적인 확인 불가

- PUT: 정보 수정하기(UPDATE), DELETE: 정보 삭제하기(DELETE)
  - GET 과 마찬가지로 파라미터들이 URL을 통해 전달

> 사실상 GET 또는 POST 방식을 많이 사용하며, POST 방식이 파라미터 정보를 노출하지 않으므로 POST 방식을 선호 <br>
> 단, 요청 기능에 따라 GET, POST, PUT, DELETE HTTP 메서드를 쓰는 것을 권장하고는 있음 ('Restful 하다' 라고 이야기함)

참조 : https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%92%80%EC%8A%A4%ED%83%9D-1/dashboard
