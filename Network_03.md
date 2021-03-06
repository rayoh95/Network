### 네트워크

<hr>


#### 4 계층 프로토콜

전송 계층(Transport layer)은 송신자의 `프로세스`와 수신자의 `프로세스`를 `연결하는 통신 서비스`를 제공한다.

* 프로세스 : 메모리에서 동작 중인 프로그램.

전송 계층은 연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어, 그리고 다중화와 같은 편리한 서비스를 제공한다.

전송 프로토콜 중 가장 잘 알려진 것은 연결 지향 전송 방식을 사용하는 전송 제어 프로토콜 (`TCP`) 이다. 보다 단순한 전송에 사용되는 사용자 데이터그램 프로토콜 (`UDP`)도 있다.



##### `포트 번호` : 4 계층에서 사용하는 주소

* 특정 프로그램이 사용하는 번호.
* `하나의 포트는 하나의 프로세스만` 사용 가능. -> 내 컴퓨터에서 웹 서비스를 80번으로 정했다면, 80번 포트는 웹 서비스만 사용 가능
* 하나의 프로세스가 여러 개의 포트를 사용하는 것은 가능. -> 웹 서비스는 꼭 80번만 쓸 필요는 없다. 
* 포트 번호는 일반적으로 정해져 있지만 `무조건 지켜야 하는 것은 아니다`.
  * 일반적으로 웹 서비스는 80번 포트를 사용하지만 웹 서비스가 항상 80번 포트를 사용하여 통신해야만 하는 것은 아니다.



##### `세계적으로 잘 알려진 포트 번호 (Well-known Port)`

FTP			   -			20번, 21번

SSH    		  -			 22번

TELNET	    - 			23번

DNS		 	 -			53번

DHCP 		  -			67번, 68번

TFTP	 	    -			69번

HTTP	 	   -			80번

HTTPS	 	 -			443번



##### 조금 유명한 포트 번호 (Registered Port)

오라클 DB 서버		-		1521번

MySQL 서버		   -		 3306번

MS 원격 데스크탑    -		 3389번



##### 일반인들(클라이언트)이 사용하는 포트 번호 (Dynamic Port)

시작 포트 번호 : 49152번

마지막 포트 번호 : 65535번





#### UDP 프로토콜 - 비연결 지향형

사용자 데이터그램 프로토콜(User Datagram Protocol, UDP) 은 `전송 방식이 너무 단순`해서 서비스의 `신뢰성이 낮고`, 데이터그램 도착 순서가 바뀌거나, 중복되거나, 심지어는 통보 없이 누락시키기도 한다.

UDP 는 일반적으로 `오류의 검사와 수정이 필요 없는` 프로그램에서 수행할 것으로 가정한다.



#### UDP 프로토콜의 구조

* Source Port : 2 byte
* Destination Port : 2byte
* Length = UDP 프로토콜 헤더 + 페이로드 데이터 : 2 byte
* Checksum : 2 byte



#### UDP 프로토콜을 사용하는 프로그램

1. 도메인을 물으면 IP 를 알려주는 `DNS 서버`

2. 파일전송 프로그램인 tftp 서버

3. 라우터끼리 라우팅 정보를 공유하는 RIP 프로토콜





#### `TCP 프로토콜` - 연결 지향형

전송 제어 프로토콜(Transmission Control Protocol, TCP) 은 인터넷에 연결된 컴퓨터에서 실행되는 프로그램 간에 통신을 `안정적으로`, `순서대로`, `에러없이` 교환할 수 있게 한다.

TCP 의 안정성을 필요로 하지 않는 애플리케이션의 경우 일반적으로 TCP 대신 비접속형 사용자 데이터그램 프로토콜(UDP) 을 사용.

TCP 는 UDP 보다 안전하지만 느리다.



#### TCP 프로토콜의 구조

4 byte 씩 5층

