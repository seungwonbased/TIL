# Storage Gateway
## Hybrid Cloud for Storage

- AWS에서는 하이브리드 클라우드를 권장
	- 이는 일부 인프라는 AWS 클라우드에 있고 나머지는 그대로 온프레미스에 두는 방식을 뜻함
- 여기에는 여러 이유가 있음
	- 클라우드 마이그레이션이 오래 걸리거나 보안 또는 규정 준수 요건이 있는 경우가 그러함
- 전략에 따라서 일래스틱 워크로드에만 클라우드를 활용하고 나머지는 온프레미스에 두는 방법을 사용할 수도 있음
- AWS의 여러 인기 서비스 중 Amazon S3는 독점 스토리지 기술로 NFS 규정 준수 파일 시스템인 EFS와는 다름
- S3 데이터를 온프레미스에 두려면 AWS Storage Gateway가 S3와 여러분의 온프레미스 인프라를 이어주는 가교 역할

## AWS Storage Cloud Native Options

![gate1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate1.png)

- AWS의 스토리지 클라우드 네이티브 옵션
- Amazon EBS나 EC2 인스턴스 같은 블록 스토리지
- Amazon EFS나 Amazon FSx 같은 파일 시스템
- Amazon S3나 Amazon Glacier 같은 객체 수준 스토리지

## AWS Storage Gateway Overview

- AWS Storage Gateway는 온프레미스 데이터와 클라우드 데이터 간의 가교 역할
- AWS Storage Gateway를 이용해서 온프레미스 데이터를 클라우드로 이동
- 간단해 보이지만 **활용 방법**에는 여러 가지가 있음
	- 먼저 재해 복구 목적으로 온프레미스 데이터를 클라우드에 백업할 수 있음
	- 혹은 백업과 복구 목적으로 클라우드 마이그레이션, 혹은 온프레미스에서 클라우드 간 스토리지 확장을 사용할 수 있음
		- 클라우드에는 콜드 데이터를 두고 온프레미스에는 이보다 더 자주 쓰는 웜 데이터를 두는 식
	- 데이터의 대부분을 AWS에 저장하고 파일 액세스 지연 시간을 줄이기 위해 AWS Storage Gateway를 온프레미스 캐시로 사용하는 방법도 있음
- Storage Gateway 타입
	- S3 파일 게이트웨이
	- FSx 파일 게이트웨이
	- 볼륨 게이트웨이
	- 테이프 게이트웨이

## Amazon S3 File Gateway

![gate2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate2.png)

- Amazon S3 파일 게이트웨이
- S3 버킷에는 원하는 스토리지 클래스를 임의로 사용할 수 있음
	- S3 Standard
	- S3 Standard-IA
	- S3 One Zone-IA
	- S3 Intelligent-Tiering
	- 모두 되지만 Glacier는 안 됨
- 예시
	- 이 S3 버킷을 온프레미스 상의 애플리케이션 서버에 연결하려는데, 이때 표준 네트워크 파일 시스템을 활용하고자 함
	- 이를 위해 S3 파일 게이트웨이를 생성하여 애플리케이션 서버가 NFS나 SMB 프로토콜을 사용하도록 함
	- 이 프로토콜을 통해 S3 파일 게이트웨이는 해당 요청을 HTTPS 요청으로 변환시켜 Amazon S3 버킷으로 보냄
	- 따라서 애플리케이션 서버가 보기에는 일반적인 파일 공유 액세스로 보이나 실제로는 Amazon S3 버킷을 사용하는 셈
	- 이렇게 S3 객체를 온프레미스 애플리케이션 서버를 통해 가져올 수 있음
- 객체를 아카이브하고자 하는 경우 S3 버킷에 수명 주기 정책을 생성하여 이후 S3 Glacier로 객체를 옮겨서 아카이브되도록 함
- S3 파일 게이트웨이로 구성한 모든 버킷은 NFS 및 SMB 프로토콜을 이용해서 액세스할 수 있고 이 외에도 사용된 데이터는 신속한 액세스를 위해 파일 게이트웨이에 캐시로 저장됨
	- 따라서 전체 S3 버킷이 아닌 최근에 사용한 파일만 파일 게이트웨이에 있음
- S3 버킷에서는 여러 스토리지 클래스를 지원, 수명 주기 정책을 사용하면 S3 Glacier로도 옮길 수 있음
- 인증
	- 버킷에 액세스하려면 각 파일 게이트웨이마다 IAM 역할을 생성해야 함
	- Windows 파일 시스템 네이티브인 SMB 프로토콜을 사용하는 경우에는 사용자 인증을 위해 Active Directory와 통합해야 함
	- 이렇게 하면 S3 파일 게이트웨이에 사용자가 액세스할 때 인증을 거치며 결국 S3 버킷에 액세스할 때도 인증을 거친다고 할 수 있음

