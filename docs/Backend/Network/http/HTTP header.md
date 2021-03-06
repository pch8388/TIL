# HTTP header
- header-field = field-name":"OWS field-value OWS (OWS : 띄어쓰기 허용)
- field-name 은 대소문자 구문 없음
```
GET /search?q=hello
Host: www.google.com
```

## 용도
- HTTP 전송에 필요한 모든 부가정보 기술
- 필요시 임의의 헤더 추가 가능

## 분류
- General 헤더 : 메시지 전체에 적용되는 정보
- Request 헤더 : 요청정보
- Response 헤더 : 응답정보
- Entity 헤더 : 엔티티 바디 정보

## RFC723x 변화
- 엔티티 -> 표현(Representation)
- 표현 : 표현 메타데이터 + 표현 데이터
- 메시지 본문(payload) 를 통해 표현 데이터 전달

## 표현(Representation)
- 전송, 응답에 둘다 사용
- 서버에서 어떤 형태로 데이터를 내려줄지 알려줌
- Content-Type : 표현 데이터 형식 (ex: application/json)
- Content-Encoding : 표현 데이터의 압축 방식 (ex: gzip)
- Content-Language : 표현 데이터의 자연어 (ex: en)
- Content-Length : 표현 데이터의 길이(바이트 단위), Transfer-Encoding 사용시 같이 쓰면 안됨

## Content negotiation
- 클라이언트가 선호하는 표현 요청
- Accept : 미디어 타입
- Accept-Charset : 문자 인코딩
- Accept-Encoding : 압축 인코딩
- Accept-Language : 언어
- Content negotiation 헤더는 요청시에만 사용

### 우선순위
- Quality Values(q) 값 사용
- 0 ~ 1 클수록 높은 우선 순위를 가짐(생략하면 1)
- Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8
  - ko-KR => Quality Values 1 (생략)
  - ko => Quality Values 0.9
  - en-US => Quality Values 0.8
- 우선순위에 따라 서버에서 지원하는 언어 중에 포함된 것을 반환할 수 있게 요청한다.
- 구체적인 것을 우선함 => 좀 더 나타내는 폭이 좁은(명시적인) 요청을 우선순위로 한다.

## 전송방식
- 단순전송 : 요청 <-> 응답(Content-Length 를 지정 => 컨텐츠에 대한 길이를 알때), 한번에 요청하고 한번에 받음
- 압축전송 : 서버에서 gzip 등으로 응답을 압축(Content-Encoding: gzip 과 같이 압축 인코딩을 응답)
- 분할전송 : 서버에서 전송시 응답을 나눠서 전송 (Transfer-Encoding: chunked) => Content-Length 를 보내면 안됨
- 범위전송 : 범위를 지정해서 요청(Range) <-> 범위에 대해 응답(Content-Range)

## 일반정보
- From : 유저 에이전트의 이메일 정보 => 잘 사용하지 않음, 검색엔진 등에 정보 제공을 위해 사용
- Referer : 이전 페이지 정보, 유입경로 분석 등에 이용한다
- User-Agent : 클라이언트 애플리케이션 정보
- Server : 요청을 처리하는 origin(실제 응답을 요청하는) 서버의 소프트웨어 정보

## 특별한 정보
- Host : 요청한 호스트 정보(도메인) => 실제로 어떤 도메인에 요청을 한 건지에 대한 정보를 가지고 있어야 한다
- Location : 페이지 리다리렉션, 300번대 응답이면 이 헤더로 어디로 리다이렉션 해야할지 알려줌
- Allow: 405 응답을 내리면서 어떤 메소드를 응답하는 지 알려줌
  - 405 : 해당 메소드가 서버에서 지원하지 않을 때 응답하는 http status
- Retry-After : 503 응답일때 서비스가 언제부터 이용가능한지 알려주는 헤더
  - 503 : 서비스가 불가할 때 응답해주는 http status
  