* Source Port : 2 byte
* Destination Port : 2 byte
* Sequence Number : 4 byte
* Acknowlegement Number : 4 byte
* Offset = 헤더의 길이를 얘기한다. 실제 길이의 4로 나눈 값을 갖는다 : 0.5 byte
* Reserved = 예약된 필드로서, 사용하지 않는 필드다 : 0.5 byte
* `TCP Flags` : 1byte
  * TCP 프로토콜은 연결을 지향한다. 때문에 서로 연결되어서 계속해서 데이터를 주고받는 일에 대해 소통한다. 데이터를 전달해도 되는지, 받을 수 있는지 등. 이런 소`통을 나타내는 데이터 값`이 `Flag값` 다. TCP의 주된 기능이 Flag 값에 따라 나뉜다. 
  * U (Urgent) : 긴급 비트. 보내는 데이터의 우선순위가 높다고 지정한다. Urgent Pointer 와 세트
  * `A (Ack)` : 물음(연결해도 돼? 보내도 돼?)에 대한 승인을 해줄 때 사용하는 데이터. 
  * P (Push) : 밀어넣기 비트. 본래는 TCP 버퍼가(저장 공간) 일정 크기 쌓여야 패킷을 추가적으로 전송한다. 이 비트를 사용하면 버퍼에 상관없이 데이터를 밀어 넣는다.
  * `R (Reset)` : 연결 관계에 문제가 생겼을 때, 연결을 초기화
  * `S (Synch)` : 동기화 비트. 상대방과 연결을 시작할 때 무조건 사용하는 flag. 이 비트가 보내지고 난 후에야 동기화가 시작된다.
  * `F (Fin)` : 종료 비트.
* Window = 상대방과 연결 된 상태에서 데이터를 주고 받는다. 상대방에게 얼마만큼 데이터를 더 보내도 되는지 알려줄 때 사용 : 2 byte
* Checksum : 2 byte
* Urgent Pointer : 2 byte
* TCP Options : IPv4 프로토콜과 마찬가지고 잘 붙는 데이터는 아니며, 붙는다면 4 byte 씩 총 10개 까지 붙을 수 있다.





#### TCP 프로토콜의 연결 수립 과정

TCP 를 이용한 데이터 통신을 할 때, 프로세스와 프로세스를 연결하기 위해 `가장 먼저 수행되는 과정`

1. 클라이언트가 서버에게 요청 패킷을 보낸다.
   * TCP Flags 의 Synck  번호가 세팅되어 전달된다.
   * SEQ : 100, A: 0  (랜덤 번호)
2. 서버가 클라이언트의 요청을 받아들이는 패킷을 보낸다.
   * 디캡슐레이션 후 요청을 확인
   * TCP Flags 의 Synch 와 Ack 가 같이 세팅되어 클라이언트에게 전달
   * SEQ : 2000, A : 101
3. 클라이언트는 이를 최종적으로 수락하는 패킷을 보낸다.
   * 디캡슐레이션 후 요청을 확인
   * 연결을 수락하는 응답 요청을 보낸다.
   * SEQ : 101, a : 2001

위 3개의 과정을 `3 Way Handshake` 라고 부른다. 이후 ACK 번호를 그대로 갖고서 클라이언트와 서버가 통신한다.

TCP 를 이용한 통신을 할 때 단순히 TCP 패킷만을 캡슐화해서 통신하는 것이 아닌 페이로드를 포함한 패킷을 주고받을 떄의 일정한 규칙

1. 보낸 쪽에서 또 보낼 때는 SEQ 번호와 ACK 번호가 그대로이다.
   * Flag : PSH + ACK
   * S : 101, A : 2001
2. 받는 쪽에서 SEQ 번호는 받은 ACK 번호가 된다.
   * Flags : PSH + ACK
   * s : 2001, A : 101 + 클라이언트가 보낸 data(페이로드) 크기
3. 받는 쪽에서 ACK 번호는 받은 SEQ 번호 + 데이터의 크기
   * Flags : ACK
   * S : 101 + 클라이언트가 보낸 data 크기, A : 2001 + 서버가 보내준 data 크기

데이터를 주고받는 과정이 끝난 후 연결을 끊는다.





#### `NAT 와 포트포워딩`

NAT(Natwork Address Translation) 은 IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술이다.

패킷에 변화가 생기기 때문에 IP 나 TCP/UDP 의 체크섬(checksum)도 다시 계산되어 재기록해야 한다.

NAT를 이용하는 이유는 대개 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함이다.

하지만 꼭 사설 IP를 공인 IP로 변환 하는 데에만 사용하는 기술은 아니다.



#### 포트포워딩

포트 포워딩 또는 포트 매핑(port mapping)은 패킷이 라우터나 방화벽과 같은 네트워크 장비를 가로지르는 동안 `특정 IP 주소와 포트 번호의 통신 요청을 특정 다른 IP와 포트 번호로 넘겨주는` 네트워크 주소 변환(NAT)의 응용이다.

이 기법은 게이트웨이(외부망)의 반대쪽에 위치한 사설 네트워크에 상주하는 호스트에 대한 서비스를 생성하기 위해 흔히 사용된다.

