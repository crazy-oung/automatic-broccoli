<details>
  <summary>목차</summary>
  <div markdown="1">

- [DAY4 web-security](#day4-web-security)
  - [1. XSS란, 그리고 그 대비책은?](#1-xss란-그리고-그-대비책은)
    - [XSS (cross-site scripting, 사이트 간 스크립팅) ?](#xss-cross-site-scripting-사이트-간-스크립팅-)
    - [XSS 종류/공격기법](#xss-종류공격기법)
    - [1. 반사(Reflected) XSS](#1-반사reflected-xss)
    - [2. 저장(Stored) XSS](#2-저장stored-xss)
    - [3. DOM-based XSS (문서 객체 모델-기반)](#3-dom-based-xss-문서-객체-모델-기반)
    - [XSS 대응 방안](#xss-대응-방안)
    - [CSRF (ross-Site Request Forgery, 사이트 간 요청 위조)?](#csrf-ross-site-request-forgery-사이트-간-요청-위조)
    - [CSRF 대응 방안](#csrf-대응-방안)
    - [CSRF & XSS](#csrf--xss)
  - [2. CORS는 왜 존재하는가?](#2-cors는-왜-존재하는가)
    - [CORS (Cross-Origin Resource Sharing, 교차 출처 리소스 공유)?](#cors-cross-origin-resource-sharing-교차-출처-리소스-공유)
      - [출처 구분](#출처-구분)
    - [CORS 작동 과정](#cors-작동-과정)
    - [Preflight Request](#preflight-request)
    - [Simple Request](#simple-request)
    - [Credentialed Request](#credentialed-request)
    - [CORS를 사용하는 요청](#cors를-사용하는-요청)
      - [CORS 정책 위반 에러 해결](#cors-정책-위반-에러-해결)
  - [3. oAuth2.0 과 jwt (현대적인 로그인 세션(혹은 state) 관리)](#3-oauth20-과-jwt-현대적인-로그인-세션혹은-state-관리)
    - [OAuth2.0?](#oauth20)
    - [OAuth 2.0 Authorization Code Grant](#oauth-20-authorization-code-grant)
    - [OAuth 2.0 Authorization Code Grant 과정](#oauth-20-authorization-code-grant-과정)
    - [JWT (JSON WEB TOKEN)?](#jwt-json-web-token)
    - [JWT 구성](#jwt-구성)
    - [Header](#header)
    - [Payload](#payload)
      - [등록 클레임(Registered Claim)](#등록-클레임registered-claim)
      - [공개 클레임(Public Claim)](#공개-클레임public-claim)
      - [비공개 클레임(Private Claim)](#비공개-클레임private-claim)
    - [Signature](#signature)
    - [JWT 과정](#jwt-과정)
    - [JWT 장점](#jwt-장점)
    - [JWT 단점](#jwt-단점)
    - [JWT 사용](#jwt-사용)
    - [JWT 사용 예](#jwt-사용-예)
    - [로그인 세션(혹은 state) 관리](#로그인-세션혹은-state-관리)
  - [4. 비대칭 암호화 방식 원리](#4-비대칭-암호화-방식-원리)
    - [비대칭 암호화](#비대칭-암호화)
    - [암호화 방식](#암호화-방식)
  - [부록](#부록)
    - [동일 출처 정책(same-origin policy)](#동일-출처-정책same-origin-policy)
  </div>
</details>

---
# DAY4 web-security
## 1. XSS란, 그리고 그 대비책은?
### XSS (cross-site scripting, 사이트 간 스크립팅) ? 
- 웹 애플리케이션에서 많이 나타나는 취약점
  - 웹사이트 관리자가 외부인이 웹 페이지에 악성 스크립트를 삽입
- 웹 서버와 클라이언트간 통신 방식인 HTTP 프로토콜 동작과정 중에 발생
- 이 취약점으로 인해 일어나는 일?
  - 사용자의 정보(쿠키, 세션 등) 탈취 가능
  - 관리자 권한 탈취
  - 자동으로 비정상적인 기능 수행
    - 악성코드 다운로드
  - 다른 웹사이트와 정보를 교환하는 식의 공격
  
### XSS 종류/공격기법
> 스크립팅의 유형을 구분
1. 반사(Reflected) XSS
2. 저장(Stored) XSS
3. DOM 기반 XSS

### 1. 반사(Reflected) XSS
> - 비지속적(Non-persistent) 기법
> - 서버측 취약점
- 지정된 파라미터를 사용할 때 발생
- 웹 클라이언트가 제공하는 HTTP 쿼리 매개 변수(HTML 양식 제출)에 악성 스크립트 포함
  - 검증 되지 않은 사용자가 URL 파라미터나 HTTP 요청 헤더 정보를 수정하여 공격
    1. URL에 악성스크립트를 포함해 다른 사용자에게 전달 (링크, 메일 등)
    2. URL을 받은 사용자가 해당 URL을 통해 조회/열람 (해당 url을 서버에 요청)
    3. 악성스크립트가 포함된 페이지를 웹서버로 부터 받음
    4. 링크를 열어본 사용자의 브라우저에서 스크립트를 실행 = XSS 공격

- ex) 검색창
  - 문자열을 검색 → 해당 문자열을 결과 페이지에서 다시 그대로 표시 = 표시하는 과정에서 스크립트로 인식되어 해당 로직을 실행 = 오동작/취약점이 됨

### 2. 저장(Stored) XSS
> - 지속적(persistent) 기법
> - 비지속적 보다 더 치명적
> - 서버측 취약점
- 웹서버에 스크립트를 영속성으로 저장
  1. 입력필드(폼)을 통해 POST로 스크립트 전달
  2. 서버에서 내용 저장
  3. 다른 사용자가 데이터 열람 = 서버에 요청 → 서버에서 악성스크립트가 저장된 페이지를 전달
  4. 다른 사용자 브라우저에서 스크립트 실행  = XSS 공격
- 공격자가 제공한 데이터를 서버에 저장 → 사용자가 해당 데이터 열람시 스크립트 실행된 페이지를 노출시켜 공격
- 사용자가 읽을 수 있는 HTML 형식의 메시지를 게시 할 수 있는 온라인 게시판에서 주로 발생
  - 악성스크립트가 포함된 게시글을 다른 사용자가 조회하면 조회한 사용자가 피해를 받는 형식

### 3. DOM-based XSS (문서 객체 모델-기반)
> 소스코드 추가로 발생, 클라이언트측 취약점
- 웹서버에 스크립트를 영속성으로 저장
  1. URL에 악성스크립트를 포함해 다른 사용자에게 전달
  2. URL을 받은 사용자가 해당 URL을 통해 조회/열람 (해당 url을 서버에 요청)
  3. 웹 페이지를 웹서버로 부터 받음
  4. 사용자의 브라우저에서 렌더링 하는 과정에서 공격 = XSS 공격
      - 응답받은 웹페이지는 스크립트가 없는 정상 페이지 
      - 렌더링 과정에서 악성 스크립트가 포함된 사용자 입력으로 인해 HTML 태그가 추가되어 공격 발생
- 공격자가 제공한 데이터를 서버에 저장 → 서비스를 제공하는 페이지에서 다른 사용자에게 노출/전달
- 사용자가 읽을 수있는 HTML 형식의 메시지를 게시 할 수 있는 온라인 게시판에서 주로 발생
  - 악성스크립트가 포함된 게시글을 다른 사용자가 조회하면 조회한 사용자가 피해를 받는 형식
- 웹방화벽 의존하거나 서버에서만 필터링을 진행할 경우
  - URL 끝에 #을 두면 브라우저에서 #뒤의 문자를 서버에 전달하지 않으므로 우회 실행이 가능해짐

### XSS 대응 방안
- 입력값 치환
  - script에 사용하는 특수문자를 특수코드로 변환 ex)
    - `<` → &lt;
    - `>` → &gt;
    - `(` → &#40;
    - `)` → &#41;
    - `#` → &#35;
    - `&` → &amp;
  - contents 구문 사용
  - replace 구문 사용
  - 시큐어 코딩 
  - 필터사용
- 입력값 제한
  - 링크 요청을 수행할 때 변수에 대한 값에 스크트가 오지 못하도록 타입 제한
- 서버에 중요 정보를 저장
- 난독화/암호화
- 쿠키에 접속하는 것을 방지
- 인코딩 설정

### CSRF (ross-Site Request Forgery, 사이트 간 요청 위조)?
- 사용자 의지와 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 하는 공격
- 특정 웹사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 형태
  - 로그인한 상태에서 사이트간 요청 위조 공격 코드가 삽입된 페이지를 열람
  - 공격 명령이 믿을 수 있는 사용자로부터 발송된 것이라 판단 => 공격에 노출
- 서버를 공격하는게 주 목적
  - 권한을 도용해 서버를 공격

### CSRF 대응 방안
- CSRF 토큰 사용
- 사용자와 상호 처리 기능 적용
  - referrer 검증
    - request 의 referrer을 확인하여 domain이 일치하는지 검증
- 재인증 요구
  - Security Token 사용
    - 세션에 임의의 난수 값을 저장하고, 사용자의 요청마다 해당 난수 값을 포함 시켜 전송
    - 세션에 저장된 토큰 값과 요청 파라미터에 전달되는 토큰 값이 일치한지 검증

### CSRF & XSS
- CSRF: 서버 공격
  - 권한/요청을 도용
- XSS: 클라이언트 공격
  - 사이트변조/백도어를 이용

---
## 2. CORS는 왜 존재하는가?
### CORS (Cross-Origin Resource Sharing, 교차 출처 리소스 공유)?
> 교차 출처 리소스/자원 공유 = 남의 자원 사용하게 해주는 거
- 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있게 허용
  - 웹페이지에서 교차 출처 이미지, 스타일시트, 스크립트, iframe, 동영상 등을 포함 가능
  - ex) `https://domain-a.com`에 XMLHttpRequest(ajax)를 사용해 `https://domain-b.com/data.json`을 요청하는 경우
  - **브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청을 제한**
    - XMLHttpRequest와 Fetch API는 [동일 출처 정책](#동일-출처-정책same-origin-policy)을 따름
    - 자신의 출처와 동일한 리소스만 불러올 수 있음 => **다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야 함**
    - 즉 CORS 를 통해 교차 출처 리소스를 사용할 수 있게 되는 것 = 타 서비스의 API를 사용가능하게 됨
- 브라우저와 서버 간의 안전한 교차 출처 요청 및 데이터 전송을 지원
  - 최신 브라우저는 API에서 CORS를 사용하여 교차 출처 HTTP 요청의 위험을 완화함
  - 리소스가 안전하다는 최소한의 보장을 받게 해주는 정책
- CORS는 브라우저가 처리
#### 출처 구분
- 두 URL의 구성 요소 중 `Scheme`(`https://`), `Host`(`example.com`), `Port`(`:3000`)가 동일하면 동일 출처

### CORS 작동 과정
1. HTTP 프로토콜을 사용하여 요청 
2. 브라우저는 요청 헤더에 `Origin`이라는 필드에 요청을 보내는 출처를 함께 담아보냄
     - `Origin`: `https://exmple.com`
3. 서버가 요청에 대한 응답 헤더의 `Access-Control-Allow-Origin`에 허용된 출처임을 알림
4. 브라우저에서 서버에서 보내준 `Access-Control-Allow-Origin` 값을 `Origin`과 비교
5. 유효한 응답인지 아닌지 결정
     - 유효한 응답인지를 결정하는 시나리오 3가지
       - Preflight Request
       - Simple Request
       - Credentialed Request

### Preflight Request
- 웹 어플리케이션을 개발할 때
- 요청을 한번에 보내지 않고 예비(Preflight) 요청과 본 요청으로 나누어서 서버로 전송
  - HTTP 메소드 중 `OPTIONS` 메소드 사용
  - 예비 요청 역할: 브라우저에서 요청을 보내는 것이 안전한지 확인
    ```
    요청
    const headers = new Headers({
      'Content-Type': 'text/xml',
    });
    fetch('https://exmple.com/sss', { headers });

    응답
    OPTIONS https://exmple.com/sss

    Accept: */*
    Accept-Encoding: gzip, deflate, br
    Accept-Language: en-US,en;q=0.9,ko;q=0.8,ja;q=0.7,la;q=0.6
    Access-Control-Request-Headers: content-type
    Access-Control-Request-Method: GET
    Connection: keep-alive
    Host: evanmoon.tistory.com
    Origin: 요청을 보낸 주소의 도메인
    Referer: 요청을 보낸 주소
    Sec-Fetch-Dest: empty
    Sec-Fetch-Mode: cors
    Sec-Fetch-Site: cross-site
    ```
    - `Access-Control-Request-Headers`로 본 요청에서 `Content-Type` 헤더를 사용할 것을 알림
    - `Access-Control-Request-Method`로 `GET` 메소드를 사용할 것을 서버에게 미리 알림
    - 결과  
      ```
        OPTIONS https://exmple.com/sss 200 OK

        Access-Control-Allow-Origin: https://exmple.com
        Content-Encoding: gzip
        Content-Length: 699
        Content-Type: text/xml; charset=utf-8
        Date: Sun, 24 May 2020 11:52:33 GMT
        P3P: CP='ALL DSP COR MON LAW OUR LEG DEL'
        Server: Apache
        Vary: Accept-Encoding
        X-UA-Compatible: IE=Edge
      ```
      - `Access-Control-Allow-Origin`에 있는 값에 요청을 보낸 출처 (나의 주소)가 없으면 CORS 정책 위반
- 응답 헤더에 유효한 `Access-Control-Allow-Origin` 값이 존재하는지에 따라 유효성 나뉨


### Simple Request
- 예비 요청 없이 본 요청부터 요청
  - 응답의 헤더에 `Access-Control-Allow-Origin`과 같은 값을 보내주면 이 때 브라우저가 CORS 정책 위반 여부를 검사
- 특정 조건을 만족하는 경우에만 예비 요청 생략가능함
  - 요청의 메소드는 `GET`, `HEAD`, `POST` 중 하나여야 할것
  - 아래를 제외한 헤더를 사용하면 안됨
    > 대부분의 HTTP API에서` text/xml`이나 `application/json` 컨텐츠 타입을 가지므로 위 조건을 충족 시키기 어려움
    - `Accept`, A`ccept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width` 
  - `Content-Type`를 사용하는 경우 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용됨
  
### Credentialed Request
- 인증된 요청을 사용하는 방법
- 서로 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용
  - 비동기 요청에서 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않음
  - `credentials`: 요청에 인증과 관련된 정보를 담을 수 있게 해줌
- **`credentials` 옵션**
  - same-origin (기본값): 같은 출처 간 요청에만 인증 정보 허용
  - include: 모든 요청에 인증 정보허용
  - omit: 모든 요청에 인증 정보를 담지 않음
- `credentials` 옵션중 인증정보를 담는 것을 혀용하는 옵션으로 설정해 요청하면 검사과정을 한번 더 거치게 됨
  - `Access-Control-Allow-Origin: *` 를 시용하면 안됨
  - `Access-Control-Allow-Credentials: true`가 존재해야 함

### CORS를 사용하는 요청
- ajax 요청
  - 비동기 리소스 요청 API `XMLHttpRequest 객체`, `fetch` API 등
- 웹 폰트 (CSS 내 `@font-face`에서 교차 도메인 폰트 사용 시)
- WebGl 
- drawImage() 를 사용해 캔버스에 이미지를 그리거나 비디오를 프레임 할때
- 이미지에서 css shape추출 할 때

#### CORS 정책 위반 에러 해결 
> No ‘Access-Control-Allow-Origin’ header is present on the requested resource. ...
- `Access-Control-Allow-Origin` 헤더에 알맞은 값을 세팅
- Webpack Dev Server 로 리버스 프로싱
  - CORS 정책을 우회가능
    ```
      module.exports = {
        devServer: {
          proxy: {
            '/api': {
              target: 'https://api.test.com',
              changeOrigin: true,
              pathRewrite: { '^/api': '' },
            },
          }
        }
      }
    ```
  - 로컬 환경에서 /api로 시작하는 URL로 보내는 요청에 대해 브라우저는 localhost:8000/api로 요청을 보낸 것으로 처리 
    - 웹팩이 https://api.test.com으로 요청을 프록싱 = CORS 정책 우회가능

---
## 3. oAuth2.0 과 jwt (현대적인 로그인 세션(혹은 state) 관리)
### OAuth2.0?
- 인증을 위한 오픈 스탠다드 프로토콜
- 외부의 서비스 기능을 다른 어플리케이션에서 사용
- 인증 절차가 간략
- 로그인 없이 서비스의 일부 기능을 사용할 수 있게 허용
  - ex) 
    - Facebook을 통해 OAuth를 진행 → Facebook에서 제공하는 API 사용 가능
      - 피드 작성, 친구 목록 불러오기 등
    - 카카오 로그인 기능

### OAuth 2.0 Authorization Code Grant
- Resource Owner : 각종 정보의 주인, 일반적으로 서비스를 이용하는 사용자가 된다.
- Client : 여기서 Client는 OAuth를 사용하는 주체. 즉, 내가 만든 애플리케이션이 된다.
- Authorization Server : 인증과 인가 절차를 담당하는 제3의 서버이다.
- Resource Server : oAuth의 사용을 통해 Client가 원하는 보호된 정보(Resource)를 제공해주는 주체이다.

### OAuth 2.0 Authorization Code Grant 과정
> OAuth2.0에서 주로 사용하는 인증방식
1. 외부앱에서 인증정보를 가지고 요청 
2. 인증 완료 → 자격정보를 반환/ 사용자에 자원 접근 허가
3. 인증코드 발급 → Redirection Url 기반으로 외부 앱에 전달
4. 외부 앱에서 인증코드를 기반으로 사용자 토큰(Access Token, Refresh Token)을 요청
5. 사용자 토큰 반환

### JWT (JSON WEB TOKEN)?
> Token? 문법적으로 더 이상 나눌 수 없는 기본적인 언어요소. ex) 키워드, 연산자, 구두점 등
- 속성정보(claim)를 JSON 데이터 구조로 표현한 웹표준(RFC 7519) 
  - 전자 서명 된 URL-safe JSON
    - Base64 인코딩 사용
    - JSON의 변조를 체크 할 수 있게되어 있음
- 가볍고 자가수용적인(self-contained) 방식으로 정보를 안전성 있게 전달
  - 필요한 모든 정보를 자체적으로 지니고 있음
  - 두 개체 사이에서 쉽게 전달가능
    - HTTP 헤더, URL 파라미터로 전달
- C, Java, Python, C++, R, C#, PHP, JavaScript, Ruby, Go, Swift 등 대부분의 주류 프로그래밍 언어에서 지원
- 토큰 검증과 검증에 필요한 비밀 키를 처리하기 위한 미들웨어가 필요
  - 서명 및 클레임등의 매개 변수를 검사
  - 토큰이 만료되는 경우로 구성

### JWT 구성
![JWT 구성](jwt.png)
- Header(헤더)
- Payload(정보)
- Signature(증명)
  
### Header
- 토큰에 대한 기본정보
> ` {"alg":"HS256","typ":"JWT"}`
- 토근의 타입과 암호화 알고리즘 정보를 포함
- typ : 토큰 타입(토큰의 유형) = JWT
- alg: 해시 암호화 알고리즘( HMAC, SHA256, RSA 등)을 나타냄

### Payload
```
{
    "iss": "test.com",
    "exp": "1485270000000",
    "https://test.com/jwt_claims/is_admin": true,
    "userId": "11028373727102",
    "username": "test"
}
```
- 전달 할 정보를 담음(유저 정보)
- 토큰에 담을 클레임(claim, 전달할 정보)들을 포함
  > 클레임? 페이로드에 담는 한 정보의 단위
  - 등록 클레임(Registered Claim)
  - 공개 클레임(Public Claim)
  - 비공개 클레임(Private Claim)

#### 등록 클레임(Registered Claim)
- 토큰에 대한 정보들을 담기위하여 이름이 이미 정해진 클레임
- 등록된 클레임은 모두 선택적으로 사용
- iss: 토큰 발급자 (issuer)
- sub: 토큰 제목 (subject)
- aud: 토큰 대상자 (audience)
- exp: 토큰의 만료시간 (expiraton)
  - 시간은 NumericDate 형식(1480849147370)  
  - 현재 시간보다 이후로 설정되어 있어야함
- nbf: 토큰의 만기 날짜  
  - NumericDate 형식
  - 날짜가 지나기 전까지는 토큰이 처리되지 않음
- iat: 토큰이 발급된 시간 (issued at)
  - 발금한지 얼마나 됐는지 식별 가능
- jti: JWT의 고유 식별자로서
  - 주로 중복적인 처리를 방지하기 위하여 사용
  - 일회용 토큰에 사용

#### 공개 클레임(Public Claim)
- 충돌이 방지된 (collision-resistant) 이름
- `{ "https://test.com/jwt_claims/is_admin": true }`

#### 비공개 클레임(Private Claim)
- 클라이언트 ←→ 서버간 협의된 클레임 이름
- 이름이 중복되어 충돌날 수 있음 `{"username" : "test"}`

### Signature
- 토큰이 검증됐다는것을 증명해주는 Signature
- secret key를 포함하여 암호화되어 있음
- 헤더의 인코딩값과, 정보의 인코딩값을 합친후 주어진 비밀키로 해쉬해 생성

### JWT 과정
1. 사용자 로그인
   - 처음 사용자를 등록할 때는 엑세스 토큰이랑 리프레쉬 토큰이 발급 되어야 함
2. 서버에서 요청 확인후 secret key를 통해 액세스 토큰 발급
3. JWT토큰을 클라이언트에 전달
4. 이후 모든 요청에 토큰을 함께 보냄
5. 서버는 이 토큰에서 signature를 검사한 후 Payload에서 유저 정보를 확인해 반환

### JWT 장점 
- 클라이언트 로그인 정보를 서버 메모리에 담아두지 않음
  - 세션과 달리 서버측 부하를 낮출 수 있게됨
  - 능률적인 접근 권한 관리 가능
- JWT에 필요한 정보를 담아 둘수 있어 서버와 상호작용할 때 오버 헤드를 최소화 할 수 있음
  - 서버와 데이터베이스에 의존하지 않음
  - 쉬운 인증 및 인가 방법 제공
- 분산/클라우드 기반 인프라스트럭처에 수월하게 대응 가능
- CORS는 쿠키를 사용하지 않는데 이때 JWT를 사용힌 API를 사용해도 문제가 발생하지 않음 
- REST 서비스로 제공 가능
- URL  파라미터와 헤더로 사용
- 디버깅 및 관리가 용이
- 트래픽 대한 부담이 낮음
- 만료 정보가 내장되어 있음

### JWT 단점
- 정보를 담는 필드가 추가되면 토큰의 크기가 커짐
- 비상태 애플리케이션인 경우에 모든 요청에 대해 토큰이 함께 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있음
- 클리어언트에 저장되므로 데이터베이스에서 사용자 정보를 변경해도 토큰에 해당 변경사항을 적용할 수 없음

### JWT 사용
- 서버와 클라이언트 간 정보를 주고 받을 때 Http 리퀘스트 헤더에 JSON 토큰을 넣음
- 서버는 별도의 인증 과정 없이 헤더에 포함되어 있는 JWT 정보를 통해 인증
- API 에서 클라이언트에 대한 Authentication(인증)과 Authorization (권한부여)가 필요할 때 
### JWT 사용 예
- 회원인증
  - 로그인 → 유저의 정보에 기반한 토큰을 발급하여 유저에게 전달 
  - 매 요청마다 JWT를 포함하여 전달
  - 서버는 클라이언트에게 요청 받을 때마다 토큰의 유효성을 검사 
  - 유저가 요청한 작업에 권한이 있는지 확인 → 요청 처리
- 정보교류
  - 정보를 보내거나 요청한 곳에 대한 출처를 검사해 조작되었는지를 검증 할 수 있음




### 로그인 세션(혹은 state) 관리



---
## 4. 비대칭 암호화 방식 원리
### [비대칭](/Day1/README.md#비대칭키) 암호화
- 

### 암호화 방식 



---
## 부록
### 동일 출처 정책(same-origin policy)
- 어떤 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하는 중요한 보안 방식
- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄여줌