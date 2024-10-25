---
tags:
  - 완성
  - Python
  - OAuth
  - Flask
aliases: 
title: Flask로 편하게 Oauth2.0 Authorization Code 얻기
date: 2023-12-02
---
작성 날짜: 2023-12-02
작성 시간: 16:01


----

## 문제 & 원인
OAuth2.0의 Authorization Code를 얻어야하는데 어떻게 얻어 올 수 있을까?

## 해결 방안
우선 간이 서버가 필요하다. 리다이렉트 URI를 가지고 있는 서버가 필요한데 Flask를 활용하면 아주 간단하고 짧게 요청 로직을 짤 수 있다.

```python
from flask import Flask, request, redirect  
  
NAVER_AUTH_URI = "naver_uri"
app = Flask(__name__)  
  
  
@app.route('/')  
def home():  
    return "home"  
  
  
@app.route("/login/naver")  
def login_authorize():  
    return redirect(NAVER_AUTH_URI)  
  
  
@app.route("/login")  
def login():  
    authorization_code = request.args.get('code')  
    return f"Authorization Code: {authorization_code}"  
  
  
if __name__ == '__main__':  
    app.run(port=3000)
```

나는 네이버 Oauth에서 redirect를 localhost:3000/login으로 해두었기 때문에 해당 리다이렉트 URI가 필요하다. 네이버 Oauth2.0에서 Authorization Code를 얻기 위해서는 다음과 같은 파라미터가 필요하다.

![[Pasted image 20231202160450.png]]

reponse_type, cliend_id, redirect_uri, state는 필수이다. state의 경우 보통 STATE_STRING으로 설정해준다.
## 질문 & 확장

(없음)

## 출처(링크)
- https://developers.naver.com/docs/login/api/api.md

## 연결 노트











