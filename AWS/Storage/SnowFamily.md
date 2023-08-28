# AWS Snow Family
## AWS Snow Family

![snow1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow1.png)

- AWS Snow은 보안성이 뛰어난 휴대용 장치의 모음으로써 AWS 내에서 두 가지 경우에 사용되고 있음
- Data Migration: AWS 안팎으로 데이터를 마이그레이션
	- Snowcone
	- Snowball Edge
	- Snowmobile
- Edge Computing: 엣지에서 데이터를 수집하고 처리
	- Snowcone
	- Snowball Edge

## Data Migrations with AWS Snow Family

![snow2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow2.png)

- 네트워크를 통해서 많은 데이터를 전송하려면 아주 오랜 시간이 걸림
- 예를 들어 100TB를 전송하는 경우 초당 1GB의 네트워크 회선을 이용한다면 전송을 완료하는데 12일이 걸림
	- 즉 전송할 데이터가 PB 이상이라면 상상 이상의 시간이 걸림
- 때때로 AWS에 빠르게 접속해야 할 때가 있는데 그런 경우에 생기는 문제점
	- 전송 가능한 데이터의 양이 적다는 것
	- 제한된 연결 및 제한된 대역폭 문제
	- 네트워크를 통한 데이터 전송으로 비용이 발생한다는 것
		- 네트워크를 사용하는 게 무료는 아니기 때문
	- 대역폭 공유 문제도 있음
		- 만약 AWS에서 영상을 다운로드하는데 그 데이터 크기가 10TB라면 사무실 전체가 차단될 수 있음
		- 사무실 내의 대역폭을 최대화하기 때문
	- 연결이 안정되지 않아서 재시도를 해야 할 수도 있음
- 이런 이유들 때문에 Snow 제품군이 사용됨
- Snow 제품군은 오프라인에서 데이터 마이그레이션을 실행하는 장치
- AWS가 우편으로 물리적 장치를 보내주면 거기에 데이터를 끌어오고 다시 AWS로 전송하는 것
- 일반적으로 데이터 전송 시 네트워크를 사용할 경우 일주일이 넘는 시간이 걸린다면 Snowball 장치를 사용해야 함

![snow3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow3.png)

- 예시
	- Amazon S3으로 직접 파일을 업로드하려면 클라이언트가 Amazon S3로 데이터를 전송
	- Snow 제품군의 Snowball 장치가 있는 경우 클라이언트가 Snowball 장치를 요청하고 우편으로 받음
		- AWS가 장치를 배송해 주면 로컬에서 데이터를 직접 장치로 가져옴
		- 그 장치를 AWS 시설의 AWS로 다시 배송하면 AWS 측에서 장치를 가져다 자체적인 인프라에 연결
		- 이때 데이터를 불러오거나 내보내는 작업을 하는데 Amazon S3 버킷에서 무엇을 하느냐에 따라 결정됨
		- 즉 AWS로 데이터를 전송하는 방법이 맞는데 네트워크가 아닌 물리적인 경로를 이용하는 것

### Snowball Edge

![snow4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow4.png)

- Snowball Edge는 커다란 상자인데 TB 혹은 PB 크기의 데이터를 AWS 안팎으로 전송할 수 있음
	- 네트워크를 대신해서 데이터를 옮길 수 있음
- 데이터 전송 건마다 비용이 청구됨
- Snowball Edge 인터페이스는 블록 스토리지를 제공하거나 Amazon S3 호환 객체 스토리지를 제공
- Snowball Edge의 두 가지 옵션
	- Snowball Edge Storage Optimized
		- 블록 볼륨으로 사용할 수 있도록 80TB의 하드웨어 디스크 용량을 제공하거나 S3 호환 객체 스토리지를 줌
	- Snowball Edge Compute Optimized
		- 42TB의 HDD 용량을 제공
		- 더 큰 스토리지가 필요할 때 사용할 옵션은 Snowball Edge Storage Optimized
