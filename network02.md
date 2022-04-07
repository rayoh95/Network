#### 네트워크



##### 네트워크 침의 예

`CS8900A`

* 10Mbps 를 지원하는 `Ethernet Controller`
* EEPROM(MAC 주소가 담겨진다.) 을 제공하여 점퍼설정을 최소화한 설정이 가능
* IEEE 802.3 기준(표준)을 따르고 Direct ISA-Bus 연결을 사용



##### CSMA/CD (충돌 제어) 기본 알고리즘

데이터 있슴 -> 채널이 사용 중인가? (`carrier sensing`) -no-> 전송 -> 충돌발생? (`collision detection`) -no-> 전송 끝

* 채널이 사용 중이면 backoff
* 충돌이 발생하면 jamming signal 전송 -> backoff

기다리는 시간에 따라 알고리즘이 결정된다.

##### CSMA/CD 는 두 가지 옵션이 있다.



##### 이더넷 계층 구조

* LLC (Logical Link Control) : 흐름제어와 오류 제어를 담당
* MAC (Media Access Control) : CSMA/CD 접근 방법에 대한 동작을 담당

위 두 가지는 Data link layer 이다.



##### 이더넷 - 프레임 Encapsulation 과 Decapsulation 중 전송 시

1. 한 번에 보내는 적절한 프레임 바이트 수 (5, 381, 1021 또는 full프레임)가 CS8900A 메모리에 전송되고 네트워크에 접속 할 수 있게 한다.
2. 이어서 MAC 은 Start-of-Frame Delimiter(SFD, 10101011b) 뒤에 7byte preamble(1010101b...) 을 전송한다.
3. SFD 와 FCS 사이의 데이터는 host 에서 공급된다. CS8900A 가 제공하는 FCS 생성기는 inhibitCRC bit 가 set 됨에 따라 비활성화 된다.



##### 프레임의 길이

Preamble(7) | SFD | Destination Address(6) | Source Address(6) | Length(2) | Data(46 ~ 1500) | FCS(4)

​						          <------------------------------Ethernet Header-------------------------->

Data(46 ~ 1500)  : DSAP(1) | SSAP(1) | CTRL(1) | DATA

​								<--------------Header-------------> <----------Data---------->

`Data 는 상위 계층에서 내려온 데이터들이다.`



##### Network Protocol Stack

Application

​			|

BSD socket Interface : 내부 로컬 네트워크

​			|

INET socket Interface : 인터넷 프로토콜

   |	  | 	  |

TCP	|    UDP

​     \     |    /

​		  IP

​		  |

Network Device Driver

​		  |

Ethernet H/W





##### 네트워크 소켓

`네트워크는 소켓(Socket)으로 관리`

* 소켓이라는 단일한 인터페이스로 묶어서 사용
* 네트워크를 연결한다는 의미
* 소켓 인터페이스를 통해 하위의 프로토콜 스택에서 일어나는 일들에 접근 가능

* 소켓은 프로토콜, IP 주소, 포트 넘버로 정의된다.