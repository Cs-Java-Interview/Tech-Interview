![image](https://user-images.githubusercontent.com/77563814/168789780-178d5648-5619-415a-9eea-add3607e203a.png)


## **데이터 캡슐화**

- 데이터 캡슐화란
    - 어떤 네트워크를 통과하기 위해 전송하려는 데이터를 다른 무언가로 감싸서 보내고, 
    해당 네트워크를 통과하면 감싼 부분을 다시 벗겨내어 전송하는 기능을 말한다.
- 캡슐화 : 데이터에 제어 정보(헤더)를 덧붙여 다음 계층으로 보내는 것
- 디캡슐화 : 캡슐화와 반대로, 헤더를 떼어내는 것

## PDU **(Protocol Data Unit)**

- PDU **:** 프로토콜 데이터 단위 (각 계층마다 사용하는 PDU의 명칭)
- PDU = SDU(Service Data Unit) + PCI(Protocol Control Information)로 구성
    - SDU : 전송하려는 데이터
    - PCI : 제어 정보 (송신자와 수신자 주소, 오류 검출 코드, 프로토콜 제어 정보 등)
- 어플리케이션 계층 :
    - Message(Data) 보내려는 데이터
- 전송 계층 :
    - Segment = Data + Port 번호(가 포함된 TCP or UDP 헤더)
    - 이때 TCP의 경우 데이터 단편화가 이뤄진다. (Data를 분할하여 Segment를 만듦)
- 인터넷 계층 :
    - Packets = Segment + IP 주소
- 네트워크 인터페이스 계층 :
    - Frame = Packets + MAC 주소
    - LAN일 경우 MAC 주소가 이고, WAN 영역일 경우 WAN 영역에 대한 정보로 채워짐
