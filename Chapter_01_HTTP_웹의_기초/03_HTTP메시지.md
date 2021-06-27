# 3. HTTP 메시지
## 3.1 메시지의 흐름
- HTTP 메시지? HTTP 애플리케이션 간에 주고받은 데이터의 블록
- 클라이언트 >>>> 서버(인바운드)
- 서버 >>>> 클라이언트 (아웃바운드)

## 3.2 메시지의 각 부분
- 시작줄(어떤 메시지) / 헤더(속성) / 본문(데이터)
- 시작줄과 헤더는 줄 단위로 분리된 아스키 문자열
- 줄바꿈 문자열(CRLF)로 끝난다
- 본문은 데이터 덩어리

### 3.2.1  메시지 문법
1) 요청 시
```
<메서드> <요청URL> <버전>    ---- 시작줄
<헤더>                    ---- 헤더
                              (CRLF = 끝)
<본문>                    ---- 본문
```


2) 응답 시
```
<버전><상태코드><사유>
<헤더>

<본문>
```
- 메서드: 리소스에 대해 서버가 수행해주길 바라는 동작 (GET, POST, HEAD, POST)
- 요청 URL: 요청 대상이 되는 리소스를 지칭하는 URL 혹은 경로 구성 요소
- 버전: HTTP 버전
- 상태코드: 요청 중에 무슨 일이 일어났는지
- 사유: 상태코드를 사람이 이해할 수 있게 설명
- 헤더: 이름, 콜론, 공백, 값, CRLF
- 본문: 데이터 블록 포함(모든 메시지에 다 있는 건 아님)

### 3.2.2 시작줄
- 요청 시
    - 서버에게 부탁하는 동작
    - 동작에 대한 대상을 지칭하는 URL
    - HTTP 버전
    - 위 필드는 공백으로 구분
- 응답 시
    - 수행 결과
    - 상태 정보
    - 결과 데이터를 클라이언트에 반환
    - 필드는 공백으로 구분

- 메서드
```  
 GET     서버에서 문서를 가져옴
 HEAD    서버에서 문서의 헤더만 가져옴
 POST    서버가 처리할 데이터를 보냄 (메시지 본문 있음)
 PUT     서버가 요청 메세지의 본문 저장(메시지 본문 있음)
 TRACE   메시지가 프락시를 거쳐 서버에 도달하는 과정 추적
 OPTIONS 서버가 어떤 메서드를 수행할 수 있나 확인
 DELETE  서버에서 문서 제거
  ```
  
- 상태코드
```
100~  정보
200~  성공
300~  리다이렉션
400~  클라이언트 에러
500~  서버 에러
```

- 사유 구절
    - 상태코드를 글로 설명한 것
    - 예) 200 OK, 401 Unauthorized 처럼 코드 옆에 설명된 문자열

- 버전 번호
- 요청, 응답 메시지 모두에 기술됨
- 애플리케이션이 그 버전까지 이해할 수 있다는 의미

### 3.2.3 헤더
- 이름:값 쌍의 목록
    - 일반헤더: 요청, 응답 양쪽에 모두 나타날 수 있음
    - 요청헤더: 요청에 대한 부가 정보
    - 응답헤더: 응답에 대한 부가 정도
    - Entity 헤더: 본문 크기, 콘텐츠, 리소스 서술
    - 확장헤더: 명세에 정의되지 않은 새로운 헤더
- 예시
```
Date: Tue.3 Oct 2020 02:02:02 GMT
Content-length: 11040
Content-type: image/gif
Accept: imgage/gif, image/jpeg, text/html
```

### 3.2.4 엔터티 본문
- HTTP가 수송하도록 설계된 것
- 이미지, 비디오, HTML문서 등등..

## 3.3 메서드
### 3.3.1 GET
- 서버에 리소스 요청
- HTTP/1.1 준수를 위해 구현되어 있어야 함

### 3.3.2 HEAD
- 리소스를 요청하나 서버는 응답으로 헤더만 돌려 줌
- 리소스를 다 안 받아도 그게 무엇인가를 알아낼 수 있음
- 상태코드로 존재 유무를 파악할 수 있음
- 리소스가 변경되었는지 검사할 수 있음
- HTTP/1.1 준수를 위해 구현되어 있어야 함

