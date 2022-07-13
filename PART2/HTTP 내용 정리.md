# 스프링의 강의 리뷰📽
> LoadMap Part : 모든 개발자를 위한 HTTP 웹 기본 지식  
> Section : 05~08   
> CreateDate : 2022.07.04  
> UpdateDate :


<br></br>

## 목차
### HTTP 메서드 활용
- [클라이언트에서 서버로 데이터 전송](#dataSend)
- [HTTP API 설계 예시](#)

### HTTP 상태코드
- [HTTP상태코드 소개](#introduce)
- [2xx](#)
- [3xx](#)
- [4xx](#)
### HTTP 해더_ 일반해더
- [HTTP 헤더 개요](#HTTPHeader)
- [표현](#Expression)
- [콘텐츠 협상](#ContentNego)
- [전송방식](#TransferMethod)
- [일반정보](#General)
- [특별한 정보](#Special)
- [인증](#Authorization)
- [쿠키](#Cookie)
### HTTP 해더_캐시와 조건부 요청
- [캐시 기본 동작](#CacheBasic)
- [검증 헤더와 조건부요청](#VerificationHeader)
- [캐시와 조건부 요청 헤더](#IfRequest)
- [프록시 캐시](#ProxyCache)
- [캐시 무효화](#CacheInvalidation)

<br></br>
<br></br>

# HTTP 메서드 활용
## 클라이언트에서 서버로 데이터 전송<a name="dataSend"></a>

### 데이터 전달 방식

> 데이터 전달 방식은  크게 2가지이다.

 - **쿼리** 파라미터(URL에 포함)를 통한 데이터 전송
   - GET
   - 주로 정렬 필터(검색어)
 - 메시지 **바디**를 통한 데이터 전송
   - POST, PUT, PATCH
   - 회원가입, 상품 주문, 리소스 등록, 리소스 변경

### 4가지 상황
> 클라이언트에서 서버로 데이터 전송하는 상황은 크게 4가지이다.
- 정적 데이터 조회
  - 이미지, 정적 텍스트 문서
- 동적 데이터 조회
  - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
- HTML Form을 통한 데이터 전송
  - 회원 가입, 상품 주문, 데이터 변경
- HTTP API를 통한 데이터 전송
  - 회원 가입, 상품 주문, 데이터 변경
  - 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

> 위 4가지 상황을 좀더 살펴보자 

<br></br>

### 정적데이터 조회
 > 필요한 정적 데이터를 조회할때에는 클라이언트에서 전송할 데이터가 없이 조회만 하면된다. 
 - 이미지, 정적 텍스트 문서
 - GET 메서드로 조회
 - 일반적으로 쿼리 파라미터 없이 `리소스 경로`로 단순하게 조회 가능

### 동적데이터 조회
> 쿼리 파라미터를 사용하여, 클라이언트가 서버로 보내는 데이터에 따라 동적데이터가 조회된다.
 - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
   - 페이지네이션 
 - 조회 조건을 줄여주는 `필터`, 조회 결과를 정렬하는 정렬 조건에 주로 사용
 - GET 메서드로 조회
 - `쿼리 파라미터` 사용해서 데이터를 전달
   
<br></br>

## HTML Form
> HTML폼에 의해 HTTP 메시지를 생성 시키고, 이 메시지를 가지고 요청을 보낸다.
### HTML Form 데이터 전송
 - 폼태그 속성에 `method = "post"`으로 되어 있으면, 바디에 데이터를 넣어 POST 전송
 - `Content-Type: application/x-www-form-urlencoded` 사용
   - form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
   - 전송 데이터를 url encoding 처리
     -  ex) abc김 -> abc%EA%B9%80 (UTF8)
 - 폼태그 속성에 `method = "get"`으로 되어 있으면, 쿼리 파라미터에 데이터를 넣어 POST 전송
 - 참고: HTML Form 전송은 GET, POST만 지원

### HTML Form 파일 전송
 - Content-Type: `multipart/form-data`
   - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 멀티파트)
     - ex) 이미지파일과 텍스트데이터를 같이 보낼 경우
   - 이미지 같은 파일은 바이너리 데이터로 변환시켜 전송한다.

<br></br>

### HTTP API 데이터 전송
- 서버 to 서버
  - 백엔드 시스템 통신
- 앱 클라이언트
  - 아이폰, 안드로이드
- 웹 클라이언트
  - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
  - 예) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
- GET: 조회, 쿼리 파라미터로 데이터 전달
- Content-Type: `application/json`을 주로 사용 (사실상 표준)
  - TEXT, XML, JSON 등등


<br></br>
<br></br>


# HTTP API 설계 예시
> 크게 3가지 방식이 있다 .
- HTTP API - 컬렉션
- HTTP API - 스토어
- HTML FORM 사용


## HTTP API - 컬렉션
> POST 기반 등록
- API 설계
  - 회원 목록 /members -> GET
  - 회원 등록 /members -> POST
  - 회원 조회 /members/{id} -> GET
  - 회원 수정 /members/{id} -> PATCH, PUT, POST
  - 회원 삭제 /members/{id} -> DELETE
- 서버가 새로 등록된 리소스 URI를 생성해준다. 
  - `HTTP/1.1 201 Created`  (응답메시지)
  - `Location: /members/100` (리소스 URI)

### 컬렉션
- 서버가 관리하는 리소스 디렉토리
- 서버가 리소스의 **URI를 생성하고 관리**
- 여기서 컬렉션은 `/members`

<br></br>

## HTTP API - 스토어
> PUT 기반 등록
- API 설계
  - 파일 목록 /files -> GET
  - 파일 조회 /files/{filename} -> GET
  - 파일 등록 /files/{filename} -> PUT
  - 파일 삭제 /files/{filename} -> DELETE
  - 파일 대량 등록 /files -> POST
- 클라이언트가 리소스 URI를 알고 있어야 한다
  - 파일 등록 /files/{filename} -> PUT
  - PUT /files/star.jpg 
  
### 스토어(Store)
- 클라이언트가 관리하는 리소스 저장소
- 클라이언트가 리소스의 **URI를 알고 관리**
- 여기서 스토어는 `/files`

<br></br>

## HTML FORM
- HTML FORM은 GET, POST만 지원
- 이런 제약을 해결하기 위해 동사로 된 리소스 경로 사용
  - 회원 목록 /members -> GET
  - 회원 등록 폼 /members/new -> GET
  - 회원 등록 /members/new, /members -> POST
  - 회원 조회 /members/{id} -> GET
  - 회원 수정 폼 /members/{id}/edit -> GET
  - 회원 수정 /members/{id}/edit, /members/{id} -> POST
  - 회원 삭제 /members/{id}/delete -> POST
> HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)

<br></br>

## 정리
- HTTP API - 컬렉션
  - POST 기반 등록
  - 서버가 리소스 URI 결정
- HTTP API - 스토어
  - PUT 기반 등록
  - 클라이언트가 리소스 URI 결정
- HTML FORM 사용
  - 순수 HTML + HTML form 사용
  - GET, POST만 지원
- 문서(document)
  - 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  - 예) /members/100, /files/star.jpg
- 컨트롤러(controller), 컨트롤 URI
  - 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  - 동사를 직접 사용
  - 예) /members/{id}/delete

<br></br>
<br></br>

# HTTP 상태코드
- 1xx (Informational): 요청이 수신되어 처리중
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함

<br></br>
## 1xx (Informational)

<br></br>
## 2xx (Successful)

<br></br>
## 3xx (Redirection)

<br></br>
## 4xx (Client Error)

<br></br>
## 5xx (Server Error)
- 서버 문제로 오류 발생
- 서버에 문제가 있기 때문에 재시도 하면 성공할 수도 있음(복구가 되거나 등등)


<br></br>
<br></br>
# HTTP 해더_ 일반해더
## HTTP 헤더 개요 <a name="HTTPHeader"></a>
 - `header-field` = field-name ":" OWS field-value OWS

 - HTTP 전송에 필요한 모든 부가정보

### RFC2616 (과거)
- [헤더 분류](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)
  - General 헤더: 메시지 전체에 적용되는 정보, 예) Connection: close
  - Request 헤더: 요청 정보, 예) User-Agent: Mozilla/5.0 (Macintosh; ..)
  - Response 헤더: 응답 정보, 예) Server: Apache
  - Entity 헤더: 엔티티 바디 정보, 예) Content-Type: text/html, Content-Length: 3423
- 바디
  - 메시지 본문(message body)은 엔티티 본문(entity body)을 전달하는데 사용
  - 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
  - `엔티티 헤더`는 엔티티 `본문의 데이터`를 **해석할 수 있는 정보 제공**
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등

> 현재는 RFC2616은 폐기됨  
> 2014년 RFC7230~7235 등장

### RFC723x변화
- 엔티티(Entity) -> 표현(Representation)
- Representation = representation Metadata + Representation Data
- **표현 = 표현 메타데이터 + 표현 데이터**

<img src="https://user-images.githubusercontent.com/104331549/178142522-0389439f-1baf-4189-a13b-13a1a7ad7081.png">

- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
- 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
- 참고: 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야 하지만, 여기서는 생략

<br></br>

## 표현<a name="Expression"></a>
> 크게 4가지
- Content-Type: 표현 데이터의 형식
- Content-Encoding: 표현 데이터의 압축 방식
- Content-Language: 표현 데이터의 자연 언어
- Content-Length: 표현 데이터의 길이

### Content-Type
- 표현 데이터의 형식 설명
- 미디어 타입, 문자 인코딩
  - text/html; charset=utf-8
  - application/json
    - Json 문자 인코딩 기본이 UTF-8 이다.
  - image/png
### Content-Encoding
- 표현 데이터 인코딩
  - 표현 데이터를 `압축`하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가  
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- 예)
  - gzip
  - deflate
  - identity
### Content-Language
- 표현 데이터의 자연 언어를 표현

<img src="https://user-images.githubusercontent.com/104331549/178142790-26bdd31e-97f3-43d9-a951-31e383f6e653.png">

 - ko : 한국어
 - en : 영어

### Content-Length
- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

<br></br>
<br></br>

## 콘텐츠 협상<a name="ContentNego"></a>
> 클라이언트가 선호하는 표현 요청
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어
- 협상헤더는 **요청시에만 사용**

> MediaType은 이렇게 해주고, Char-set도 이렇게해주고, 압축은 이렇게, 언어는 한국어로 해줘~

### 예시 
> 언어 같은 경우 웹애플리케이션이 지원하는 언어가 일어, 영어, 독일어가 있다고 가정하자.  
> 하지만, 우리는 한국어로 보고싶지만, 아쉽게도 지원하는 언어가 없다.   
> 이 경우 우선순위를 활용한다.   

#### 원칙1 우선순위
- 1~0 까지 수가 클 수록 우선순위가 높다.
  - Accept-Language: `ko-KR`,`ko;q=0.9`,`en-US;q=0.8`,`en;q=0.7`
  - `1`은 생략가능
  - `ko-KR`은 남한, `ko`은 대한민국(남한+북한)

#### 원칙2 구체적인것 
 - 수가 없을땐, 정보가 구체적일수록 우선순위가 높다.
   - Accept: text/*, text/plain, text/plain;format=flowed, */*
   - 1. `text/plain;format=flowed`
   - 2. `text/plain`

<br></br>
<br></br>

## 전송방식<a name="TransferMethod"></a>
 > `서버`-> `클라이어트`단순하게 네가지가 있다.
 - 단순 전송(Content-Length)
   - 길이 값을 알고 있을 때, 해당 길이 만큼 한번에 전송하는 방법
 - 압축 전송(Content-Encoding)
   - Gzip같은 걸로, 바이트코드를 압축시켜, 전송하는 방법
   - 압축정보인 `Content-Encoding: gzip`표기 필요
 - 분할 전송(Transfer-Encoding)
   - `Transfer-Encoding : chunked`
   - 분할하여 전송, 클라이언트는 받는 데로 사용
   - `Content-Length`를 넣으면 안됨
 - 범위 전송(Range, Content-Range)
   - 용량이 큰 이미지파일의 경우, 범위를 지정하여 나눠서 전송
   - `Content-Range : 1001-2000/2000`

<br></br>
<br></br>

## 일반정보<a name="General"></a>
- From: 유저 에이전트의 이메일 정보
  - 검색 엔진 같은 곳에서 사용하고, 일반적으로는 잘 사용되지 않음
  - 요청(request)에서 사용
- **Referer**: 이전 웹 페이지 주소
  - 현재 요청된 페이지의 **이전 웹 페이지 주소** 
  - 유입경로 분석가능 
  - 요청(request)에서 사용 
- User-Agent: 유저 에이전트 애플리케이션 정보
  - `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36`
  - 클리이언트의 애플리케이션 정보(어떤 웹 브라우저인지 정보)
  - 통계 정보 수집 가능
  - 요청(request)에서 사용
- Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
  - 마지막 도착한 서버 
  - 응답(response)에서 사용
- Date: 메시지가 생성된 날짜
  - 응답(response)에서 사용

<br></br>
<br></br>

## 특별한 정보<a name="Special"></a>

### Host 
 - 필수 헤더!
 - 하나의 서버가 여러 도메인을 처리해야할 때 
 - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때 
 - 집전화 느낌(번호는 하나인데, 여러사람이 있을 때)
 - 요청(request)에서 사용
### Location 
 - Location 헤더가 있으면, Location 위치로 자동 이동
   - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
   - 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴

### Allow 
 - 허용 가능한 HTTP 메서드
 - 응답에서 사용, 잘 사용 안함.

### Retry-After
- 유저 에이전트가 다음 요청을 하기까지 기다려야하는 시간
  - `Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)`
  - 실제 적용하기 어렵다.

<br></br>

## 인증<a name="Authorization"></a>
- Authorization: 클라이언트 인증 정보를 서버에 전달
  - 요청에서 사용
- 401 Unauthorized 오류가 났을때는 응답에서 사용
<br></br>

## 쿠키<a name="Cookie"></a>
 > 2개의 헤더를 사용한다. 
  - Set-Cookie(서버 -> 클라이언트)
    - 응답에서 사용 
  - Cookie(클라이언트 -> 서버)
    - 요청에서 사용

### 쿠키미사용

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178636509-c818f5ae-7e41-434b-af83-64e560130949.png" width="80%"></p>


- 로그인 이후 환영 페이지에 접근하였지만, 다시 요청왔을 때, 로그인 정보가 없기에, 특정 유저가 로그인했다라는 사실을 서버가 알지 못한다.

> 서버가 무상태이기 때문에, 모든 요청에 정보를 포함해서 보내면 된다.
> 하지만 이것 또한, 문제가 발생한다.
- 모든 요청에 사용자 정보를 포함되도록 개발 해야하고, 
- 브라우저를 완전히 종료하고 다시 열면 내용이 다 사라진다.(또 다른 해결책 - 웹스토리지)
 
### 해결책 저장소 쿠키

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178637042-bcc1de8d-ea7f-4236-a93e-14280c221daa.png" width="80%"></p>

- 서버에서 `Set-Cookie`를 통해 클라이언트 쿠키에 정보를 저장한다.
  - `set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure`
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 클라이언트는 요청을 보낼 때, 쿠키를 확인하고, 헤더에 포함하여 전송한다. 
- 모든 요청에 쿠키 정보 자동 포함
  - 네트워크 트래픽 추가
  - 최소한의 정보만 사용하는게 중요(세션id, 인증토큰)
  - 필요할 때만 전송하는 방법은 웹스토리지(localStorage, sessionStorage)

<br></br>

### 생명주기
 - `set-cookie: expires=Sat, 26-Dec-2020 00:00:00 GMT`
   - 만료일이 되면 쿠키 삭제
 - `Set-Cookie: max-age=3600`
   - 일정 시간 뒤에 쿠키 삭제 
 - 세션 쿠키 :  만료 날짜를 **생략**하면 브라우저 종료시 까지만 유지
 - 영속 쿠키 :  만료 날짜를 **입력**하면 해당 날짜까지 유지

<br></br>

### 도메인 
> set-cookie에서 접근가능한 쿠키를 도메인으로 표시해준다.
 - 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함 
   - ex) `domain = example.org`로 응답을 보내면, 클라이언트는 쿠키를 생성하고
     - 클라이언트가 쿠키를 다시 보낼때 그 쿠기에 접근 가능한 도메인은
     - example.org
     - dev.example.org 
 - 생략 : 현재 문서 기준 도메인만 적용
   - ex)  `example.org`에서 `domain = ..`지정을 생략하여 응답을 보내면, 클라이언트는 쿠키를 생성하고
     - 클라이언트가 쿠키를 다시 보낼때 그 쿠기에 접근 가능한 도메인은
     - example.org  

> 로그인 연동이 되려면, 도메인을 명시하여, 서브도메인도 유지시킬수 있어 보인다.

<br></br>

### 경로 
> set-cookie에서 접근가능한 쿠키를 Path=/; 으로 표시해준다.
 - 해당 경로를 포함한 하위 경로 페이지만 쿠키접근 가능
 - 일반적으로 `path=/루트`로 지정
   - ex) `path=/home`
     - /home
     - /home/level1
     - /home/level1/level2 
     - /hello -> 접근 불가능

<br></br>

### 보안
> set-cookie에서 접근가능한 쿠키를 Secure; 으로 표시해준다.
- Secure
  - 쿠키는 http, https를 구분하지 않고 전송
  - `Secure`를 적용하면 **`https`인 경우에만 전송**
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP 전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전

  
<br></br>
<br></br>
# HTTP 해더_캐시와 조건부 요청

## 캐시<a name="CacheBasic"></a>
 - 매번 웹브라우저에 접속할 때마다, (응답헤더+응답바디)HTTP메시지를 다운받아, 화면에 띄우는 작업은 비효율적이다.
 - 그래서 캐시라는 웹 저장소에 일정시간동안 보관하고, 바로 사용할 수 있게 해준다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178640491-8a495047-35ea-4f76-9619-1a998ecbd8c3.png" width="80%"></p>


### 캐시가 없을때
- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리다.
- 느린 사용자 경험

> 캐시를 적용하면, 한번 접속했던 브라우저에 다시 접근했을 때 더 빨리 로딩할 수 있다.

### 캐시가 사라진 후 다시 요청
 - 캐시 유효 시간이 초과하고, 다시 서버에 요청을 보내면 캐시를 갱신한다. 
 - 캐시가 유효시간이 있는 이유는, 신선하지않을 수 있는 데이터는 서버와, 클라이언트가 가진 데이터가 다를 수 있기 때문에 존재한다. 
 - 그래서 다시 네트워크 다운로드가 발생한다. 

> 하지만, 만약 두 데이터가 같다면??


<br></br>
<br></br>

## 검증 헤더와 조건부 요청<a name="VerificationHeader"></a>
> 캐시 유효시간이 초과해서 서버에 다시 요청하면 2가지 상황
 
 - 서버 데이터가 그대로거나, (굳이?)
 - 서버 데이터가 변경되거나, (갱신 필요)

 - 클라이언트 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법이 **검증해더!**

### 검증 헤더 추가

- 아래 그림과 같이, 응답 헤더 정보에  `last-modified`에 데이터 최종 수정일을 전송시켜, 클라이언트 캐시에 저장시킨다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178731372-2c2bb198-c5c1-4a97-a1b7-21fea413fc2a.png" width="80%"></p>

> 그리고 캐시시간이 초과!!
  
 - 다시 요청할때, HTTP 메시지 요청 헤더에, 캐시에 입력된 최송수정일을 헤더에 포함시켜 같이 보낸다.(요청헤더의 cookie정보는 암호화되어 있어 알아보기 힘듬)
 - 여기서, `if-modified-since`을 **조건부 요청**이라고 한다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178731663-667cfdd7-50a1-4470-9e22-54a2ed541d90.png" width="80%"></p>

 - 아직 데이터가 수정되지 않았다는 사실을 확인할 수 있다. 
   - 그럼 HTTP 응답 헤더에서 `status 304 Not Modified`를 보낸다.
   - HTTP 응답 **바디는 전송하지 않는다.**
   - 기존 캐시에 있던 정보는 갱신된다.

<br></br>

#### 검증 헤더 
- 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
- Last-Modified , ETag

#### 조건부 요청 헤더
- 조건부 요청 헤더
- 검증 헤더로 조건에 따른 분기
- If-Modified-Since: Last-Modified 사용
- If-None-Match: ETag 사용
- 조건이 만족하면 200 OK 
  - 캐시를 갱신하기위해 모든 데이터를 다운 받음
- 조건이 만족하지 않으면 304 Not Modified 
  - 기존 캐시를 사용하기에, 헤더 데이터만 다운 받음

<br></br>
### 단점
- 1초 미만(0.x초) 단위로 캐시 조정이 불가능
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

> ETag로 해결가능

### ETag, If-None-Match
 - ETag(Entity Tag)
- 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178742894-e2222c23-c6fc-40c4-b42f-8c53eec505fe.png" width="60%"></p>
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178742913-a72ea3ed-3867-44c2-a3ef-4625b09d30ac.png" width="70%"></p>

 - 캐시 제어 로직을 서버에서 완전히 관리(최종 수정일과 다르게, 서버에 의해 관리된다.)

<br></br>
<br></br>


 
## 캐시와 조건부 요청 헤더<a name="IfRequest"></a>
> 캐시도 제어 관련된 헤더가 있다.

- **Cache-Control**: 캐시 제어
- Pragma: 캐시 제어(하위 호환) 사용x
- Expires: 캐시 유효 기간(하위 호환)


### Cache-Control 
 - `max-age` : 캐시 유효시간, **초단위** 유용!!
   - 보통 길게 잡음
   - 캐시의 수명을 결정
 - `no-cache` 
   - 데이터는 캐시해도 되지만, 항상 오리진(origin) 서버에 검증하고 사용
   - 캐시의 행동 방식을 결정
 - `no-store`
   - 데이터에 민감한 정보가 있으므로 저장하면 안됨.
   - 메모리에서 사용하고 최대한 빨리 삭제
   - 캐시의 행동 방식을 결정

### Expires
 
 - 캐시 만료일을 정확한 **날짜**로 지정
   - expires: Mon, 01 Jan 1990 00:00:00 GMT
 - Cache-Control: `max-age`와 함께 사용하면 Expires는 무시


<br></br>
<br></br>

## 프록시 캐시<a name="ProxyCache"></a>
 - 원(origin)서버에서 웹브라우저까지 정보가 오려면, 오래걸린다. 
 - 그래서 중간에 프록시 캐시 서버를 둔다.
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178745735-14127899-c8d5-40b3-a7eb-35b6f671034e.png" width="80%"></p>

- CDN서버라고 사용하고 있다.
- AWS에서는 cloud_Front 라고 서비스 제공

### Cache-Control
- Cache-Control: **public** 
  - 응답이 public 캐시에 저장되어도 됨
  - 중간 프록시 캐시 서버에 저장됨.
- Cache-Control: **private**
  - 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
- Cache-Control: s-maxage
  - 프록시 캐시에만 적용되는 max-age
- Age: 60 (HTTP 헤더)
  - 오리진(origin) 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

<br></br>
<br></br>

## 캐시 무효화<a name="CacheInvalidation"></a>
> 캐시를 적용하지 않아도 웹 브라우저가 임의로 캐시를 하는 경우가 많다.   
> 즉, 확실하게 캐시되지않게 해야될 경우가 필요하다.  
 - 확실한 캐시 무효화 응답
 - `Cache-Control: no-cache, no-store, must-revalidate`
 - `Pragma : no-cache` (혹시 모를 과거 버전의 캐시 방지)

### Cache-Control: must-revalidate
- 캐시 만료후 최초 조회시 원 서버에 검증해야함
- 원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
- `must-revalidate`는 캐시 유효 시간이라면 캐시를 사용함

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178749747-b119cc48-22cf-4caf-b18d-cca41b2eb01a.png" width="80%"></p>

 - 이와 같은 경우 프록시 캐시 서버가 옛날 데이터를 보여줌으로써, `200 OK` 응답을 보낼 수도 있다.
 - 여기서 확실하게, 캐시를 사용하지 못하게 하려면 `must-revalidate`를 사용해야된다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/178750342-ccd0c573-7a98-4c24-9524-fc5bb1658f18.png" width="80%"></p>
