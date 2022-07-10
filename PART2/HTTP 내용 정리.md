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
- [HTTP 헤더 개요](#)
- [표현](#)
- [콘텐츠 협상](#)
- [전송방식](#)
- 일반정보
- 특별한 정보
- 인증
- 쿠키
### HTTP 해더_캐시와 조건부 요청
- [캐시 기본 동작](#)
- [검증 헤더와 조건부요청](#)
- [캐시와 조건부 요청 헤더](#)
- 프록시 캐시
- 캐시 무효화

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


<br></br>
<br></br>
# HTTP 해더_ 일반해더

<br></br>
<br></br>
# HTTP 해더_캐시와 조건부 요청