## Amazon FSx File Gateway

![gate3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate3.png)

- Amazon FSx 파일 게이트웨이는 Amazon FSx for Windows File Server에 네이티브 액세스를 제공
- 예시
	- FSx for Windows File Server가 Amazon FSx 파일 시스템에 배포되어 있고 여러분의 회사 데이터 센터에 있는 SMB 클라이언트에 액세스하려 한다고 가정
	- Amazon FSx for Windows File Server를 사용하고 있는 경우에는 별 다르게 할 작업이 없음
		- 이미 온프레미스 시스템에서 액세스 가능하기 때문
	- 액세스 가능한데 번거롭게 Amazon FSx를 생성하려는 이유?
		- 게이트웨이를 생성하면 자주 액세스하는 데이터의 로컬 캐시를 확보할 수 있음
			- 즉 중요한 파일의 로컬 캐시가 회사 데이터 센터에 쌓이고 액세스 시 지연 시간을 단축시킬 수 있음
- 파일 게이트웨이에서 Windows 네이티브인 SMB, NTFS, Active Directory가 호환 가능
	- 따라서 그룹 파일 공유나 온프레미스를 연결할 홈 디렉터리로 사용할 수 있음

## Volume Gateway

![gate4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate4.png)

- 볼륨 게이트웨이는 블록 스토리지로 Amazon S3가 백업하는 iSCSI 프로토콜을 사용
- 볼륨이 EBS 스냅샷으로 저장되어 필요에 따라 온프레미스 볼륨을 복구할 수 있음
- 볼륨 게이트웨이의 두 가지 유형
	- 캐시 볼륨
		- 최근 데이터 액세스 시 지연 시간이 낮음
	- 저장 볼륨
		- 전체 데이터 세트가 온프레미스에 있으며 주기적 Amazon S3 백업이 따름
- 애플리케이션 서버 백업이 필요한 경우 iSCSI 프로토콜로 볼륨 게이트웨이를 생성하고 이 볼륨 게이트웨이가 Amazon S3에 저장되는 Amazon EBS 스냅샷을 생성
- 온프레미스 서버에 볼륨을 백업하는 데에 의의가 있음

## Tape Gateway

![gate5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate5.png)

- 테이프 게이트웨이는 물리적으로 테이프를 사용하는 백업 시스템이 있는 회사가 백업에 테이프 대신에 클라우드를 활용해 데이터를 백업할 수 있게 해줌
- 가상 테이프 라이브러리(VTL)는 Amazon S3와 Glacier를 이용
- 테이프 기반 프로세스의 기존 백업 데이터를 iSCSI 인터페이스를 사용하여 백업
- 업계를 선도하는 백업 소프트웨어 벤더가 사용하는 서비스
- 도식을 보면 테이프 기반인 회사 데이터 센터의 백업 서버가 있을 때 테이프 게이트웨이가 이를 클라우드에 연결하여 Amazon S3나 Amazon Glacier에 해당 테이프를 저장하는 방식

## Storage Gateway – Hardware Appliance

- 게이트웨이는 모두 회사 데이터 센터에 설치되어 있어야 함
	- 회사 데이터 센터 내에서 운영해야 함
- 하지만 종종 게이트웨이를 실행할 가상 서버가 없는 경우가 있음
	- 이 경우 AWS의 하드웨어를 사용할 수 있음
	- 이 서비스를 Storage Gateway 하드웨어 어플라이언스라고 함
- 온프레미스에 서버가 없는 경우 Storage Gateway 하드웨어 어플라이언스를 사용할 수 있음
	- amazon.com에서 주문할 수 있음
- 미니 서버가 될 하드웨어 어플라이언스를 여러분의 인프라에 설치한 후 파일 게이트웨이, 볼륨 게이트웨이 혹은 테이프 게이트웨이로 설정하면 됨
- 물리적으로 설치해야 하는데, 제대로 작동하려면 충분한 CPU, 메모리 네트워크, 그리고 SSD 캐시 리소스가 필요
- 소규모 데이터 센터의 일일 NFS 백업처럼 가상화가 없는 경우 상당히 유용

## AWS Storage Gateway 정리

![gate6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate6.png)