- Snowball Edge를 데이터 전송에 쓰는 경우는 데이터 센터 폐쇄를 위한 대량의 데이터 클라우드 마이그레이션이나 AWS에 데이터를 백업함으로써 재해 복구를 하는 경우

### Snowcone

![snow5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow5.png)

- Snowcone 크기가 더 작음
- 이더넷 포트도 있음
- 어디서나 컴퓨팅 가능한 작은 휴대용 장치이자 견고하고 안전하며 가혹한 환경을 견딜 수 있어 사막에서든 물속이든 사용할 수 있음
- 엣지 컴퓨팅, 스토리지 및 데이터 전송에 사용됨
- Snowcone: 8 TB of HDD Storage
- Snowcone SSD: 14 TB of SSD Storage
- Snowcone의 스토리지에는 8TB를 저장할 수 있으며 Snowball Edge Storage Optimized와 비교하면 10배는 적은 용량
- Snowball 사용이 불가능할 때 Snowcone을 쓸 수 있음
	- 예를 들면 공간의 제약을 받는 환경일 때
		- 필요하면 Snowcone을 드론 위에 설치할 수도 있음
- 배터리와 케이블은 직접 준비해야 함
- AWS 오프라인으로 다시 전송될 수 있으며 아니면 네트워크에 연결해서 AWS DataSync를 사용해 데이터를 재전송할 수 있음
- 네트워크 연결이 전혀 없는 환경에 Snowcone이 있다고 가정
	- 데이터를 끊임없이 수집
	- 그리고 집으로 돌아가 데이터 센터에 가서 Snowcone을 데이터 센터에 연결하면 데이터가 자동으로 AWS에 전송됨
	- 혹은 오프라인으로 AWS에 재전송 할 수도 있음

### Snowmobile

![snow6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow6.png)

- Snowmobile은 실제 트럭
- Snowmobile이 전송하는 데이터는 EB(엑사바이트)에 달함
	- 1EB는 1,000PB이며 1백만 TB와 같음
- 각 Snowmobile의 용량은 100PB
	- 따라서 데이터 1EB를 처리하려면 Snowmobile을 열 대 주문해야 함
- 보안성이 뛰어나고 온도 조절이 가능하며 GPS 추적 및 연중무휴 비디오 감시로 굉장히 안전한 데이터 전송 방법
	- 그러니 10PB 이상의 데이터를 전송하려면 Snowball보다 좋은 방법이라고 할 수 있음

### AWS Snow Family for Data Migrations

![snow7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow7.png)

- 정리하자면, Snowcone, Snowball Edge 그리고 Snowmobile이 있고 각각의 스토리지 용량이 서로 다름
	- 8TB, 80TB, 100 PB
- AWS에서 권장하는 마이그레이션 크기
	- Snowcone이 24TB까지
	- Snowball Edge는 PB까지
	- Snowmobile는 최대 EB 데이터까지
- Snowcone에는 DataSync가 미리 설치되어 있어서 DataSync는 네트워크 연결을 통해 AWS에도 데이터를 전송할 수 있음
- Snowball Edge에서는 스토리지 클러스터링으로 Snowball Edge 15개를 함께 구축하면 스토리지 크기를 늘릴 수도 있음

### Snow Family – Data Migration Usage Process

- Snow 제품군 장치를 사용하는 방법
1. 우선 배송을 위해 콘솔에서 장치를 요청
2. Snowball 클라이언트나 AWS OpsHub를 서버에 설치
3. Snowball을 서버에 연결하고 클라이언트를 사용해서 파일을 복사
4. 준비가 끝나서 장치를 다시 보내면 올바른 AWS 시설로 바로 옮겨짐
	- E 잉크 마커 덕분
5. S3 버킷에 해당 데이터를 불러들임
6. 가장 높은 보안 조치에 따라 Snowball은 전부 지워짐

## What is Edge Computing?

