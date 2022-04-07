#### 네트워크



##### 통신을 위한 기본 동작

* 요청(Request)
* 인지(Indicate)
  * 수신하는 장치에서 작업 요청(이벤트) 확인.
* 응답(Response)
* 확인(confirm)
  * 전속 측에서 응답 데이터를 최종적으로 확인.



##### 네트워크 유형

* `LAN (Local Area Network)`
* `WAN (Wide Area Network)`
* 크기 유형
  * LAN < WAN < Internet



##### OSI 7 Layer 와 TCP/IP 의 관계

`OSI 7 계층 vs TCP/IP`

1. ##### Physical

   * 일련의 2진 bit(전기적 신호를 매체로 전달)

2. ##### Datalink

   * 두 node간의 통신관여. 흐름제어

3. ##### Network

   * 해당주소 체계에 따라 경로설정(IPv4 / IPv6 가 사용)

4. ##### Transport

   * 신뢰성 보장을 위해 TCP / UDP 등 프로토콜을 갖는다. 전송계층

​	`1~4 계층은 대부분 운영체제(Kernel)에서 담당`

<hr>

5. ##### Session

   * 프로그램간의 논리적 접속. 프로그래머의 역할. 문법, 구문 체계 확립

6. ##### Presentation

7. ##### Application

   * Email, Web 브라우저, FTP, ping, Appl...

`5~7 계층은 운영단(User space) 에서 담당. 3계층을 합쳐 Application 계층이라고도 한다.`



##### `물리 계층(physical layer)`

* 물리적 매체를 통한 `비트 스트림 전송에 요구되는 기능`을 담당 (기계적, 전기적, 전송매체)

* 기능
  * 비트의 표현
  * 데이터 속도 : 신호가 유지되는 비트의 주기를 규정
  * 비트의 동기화 : 송신자와 수신자는 같은 클록(clock)을 사용 (클록 사이클에 맞춰 비트 데이터를 받아들인다.)

하드웨어에 존재하는 물리계층 ex) CS8900A (이더넷 통신)

`CSMA/CD` 원리를 통해 충돌을 해결

* MAC(Medium Access Control) 필요
  * 데이터를 주고받는 두 매체에서 2진 비트의 충돌을 막기위해 필요
  * 자유경쟁 (선착순) - 경쟁을 위한 알고리즘이 존재(`CSMA/CD (CSMA/Collision Detection)`)
  * Token
* CSMA/CD    ->    IEEE 802.3
  * 충돌 시 잼 신호를 보낸다.
  * 최대 16번 까지 재시도



##### `데이터 링크 계층(data link layer)`

* 노드 대 노드 전달의 책임을 가진다.
  * 실제로 통신이 일어난다. 때문에 가장 기초적인 `물리주소 MAC` 이 필요.

* 기능
  * 프레임 구성
  * 물리주소 MAC 지정 : 송신자와 수신자의 물리 주소를 헤더에 추가
  * 흐름제어 : 데이터 전송률을 고려해 데이터 전송
  * 오류제어 : 오류난 프레임을 발견/재전송
  * 접근제어 : 한 순간 하나의 장치만 동작하도록 제어

데이터 링크 계층에서 사용하는 주요 프로토콜

* `ARP (Adress Resolution Protocol)`
  * 주소를 해석하기 위한 프로토콜
  * 논리적인 IP주소를 물리적인 MAC 주소로 바꾼다.
  * 캐시를 통해 얻은 정보가 저장되고 보통 20분의 수명을 가진다.
* `RARP (Reserve Address Resolution Protocol)`
  * 저장 장치가 없는 네트워크 단말기등이 IP 주소를 얻기 MAC 을 알아내기 위해 사용.

데이터 링크 - 노드 대 노드(Hop-to-Hop)의 전달 책임을 담당

* 데이터를 주고받는 기기들 = 노드
* 데이터 링크는 노드끼리의 통신을 책임지는 것
* MAC 주소를 통해 다음 노드의 위치를 알 수 있다.

데이터 링크 계층의 전달요소

* 물리주소의 데이터 전달 과정

  * 물리주소 10 인 노드(Source adress)는 물리주소 87 인 노드(Destination address)에 프레임을 보낸다.

    Ex) 07:01:02:01:2C:4B

  * 헤더의 끝에는 물리주소 이외에 필요한 다른 정보가 있다. 



##### `네트워크 계층(network layer)`

* 패킷을 `발신지-대-목적지` 전달에 대한 책임을 가진다.
* 기능
  * `논리 주소지정(Logical addressing)`
    * 상위 계층에서 받은 패킷에 발신지와 목적지의 논리주소를 헤더에 추가
  * 라우팅(Routing)
    * 패킷이 최종 목적지에 전달될 수 있도록 경로를 지정하거나 교환하는 기능

네트워크 계층의 주요 프로토콜

* `IP (Internet Protocol)`
  * 네트워크 기기에서 논리적 식별을 위한 주소
    * IPv4 : 약 40억개의 주소 - ex) 123.321.234.232
    * IPv6: 2의 128제곱의 개수를 가진 주소 - ex) 21DA:00D3:0000:2F3B:02AA:00FF:FE28:9C5A

* ICMP
* IGMP

네트워크 계층 - 발신지 대 목적지 전달

* End-to-end 통신이 가능해진다.



##### `전송 계층(transport layer)`

* 전체 메시지의 `프로세스 대 프로세스` 전달에 대한 책임을 가진다.
* 프로세스 : 구동하고 있는 앱/프로그램
* 포트(port) 번호가 필요 : 어떤 프로그램이 구동되고 있는지 나타낸다.
  * http : 웹의 프로토콜로 80 포트 번호를 사용한다.
* 전체 메세지를 분할하여 담아주는 역할을 한다. 분할된 데이터는 각각 `Segment` 라고 불린다.
* 분할된 데이터를 받아 다시 잇는 역할.

전송 계층의 기능

* `포트 주소 지정 (port addressing)` : 포트 주소를 포함
  * 네트워크 계층은 각 패킷을 정확한 컴퓨터에, 전송 계층은 해당 컴퓨터의 정확한 프로세스(프로그램)에게 전달
* 분할과 재조립 (Segmentation and reassembly)
  *  전달 가능한 세그먼트 단위로 나눈다.
  * 각 세그먼트는 `순서번호`를 가지며 이를 통해 재 조립 또는 패킷의 손실 여부를 판단한다.

전송 계층의 프로토콜

* `TCP (Transmission Control Protocol)`
  * 가상 회선 방식을 제공
  * 3-way handshaking 과정을 통해 연결
  * 속도가 낮다.
* UDP (User Datagram Protocol)
* SCTP (Stream ControlTransmission Protocol)



##### `응용 계층(application layer)`

* 사용자가 네트워크에 접근할 수 있도록 해준다.
* 사용자 인터페이스 제공
* 서비스
  * 원격 로그인, 파일 액세스, 전송, 관리, 메일 서비스. Http www 웹 등등

Application 계층의 프로토콜 및 프로그램

* FTP (File Transfer Protocol)
* Telnet
* SMTP (Simple Mail Transfer Protocol)
* DNS (Domain Name System)
* HTTP
* DHCP : 동적 IP 할당
* Ping
* ...



