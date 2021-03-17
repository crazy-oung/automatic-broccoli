<details>
  <summary>목차</summary>
  <div markdown="1">

- [DAY2 network](#day2-network)
  - [1. 웹페이지가 출력되는 과정](#1-웹페이지가-출력되는-과정)
    - [주소를 치면 일어나는 일](#주소를-치면-일어나는-일)
    - [브라우저에 데이터가 전달되는 과정](#브라우저에-데이터가-전달되는-과정)
    - [DOM (Document Object Model)?](#dom-document-object-model)
    - [HTML을 DOM으로 변환하는 과정](#html을-dom으로-변환하는-과정)
    - [CSSOM (CSS Object Model)?](#cssom-css-object-model)
    - [Render-Tree?](#render-tree)
    - [렌더링 트리 만드는 과정](#렌더링-트리-만드는-과정)
    - [브라우저가 보여주는 과정](#브라우저가-보여주는-과정)
  - [2. http 통신이란 무엇인가?](#2-http-통신이란-무엇인가)
    - [HTTP (Hypertext Transfer Protocol)?](#http-hypertext-transfer-protocol)
    - [브라우저가 HTTP방식으로 서버로부터 데이터를 받는 과정](#브라우저가-http방식으로-서버로부터-데이터를-받는-과정)
    - [HTTPS (HTTP + Secure)](#https-http--secure)
    - [대칭키](#대칭키)
    - [비대칭키](#비대칭키)
    - [신뢰할 수 있는 사이트?](#신뢰할-수-있는-사이트)
    - [HTTP vs HTTPS](#http-vs-https)
  - [3. web server의 역할](#3-web-server의-역할)
    - [web server?](#web-server)
    - [웹서버와 WAS는 다르다.](#웹서버와-was는-다르다)
    - [WAS(Web Application Server)](#wasweb-application-server)
    - [WAS를 쓰는 이유](#was를-쓰는-이유)
    - [Web Server 와 WAS의 동작 과정](#web-server-와-was의-동작-과정)
  - [4. OSI 7 layer 계층별 역할](#4-osi-7-layer-계층별-역할)
    - [OSI(Open Systems Interconnection) 7계층](#osiopen-systems-interconnection-7계층)
    - [OSI 7계층의 PDU](#osi-7계층의-pdu)
      - [PDU(Process Data Unit)](#pduprocess-data-unit)
    - [각 계층 별 특징 및 역할](#각-계층-별-특징-및-역할)
      - [1. 물리 계층(Physical layer)](#1-물리-계층physical-layer)
      - [2. 데이터 링크 계층(Data link layer)](#2-데이터-링크-계층data-link-layer)
      - [3. 네트워크 계층(Network layer)](#3-네트워크-계층network-layer)
      - [4. 전송 계층(Transport layer)](#4-전송-계층transport-layer)
      - [5. 세션 계층(Session layer)](#5-세션-계층session-layer)
      - [6. 표현 계층(Presentation layer)](#6-표현-계층presentation-layer)
      - [7. 응용 계층(Application layer)](#7-응용-계층application-layer)
    - [TCP/IP 모델](#tcpip-모델)
    - [캡슐화와 디캡슐화](#캡슐화와-디캡슐화)
  - [5. rest api](#5-rest-api)
    - [API (Application Programming Interface)?](#api-application-programming-interface)
    - [REST (Representational State Transfer)?](#rest-representational-state-transfer)
    - [REST/RESTful API?](#restrestful-api)
    - [REST 6원칙](#rest-6원칙)
    - [REST API가 쓰는 방식](#rest-api가-쓰는-방식)
  - [부록.](#부록)
    - [DNS (Domain Name System)?](#dns-domain-name-system)
    - [REST API와 SOAP](#rest-api와-soap)
    - [SOAP (Simple Object Access Protocol)?](#soap-simple-object-access-protocol)
    - [로드 밸런싱](#로드-밸런싱)
    - [공유캐싱](#공유캐싱)
    - [SSL](#ssl)
  </div>
</details>
---

# DAY2 network

## 1. 웹페이지가 출력되는 과정
### 주소를 치면 일어나는 일
1. 브라우저에 URL 입력시 [DNS](#dns-domain-name-system)서버에서 진짜 주소(ip) 가져옴
  - **feed dns서버도 순서가 있음. 어디서부터 받아오는지 확인해볼 것**
2. 웹 페이지를 표시하기 위해 필요한 자료를 요청
  - **feed 아 여기도 조금 아쉽긴한데, 결국 특정 ip(서버) 내 특정 폴더내의 파일, 일반적으로 index.html에 접근함까지 있으면 좋을듯**
3. 웹 사이트에서 요청을 받고 승인후에 브라우저에게 정보를 전송
  - **feed사실 이 전송하는과정도 client <=> server 로 바로 되지는 않음, 거치는 서버들이 굉장히 많음. 그것에 대한 인지가 필요함(why? 프록시서버와 캐시에대한 이해에 필요하기 때문)**
4. 브라우저는 웹 사이트에서 받은 정보들을 조립하여 유저에게 보여줌
> - [http](#http-hypertext-transfer-protocol) 통신을 통해 받기 때문에 Request(요청)와 Response(응답)이 발생 
>   - **Request**? 클라이언트(브라우저)가 서버에게 요청하는 사항이 정리된 객체
>   - **Response**?  Request에서 받은 요청을 처리하고 그에 해당하는 데이터를 담은 객체
> - Request(요청)후 Response(응답)을 받음
- 브라우저는 마지막에 받은 정보를 가지고 웹페이지를 그림
  - HTML은 [DOM](#domdocument-object-model)로 변환되며 CSS는 [CSSOM](#cssom-css-object-model)으로 변환
  - [Render-Tree](#render-tree), [Paint](#paint) 발생
    > 바이트 > 문자 > 토큰 > 노드 > 객채 모델 순으로 해석
    - **feed 여기까지 대답하되, 좀 더 상세하게 페인트 과정을 설명하면 FE개발자 신입공채 위 질문에 대한 점수는 100점 ㅋ**

### 브라우저에 데이터가 전달되는 과정
> [http](#http-hypertext-transfer-protocol) 통신

### DOM (Document Object Model)?
- 필요한 자료를 요청해 바이트(Bytes) 상태로 받은 HTML파일

### HTML을 DOM으로 변환하는 과정 **feed 여기 완벽한 답이 있군 ㅋㅋㅋㅋ**
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
4. 완료된 렌더링 트리를 기준으로 브라우저가 화면에 그림
- **feed 위 과정을 토대로 웹페이지가 구성된다고 한다면, 구글에 돌아다니는 fontend theme?들의 DOM 구성요소(html tags)들을 보면 굉장히 depth가 깊음. why? 더 짜임새 있고, 웹 페이지를 잘 구성하려면 구조를 최대한 쪼개야하므로 depth가 깊을 수 밖에 없음. 그 결과 DOM트리가 굉장히 복잡해짐**
- **feed DOM트리가 복잡해지면 발생하는 현상, jquery로 직접 애ㅡ**

### 브라우저가 보여주는 과정
1. HTML 마크업을 처리하고 DOM 트리를 빌드
2. CSS 마크업을 처리하고 CSSOM 트리를 빌드
3. DOM 및 CSSOM을 결합하여 렌더링 트리를 형성
4. 렌더링 트리에서 레이아웃을 실행하여 각 노드의 기하학적 형태를 계산
5. 개별 노드를 화면에 페인트


---
## 2. http 통신이란 무엇인가?
### HTTP (Hypertext Transfer Protocol)?
- 인터넷에서 데이터를 주고받기 위한 약속
  - 브라우저는 http 프로토콜을 이용해 전달하고 요청한 데이터를 전달받음
- 주로 TCP를 사용하고 HTML문서를 주고받는데 사용
- 요청(Request)과 응답(Response)으로 구성
  - 클라이언트가 서버에 정보를 요청하면 응답 코드와 내용을 전송하고 클라이언트와 연결을 종료
- 전송할 때 일반 텍스트 타입으로 전송됨

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

### 대칭키
- 암호화할때 사용
- 기존 방식은 대칭키 방식
  - 메세지 송수신을 하는 곳에서 같은 키를 공유함
  - 공유되는 하나의 키를 가지고 암호화/복호화를 진행함
- 대칭키가 노출되는 경우 중간에서 정보를 탈취할 가능성이 높아짐

### 비대칭키
- 대칭키의 단점을 보완하고자 나옴
- 서로 다른 쌍의 키로 암복호화를 진행함
  - 하나로 암호화하고 다른 하나로 복호화를 진행
  - 공개키로는 암호화된 정보를 복호화 할 수 없으므로 보안 문제가 해결됨

### 신뢰할 수 있는 사이트?
- 공개키를 가지고 
  - 신뢰할 수 있는 기관에서 이를 검증해줘야함
    - 검증받으면 CA(Certificate Authority)가 생성됨
    - CA는 브라우저에 내장되어 있음 
  - 탐색과정(핸드쉐이크)를 통해 검증
    - 서버에 요청을 보내면 인증서를 사이트에서 같이 보내옴
    - 브라우저에서 CA를 가지고 인증서를 복호화하게됨
    - 만일 브라우저에서 CA과정에 실패하면 인증되지 않은 사이트로 신뢰할 수 없는 사이트가 됨.

### HTTP vs HTTPS
- 개인정보나 민감한 정보를 사용할 경우 HTTPS를 사용
- 암호화될 필요가 없는 데이터만 있을 경우 HTTP 사용
- 모든 통신에 HTTPS 를 사용할 경우 암호화가 불필요한 정보도 암호화 하게되므로 부하가 높고 효율적이지 못함
  - 그러므로 두 방식을 혼용해서 사용함

---
## 3. web server의 역할
### web server?
- HTML, CSS, IMG 등과 같이 정적인 요소의 요청을 받아 처리하는 곳
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
- 하위 계층(물리-데이터링크, 네트워크, 전송)은 하드웨어로, 상위 계층(세션-표현-응용)은 소프트웨어로 구현

### OSI 7계층의 PDU
#### PDU(Process Data Unit)
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
- 프레임에 주소부여(MAC - 물리적주소)
- 에러검출/재전송/흐름제어
- **이더넷**, 토큰링, FDDI,
- PDU: 프레임(Frame)
- 브릿지, 스위치
- 포인트 투 포인트(Point to Point) 간 **신뢰성있는 전송을 보장**하기 위한 계층
  - CRC 기반의 오류 제어와 흐름 제어가 필요
  - 네트워크 위의 개체들 간 데이터를 전달

#### 3. 네트워크 계층(Network layer)
- 주소부여(IP)
- 경로설정(Route)
- **IP**, ICMP, IPsec, ARP, RIP, BGP
- PDU : 패킷(Packet)
  - 패킷단위로 전송 후 다시 합쳐짐
- 라우터, L3 스위치
- 여러개의 노드를 거칠때마다 **경로를 찾아주는 역할**을 하는 계층
  - 다양한 길이의 데이터를 네트워크들을 통해 전달
  - 전송 계층이 요구하는 서비스 품질(QoS)을 제공하기 위한 기능적, 절차적 수단을 제공
- 한마디로, 각 패킷이 성공적이고 효과적으로 전달 되도록 함

#### 4. 전송 계층(Transport layer)
- **종단간(end-to-end) 통신을 다룸**
- 종단간 신뢰성 있고 효율적인 데이터를 전송하며, 오류검출 및 복구와 흐름제어, 중복검사 등을 수행
  - **TCP**, UDP, RTP, SCT
- 양 끝단(End to end)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해줌
  - 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않음

#### 5. 세션 계층(Session layer)
- 통신을 하기 위한 세션을 확립/유지/중단 (운영체제가 해줌)
- **TCP의 세션 관리** 부분
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

### TCP/IP 모델
> 클라이언트가 요청하면 응용 > 전송 > 네트워크 > 네트워크 인터페이스 계층 순으로 통과
- 응용 > 표현 > 세션 [응용 계층]
- 전송 [전송 계층]
- 네트워크 [네트워크 계층]
- 데이터링크 > 물리 [네트워크 인터페이스 계층]

### 캡슐화와 디캡슐화
- TCP/IP 통신을 하면 하위 계층으로 가면서 디캡슐화 하고 하위에서 상위로 올라면서 캡슐화 함
- 데이터를 전송할 때 각각의 층마다 인식할 수 있어야 하는 헤더를 붙임
  - 각 단계별로 헤더를 붙여서 캡슐화 하게됨
  - 반대로 요청한 데이터를 받을때는 헤더를 없에는 디캡슐화를 진행함
  

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
- 클라이언트가 요청을 하면 JSON타입의 데이터를 받을 수 있게 만드는 것
- HTTP: JSON(Javascript Object Notation), HTML, XLT 또는 일반 텍스트를 통해 몇 가지 형식으로 전송

### REST/RESTful API?
- REST 아키텍처의 제약 조건을 준수하는 API
- 웹 서비스와 모바일 애플리케이션 **경량화**의 필요에 맞춘 아키텍처 원칙 세트/스타일
- 하이퍼텍스트 전송 프로토콜(HTTP)을 통해 이루어짐
  - HTTP 프로토콜의 Method을 사용함
  
### REST 6원칙
1. 표준화된 형식으로 정보를 전송할 수 있도록 구성 요소 간 **통합된(일관적인) 인터페이스** 필요
2. 요청하는 서버에 클러이언트의 콘텐츠가 저장되지 않는 **무상태성(stateless)**
      - 세션 정보가 클라이언트에 저장됨
3. 응답을 캐싱할 수 있어야 하는 **캐시 처리 가능성** 
     - 상호 작용의 필요성을 제거할 수 있음
4. 클라이언트-서버 간의 상호 작용을 계층적으로 조정할 수 있도록 **계층화**된 시스템 제약이 필요
     - 로드 밸런싱 기능이나 공유 캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는데 유용함
5. 실행 가능한 코드를 전송해 서버가 클라이언트의 기능을 확장할 수 있게 해주는 **코드 온디맨드(Code on demand)**
     - 자바 애플릿이나 자바스크립트의 제공을 통해 서버에서 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있음
6. 클라이언트, 서버, 리소스로 구성된 **클라이언트-서버 아키텍처**가 필요
     - 아키텍처를 단순화시키고 작은 단위로 분리함
       - 각 파트가 독립적으로 개선될 수 있음
  
### REST API가 쓰는 방식
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



---
## 부록.

### DNS (Domain Name System)?
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
  - 기본 보안과 트랜잭션 컴플라이언스를 제공
  - = 무거움
### SOAP (Simple Object Access Protocol)?
- 다른 언어나 플랫폼에서 빌드된 애플리케이션이 통신할 수 있도록 설계된 최초의 표준 프로토콜
  - XML 메시징과 같은 특정 요건이 있음
- 과거에 메시지 형식 및 요청을 표준화하기 위해 개발
  - 다양한 환경에 있거나 다양한 언어로 작성된 애플리케이션이 보다 쉽게 통신할 수 있도록 하기위해 고안
- HTTP(웹 브라우저), SMTP(이메일), TCP 등의 다양한 애플리케이션 레이어 프로토콜을 통해 처리가능

### 로드 밸런싱

### 공유캐싱

### SSL


---
<small>[목차로 돌아가기](/README.md)</small>