### 3.3.3 PUT
- 서버에 문서를 쓴다
- 서버가 요청 본문을 가지고 URL 이름대로 새 문서를 만들거나 본문을 교체할 것을 요청
- 콘텐츠를 변경

### 3.3.4 POST
- 서버에 입력 데이터를 전송
- 데이터는 메세지 본문에 들어감

### 3.3.5 TRACE
- 요청이 서버에 도달했을 때 어떻게 보이는지 알려줌(요청은 방화벽, 프록시 등을 거쳐 지나갈 수 있고 그 과정에서 수정될 수도 있음)
- 서버에서 루프백(loopback) 진단을 통해 받은 요청 메시지를 본문에 넣어 응답으로 보냄 > 클라이언트는 받아서 자신이 보낸 메시지가 망가졌나 수정됬나 어떻게 변경되었나 확인.
- 주로 진단 목적(의도한 요청/응답 연쇄를 거치는가. 다른 애플리케이션이 요청에 어떤 영향을 미치나)
- 요청에 본문 보낼 수 없음
- 응답 본문은 서버가 받은 요청

### 3.3.6 OPTIONS
- 서버에서 어떤 메서드를 지원하는지 확인

### 3.3.7 DELETE
- 요청 URL로 지정한 리소스를 삭제 요청

### 3.3.8 확장 메서드
- HTTP/1.1 명세에 정의되지 않은 메서드
- 필요에 따라 새로운 기능을 추가해서 씀
- 예) LOCK(리소스 잠금), MKCOL(문서 생성), COPY(복사), MOVE..

## 3.4 상태코드
### 3.4.1 100-199: 정보성 상태코드
```
100  Contine             요청 시작 일부가 받아들여졌다. 
101  Switching protocols 서버가 프로토콜을 바꿨다.
```
- 클라이언트 > 서버: 100-continue(엔터티를 보내기 전에 확인. 100 continue 응답 기다리지만 말고 타임아웃 후 보내야 함)
- 서버> 클라이언트: 100-continue 요청을 받으면 continue 응답 or 에러를 보내야함. 응댭 전에 엔터티 받으면 상태코드 안 보내도 됨.

### 3.4.2 200-299: 성공 상태 코드
```
200  OK          요청 정상. 본문은 요청된 리소스 포함
201  Created     개체 생성 요청에 대한 응답. 리소스 Location헤더와 함께 참조할 수 있는 URL을 본문에 포함
202  Accepted    요청은 받았다. 아직 동작 수행x
203  Non-authoritative Information      엔터티 헤더가 원래 서버가 아닌 리소스 사본에서 왔다.
204  No Content     헤더, 상태줄 포함. 본문x. 새 문서로 이동하지 않고 리프레시 할 때
205  Reset Content  브라우저에게 현재 페이지 HTML폼의 값을 비워라.
206  Partial Content   부분 요청 성공
``` 

### 3.4.3 300-399: 리다이랙션 코드
- 리소스가 옮겨졌으면 그 위치를 알려줌(Location헤더와)
- 로컬 복사본이 원래 서버와 비교했을 때 유효한지 확인(If-Modified-Since 요청 헤더)
```
300  Multiple Choices     여러 리소스를 가리키는 URL을 요청했을 때 그 목록과 반환
301  Moved Permenantly    요청한 URL이 옮겨졌음. Location 헤더에 URL 포함해야 함
302  Found                301과 같음. 클라이언트에서는 Location헤더로 받은 URL을 임시로 가리키는 목적으로 사용해야 함. 요청은 원래 URL로
303  See Other            리소스를 다른 URL에서 가져와야한다고 할 때. 새 URL은 Location헤더에.
304  Not Modified         If-Modified-Since 조건부 요청 헤더에 대한 응답
305  Use Proxy            리소스가 반드시 프락스를 통해 접근되어야 함. Location으로 프락시 위치 보냄.
307  Temporary Redirect   Location헤더의 URL을 임시로 가리키고 이후 요청은 원래 URL로
```