- 온프레미스에서 Storage Gateway VM을 배포하거나 하드웨어 어플라이언스에서 배포할 수 있는데, Storage Gateway 서비스와 AWS 클라우드로 나뉨
- 로컬 캐시를 필요로 하는 파일 게이트웨이의 경우 사용자 그룹 파일 공유 시에 NFS나 SMB 프로토콜을 이용하여 액세스할 수 있음
- S3 파일 게이트웨이를 사용하는 경우
	- S3 파일 게이트웨이에 연결해서 데이터가 Amazon S3에 백업되도록 하는 방법이 있는데, 이 경우 Glacier와 Glacier Deep Archive를 제외한 많은 스토리지 티어를 포함
	- 하지만 수명 주기 정책을 생성하여 이를 S3 Glacier를 포함한 모든 Amazon S3 스토리지 클래스에 전송할 수 있음
- FSx 파일 게이트웨이를 사용하는 경우
	- Amazon FSx for Windows File Server에 데이터를 전송하는데, 이때 데이터는 주기적으로 Amazon S3에 백업됨
- 볼륨 게이트웨이를 사용하는 경우
	- iSCSI 프로토콜을 통해 애플리케이션 서버를 갖고 볼륨을 마운트
	- 이렇게 하면 볼륨 게이트웨이는 Storage Gateway를 통해 Amazon S3에 연결되며 해당 위치에 데이터 볼륨이 저장됨
	- 그리고 Amazon S3에서 해당 데이터가 AWS EBS 볼륨으로 변환되어 AWS에 저장됨
- 테이프 게이트웨이를 사용하는 경우
	- iSCSI VTL 프로토콜을 통해 백업 애플리케이션에 연결
	- 테이프 게이트웨이는 Amazon S3를 테이프 라이브러리로 사용
	- 그리고 해당 테이프를 Glacier와 Glacier Deep Archive 티어로 이동시켜서 테이프 아카이브를 생성

# AWS Transfer Family

- AWS 전송 제품군
- 예시
	- Amazon S3 또는 EFS의 안팎으로 데이터를 전송하려고 하는데 대신 S3 APIs는 사용하고 싶지 않을 때, EFS 네트워크 파일 시스템도 사용하지 않고 FTP 프로토콜만 사용하려는 경우
	- 이런 경우에 AWS 전송 제품군을 사용
- 세 가지 프로토콜을 지원
	- AWS Transfer for FTP (File Transfer Protocol (FTP))
		- FTP의 AWS 전송
	- AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))
		- SSL을 통한 파일 전송 프로토콜인 FTPS는 암호화된 형태
	- AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))
		- SFT는 보안 파일 전송 프로토콜
	- FTP는 암호화되지 않는 반면에 FTPS와 SFTP는 전송 중에 암호화됨
	- FTP 프로토콜을 사용해서 S3 혹은 EFS에 업로드할 수 있음
- 특징
	- 완전 관리형 인프라
	- 확장성, 안정성이 높음
	- 가용성 높음
- 가격 책정
	- 시간당 프로비저닝된 엔드 포인트 비용에 전송 제품군 안팎으로 전송된 데이터의 GB당 요금을 더함
- 인증
	- 서비스 내에서 사용자 자격 증명을 저장 및 관리할 수 있음
	- 다음과 같은 기존의 인증 시스템과 통합할 수도 있음
		- Microsoft Active Directory 또는 LDAP, Okta, Amazon Cognito 또는 사용자 지정 소스
- 사용 사례
	- Amazon S3나 EFS의 FTP 인터페이스를 갖기 위해 사용
	- 파일 공유 및 공개 데이터셋 공유를 위해 사용
	- CRM, ERP 등을 하기 위해 사용

![gate7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate7.png)

- 전송 제품군에는 세 가지 종류가 있음
- 사용자는 FTP의 엔드 포인트를 통해 직접 액세스하거나 선택적으로 Route 53라는 이름의 DNS를 사용하여 FTP 서비스에 고유의 호스트 이름을 제공할 수 있음
- FTP 서비스의 전송에는 IAM 역할이 있어서 Amazon S3나 Amazon EFS의 파일을 보내거나 읽도록 함
	- 이 과정은 손쉽게 이루어지며 많은 설정이 필요 없음
- 전송 제품군 서비스의 보안을 위해 외부 인증 시스템을 통해서 사용자를 인증할 수 있는데 그런 시스템에는 Active Directory와 LDAP 등이 있음

# AWS Datasync