- Snow 제품군의 두 번째 사용 사례는 엣지 컴퓨팅
- 엣지 컴퓨팅은 데이터가 엣지 로케이션에서 생성될 때 실시간으로 처리하는 방식을 뜻함
- 엣지 로케이션은 인터넷이 없는 곳이나 클라우드에서 멀리 있는 곳은 무엇이든 여기 해당될 수 있음
	- 예를 들면 도로에 있는 트럭이나 바다 위의 배 혹은 지하의 광업소 등의 모두가 엣지 로케이션이 될 수 있음
	- 이곳에서 데이터를 만들기는 하지만 인터넷은 연결되지 않을 수도 있기 때문
	- 즉 연결이 제한되어 있거나 인터넷 액세스가 없거나 컴퓨팅을 할 수 없는 곳
- 이런 장소에서 컴퓨팅이나 데이터 처리를 해야 할 경우 엣지 컴퓨팅이 필요
- 따라서 Snowball Edge나 Snowcone을 주문해서 엣지 로케이션에 장착시키면 엣지 컴퓨팅을 시작할 수 있음
- 엣지 컴퓨팅의 예시를 들면 데이터 전처리, 또는 클라우드로 보내지 않고 엣지에서 머신 러닝하는 경우와 사전 미디어 스트림 트랜스코딩 등이 있으며, 최종적으로는 데이터를 AWS로 재전송해야 하는 경우 Snowcone이나 Snowball Edge 장치를 보내면 됨
- 다시 말해 데이터가 생성되는 곳의 아주 가까이에서 그 데이터를 처리하고, AWS로 보내는 것

## Edge Computing with AWS Snow Family

- Snowcone
	- CPU 2개와 4GB 메모리, 유무선 액세스 즉 Wi-Fi를 가지고 있음
	- 그리고 USB-C 혹은 선택적 배터리로 작동됨
- Snowball Edge
	- 두 가지 종류
		- Compute Optimized
			- 52개의 vCPU를 가지며 200GB의 RAM과 더불어 선택적 GPU가 있음
			- 영상 처리나 머신 러닝을 할 경우 사용
			- 사용 가능한 스토리지는 42TB
		- Storage Optimized
			- 더 적은 40개 vCPU와 80GB RAM을 가짐
			- 여기서는 객체 스토리지 클러스터링을 할 수 있음
- 모든 장치들은 내부 EC2 인스턴스나 람다 함수를 실행할 수 있음
	- AWS IoT Greengrass라는 서비스를 통해서 가능
- 엣지 컴퓨팅을 할 때 여러분의 트럭, 보트 등의 시설에서 아주 오랫동안 장치를 사용해야 할 경우 장기 배포 옵션을 선택할 수 있음
	- 장치를 1년에서 3년 빌리면 가격 할인을 받을 수 있음

## AWS OpsHub

![snow8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow8.png)

- Snow 제품군 중 OpsHub는 컴퓨터나 노트북에 설치하는 소프트웨어라서 클라우드를 쓰지는 않고 컴퓨터에 다운로드하여 사용해야만 함
- 연결이 되면 그래픽 인터페이스를 통해 Snow 장치에 연결해서 구성 및 사용할 수 있으니 아주 손쉬운 방법
- 이것으로 단일 장치와 클러스터 장치를 잠금 해제하고 구성할 수 있으며 파일 전송이 가능해지고 Snow 장치에서 실행되는 EC2 인스턴스를 시작 및 관리할 수 있게됨
- 또한 장치 메트릭 모니터링과 AWS 호환 서비스 실행이 가능
	- EC2 인스턴스, DataSync 혹은 네트워크 파일 시스템 등

# Solution Architecture: Snowball into Glacier

![snow9](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/snow9.png)

- 시험에 나올만한 시나리오
- Snowball을 통해 직접 Glacier에 데이터를 직접 끌어올 순 없고 Amazon S3를 사용해서 수명 주기 정책을 생성하여 Amazon Glacier로 객체를 전환할 수 있음
- Snowball이 데이터를 Amazon S3로 가져오면 S3의 수명 주기 정책을 통해 해당 데이터가 Amazon Glacier로 전환됨