### 3.4.4 400-499: 클라이언트 에러 상태 코드
```
400  Bad Request                잘못된 요청
401  Unauthorized               리소스 얻기 전 인증하라
402  Payment Required           ..
403  Forbidden                  요청이 서버에 의해 거부됨.
404  Not Found                  서버가 요청 URL을 찾을 수 없음
405  Method Not Allowed         미지원 메서드 요청 받음. Allow 헤더와 반환
406  Not Acceptable             URL 리소스 중 클라이언트가 받아들일 수 있는 것이 없는 경우
407  Proxy Authentication Required 인증을 요구하는 프락시 서버를 위해 사용
408  Request Timeout            요청완수에 시간이 너무 걸릴 때. 응답 보내고 연결 끊음
409  Conflict                   요청이 리소스에 대해 일으킬 수 있는 충돌 지칭
410  Gone                       있던 리소스가 제거됬다.
411  Length Required            요청 메시지에 Content-Length 헤더를 요구할 때
412  Precondition Faile         조건부 요청 중 하나가 실패했을 때(요청에 Expect헤더 있을 때)
413  Request Entity Too Large   서버가 처리할 수 있는 한계를 넘은 크기의 요청임
414  Request URI Too Long       서버가 처리할 수 있는 한계를 넘은 길이의 요청URL
415  Unsupported Media Type     서버가 지원하지 않는 엔터티
416  Requested Range Not Satisfiable    요청하 리소스의 특정 범위가 잘못됨
417  Expectation Failed         요청에 포함된 Expect 헤더에 서버가 만족시킬 수 없다.
```

### 3.4.5 500-599: 서버 에러 상태 코드
``` 
500  Internal Server Error      서버가 요청을 처리할 수 없게 만드는 에러
501  Not Implemented            서버 능력을 넘는 요청을 받았을 때
502  Bad Gateway                프락시나 게이트웨이처럼 동작하는 서버가 그 요청응답 연쇄에 있는 다음 링크에서 가짜 응답을 받았을 때(부모 게이트웨이 접속이 불가능)
503  Service Unavailable        지금은 요청을 처리할 수 없다. Retry-After헤더로 언제 가능한 지 보내줌
504  Gateway Timeout            다른 서버에 요청을 보내고 기다리다가 타임아웃이 발생한 게이트웨이나 프락시가 보냄
505  HTTP Version Not Supported 서버가 지원하지 않는 프로토콜 요청을 받았을 때
```

## 3.5 헤더
### 3.5.1 일반 헤더
- 클라이언트, 서버 모두 사용되는 일반적 내용

- 일반 정보 헤더
```
Connection      요청/응답 연결에 대한 옵션
Date            메시지 생성 날짜 시간
MIME-Version    발송자가 사용한 MIME 버전
Trailer chunked transfer 인코딩된 메세지 끝 부분에 위치한 헤더 목록 나열
Transfer-Encoding    어떤 인코딩이 적용되었는지
Upgrade         이렇게 업그레이드 하라는 새 버전이나 프로토콜
Via             어떤 중개자(프락시, 게이트웨이)를 거쳤는지
```
- 일반 캐시 헤더
```
Cache-Control    캐시 지시자 전달
Pragma           지시지를 전달하는 또다른 방법. *Deprecated
```

### 3.5.2 요청 헤더
- 요청 정보 헤더
```
Client-IP    클라이언트 컴퓨터의 IP
From         클라이언트 사용자의 메일 주소
Host         요청대산 서버의 호스트명, 포트
Referer      요청 URI가들어있었던 문서의 URL
UA-Color     기기 디스플레이 색상 능력 정보
UA-CPU       CPU 종류, 제조사
UA-Disp      디스플레이 능력 정보
UA-OS        운영체제 이름, 버전
UA-Pixels    기기 디스플레이 픽셀 정보
User-Agent   요청을 보낸 애플리케이션 이름
```

- Accept 헤더
```
Aceept          서버가 보내도 되는 미디어 종류
Accept-Charset  서버가 보내도 되는 문자 집합
Accept-Encoding 서버가 보내도 되는 인코딩
Accept-Language 서버가 보내도 되는 언어
TE              서버가 보내도 되는 확장 전송 코딩
```

