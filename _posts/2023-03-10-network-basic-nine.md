---
title: NetWork[HTTP-Header-1]
date: 2023-03-10 22:12:00 +0800
categories: [CS, Network]
tags: [Network,HTTP-Header]
---

# HTTP-Header
실생활에서 택배를 보낼떄, 택배 운송장에 수신자의 주소, 발신자의 정보, 택배 내용물 등을 적어서 보내는데, 웹상에서는 HTTP-Header가 있습니다.      

HTTP Header에는 클라이언트와 서버 간의 통신에 필요한 부가적인 정보를 담고 있습니다.     

또한, HTTP의 표준 스펙이 1999년까지 RFC2616으로 사용되었으나, 이후 2014년에 RFC723x 시리즈로 대체되었습니다.<br/>

## HTTP BODY
### 예시
```
HTTP/1.1 200 OK
/*표현 헤더*/
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Length: 3423
k
/*표현 데이터*/
<html>
 <body>...</body>
</html>
```
- Content-Type : 표현 데이터의 형식
    - 미디어 타입, 문자 인코딩
    - text/html;charset=UTF-8 이 외에도 여러가지가 존재 ( application/json, image/png 등 )
- Content-Encoding : 표현 데이터의 압축 방식
    - 표현 데이터 인코딩
    - 주로 표현 데이터를 압축하기 위해 사용되고, 압축하여 인코딩 헤더를 추가하고, 읽는 쪽에서 인코딩 헤더의 정보로 압축을 해제합니다.
    - gzip, deflate, identity 등
- Content-Language : 표현 데이터의 자연 언어
    - 표현 데이터의 자연 언어를 표현하기 위해 사용됩니다.
    - ko, en, en-US 등
- Content-Length : 표현 데이터의 길이
    - 표현 데이터의 길이

## 협상
클라이언트와 서버간의 리소스 전송 및 처리방법을 협의하는 프로세스를 나타냅니다.<br/>
2가지 유형으로 나뉩니다.
1. 컨텐츠 협상(Content Negotiation)
    - 클라이언트아 서버간의 컨텐츠, 표현 형식을 사용할지 협의하는 과정입니다.
    - 주로 Accept, Content-Type 헤더 등이 사용 됩니다.
2. 전송 협상 ( Transfer Negotiation)
    - 클라이언트와 서버 간에 데이터 전송에 사용할 인코딩 방식, 압축 방법 등을 협의하는 과정입니다.
    - 주로 Accept-Encoding, Content-Encoding 헤더 등이 사용 됩니다.
### 표현 요청
- Accept
    - 클라이언트가 선호하는 미디어 타입 전달입니다.
- Accept-Charset
    - 클라이언트가 선호하는 문자 인코딩입니다.
- Accept-Encoding
    - 클라이언트가 선호하는 압축 인코딩입니다.
- Accept-Langauge
    - 클라이언트가 선호하는 자연 언어입니다.

## 결론
HTTP 헤더는 클라이언트와 서버 간의 통신에 필요한 정보를 담고, HTTP 바디에는 실제 데이터가 포함됩니다.       

예시로 나온 헤더에는 표현 데이터의 형식, 압축 방식, 언어, 길이 등이 포함됩니다.     

또한, HTTP 헤더를 통한 협상은 컨텐츠와 전송 방식에 대한 두 가지 유형이 있으며, 각각 클라이언트가 선호하는 정보를 전달하는 헤더들이 사용됩니다.