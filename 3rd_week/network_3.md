# 네트워크 

## IP 주소
ip주소는 4바이트로 주소를 표현한다 숫자당 8비트로 구분되기 때문에 0~255까지 범위

* ip 기능: ip주소 지정, ip 단편화가 있음. ip 주소 지정은 ip 주소를 바탕으로 송수신 대상을 지정
* ip 주소 지정: ip 주소를 바탕으로 송수신 대상을 지정하는 것을 의미
* ip 단편화: 전송하고자 하는 패킷의 크기가 MTU라는 최대 전송 단위보다 클 경우, 이를 MTU 크기 이하의 복수 패킷으로 나눔

### ipv4
ipv4 패킷은 프레임의 페이로드로 데이터 필드에 명시

1. 식별자: 패킷에 할당된 번호 쪼개진 패킷을 합치기 위해서 이용
2. 플래그: DF는 단편화 수행 가능 여부, MF는 단편화 된 패킷이 더 있는지를 나타냄
3. 단편화 오프셋: 패킷의 초기 데이터에서 몇 번째로 떨어진 패킷인지를 나타냄
4. TTL: time to live의 약어, 패킷의 수명을 의미. 패킷이 하나의 라우터를 거칠 때마다 TTL이 1씩 감소하며, TTL 값이 0이면
폐기됨
5. 프로토콜: 상위 계층의 프로토콜이 무엇인지를 나타내는 필드
6. 송수신지 IP주소: 송수신 ip주소를 기록

### ipv6
16비트로 주소를 표현, 8개 그룹으로 이우어짐

1. 다음 헤더: 상위 계층의 프로토콜, 확장 헤더를 가리킴
2. 홉 제한: 패킷의 수명을 나타내는 필드
3. 송수신지 ip 주소: 주소 지정이 가능

## ARP

상대 호스트의 ip주소는 알지만 MAC 주소는 알지 못하는 상황일 때 사용하는 프로토콜
ip 주소를 이용해 MAC 주소를 알아낸다.

1. ARP 요청: 모든 호스트에게 브로드캐스트 메세지 보냄
2. ARP 응답: 알맞은 ip 주소를 가진 호스트가 자신의 MAC 주소를 응답으로 송신
3. ARP 테이블 갱신: ARP 테이블 정보를 시간에 따라, 요청에 따라 갱신

# ip 주소

## 네트워크 주소

비트를 지정해 네트워크 주소와 호스트 주소를 ip주소에서 나눠서 할당한다.
ex) 172.16.12.45 에서 네트워크 주소가 16비트면 172.16, 호스트는 나머지 12.45

## 클래스풀 주소 체계

네트워크 크기에 따라 ip 주소를 분류하는 기준, 호스트 ip 개수에 따라 네트워크 크기를
가변적으로 조정해 네트워크 주소와 호스트 주소를 구획할 수 있다

## 클래스리스 주소 체계

클래스 개념 없이 네트워크의 영역을 나누어서 호스트에게 IP 주소 공간을 할당하는 방식

* 서브넷 마스크: 네트워크 주소와 호스트 주소를 구분 짓는 지점
ip 주소상에서 네트워크 주소는 1, 호스트 주소는 0으로 표기한 비트열

* 서브네팅: ip 주소와 서브넷 마스크를 비트 and 연산하면 네트워크와 호스트를 구분가능

* 서브넷 마스크 표기: CIDR 표기법, 이진수로 생각해 1의 총 개수를 표시 앞부터 1의 개수임

## 공인 ip 주소와 사설 ip 주소

* 공인 ip 주소: 전 세계에서 고유한 ip주소, ISP나 할당 기관을 통해 할당받는다
* 사설 ip 주소: 인터넷, 외부 네트워크에 공개되지 않은 네트워크에서 사용
일반적으로 라우터가 할당함, 다른 ip와 중복가능, 외부와 통신할땐 NAT을 이용해 변환

## 정적 ip 주소와 동적 ip 주소

* 정적 할당: 호스트에 직접 수작업으로 ip 주소를 부여
* 동적 할당: 호스트에 ip 주소가 동적으로 할당, DHCP란 프로토콜을 이용
* DHCP: ip를 할당 받고자 하는 호스트와 DHCP 서버간 메세지를 주고 받아 ip 주소 할당이 이루어짐
할당 받은 ip 주소는 기간이 정해져 있음
* DHCP 메세지: discover -> offer -> request -> ask, DHCP 서버 찾고, 메세지로 ip 할당하고, 연결 응답하고, 사용 허락함

# 라우팅

패킷이 이동할 최적의 경로를 설정한 뒤 해당 경로로 패킷을 이동시키는 것

## 라우터

라우팅을 하는 장치, L3 스위치라고도 부름

라우터와 라우터 간에 이동하는 하나의 과정을 hop이라고 부른다

## 라우팅 테이블

라우팅에 필요한 정보를 명시한 표

* 수신지 ip주소, 서브넷  마스크: 패킷을 전달할 대상의 의미
* 다음 홉: 다음으로 거쳐야 할 호스트의 ip주소, 인터페이스를 의미, 게이트웨이라고도 부름
* 네트워크 인터페이스: 패킷을 보낼 통로, NIC나 인터페이스의 ip주소로 표시
* 메트릭: 해당 경로로 가는 비용을 의미
* 디폴트 라우터: 라우팅 테이블에 없는 경로면 기본적으로 설정된 경로로 패킷을 보냄

## 정적 라우팅

사용자가 수동으로 직접 채워 넣은 라우팅 테이블을 토대로 삼음

## 동적 라우팅

자동으로 라우팅 테이블 항목을 만들고 라우팅하는 방식

### 라우팅 프로토콜

라우터끼리 자신들의 정보를 교환하며 패킷이 이동할 최적의 경로를 찾기 위한 프로토콜

* IGP: 최적 경로를 거리 벡터로 나타내면 RIP, 링크 상태로 나타내면 OSPF
* RIP: 거리 벡터 기반 라우팅 프로토콜, 거리는 패킷이 경유한 라우터의 수, 즉 홉의 수를 의미
* OSPF: 그래프를 이용해 거리, 연결, 비용 등을 기록, 효율을 위해 AS란 범위안에서 담당
* EGP: AS 간의 통신이 가능한 프로토콜, EGP를 사용하는 라우터끼리 메세지 주고 받음, 이런 라우터를 피어라고 함