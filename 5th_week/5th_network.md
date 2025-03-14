# 도메인 네임

상대 호스트를 특정하기 위한 호스트 ip와 대응되는 이름 정보, 네임 서버에서 ip와 쌍으로 관리
점(.)을 기준으로 계층적으로 분류됨
도메인의 마지막 부분이 최상위 도메인으로 역순으로 계층

이러한 도메인 네임을 관리하는 체계가 도메인 네임 시스템(DNS)임

## 로컬 네임 서버

클라이언트와 맞닿아 있는 네임 서버, 클라이언트가 도메인 네임을 통해 ip 주소를 알아내고자 할 때
가장 먼저 찾게 되는 네임 서버입니다, ISP에서 할당해 줌

## 루트 네임 서버

로컬 네임 서버가 IP주소를 모른다면 루트 네임 서버에게 해당 도메인 네임을 질의함

## 책임 네임 서버

자신이 관리하는 도메인 영역의 질의에 대해서는 다른 네임 서버에게 떠넘기지 않고 곧바로 답할 수 있는 네임 서버

## DNS 캐시

질의가 오래 걸리기 때문에 응답받은 결과를 임시로 저장했다가 활용한다, TTL 값을 저장함

# URI

네트워크상에서 자원을 식별할 수 있는 정보, 위치를 이용해 자원을 식별하는 걸 URL, 이름을 이용한 건 URN

## 형식

1. scheme: URL의 첫 부분, 자원에 접근하는 방법을 의미, http, https
2. authority: 호스트를 특정할 수 있는 정보, IP주소 또는 도메인 네임
3. path: 자원이 위치한 경로, /를 기준으로 계층적으로 표현
4. query: http 기반 요청-응답 프로토콜에서 물음표로 시작되는 키=값 형태
5. fragment: 자원의 한 조각을 가리키기 위한 정보

# HTTP

스테이트리스 프로토콜, 요청을 보낸 클라이언트와 관련된 상태를 기억하지 않는다

## 메시지 구조

### 시작 라인: http 요청 또는 응답 메시지
* 요청 라인: 메서드/ 요청 대상/ http 버전
* 메서드: 클라이언트가 서버의 자원에 대해 수행할 작업의 종류, GET, POST, PUT, DELETE
* 요청 대상: 요청을 보낼 서버의 자원
* HTTP 버전

* 상태 라인: 응답 메시지일 경우, 다음과 같은 형식이 됨, HTTP 버전 /  상태 코드/ 이유 구문/
* 상태 코드: 요청에 대한 결과를 나타내는 3자리 정수
* 이유 구문: 상태 코드에 대한 문자열 형태의 설명

### 헤더 라인

HTTP 통신에 필요한 부가 정보, :을 기준으로 헤더 이름과 헤더 값으로 구성된다
HTTP 요청 또는 응답 메시지에서 본문이 필요할 경우 메시지 본문에 명시된다

## HTTP 메서드

* GET: 자원을 습득
* HEAD: GET과 동일하지만 헤더만 받음
* POST: 특정 작업을 처리하게끔 함
* PUT: 자원을 대체함
* PATCH: 자원의 부분 수정 요청
* DELETE: 자원 삭제 요청
* CONNECT: 자원의 양방향 연결 요청
* OPTIONS: 사용 가능한 메서드를 확인
* TRACE: 자원에 대한 루프백 테스트

## HTTP 상태 코드

* 200번대: 성공 상태 코드
* 300번대: 리다이렉션
* 400번대: 에러 상태 코드
* 500번대: 서버 에러 상태 코드

## HTTP 헤더

요청 시 활용되는 헤더는 다음과 같다

* Host: 요청을 보낼 호스트를 나타냄, 주도 도메인 네임으로 명시
* User-Agent: 요청을 시작하는 클라이언트 측의 프로그램을 의미
* Referer: 클라이언트가 요청을 보낼 때 머무르고 있던 URL이 명시
* Authorizaion: 클라이언트의 인증 정보를 담는 헤더, <type> <credentials> 형태

응답 시 활용되는 헤더

* Server: 요청을 처리하는 서버 측의 소프트웨어와 관련된 정보를 명시
* Allow: 클라이언트에게 허용된 HTTP 메서드 목록을 알려줌
* retry-after: 자원을 사용할 수 있는 날짜 혹은 시각을 나타냄
* Location: 클라이언트에게 자원의 위치를 알려줌
* WWW-authenticate: 자원에 접근하기 위한 인증 방식을 설명하는 헤더

요청, 응답 모두에서 활용되는 헤더

* Date: 메세지 생성된 날짜와 시각에 관련된 정보를 담은 헤더
* Connection: 클라이언트의 요청과 응답 간의 연결 방식을 설정
* Content-length: 본문의 바이트 단위 크기를 나타냄
* Content-type, language, encoding: 본문의 타입, 언어, 인코딩 정보

## 캐시

불필요한 대역폭 낭비를 막기 위해서 정보의 사본을 임시로 저장하는 기술
웹 브라우저에 저장되어 있기도 하고, 클라이언트, 서버 사이의 중간 서버에 저장되기도 함

캐시는 사본 데이터의 저장이기 때문에 유효한 기간을 정해서 갱신함

날짜 기반, 자원의 변경되면 바뀌는 자원의 버전 기반으로 캐시를 갱신

## 쿠키

서버에서 생성되어 클라이언트 측에 저장되는 데이터로 서버와 클라이언트 상태를 저장

쿠키는 요청에서 정보를 뽑아 저장, 브라우저에서 저장되고 관리됨

경로 별로 따로 상태를 저장하기도 함, 쿠키 역시 유효 기간을 정해서 지나면 삭제함


