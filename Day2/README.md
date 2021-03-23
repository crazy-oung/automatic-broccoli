<details>
  <summary>목차</summary>
  <div markdown="1">

- [DAY2 network](#day2-network)
  - [1. 웹페이지가 출력되는 과정](#1-웹페이지가-출력되는-과정)
    - [주소를 치면 일어나는 일](#주소를-치면-일어나는-일)
    - [브라우저에 데이터가 전달되는 과정](#브라우저에-데이터가-전달되는-과정)
    - [DOM (Document Object Model)?](#dom-document-object-model)
    - [**HTML을 DOM으로 변환하는 과정**](#html을-dom으로-변환하는-과정)
    - [CSSOM (CSS Object Model)?](#cssom-css-object-model)
    - [Render-Tree?](#render-tree)
    - [렌더링 트리 만드는 과정](#렌더링-트리-만드는-과정)
    - [DOM 트리가 복잡하면...](#dom-트리가-복잡하면)
    - [Layout (reflow)](#layout-reflow)
    - [Paint?](#paint)
    - [브라우저가 보여주는 과정](#브라우저가-보여주는-과정)
  - [2. http 통신이란 무엇인가?](#2-http-통신이란-무엇인가)
    - [HTTP (Hypertext Transfer Protocol)?](#http-hypertext-transfer-protocol)
    - [브라우저가 HTTP방식으로 서버로부터 데이터를 받는 과정](#브라우저가-http방식으로-서버로부터-데이터를-받는-과정)
    - [HTTPS (HTTP + Secure)](#https-http--secure)
    - [대칭키](#대칭키)
    - [비대칭키](#비대칭키)
    - [해쉬함수](#해쉬함수)
    - [SSL (Secure Sockets Layer, 보안 소켓 레이어)](#ssl-secure-sockets-layer-보안-소켓-레이어)
    - [TLS (Transport Layer Security)](#tls-transport-layer-security)
    - [TLS 특징](#tls-특징)
    - [암호화 방식(*)](#암호화-방식)
    - [TLS 과정](#tls-과정)
    - [신뢰할 수 있는 사이트?](#신뢰할-수-있는-사이트)
    - [브라우저에서의 TSL/SSL 절차](#브라우저에서의-tslssl-절차)
    - [HTTP vs HTTPS](#http-vs-https)
  - [3. web server의 역할](#3-web-server의-역할)
    - [web server?](#web-server)
    - [웹서버와 WAS는 다르다.](#웹서버와-was는-다르다)
    - [WAS(Web Application Server)](#wasweb-application-server)
    - [WAS를 쓰는 이유](#was를-쓰는-이유)
    - [Web Server 와 WAS의 동작 과정](#web-server-와-was의-동작-과정)
  - [4. OSI 7 layer 계층별 역할](#4-osi-7-layer-계층별-역할)
    - [OSI(Open Systems Interconnection) 7계층](#osiopen-systems-interconnection-7계층)
    - [PDU(Process Data Unit)](#pduprocess-data-unit)
    - [각 계층 별 특징 및 역할](#각-계층-별-특징-및-역할)
      - [1. 물리 계층(Physical layer)](#1-물리-계층physical-layer)
      - [2. 데이터 링크 계층(Data link layer)](#2-데이터-링크-계층data-link-layer)
      - [3. 네트워크 계층(Network layer)](#3-네트워크-계층network-layer)
      - [4. 전송 계층(Transport layer)](#4-전송-계층transport-layer)
      - [5. 세션 계층(Session layer)](#5-세션-계층session-layer)
      - [6. 표현 계층(Presentation layer)](#6-표현-계층presentation-layer)
      - [7. 응용 계층(Application layer)](#7-응용-계층application-layer)
    - [캡슐화와 디캡슐화](#캡슐화와-디캡슐화)
    - [TCP/IP 모델](#tcpip-모델)
    - [TCP (Transmission Control Protocol)?](#tcp-transmission-control-protocol)
    - [TCP 특징](#tcp-특징)
    - [TCP 과정](#tcp-과정)
    - [TCP 서버는](#tcp-서버는)
    - [UDP (User Datagram Protocol)?](#udp-user-datagram-protocol)
    - [UDP 특징](#udp-특징)
    - [UDP 과정](#udp-과정)
    - [UDP 서버는](#udp-서버는)
  - [5. rest api](#5-rest-api)
    - [API (Application Programming Interface)?](#api-application-programming-interface)
    - [REST (Representational State Transfer)?](#rest-representational-state-transfer)
    - [REST/RESTful API?](#restrestful-api)
    - [REST의 구성](#rest의-구성)
    - [REST 특징 6가지](#rest-특징-6가지)
    - [REST API가 쓰는 방식](#rest-api가-쓰는-방식)
    - [REST API가 쓰는 HTTP Method들](#rest-api가-쓰는-http-method들)
    - [REST API가 따르는 규칙](#rest-api가-따르는-규칙)
    - [HTTP 상태코드](#http-상태코드)
  - [부록.](#부록)
    - [DNS (Domain Name System)?](#dns-domain-name-system)
    - [REST API와 SOAP](#rest-api와-soap)
    - [SOAP (Simple Object Access Protocol)?](#soap-simple-object-access-protocol)
    - [프록시서버 (proxy server, 대리 서버)](#프록시서버-proxy-server-대리-서버)
    - [캐시](#캐시)
    - [로드 밸런싱 (load balancing, 부하분산)](#로드-밸런싱-load-balancing-부하분산)
    - [L4 switch](#l4-switch)
    - [CA (Certificate Authority, 인증기관)](#ca-certificate-authority-인증기관)
    - [ROP(Resource Oriented Architecture, 자원 지향 설계)](#ropresource-oriented-architecture-자원-지향-설계)
    - [URI(Uniform Resource Identifier)](#uriuniform-resource-identifier)
    - [URL(Uniform Resource Locator)](#urluniform-resource-locator)
    - [Last-Modified](#last-modified)
    - [E-Tag](#e-tag)
  </div>
</details>
---

# DAY2 network

## 1. 웹페이지가 출력되는 과정
### 주소를 치면 일어나는 일
1. 브라우저에 URL 입력시 [DNS](#dns-domain-name-system)서버에서 진짜 주소(ip) 가져옴
2. 웹 페이지를 표시하기 위해 필요한 자료를 요청
     - 특정 ip(서버) 내 특정 폴더내의 파일, 일반적으로 index.html에 접근함 
3. 웹 사이트에서 요청을 받고 승인후에 브라우저에게 정보를 전송
     - [클라이언트와 서버가 주고받는 과정]()
4. 브라우저는 웹 사이트에서 받은 정보들을 조립하여 유저에게 보여줌
> - [http](#http-hypertext-transfer-protocol) 통신을 통해 받기 때문에 Request(요청)와 Response(응답)이 발생 
>   - **Request**? 클라이언트(브라우저)가 서버에게 요청하는 사항이 정리된 객체
>   - **Response**?  Request에서 받은 요청을 처리하고 그에 해당하는 데이터를 담은 객체
> - Request(요청)후 Response(응답)을 받음
- 브라우저는 마지막에 받은 정보를 가지고 웹페이지를 [그림(페인트)](#paint)
  - HTML은 [DOM](#dom-document-object-model)로 변환되며 CSS는 [CSSOM](#cssom-css-object-model)으로 변환
  - [Render-Tree](#render-tree), [Paint](#paint) 발생
    > 바이트 > 문자 > 토큰 > 노드 > 객채 모델 순으로 해석

### 브라우저에 데이터가 전달되는 과정
> [http](#http-hypertext-transfer-protocol) 통신

### DOM (Document Object Model)?
- 필요한 자료를 요청해 바이트(Bytes) 상태로 받은 HTML파일

### **HTML을 DOM으로 변환하는 과정** 
1. 변환: HTML 바이트를 지정된 인코딩(주로 UTF-8)에 따라 문자로 변환
2. 토큰화: UTF-8로 변환된 문자열을 W3C HTML5 표준에 의거하는 토큰(태그) 로 변환
3. 렉싱: 각 토큰(태그)를 속성과 규칙을 가지는 객체로 변환
4. **DOM 생성**: HTML마크업이 여러 태그(일부는 다른태그안에 포함) 간의 관계를 정의하기 떄문에 트리 데이터 구조 내에 연결됨
  - 부모태그와 자식태그의 관계가 포함됨
- 최종 출력: HTML페이지의 DOM
  - HTML은 DOM으로, CSS는 CSSOM으로
- 브라우저는 이후 모든 페이지 처리에 이 DOM을 사용

### CSSOM (CSS Object Model)?
- HTML 대신 CSS가 대상인 DOM
- JavaScript에서 CSS를 조작할 수 있는 API 집합
- 페이지에 CSS가 참조 될경우 DOM을 생성하는 동안 외부 스타일시트를 받아옴
  - CSS는 위의 HTML을 DOM으로 변환하는 과정과 같은 프로세스를 진행

### Render-Tree?
- CSSOM 및 DOM 트리를 결합해 렌더링 트리 생성
- 표시되는 각 요소의 레이아웃을 계산하는데 사용
- 픽셀을 화면에 렌더링하는 페인트 프로세스에 대한 입력으로 처리됨
- CSSOM이 트리구조를 가지는 이유
  - HTML에서 나온 객체의 최종 스타일을 계산할때 **상속되는 값도 같이 계산해야 해서**

### 렌더링 트리 만드는 과정
1. DOM 트리의 루트에서 시작하여 표시되는 노드(태그) 각각을 트레버스 
   - script 나 meta 등은 생략
   - CSS를 통해 숨겨진(display:none)요소도 렌더링 트리에서 생략
2. 표시된 각 노드에 대해서 일치하는 CSSOM 규칙을 적용
3. 표시된 노드를 콘텐츠 및 스타일과 함께 반환
4. 완료된 렌더링 트리를 기준으로 브라우저가 화면에 페인트

### DOM 트리가 복잡하면...
- DOM 구성요소(html tags)의 depth가 깊으면?
  - 장점
    - 웹 페이지 구성 좋음 
  - 단점
    - DOM트리가 굉장히 복잡
    - 검색 비용이 증가 (검색 비용 줄일 수 없음, 트리 탐색 알고리즘 한계)
      - 화면에 수정된 요소가있을때 성능이 떨어짐
        - 렌더 트리를 재구축하고
        - 재생성된 렌더트리를 바탕으로 렌더링을 진행
        - 리페인트(브라우저가 변경된 스타일을 다시 입히는 과정) 진행
        - 리플로우(HTML내 크기나 위치가 변경된 요소를 재배치하는 과정) 진행
  - So, 렌더링 비용을 줄이려면
    - DOM select하는 케이스를 줄이는게 최선 = [가상 DOM](/Day3/README.md#가상-dom)으로 해소가능

### Layout (reflow)
- 각 노드들에 스크린의 좌표를 설정해 나타나야 할 위치설정

### Paint?
- 렌더링 된 요소들에 색을 입히는 과정
- 돔 트리의 각 노드들을 거쳐가면서 paint() 메소드를 호출해요. 



### 브라우저가 보여주는 과정
1. HTML 마크업을 처리하고 DOM 트리를 빌드
2. CSS 마크업을 처리하고 CSSOM 트리를 빌드
3. DOM 및 CSSOM을 결합하여 렌더링 트리를 형성
4. 렌더링 트리에서 레이아웃을 실행하여 각 노드의 기하학적 형태를 계산
5. 레이아웃 처리
6. 개별 노드를 화면에 페인트


---
## 2. http 통신이란 무엇인가?
### HTTP (Hypertext Transfer Protocol)?
- 인터넷에서 데이터를 주고받기 위한 약속
  - 브라우저는 http 프로토콜을 이용해 전달하고 요청한 데이터를 전달받음
- 주로 [TCP](#tcpip-모델)를 사용하고 HTML문서를 주고받는데 사용
  > 데이터를 보내기 위해 사용하는 프로토콜에는 [UDP](#udp-user-datagram-protocol)와 [TCP](#tcp-transmission-control-protocol)가 있는데 HTTP는 TCP방식을 활용. 
- 요청(Request)과 응답(Response)으로 구성
  - 클라이언트가 서버에 정보를 요청하면 응답 코드와 내용을 전송하고 클라이언트와 연결을 종료
- 전송할 때 일반 텍스트 타입으로 전송됨
  - 응용 계층에서 텍스트를 전송
  - 그러면 TCP 통신 과정을 거쳐 물리 계층에서는 전기신호로 전달

### 브라우저가 HTTP방식으로 서버로부터 데이터를 받는 과정
> [네트워크](#4-osi-7-layer-계층별-역할)에 대한 이해가 필요함
- 브라우저는 http 통신을 통해서 사이트 문서를 가져오고 이를 해석해 화면에 출력함
  - [TCP/IP 계층](#tcpip-모델)의 순서로 네트워크에 접근
    1. 응용 계층에서 정보를 만들어 전달하면 
    2. 전송 계층(TCP)에서 통신 노드를 연결
    3. 전송 계층(IP)에서 통신노드간 패킷을 전송/라우팅
    4. 네트워크 인터페이스 계층에서 전기적 신호로 변환하여 하드웨어에 전달 

### HTTPS (HTTP + Secure)
- 전송을 할때 일반 텍스트가 아닌 암호화된 텍스트로 전송됨
  - 보안이 더 강력한 통신 방식
- HTTP 에서 SSL/TSL을 이용하면 HTTPS
  
### 대칭키
- 암호화할때 사용
- 기존 방식은 대칭키 방식
  - 메세지 송수신을 하는 곳에서 같은 키를 공유함
  - 암호화/복호화를 하나의 비밀키를 가지고 진행함
- 대칭키가 노출되는 경우 중간에서 정보를 탈취할 가능성이 높아짐
- 속도빠름
  - 암호해독 쉬움, 키관리 어려움
- 암호화 방식 : DES, 트리플 DES, SEED, ARIA, RC -4 등
  
### 비대칭키
- 대칭키의 단점을 보완하고자 나옴
- 서로 다른 쌍의 키로 암복호화를 진행함
  - 하나로 암호화하고 다른 하나로 복호화를 진행 (공개키 & 개인키)
  - 공개키로는 암호화된 정보를 복호화 할 수 없으므로 보안 문제가 해결됨
- 대칭키보다 속도 낮음
  - 암호해독이 어려움 = 해독시간 오래걸림(전자서명)
- 암호화 방식 : RSA, DSA, 등
  
### 해쉬함수
- 복호화가 불가능한 방식

### [SSL (Secure Sockets Layer, 보안 소켓 레이어)](/Day2/_ssl/README.md)
- TLS의 이전의 프로토콜
- 컴퓨터 네트워크에 통신 보안을 제공하기 위해 설계된 암호 규약
- 메시지 인증 코드들은 비표준 무작위 함수 사용
- **현재는 폐기됨**

### TLS (Transport Layer Security)
- SSL에서 보안이 더 업그레이드 된 버전 
- TCP/IP 네트워크를 사용하는 통신에 적용
-  전송계층 종단간 보안과 데이터 무결성을 확보
   -  네트워크로 통신을 하는 과정에서 도청, 간섭, 위조를 방지
   -  암호화를 해서 최종단의 인증, 통신 기밀성을 유지
-  웹 브라우징, 전자 메일, 인스턴트 메신저, voice-over-IP (VoIP) 등에 적용
-  메시지 인증 코드들은 HMAC 해시 함수로 만듬

### TLS 특징
- 기밀성 우선 = 스니핑 공격에 보안 좋음
- 개인정보유출, 기밀정보유출, DDoS, APT, 악성코드 공격이 발생할 경우 무력화
  - 암호화된 패킷이 클라이언트PC <=> 서버로 전송 = 송수신자간 데이터 교환이 일어나는 일에 대해서는 무력

### 암호화 방식(*)
- 키교환 - RSA, Diffie-Hellman, ECDH, SRP, PSK
- 인증 - RSA, DSA, ECDSA
- 대칭키 암호
  - TLS → RC4, DES, 트리플 DES, AES, IDEA, 아리아 (암호), ChaCha20, Camellia
  - 옛날 버전 SSL → RC2
- 해시 함수
  - TLS → HMAC-MD5 또는 HMAC-SHA
  - SSL → MD5와 SHA
  - 옛날 버전 SSL → MD2와 MD4

### TLS 과정
1. 지원 가능한 알고리즘 서로 교환
2. 키 교환, 인증
3. 대칭키 암호로 암호화하고 메시지 인증]

### 신뢰할 수 있는 사이트?
- 공개키를 가짐
  - 신뢰할 수 있는 기관에서 이를 검증
    - 검증받으면 [CA(인증기관)](#ca-certificate-authority) 생성
    - 브라우저에 내장되어 있는 CA도 있음 
  - 탐색과정(핸드쉐이크)를 통해 검증
    - 서버에 요청을 보내면 인증서를 사이트에서 같이 보내옴
    - 브라우저에서 공개키를 가지고 인증서를 복호화하게됨
    - 복호회된 인증서가 CA인증에 성공하면 검증된 신뢰가능한 사이트
    - 만일 브라우저에서 CA인증에 실패하면 인증되지 않은 사이트로 신뢰할 수 없는 사이트

### 브라우저에서의 TSL/SSL 절차
1. HTTPS 요청
2. 웹서버로부터 공개키를 인증서와 함께 받음
3. 브라우저는 인증서가 유효한지 확인
   1. 여기서 인증서 발금한 CA가 내장된 CA리스트에 있는지 확인
   2. 내장된 CA리스트에 있다면 서버가 보내온 공개키를 이용해 인증서 복호화.
   3. 내장된 CA리스트에 없다면 인증기관으부터 유효성 검사실시 
4. 유효성 인증시, 브라우저는 받은 공개키를 사용해 데이터를 암호화한 후 서버로 전송/요청
5. 웹서버에서 개인키/공개키로 복호화 
6. 웹서버에서 작업 수행후 수행결과를 암호화해서 전송
7. 브라우저는 결과를 복호화 해서 출력


### HTTP vs HTTPS
- 개인정보나 민감한 정보를 사용할 경우 HTTPS를 사용
- 암호화될 필요가 없는 사이트는 HTTP 사용
- HTTPS를 사용하는 사이트의 경우 HTTP 통신을 하지 않음
- 모든 통신에 HTTPS 를 사용할 경우 암호화가 불필요한 정보도 암호화 하게되므로 부하가 높고 효율적이지 못함
  - 그러므로 HTTP만 쓰는 페이지와 HTTPS만 사용하는 페이지를 따로 구성하기도 함
    - ex) 로그인 화면 =  HTTPS, 홈페이지 화면 = HTTP

---
## 3. web server의 역할
### web server?
- HTML, CSS, IMG 등과 같이 정적인 요소를 저장하고 보여주는 곳
  - SSR의 경우 요청에 대한 작업을 수행
- WAS에서 받은 동적으로 생성된 요소를 HTML 등과 같은 정적인 요소에 합쳐서 만들어진 결과를 클라이언트로 반환
  
### 웹서버와 WAS는 다르다.
- Web Server: 정적인 요소를 담당
- WAS: 동적 요소를 담당

### WAS(Web Application Server)
- PHP, JSP, ASP 등과 같이 동적인 요소의 요청을 받아 처리하는 곳
- 데이터베이스와 상호작용 함
- 웹서버의 역할 대체가능
- Apache Tomcat, Jeus, NGINX 등이 있음

### WAS를 쓰는 이유
> WAS가 웹서버 대체가능한데 웹서버를 쓸 필요가 없지 않나?
- 규모가 커지거나 사용량이 많을 경우 로직을 수해하는 WAS가 정적 컨텐츠까지 다 만들어야 함 = 부하 up
- WAS와 웹서버를 분리해서 부하를 덜고 장애를 예방함
  - 로드밸런싱과 L4를 통한 장애 감지/예방
- 대용량이거나 트래픽이 많은 서비스의 경우 서버를 여러대 두는 경우가 있음
  - WAS에 문제가 생겨 재시작 할 때 웹서버가 정상적으로 서비스하고 있으면 사용자는 장애를 겪지 않고 사용가능

### Web Server 와 WAS의 동작 과정
> 클라이언트 > 웹서버 > WAS > 데이터베이스 
1. 클라이언트(사용자)가 웹서버에 HTTP 요청
2. 웹서버에서 WAS로 로직 실행 요청
3. WAS는 로직을 수행하며 DB에 요청을 보내 상호작용
   
   
---
## 4. OSI 7 layer 계층별 역할
### OSI(Open Systems Interconnection) 7계층
- 프로토콜을 기능별로 나눈 것
- 각 계층은 하위 계층의 기능만을 이용하고 상위 계층에게 기능을 제공함
- 하위 계층(물리-데이터링크-네트워크-전송)은 하드웨어로, 상위 계층(세션-표현-응용)은 소프트웨어로 구현

### PDU(Process Data Unit)
- 각 계층에서 전송되는 단위

### 각 계층 별 특징 및 역할
#### 1. 물리 계층(Physical layer)
- 네트워크의 기본 **하드웨어 전송 기술을 이루는 계층**으로 필수임
  - 프로토콜을 사용하지 않고 다른 시스템에 전기적 신호를 전송
- 다양한 특징의 하드웨어 기술이 접목
- Modem, Cable, Fiber, RS-232C
- PDU: 비트(Bit)
- 허브, 리피터
   
#### 2. 데이터 링크 계층(Data link layer)
- 프레임에 주소부여(MAC - 물리적주소), 에러검출/재전송/흐름제어
- **이더넷**, 토큰링, FDDI,
- PDU: 프레임(Frame)
- 브릿지, 스위치
- 포인트 투 포인트(Point to Point) 간 **신뢰성있는 전송을 보장**하기 위한 계층
  - CRC 기반의 오류 제어와 흐름 제어가 필요
  - 네트워크 위의 개체들 간 데이터를 전달

#### 3. 네트워크 계층(Network layer)
- 주소부여(IP), 경로설정(Route)
- **IP**, ICMP, IPsec, ARP, RIP, BGP
- PDU : 패킷(Packet, 데이터를 여러 개의 조각들로 나눈 것)
  - 패킷단위로 전송 후 다시 합쳐짐
- 라우터, L3 스위치
- 여러개의 노드를 거칠때마다 **경로를 찾아주는 역할**을 하는 계층
  - 다양한 길이의 데이터를 네트워크들을 통해 전달
  - 전송 계층이 요구하는 서비스 품질(QoS)을 제공하기 위한 기능적, 절차적 수단을 제공
- 한마디로, 각 패킷이 **성공적이고 효과적으로 전달** 되도록 함

#### 4. 전송 계층(Transport layer)
- 종단간(end-to-end) 통신을 다룸
- 종단간 신뢰성 있고 효율적인 데이터를 전송하며, 오류검출 및 복구와 흐름제어, 중복검사 등을 수행
  - **[TCP](#tcp-transmission-control-protocol)**, **[UDP](#udp-user-datagram-protocol)**, RTP, SCT
- 양 끝단(End to end)의 사용자들이 **신뢰성있는 데이터를 주고 받을 수 있도록** 해줌
  - 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않음

#### 5. 세션 계층(Session layer)
- 통신을 하기 위한 세션을 확립/유지/중단 (운영체제가 해줌)
- TCP의 세션 관리 부분
  - NetBIOS, **SSH**, TLS
  - 포트(Port)번호를 기반으로 연결
- 양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공
  > 동시 송수신 방식(duplex), 반이중 방식(half-duplex), 전이중 방식(Full Duplex)의 통신, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행

#### 6. 표현 계층(Presentation layer)
   - 사용자의 명령어를 완성및 결과 표현
   - **ASCII, JPEG, MPEG, XDR**
   - 포장/압축/암호화
   - 코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어줌

#### 7. 응용 계층(Application layer)
   - 네트워크 소프트웨어 UI 부분
   - 사용자의 입출력(I/O)부분
   - **HTTP, SMTP, SNMP, FTP,** 텔넷, NFS, NTP
   - 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행

### 캡슐화와 디캡슐화
- TCP/IP 통신을 하면 하위 계층으로 가면서 캡슐화 하고 하위에서 상위로 올라면서 디캡슐화 함
- 데이터를 전송할 때 각각의 층마다 인식할 수 있어야 하는 헤더를 붙임
  - 브라우저에서 보낼때 상위계층 → 하위계층으로 갈 때, 각 단계별로 헤더를 붙여서 캡슐화함
  - 반대로 요청한 데이터에 대한 결과를 보낼때는 헤더를 없에는 디캡슐화를 진행함
    - 브라우저는 디캡슐화된 결과를 받게됨

![OSI 7계층과 TCP/IP](/Day2/OSI7.png)

### TCP/IP 모델
> 클라이언트가 요청하면 응용 > 전송 > 네트워크 > 네트워크 인터페이스 계층 순으로 통과
- 응용 > 표현 > 세션 [응용 계층]
- 전송 [전송 계층]
- 네트워크 [네트워크 계층]
- 데이터링크 > 물리 [네트워크 인터페이스 계층]

### TCP (Transmission Control Protocol)?
> IP는 배달을 담당한다면 TCP는 패킷을 추적
- 인터넷상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜
- 패킷을 추적 및 관리
  - 연결형 프로토콜
  - 각 패킷 연결을 위해 할당되는 논리적인 경로가 있음
- 연속성보다 **신뢰성있는 전송이 중요할 때 사용**

### TCP 특징
- 연결형 서비스 = 가상 회선 방식 제공
  - 발신지와 수신지를 연걸 → 패킷을 전송하기 위한 논리적 경로 배정
  - 전송 순서를 보장함 = 데이터 경계 구분 안함
- 흐름/혼잡 제어
- 신뢰성 높음
  -  파일 전송 처럼 신뢰성이 중요한 경우에 쓰임
- UDP보다 속도 느림
- 스트리밍 서비스에 불리함 (손실된 경우 신뢰성을 위해 다시 재전송을 요청함)
  - 순서가 엉켜서 틀리면 많은 시간 소요 (다시검사하니까)
- 전이중(Full-Duplex, 전송이 양방향 동시에), 점대점(Point to Point, 각 연결이 정확히 2개의 종단점 가짐) 

### TCP 과정
- 3-way handshaking과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제
  - 3-way handshaking: 목적지와 수신지를 확실히 하여 정확한 전송을 보장하기 위해서 세션을 수립하는 과정
    - SYN(접속요청) → SYN & ACK(서버는 요청받고 수락 보냄) → ACK(클라이언트는 요청+수락 받은 증명을 서버로 보냄 그러면 서버는 ㅇㅋ 연결 성립, **패킷 보낸다! 알림**)
      > - 여기서 2-way 를 쓰지 않는 이유
      >   - ex) SYN(들려?) → ACK(ㅇㅇ 들림) 끝
      > - 존재를 알리고 패킷을 보낸다는걸 알려야 하는데 인사만 하고 끝나게 되니 의미가 없음
  - 4-way handshaking: TCP Connection closee
    - FIN(클라이언트: 요청) → ACK(서버: ㅇㅋ확인 ⇢ 데이터 다 보낼때까지 ㄱㄷ TIME_OUT) → FIN(서버: 데이터 모두 보냄 ㅂㅂ) → ACK(클라어인트: ㅇㅋ확인 ㄱㅅ) 끝
      > 받지 못한 데이터를 대비해서 잉여 패킷 대기 하기위해 끝이 났어도 일정시간 세션남겨둠
  
### TCP 서버는
- 서버소켓은 연결만을 담당
- 연결과정에서 반환된 클라이언트 소켓은 데이터의 송수신에 사용
- 서버와 클라이언트는 1대1 연결을 함
- 전송 크기 무제한 (스트림 전송)
- 패킷에 대한 응답이 필수임 = CPU소모 = 시간지연 = 성능 낮음


### UDP (User Datagram Protocol)?
- 데이터를 데이터그램(독립적 패킷) 단위로 처리하는 프로토콜
  - 비연결형 프로토콜
  - 각 패킷 연결을 위해 할당되는 논리적인 경로가 없음
  - 각 패킷이 서로 다른경ㄹ로 독립적으로 처리됨
- **연속성을 우선으로 할때 사용**

### UDP 특징
- 연속성을 우선시 함
  - 신뢰성 낮음
  - TCP보다 속도 빠름 (TCP 처럼 흐름/혼잡 제어를 하지않음)
- 정보를 보내거나 받는다 라는 신호절차를 거치지 않음 
- 비연결성이므로 연결 설정/해제 과정 없음 = 데이터 경계를 구분함
  - 전송 순서 없음 = 전송 순서 뒤죽박죽 가능
  - Checksum을 사용하여 오류가 있음을 발견하면 그 데이터그램은 폐기
- 스트리밍이나 실시간 서비스에 적합 (성능이 우선시되는 작업) 

### UDP 과정
1. 운영체제에 포트번호 요청
2. 입력큐 하나(서버에서 전송)와 출력큐 하나(클라이언트에서 받음) 생성
3. 각 큐에서 입출력 후 프로세스 종료되면 큐 제거
   1. 받을 큐가 있으면 데이터그램을 큐의 끝에 추가 
   2. 없으면 큐 제거하고 종료
- 이 때 출력큐에서 오버플로우 걸릴 수 있음 
  - 그러면 운영체제가 클라이언트한테 대기 요청 보냄

### UDP 서버는
- 연결 안함 = 서버/클라이언트 소켓 구분 없음
- 소켓 대신 IP를 기반으로 전송
- 서버와 클라이언트는 1:1/ 1:N/ N:M 연결 가능
- 메세지 단위로 보내는데 용량 초과하면 잘라서 보냄
- 오류 났는지 확인 못함
  - 흐름제어 안해서


---
## 5. rest api
### API (Application Programming Interface)?
- 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스
  - 애플리케이션 소프트웨어를 구축하고 통합하는 정의 및 프로토콜 세트
  - 정보 제공자와 정보 사용자 간의 계약으로 지칭
- 공통으로 쓰는 걸로 운영체제 프로그램을 만들때도 사용
  - mac: Cocoa API
  - microsoft(window 10): Windos API
- 외부 라이브러리나 프레임워크/프로젝트 내에서 쓰는 함수나 기능들을 API라고 칭하기도 함
  - Web API, files API, Locatoins API, DOM API, context API 등이 있음
    - [RESTful](#restful)하게 만들어서 쓰임
  - 소비자에게 필요한 콘텐츠(호출)와 생산자에게 필요한 콘텐츠(응답)로 구성

### REST (Representational State Transfer)?
- 웹 상에 존재하는 다양한 자원들을 **HTTP 프로토콜을 이용하여 전송하는 인터페이스**
  - [ROP](#ropresource-oriented-architecture-자원-지향-설계) 
- [URI](#uriuniform-resource-identifier)를 부여해서 사용
- JSON(Javascript Object Notation), HTML, XLT 또는 일반 텍스트 등을 통해 몇 가지 형식으로 전송

### REST/RESTful API?
- REST 아키텍처의 제약 조건을 준수하는 API
- 웹 서비스와 모바일 애플리케이션 **경량화**의 필요에 맞춘 아키텍처 원칙 세트/스타일
- 하이퍼텍스트 전송 프로토콜(HTTP)을 통해 이루어짐
  - HTTP 프로토콜의 Method을 사용함
  - 그러므로, 웹의 장점을 최대한 활용하는 셈

### REST의 구성
- Resource(자원) : URI
- Verb(행위) : HTTP METHOD
- Representations(표현)

### REST 특징 6가지
>  HTTP의 장점을 최대한 쓰기 위해 고안한 만큼 HTTP의 특징을 가져가는 것이 장점이자 특징  
> - 아래의 특징은 HTTP의 특징에 해당되기도 함
1. 통합된(일관적인) 인터페이스 (Uniform Interface)
     - 표준화된 형식으로 정보를 전송할 수 있도록 구성 요소 간 통일되고 한정적인 인터페이스로 수행
2. 무상태성(stateless)
    - 요청하는 서버에 클러이언트의 콘텐츠가 저장되지 않음 
      - 세션이나 쿠키의 **정보를 서버가 저장하거나 관리하지 않음 = 자유도 up =  불필요한 구현x = 구현 단순화**
      - 세션 정보가 클라이언트에 저장됨 
3. 캐싱 가능(Cacheable) 
     - **[REST의 가장 큰 특징] HTTP 웹표준을 사용** = 웹 인프라를 그대로 사용가능 = HTTP가 가진 캐싱기능 적용가능
       - Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능
4. 계층형 구조
   - 상호 작용을 계층적으로 조정할 수 있도록 **계층화**된 구조
     - 로드 밸런싱, 캐싱 기능이나 암호화 계층을 추가할 수 있어구조상 확작성/유연성 up
     - PROXY, 게이트웨이 같은 네트워크 기반의 중간매체 사용가능
5. 자체 표현 구조
   - 자바 애플릿이나 자바스크립트의 제공을 통해 서버에서 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있음
6. 클라이언트-서버 구조 
   - 클라이언트, 서버, 리소스로 구성된 **클라이언트-서버 아키텍처**가 필요
   - 아키텍처를 단순화시키고 작은 단위로 분리함
     - 각 파트가 독립적으로 개선될 수 있음

### REST API가 쓰는 방식
- HTTP Method 4가지 로 표현
  
### REST API가 쓰는 HTTP Method들
> CRUD 가능
- POST
  - CREATE (생성/추가)
- GET
  - READ (조회)
- PUT
  - UPDATE (수정)
- DELETE
  - DELETE (수정)
- HEAD
  - hedaer 정보 조회 

### REST API가 따르는 규칙
- URI는 정보의 `자원`을 표현
  - ex) **GET** /`members`/`1`
- 자원에 대한 행위(Verb)는 **HTTP Method를 사용해 표현**
  - ex) **PUT** /members/1
    - 유저 정보를 가져올 때) **GET**  `/user/1`
    - 유저 저보를 추가할 때) **POST** `/user/1`

### HTTP 상태코드
|상태코드|의미    |
|:---:|:---   |
|1xx  |요청받았고 계속 수행 (Continue)|
|200  |요청 수행 성공 (Success)|
|201  |POST 요청 수행 성공 (Success)|
|400  |요청 부적절 (Reject/Error)|
|401  |미인증 상태에서 보호된 리소스 요청, 허가 되지 않은 요청  (Reject/Error)|
|403  |허가 상태와 상관없이 리소스 제공 거부  (Reject/Error)|
|404  |그런 거 없음 (존재 하지 않는 요청)  (Not found)|
|405  |사용 불가능한 Method를 이용했을 경우 (Reject)|
|301  |요청한 리소스에 대한 URI가 변경 되었을 때 (Changed/Redirect) :: 응답 시 Location header에 변경된 URI를 적음|
|500  |서버에 문제가 있을 경우 (Server Down/Error) :: SQL/서버 오류|
> **403 보다는 400 이나 404를 사용할 것** (403은 "여기 데이터 있는데 안 줄거에요" 라고 표내는 것이기 때문에 감추고 싶다면 확실히 할 것)

---
## 부록.

### [DNS (Domain Name System)](/Day2/_dns/README.md)?
- 도메인 이름과 IP 주소를 서로 변환하는 역할을 함
- 리소스 레코드(Resource record)를 가지며, A, AAAA, CNAME, NS, MX, SPF, PTR 로 구성
- Forward Zone(도메인 이름 → IP), Reverse Zone(IP → 도메인 이름)이 있음
  - Forward Zone: 도메인을 구성하는 호스트에 대한 정보 기록
  - Reverse Zone: DNS 서버에 대한 정보 기록
  
### REST API와 SOAP
- API의 설계 편의성과 구현 유용성을 높이기 위해 사용
- REST는 HTML, XML, 일반 텍스트, JSON과 같은 다양한 형식으로 메시지를 반환
  - = 경량화되어 있음
- SOAP는 XML 문서 형식으로 데이터를 반환
  - 기본 보안과 트랜잭션 컴플라이언스를 제공 = 무거움

### SOAP (Simple Object Access Protocol)?
- 다른 언어나 플랫폼에서 빌드된 애플리케이션이 통신할 수 있도록 설계된 최초의 표준 프로토콜
  - XML 메시징과 같은 특정 요건이 있음
- 과거에 메시지 형식 및 요청을 표준화하기 위해 개발
  - 다양한 환경에 있거나 다양한 언어로 작성된 애플리케이션이 보다 쉽게 통신할 수 있도록 하기위해 고안
- HTTP(웹 브라우저), SMTP(이메일), TCP 등의 다양한 애플리케이션 레이어 프로토콜을 통해 처리가능


### 프록시서버 (proxy server, 대리 서버)
- 서버와 클라이언트 사이에 중계기 
  - 시스템에 방화벽을 가지고 있는 경우 외부와의 통신을 위해 만들어 놓은 서버
  - 한 마디로, 통신 대리인 셈
  - 외부 인터넷에서는 클라이언트 정보를 알 수 없음
- IP를 바꾸기 위한 용도로 많이 사용
  - 클라이언트가 프록시에 접근하여 인터넷(www.example.com)에 접속하는 방식
- 프록시 서버중 일부는 요청된 내용들을 **캐시를 이용하여 저장**함
  - 동일 요청에 대해 데이터를 가져올 필요가 없음 = 전송 시간 절약
  - 불필요하게 외부와의 연결을 하지 않아도 됨 = 트래픽 감소
  - **네트워크 병목 현상을 방지하는 효과**
  
### 캐시
- 일시적인 특징이 있는 데이터 하위 집합을 저장하는 고속 데이터 스토리지 계층
  - 저장 이후 해당 데이터에 대한 요청이 있을 경우 데이터의 기본 스토리지 위치에 액세스할 때보다 더 빠르게 요청가능
  - 이전에 검색하거나 계산한 데이터를 효율적으로 재사용가능
  
### 로드 밸런싱 (load balancing, 부하분산)
- 다수의 서버(중앙처리장치 혹은 저장장치와 같은 컴퓨터)에게 작업을 나누는 것
  - 가용성 및 응답시간을 최적화 시킬 수 있음
- 다수의 서버를 가지고 한 가지 종류의 인터넷 서비스를 지원
- 트래픽이 많은 웹 사이트, IRC 네트워크, FTP 사이트, NNTP 서버 그리고 DNS서버에 적용

### L4 switch 
> - L2  S/W: OSI 레이어 2(데이터링크)에 속하는 MAC 어드레스를 참조하여 스위칭 하는 장비
> - L3  S/W: OSI 레이어 3(네트워크)에 속하는 IP주소를 참조하여 스위칭 하는 장비
> - L4  S/W: OSI 레이어 3~4(네트워크-전송)에 속하는 IP주소 및 TCP/UDP 포트 정보를 참조하여 스위칭 하는 장비
> - L7  S/W: OSI 레이어 3~7(네트워크-전송-세션-표현-응용)에 속하는 IP주소, TCP/UDP 포트정보 및 패킷 내용까지 참조하여 스위칭

### CA (Certificate Authority, 인증기관)
- 다른 곳에서 사용하기 위한 디지털 인증서를 발급하는 하나의 단위
- 인증 기관은 공개키 인증서를 발급

### ROP(Resource Oriented Architecture, 자원 지향 설계)
- 자원들을 HTTP Method으로 처리하는 기법

### URI(Uniform Resource Identifier)
- 인터넷에 있는 자원을 나타내는 유일한 주소
- 인터넷 프로토콜에 항상 같이 존재
  
### URL(Uniform Resource Locator)
- 인터넷 상에서 자원 위치를 알려주기 위한 규약
- URI > URL (URL안에 URL)

### Last-Modified
- Last-Modified 응답은 HTTP 헤더에 서버가 알고있는 가장 마지막 수정된 날짜와 시각을 가지고 있음
- 저장된 리소스가 이전과 같은지 유효성 검사자로 사용
- ETag 헤더보다는 덜 정확, 대비책으로 사용

### E-Tag
- ETag HTTP 응답 헤더는 특정 버전의 리소스를 식별하는 식별자
- 웹 서버가 내용을 확인하고 변하지 않았으면, 웹 서버로 full 요청을 보내지 않음 = 캐쉬가 더 효율적 = 대역폭을 아낄 수 있음



---
<small>[목차로 돌아가기](/README.md)</small>