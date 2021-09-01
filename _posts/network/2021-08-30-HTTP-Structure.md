---
title: "HTTP 특징 및 요청 및 응답 구조"
last_modified_at: 2021-08-30
categories:
  - Network
tags:
  - HTTP Request
  - HTTP Response
  - HTTP Status Code
  - HTTP Method
---

- **HTTP 특징**

  - Hyper Text Transfer Protocol의 약자로, 웹 상에서 사용되는 데이터 송수신 프로토콜

  - 비연결성(Conectionless)

    - 연결을 유지하기 위한 리소스를 사용하지 않음
    - 연결하기 위해 KeepAlive 속성 사용 가능 (HTTP 1.x 이후)

  - 평문으로 전송 

    - 보안에 취약하지만, 암호화와 복호화 과정이 없어 빠르게 데이터를 전송

  - 무상태 (Stateless)

    - 클라이언트가 어떠한 상황에서 요청값을 전송하였는지 알지 못함
    - 상태를 알기 위해, 쿠키,세션, 토큰기반 인증을 사용
      * 쿠키 : 사용자 브라우저에 저장되는 데이터
      * 세션 : 서버에 저장하는 데이터
      * 토큰 기반 인증 : 보호할 데이터를 토큰으로 치환하여 사용, 즉 토큰으로 데이터를 암호화한 것 (OAuth, JWT를 사용)

    

- **HTTP 통신방식**

  - 요청 / 응답 구조로 이루어져 있으며, 상태값을 저장하지 않음(Stateless)

  

- **HTTP 구조**

  - HTTP 메세지는 Method, Path, Version, Header, Body로 구성되어 있음

  

- **HTTP Request의 구조**

  - 시작라인  : 요청 Path, HTTP Method와 Version이 명시되어 있음

    - GET /search HTTP/1.1

  - Header : 요청에 대한 추가정보가 담겨있음, Key-Value로 데이터가 구성되어 있음 (Key:Value)

    - 대표적인 Header 
      - Host : 요청을 보낸 URL 
      - User-Agent : 클라이언트 정보, 웹 브라우저 정보
      - Accept : 해당 요청을 받을 수 있는 응답 타입 ex) application/json
      - Connection : 해당 요청이 끝난 후에 클라이언트와 서버간 커넥션 유지 여부를 나타냄 ex) Keep-alive
      - Content-Type : body의 타입
      - Content-Length : body의 길이

  - Body : 요청 내용

    - GET 요청은 주로 Body가 없음

      

- **HTTP Response의 구조**

  - 상태 라인 :  HTTP 버전, 상태 코드, 상태 메세지가 명시

    - HTTP/1.1 404 Not Found

  - Header : Request의 Header와 동일

  - Body : 응답 내용


- **HTTP Method**

  - 클라이언트가 서버로 HTTP 요청을 전송할 떄 어떠한 목적의 행위인지 명시하는 방법 

  - GET : 서버에게 리소스를 요청할 때 사용

  - HEAD : GET과 동일하지만, HEAD만 요청할 때 사용

  - PUT : 서버가 가지고 있는 URI의 본문 내용변경을 요청할 때 사용

  - POST : 서버에 데이터를 전송하여 새로운 본문 입력 또는 데이터 생성 변경을 요청할 때 사용

  - DELETE : 서버에 URI 리소스를 삭제 요청할 때 사용

    ex ) GET /data/select/1 HTTP 1.1

    

- **HTTP Status Code**

  - 1XX - 메시지 정보, 2XX - 요청 성공, 3XX - 리다이렉션, 4XX - 클라이언트 오류, 5XX - 서버오류

    - 200 OK 
    - 301 Moved Permanetly
    - 400 Bad Request 
    - 401 Unauthorized 
    - 403 Forbidden 
    - 404 Not Found 
    - 500 Internal Server Error 

    
