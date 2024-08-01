---
title: NetWork[HTTP-Header-2]
date: 2023-03-13 23:16:00 +0800
categories: [CS, Network]
tags: [Network,HTTP-Header]
---

# HTTP-Header 전송 방식
### 단순 전송
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423 
<html>
 <body>...</body>
</html>
```
- 기본적인 HTTP 응답 방식으로, 단순하게 전송하는 방식 입니다.

### 압축 전송
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip 
Content-Length: 521
lkj123kljoiasudlkjaweioluywlnfdo912u34ljko98udjk
```
- 압축하여 전송하는 방법으로, Content-Encoding에 따라 다양한 방식으로 압축하여 전송합니다.

### 분할 전송
```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked 
5
Hello
5
World
0
\r\n
```
- 큰데이터를 여러개의 작은 조각으로 나누어 전송하는 방식입니다.
- 각 조각의 별도의 HTTP응답으로 전송됩니다.

### 범위 전송
```
HTTP/1.1 200 OK
Content-Type: text/plain
Content-Range: bytes 1001-2000 / 2000 
qweqwe1l2iu3019u2oehj1987askjh3q98y
```
- 클라이언트가 원하는 만큼의 범위의 데이터를 요청하고, 서버는 해당 범위에 맞게 응답하는 방식입니다.
- 주로 대용량 파일 다운로드 등에 사용됩니다.

## 결론
단순 전송은 기본적인 HTTP 응답 방식으로, 대용량 파일을 압축하지 않고 전송하는 방법입니다.  

압축 전송은 데이터를 압축하여 전송하는 방식으로, Content-Encoding 헤더에 압축 방식이 명시되어 있습니다.  

분할 전송은 큰 데이터를 여러 작은 조각으로 나눠 전송하는 방식으로, 대용량 파일일 경우, 데이터를 작은 조각으로 나누어 전송합니다.  

범위 전송은 클라이언트가 원하는 범위의 데이터만을 요청하고 서버가 해당 범위만을 응답하는 방식으로 활용 될 수 있습니다. 






