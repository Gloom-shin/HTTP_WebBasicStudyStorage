# 스프링의 강의 리뷰📽
> LoadMap Part : 스프링 핵심원리 - 기본편
> Section : 01. 객체 지향 설계와 스프링
> CreateDate : 2022.06.11
> UpdateDate :

<br></br>

## 목차
### 인터넷 네트워크
- 인터넷 통신
- IP(Internet Protocol)
- TCP, UDP
- PORT
- DNS
### URI와 웹 브라우저 요청 흐름
- URI
- 웹 브라우저 요청 흐름
### HTTP
- HTTP
- 클라이언트 서버 구조 
- Stateful, Stateless
- 비 연결성(connectionless)
- HTTP 메시지
### HTTP 메서드
- HTTP API
- HTTP Method - GET,POST
- HTTP Method - PUT, PATCH, DELETE
- HTTP 메서드의 속성

<br></br>
<br></br>

# 인터넷 네트워크
## 인터넷 통신
 - 우리가 느끼는 연결은 다이렉트로 연결되어 있어보이지만,
 - 클라이언트와 서버는 복잡한 인터넷망으로 연결되어 있다.
 - 복잡한 연결에 대하여[링크](https://github.com/Gloom-shin/TIL/blob/64d0ed09ff0438c0bd97f94ea46727d9a411c70b/%EC%9D%B8%ED%84%B0%EB%84%B7/%EC%9D%B8%ED%84%B0%EB%84%B7%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC.md)

<br></br>

## IP(인터넷 프로토콜)
 - 복잡한 인터넷 사이에서, 정확히 서버와 클라이언트를 가리키기위해 주소가 필요

### IP 역할
 - 지정한 IP주소에 데이터 전달
 - `패킷`(Packet)이라는 통신 단위로 데이터 전달
    - pack(포장하다)과 bucket(바구니)의 합친말로, 대표적인 예시로 택배의 행선지를 표시하는 꼬리표를 붙이는 것을 말한다.
    - 출발지 IP, 목적지 IP 등의 정보가 들어간다.
 - 그래서 `클라이언트 출발 IP` = `서버 도착 IP` , `클라이언트 도착 IP` = `서버 출발 IP`

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039515-7ea379e5-7fdd-4d5e-b234-4f3ee731d16a.png" width="60%"></p>

### IP 프로토콜 한계
 - 비연결성 : 받을 대상이 없거나, 서비스 불능 상태여도 전송(집에 주인이 없어도 택배 배송)
 - 비신뢰성 : 패킷이 순서대로 안오면?  , 중간에 패킷이 손실되면?
 - 프로그램 구분 : 노래프로그램과 영상프로그램의 정보가 같이 오면?

<br></br>

## TCP, UDP
### 인터넷 프로토콜 스택의 4계층 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039700-a45d785d-2028-46eb-be5a-f64a49859dd4.png" width="40%"></p>

- 위와 같이 TCP/IP 계층은 총 4계층으로 이뤄져있다.
- OSI(국제표준기구) 7계층에서 보다 간략화한 계층이라고 한다.
- 서로 밀접한 관계가 있는 계층들을 묶어서, 보다 실무적이고 효율성있게 4계층으로 나눈 것 이다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039852-d2154d70-5ac1-4593-989a-01dcc1ea5330.png" width="60%"></p>

### IP 패킷정보 담는 과정
 - 4계층이 IP 패킷정보를 담는 과정은 아래와 같다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039743-cc241ce0-9380-453d-8867-6147eac74360.png" width="60%"></p>

 - SOCKET 라이브러리를 통해 전달되며,
 - TCP 정보를 생성하고 메시지 데이터를 포함하며,
 - IP 패킷을 생성하고, TCP 데이터를 포함한다. 
 - IP 패킷정보를 포함하고, 이더넷 프레임 까지 쓰위고 전송하게 된다.
    - [이더넷 프레임](http://www.ktword.co.kr/test/view/view.php?m_temp1=2965)

### TCP 특징 
 - 전송 제어 프로토콜
 - 연결지향 TCP 3 way handshake (가상 연결)
 - 데이터 전달 보증
 - 순서 보장
 - 신뢰할 수 있는 프로토콜
 - 현재는 대부분 TCP 사용 
 

### UDP 특징
 - 사용자 데이터그램 프로토콜
 - 하얀 도화지에 비유(기능이 거의 없음)
 - TCP 3 way handshake X ,데이터 전달 보증 X ,순서 보장 X
 - 단순하고 빠름

> HTTP 3 버전은 UDP를 사용한다.

<br></br>

## PORT
 - 한번에 여러작업을 하게 되는 경우, `PORT`로 구분한다.
 - TCP 세그먼트 영역에서, 출발지 `PORT`정보와, 목적지 `PORT` 정보가 포함되어 있다.
   - 같은 IP 내에서 프로세스를 구분할 수 있다. 
 - 소켓(Socket)에 포함된다. 
### 특징 
 - 0 ~ 65535 할당 가능
 - 0 ~ 1023: 잘 알려진 포트, 사용하지 않는 것이 좋음
   - FTP - 20, 21
   - TELNET - 23
   - HTTP - 80
   - HTTPS - 443

<br></br>

## DNS
 - 도메인 네임 시스템
 - 도메인 명을 IP로 주소로 변환 
   - 흔히 `전화번호부`로 비유
 - 자세히 들어가면 DNS 시스템은 크게 3가지로 나눠지는 데
   - Root DNS 
   - TLD(Top-Level-Domain) 최상위도메인
   - Local DNS
 
### 장점
 - 이름(도메인명)만 알고 있어도 연결가능 
 - 한번도 연결된적이 없어도 연결가능

<br></br>
<br></br>

# URI와 웹 브라우저 요청 흐름
## URI
 - Uniform Resource Identifier
 - URL(locator) 과 URN(name)로 분류할 수 있다. 
   - [차이점](https://github.com/Gloom-shin/TIL/blob/64d0ed09ff0438c0bd97f94ea46727d9a411c70b/%EC%9D%B8%ED%84%B0%EB%84%B7/URL%EA%B3%BCURI.md)
 - URN은 사용 잘 안한다고 한다.
   - 최근 실습에, 인메모리 DB h2에 연결할 때, JDBC URL이  `jdbc:h2:mem:test` URN이다.

### URI뜻
- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항목과 구분하는데 필요한 정보

### URL, URN 뜻 
- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.

### URI의 구조

`scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]`

(ex)
`file:///C:/Users/shin/Desktop/`
`https://www.google.com:443/search?q=JavaScript`
|part|설명|예시|
|--|--|--|
|scheme|사용할 프로토콜을 뜻하며, `통신프로토콜`이라함|`filel://`  `https://`|
|user, password(선택)|접근하기위한 사용자의 이름과 비밀번호| 거의 사용하지 않음|
|hosts|웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹서버, 도메인 또는 IP|`127.0.0.1`   `www.google.com`|
|port|웹 서버에 접속하기 위한 통로|`:80`  `:443`  `:3000`|
|url-path|웹 서버의 루트 디렉토리로부터 웹페이지, 이미지, 동영상 등의 파일이 위치까지의 경로|`/Users/shin/Desktop/`  `/search`|
|query|웹 서버에 전달하는 추가 질문|`q=JavaScript`|

<br></br>

## 웹 브라우저 요청 흐름

### 내가 하는 일
 - 접속하고자 하는 서버 URL 입력
### 웹브라우저(크롬)이 하는 일
 - HTTP 요청 메시지 생성 
 - 전송하기위해, SOCKET 라이브러리를 통해, IP와 PORT 추가 
 - 데이터 전달
### OS(window)가 하는 일
 - TCP/IP패킷 성성 및 포함
 - HTTP 메시지 포함
### 네트워크 인터페이스가 하는일
 - 이더넷 프레임 씌우기
 - 인터넷으로 전송

> 응답메시지가 만들어 지는 경우도 위와 동일한 메커니즘이며, 응답을 디코딩(해석)하는 작업은 반대로 생각하면 된다.

<br></br>
<br></br>

# HTTP
- HTTP 메시지에 모든 것을 전송
- HTML, TEXT 
- IMAGE, 음성, 영상, 파일
- JSOM, XML(API)
> 거의 모든 형태의 데이터 전송 가능  
> 서버 끼리 데이터를 주고 받을 때도 대부분 HTTP 사용

### HTTP 역사 
 - HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더X
 - HTTP/1.0 1996년: 메서드, 헤더 추가
 - HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전
   - RFC2068 (1997) -> RFC2616 (1999) -> RFC7230~7235 (2014)
 - HTTP/2 2015년: 성능 개선
 - HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

### 기반 프로토콜
 - TCP: HTTP/1.1, HTTP/2
 - UDP: HTTP/3
 - 현재 HTTP/1.1 주로 사용  
   - HTTP/2, HTTP/3 도 점점 증가

### HTTP 특징
 - 클라이언트 <--> 서버 구조
 - 무상태 프로토콜(Stateless), 비연결성
 - HTTP 메시지
 - 단순함, 확장가능

<br></br>

## 클라이언트 서버 구조
 - Request(요청) Response(응답) 구조
 - 클라이언트는 서버에 요청을 보내고, 응답을 대기 
 - 서버가 요청에 대한 결과를 만들어서 응답

### 무상태 프로토콜(Stateless)
 - 서버가 클라이언트의 상태를 보존하지 않는다.
 - 장점 : 서버 확장성이 높음(scale out)
 - 단점 : 클라이언트가 추가 데이터 전송

### Stateful, Stateless 차이
#### Stateful

<p align="center"><img src="" width="60%"></p>