## 인증
- Authorization : 클라이언트의 인증정보를 서버로 전달
- XXX-Authenticate : 401응답과 함께 어떤 인증방식을 사용해야 하는지 알려줌
  - 401: 인증이 되지 않았을 때 응답하는 http status
  [참고](https://github.com/pch8388/til/blob/master/docs/read-book/%EB%A6%AC%EC%96%BC%EC%9B%94%EB%93%9Chttp/2%EC%9E%A5.md#%EC%9D%B8%EC%A6%9D%EA%B3%BC-%EC%84%B8%EC%85%98)

## 쿠키
- Set-Cookie: 응답(서버 -> 클라 전달)
  - 웹 브라우저의 쿠키 저장소에 저장(document.cookie) => HttpOnly 설정되있으면 js 에서 접근할 수 없음  
- Cookie: 요청 (클라 -> 서버 전달)
- 사용처 
  - 사용자 로그인 세션 관리
  - 광고정보 트래킹 
- 쿠키정보는 항상 서버로 전송되기 때문에 최소한의 정보만 사용하는 것이 좋음
- 서버에 계속 전송할 필요가 없는 경우(선택적으로 서버로 보내거나 클라이언트에서만 사용할 경우)에는 Web storage (localStorage, sessionStorage)를 사용
- 보안에 민감한 데이터는 저장하면 안됨 (Secure 설정을 통해 https 전송에서만 사용할 수 있지만 안전하다고 보장할 수는 없음)

### 쿠키의 생명주기
- expires = 만료일이 되면 쿠키 삭제
- max-age = 초단위로 쿠키 생명주기 설정
- 세션쿠키 : 만료날짜 생략시 브라우저 종료시까지 유지
- 영속쿠키 : 만료날짜 생략시 영구적으로 유지

### 쿠키 - 도메인
- 명시 : 명시한 문서 기준 도메인과 서브도메인(domain=mozilla.org)
- 생략 : 현재 문서 기준 도메인에만 적용(서브도메인에서는 접근할 수 없음)

### 쿠키 - 경로
- 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 루트로 지정(path=/)

### 쿠키 - 보안
- Secure : https 일때만 서버로 전송
- HttpOnly : XSS 공격 방지, js 에서 접근 불가
- SameSite : XSRF 공격 방지, 요청 도메인과 설정된 도메인이 같은 경우만 쿠키 전송

## 캐시
- 캐시가 없으면 항상 네트워크를 통해 데이터를 다운로드 받아야 하기 때문에 리소스의 낭비가 심하다
- Cache-control 이라는 응답헤더로 최대 캐시 시간을 지정할 수 있고, Cache-control 이 적용된 응답에 대해서는 브라우저에서 캐시저장소에 저장해두고, 만료 시간 전에 재 요청시에는 브라우저의 캐시저장소에 있는 데이터를 불러온다(속도가 빨라짐)

### 캐시 - 검증헤더와 조건부 요청
- 검증헤더 : Last-Modified: 데이터가 마지막에 수정된 시간, ETag: 고유 버전을 달아둠
  - Last-Modified <-> If-modified-since
    - 수정이 안되었으면 304 Not Modified
    - 수정이 되었다면 200 OK 와 함께 body 가 전송
    - 1초 미만 단위로 캐시조정 불가능
    - 날짜 기반의 로직 사용
    - 데이터를 수정했지만 데이터 결과가 똑같은 경우나 서버에서 별도의 캐시 로직을 관리하고 싶은 경우에는 사용할 수 없다
  - ETag <-> If-None-Match
    - 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
    - hash 값으로 생성하여 데이터가 변경되면 이름을 바꾸어서 변경할 수 있다
    - 단순하게 ETag만 보내서 같으면 유지하고 다르면 다시 받도록 전략을 짤 수 있다 
- 캐시 유효시간이 초과해서 서버에 다시 요청시 
  1.Last-Modified 헤더가 이전응답에서 있었다면
  2.if-modified-since 를 보내서 리소스 갱신을 확인 
  3.응답으로 304 Not Modified 를 보냄(Http Body 가 없이 보낸다) 
  4.브라우저는 이 응답을 받으면 캐시를 사용
- 서버에서 기존 데이터 변경 => 서버에서 재전송
- 서버에서 기존 데이터 변경하지 않음 => 캐시를 다시 쓰라고 알려줌

### 캐시 제어 헤더
- Cache-Control : 캐시가 유효한 시간(초단위), 현재는 가장 많이 씀
  - no-cache : 데이터는 캐시해도 되지만, 항상 origin 서버에서 검증하고 사용
  - no-store : 데이터에 민감 정보가 있으므로 저장하지 않도록한다.(메모리에서만 사용)

### 프록시 캐시
- CDN 서비스 같은 것이 이에 해당 할 수 있음
- Cache-Control
  - public : 프록시에도 저장
  - private : 응답이 해당 사용자만을 위한 것 
  - s-maxage : 프록시 캐시에만 적용되는 만료기간

### 캐시 무효화
- 확실한 캐시 무효화 응답 => 웹브라우저가 임의로 캐시하는 경우가 있음(특히 get 요청)
- 캐시 무효화를 사용하여 캐시를 절대 하지 않도록 한다
  - Cache-Control: no-cache, no-store, must-revalidate
  - Pragma: no-cache => http1.0 하위호환을 위해 넣음
- must-revalidate : 캐시 만료후 최초 조회시 origin 에 검증, origin 접근 실패시 반드시 오류가 발생(504), 캐시 유효기간이면 캐시 사용

[참고강의](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