- 조건부 요청 헤더: 응답 전에 확인해달라는 제약을 포함
```
Expect              요청에 필요한 서버의 행동을 열거
If-Match            엔터티 태그가 주어진 엔터티 태그와 일치하는 경우에만 문서를 가져옴
If-Modified-Since   그 날짜 이후에 리소스가 변경되지 않았다면 요청 제한
If-None-Match       엔터티 태그가 주어진 엔터티 태그와 일치하지 않는 경우에만 문서를 가져옴
If-Range            문서의 특정 범위 요청
If-Unmodified-Since 그 날짜 이후에 리소스가 변경되었다면 요청 제한
Range               범위 요청을 지원하면 리소스에 대한 특정 범위 요청
```

- 요청 보안 헤더: 리소스에 접근하기 전 인증하게 하려 안전한 트랜잭션.
```
Authorization     클라이언트가 서버에게 제공하는 인증
Cookie            서버에 토큰을 전달할 때
Cookie2           요청자가 지원하는 쿠키 버전을 알려줄 때
```

- 프락시 요청 헤더
```
Max-Forwards        요청이 원 서버로 향하면서 프락시나 게이트웨어로 전달될 수 있는 최대 횟수. TRACE 메서드와 함께 사용
Proxy-Authorization 프락시 인증
Proxy-Connection    프락시 연결
```

### 3.5.3 응답 헤더
- 응답 정보 헤더
```
Age         응답이 얼마나 오래되었나
Public      특정 리소스에 대해 지원하는 요청 메서드 목록(최신 HTTP 명세에선 빠진)
Retry-After 현재 리소스가 사용불가지만 언제 가능해지는지
Server      서버 애플리케이션의 이름과 버전
Title       HTML 문서에서 주어진 것과 같은 제목
Warning     사유구절보다 자세한 결고 메시지
```

- 협상 헤더
```
Accept-Range     서버가 자원에 대해 받아들일 수 있는 범위의 형태
Vary             응답에 영향을 줄 수 있는 헤더의 목록
```

- 응답 보안 헤더
```
Proxy-Authenticate     프락시에서 클라이언트로 보낸 인증요구 목록
Set-Cookie             서버가 클라이언트를 인증할 수 있도록 클라이언트에 토큰을 설정하기 위해
WWW-Authenticate       서버에서 클라이언트로 보낸 인증요구 목록
```

### 3.5.4 엔터티 헤더
- 엔터티 정보 헤
```
Allow: 이 엔터티에 대해 수행될 수 있는 요청 메서드
Location: 엔터가 실제 어디에 위치하는지
Public      특정 리소스에 대해 지원하는 요청 메서드 목록(최신 HTTP 명세에선 빠진)
```

- 콘텐츠 헤더
```
Content-Base      본문에서 사용된 상대 URL을 계산하기 위한 기저 URL
Content-Encoding  본문에 적용된 인코딩
Content-Language  본문 이해를 위한 적절한 자연어
Content-Length    본문 길이, 크기
Content-Location  리소스 실제 위치
Content-MD5       본문 MD5 체크섬
Content-Range     전체 리소스에서 엔터티가 해당하는 범위를 바이트로 표현
Content-Type      본문이 어떤 종류의 객체인지
Content-MD5       본문 MD5
```

- 엔터티 캐싱 헤더: 언제 어떻게 캐시가 되어야 하는지
```
ETag          엔터티 태그
Expires       유효하지 않아 원본을 받아와야 하는 일시
Last-Modified 가장 최근 이 엔터티가 변경된 일시
```


Q. Public, Allow 헤더 차이?
- Allow
  
  - Lists the set of requests which the requesting user is allowed to issue for this URL. If this header line is omitted, the default allowed methods are "GET HEAD"

  - Example of use:
```Allow: GET HEAD PUT```

- Public
  - As "Allow" but lists those requests which anyone may use. If omitted, the default is "GET" only.
  - Example of use:
```Public: GET HEAD TEXTSEARCH```