- AWS DataSync는 요즘 시험에 자주 출제되는 내용으로 간단한 서비스지만 무엇을 하는지 핵심 내용을 알아야 함
- 이름에서 알 수 있듯이 이 서비스는 데이터를 동기화하며 이를 통해 대용량의 데이터를 한 곳에서 다른 곳으로 옮길 수 있음
- 온프레미스나 AWS의 다른 클라우드로 데이터를 옮길 수 있는데, 이때 서버를 NFS, SMB HDFS 또는 다른 프로토콜에 연결해야 하고 옮길 위치인 온프레미스나 연결할 다른 클라우드에 에이전트가 있어야 함
- 다른 유형의 마이그레이션도 실행할 수 있는데, 한 AWS 서비스에서 다른 AWS 서비스로 데이터를 옮길 수도 있음
	- 이 경우에는 에이전트가 필요 없음
- 데이터는 모든 Amazon S3의 Glacier를 포함하여 모든 스토리지 클래스에 동기화할 수 있음
	- Amazon EFS로 네트워크 파일 시스템에 저장할 수도 있음
	- Amazon FSx는 여기 모든 운영 체제에서 사용 가능
- 복제 작업은 계속 이루어지지 않고 **일정을 지정**하여 DataSync가 매 시간, 매일, 혹은 매주 실행되도록 할 수 있음
	- 지연이 발생하긴 하지만 일정에 맞춰서 데이터가 동기화됨
- **DataSync에는 파일 권한과 메타데이터 저장 기능이 있음**
	-  이는 보안과 관련되어 NFS POSIX 파일 시스템 그리고 SMB 권한을 준수
	- 이 부분은 시험에서 상당히 중요하게 다루는데, 파일을 한 곳에서 다른 곳으로 옮길 때 이를 이용하여 파일의 메타데이터를 보존할 수 있음
- DataSync 에이전트는 상당히 강력
	- 에이전트 하나의 태스크가 초당 10Gb까지 사용할 수 있으며 네트워크 성능을 초과하고 싶지 않은 경우 대역폭에 제한을 걸 수 있음

## AWS DataSync - NFS / SMB to AWS (S3, EFS, FSx...)

![gate8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate8.png)

- SMB 혹은 NFS 프로토콜을 사용하는 온프레미스 파일을 AWS로 동기화하는 아키텍처
	- AWS에는 S3, EFS 혹은 FSx가 있음
	- 온프레미스와 DataSync가 실행되는 AWS 리전이 있음
	- 온프레미스에 NFS 또는 SMB 서버가 있는데, 이때 온프레미스에 AWS DataSync 에이전트를 설치하고 이를 NFS 또는 SMB 서버에 연결시켜 이 에이전트를 이용하여 암호화를 거쳐 DataSync 서비스에 연결
		- 이 연결은 어느 위치로든 갈 수 있음
		- Amazon S3 버킷의 스토리지 클래스나 AWS EFS 혹은 Amazon FSx일 수도 있음
	- 동기화는 온프레미스에서 AWS로 단방향으로 이루어질 수 있으며 AWS에서 다시 온프레미스로 동기화를 실행할 수도 있음
		- DataSync라는 이름에 걸맞게 양방향으로 동기화가 가능
	- 가끔 시험에서 DataSync를 이용하고자 하지만 네트워크 용량이 따라 주지 못하는 경우가 나옴
		- 이때 AWS Snowcone 장치를 사용할 수 있음
		- Snowcone 장치에는 DataSync 에이전트가 사전에 설치되어 있음
		- 온프레미스에서 Snowcone을 실행하고 데이터를 가져온 다음 DataSync 에이전트를 실행하면 다시 에이전트가 AWS 리전으로 전송되면서 AWS의 스토리지 리소스 외부에 데이터를 동기화할 수 있음

## AWS DataSync - Transfer between AWS Storage Services

![gate9](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/gate9.png)

- DataSync 에이전트를 이용하여 다른 클라우드에서 동기화할 때도 같음
- DataSync를 통해 서로 다른 AWS 스토리지 서비스 간 동기화도 가능
- Amazon S3, Amazon EFS 또는 Amazon FSx를 Amazon S3, Amazon EFS Amazon FSx로 다시 동기화하려는 경우 AWS DataSync 서비스를 사용하여 데이터 복사본을 만듬
- 서로 다른 AWS 스토리지 서비스 간 메타데이터 또한 유지됨
- 이 역시 시험에서 나올 수 있는 상당히 중요한 내용
- DataSync로 거의 대부분의 데이터를 동기화할 수 있으나 지속적이지는 않고 일정에 따라 움직임
	- 매 시간, 매일, 혹은 매주 진행할 수 있음
- 메타데이터와 파일 권한은 보존됨
- NFS 또는 SMB 서버에 연결하려면 DataSync 에이전트를 실행해야 함

