# RDS

- Relational Database Service
- SQL을 쿼리 언어로 사용하는 관리형 데이터베이스 서비스
- AWS가 관리하는 클라우드의 RDS에 데이터베이스를 생성할 수 있음
- AWS가 관리하는 DB 엔진 유형
	- PostgreSQL
	- MySQL
	- MariaDB
	- Oracle
	- Microsoft SQL Server
	- Aurora (AWS Proprietary database)

## 참고: RDS 데이터베이스 포트

- PostgreSQL: 5432
- MySQL: 3306
- Oracle RDS: 1521
- MSSQL Server: 1433
- MariaDB: 3306 (MySQL과 같음)
- Aurora: 5432 (PostgreSQL와 호환될 경우) 또는 3306 (MySQL과 호환될 경우)

## Advantage over using RDS versus deploying DB on EC2

- RDS는 관리형 서비스
	- 자동 데이터베이스 프로비저닝, 기본 운영체제 패치
	- 지속적으로 백업이 생성되므로 타임스탬프, 즉 특정 시점으로 복원 가능
	- 데이터베이스 성능을 대시보드에서 모니터링할 수 있음
	- 읽기 전용 복제본을 활용해 읽기 성능 개선 가능
	- 재해 복구 목적으로 다중 AZ를 설정 가능
	- 유지 관리 기간에 업그레이드 가능
	- 인스턴스 유형을 업그레이드해 수직 확장하거나 읽기 전용 복제본을 추가해 수평 확장할 수 있음
	- 파일 스토리지는 EBS에 구성됨 (gp2 or io1)
- 한 가지 단점은 RDS 인스턴스에 SSH 액세스할 수 없다는 점

## Storage Auto Scaling - RDS

- RDS DB 인스턴스의 스토리지를 동적으로 확장하도록 도와줌
- RDS 스토리지 오토 스케일링 기능이 활성화되어 있으면 용량이 부족할 때 자동으로 확장
	- 스토리지 용량을 늘리기 위해 DB를 다운시키고 업그레이드할 필요 X
- 사용자가 수동으로 DB 스토리지를 확장하는 작업을 피할 수 있게 해줌
- 이를 위해 **최대 스토리지 임계값(Maximum Storage Threshold)을** 설정해야 함
- 다음과 같은 상황에 자동 수정됨
	- 남은 공간이 10% 미만이 됨
	- 스토리지 부족 상태가 5분 이상 지속됨
	- 지난 수정으로부터 6시간이 지남
- 워크로드를 예측할 수 없는 애플리케이션에서 유용함
- 모든 RDS 데이터베이스 엔진에서 지원됨

## RDS Read Replicas for read scalability

![RDS1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS1.png)

- 읽기 전용 복제본은 읽기를 스케일링함
- 읽기 전용 복제본을 최대 15개까지 생성할 수 있음
- 이는 동일한 AZ 또는 AZ나 리전에 걸쳐서 생성될 수 있음
- 읽기 전용 복제본 사이에 비동기식 복제가 발생함
	- 마지막에 결과가 일관적으로 유지된다는 것을 의미 (중간에 불일치하는 기간이 있을 수 있음)
- 이와 같은 복제본 중 하나를 데이터베이스로 사용하고자 하면 그에 대한 권한을 획득해 이를 데이터베이스로 승격시켜 이용할 수도 있음
- 읽기 전용 복제본을 사용하려는 경우 주요 애플리케이션에 있는 모든 연결을 업데이트해야 하며 이를 통해 RDS 클러스터 상의 읽기 전용 복제본 전체 목록을 활용할 수 있음

### Sync vs Async Replication

1. **동기식 복제 (Synchronous Replication)**
	- 동기식 복제는 데이터가 원본 데이터베이스에서 복제 대상 데이터베이스로 전송되는 동안 대상 데이터베이스가 해당 작업을 확인하고 처리한 후 응답을 보내는 방식
	- 이 과정에서 데이터가 완전히 동기화될 때까지 원본 데이터베이스의 트랜잭션은 대기
	- 이로 인해 데이터의 일관성과 신뢰성이 높아지지만, 대역폭 소요나 지연시간 문제가 발생할 수 있음
    
2. **비동기식 복제 (Asynchronous Replication)**
	- 비동기식 복제는 데이터를 원본 데이터베이스에서 복제 대상 데이터베이스로 비동기적으로 전송하는 방식
	- 원본 데이터베이스에서 트랜잭션이 완료되면 해당 데이터는 즉시 복제되지 않고, 후속 처리나 큐에 저장된 후 대상 데이터베이스로 전달됨
	- 이 방식은 대역폭 사용량이 적고 지연시간이 상대적으로 짧을 수 있지만, 데이터 복제 시점에서 일시적인 데이터의 불일치가 발생할 수 있음

### 참고: 개발 프로세스 환경

1. 개발 환경 (Development Environment)
	- 개발자들이 신규 기능을 개발하고 테스트하는 환경
	- 개발자들은 여기서 코드를 작성하고 테스트하며 초기 버전을 개발
2. 테스트 환경 (Testing Environment): 
	- 개발된 코드를 테스트하는 환경, 프로덕션과 유사한 환경을 구성
	- 다양한 종류의 테스트(단위 테스트, 통합 테스트, 성능 테스트 등)를 수행하여 코드의 문제를 발견하고 수정
3. 스테이징 환경 (Staging Environment): 
	- 프로덕션 환경과 유사한 환경을 만들어내는데, 여기에서는 실제 사용자 데이터와 유사한 데이터를 사용하여 최종 테스트를 진행
	- 실제 프로덕션 환경으로 배포하기 전에 소프트웨어의 동작을 최종적으로 확인
1. 프로덕션 환경 (Production Environment)
	- 실제 사용자에게 서비스되는 환경
	- 여기서는 개발한 소프트웨어가 운영 중인 서비스로서 사용자에게 제공됨

### RDS Read Replicas Use Cases

![RDS2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS2.png)

- 예를 들어 평균적인 로드를 감당하고 있는 프로덕션 데이터베이스가 있다고 가정
- 프로덕션 DB에선 메인 RDS DB 인스턴스에 대한 읽기 및 쓰기가 수행됨
- 이때 새로운 팀이 와서 데이터를 기반으로 몇 가지 보고와 분석을 실행한다고 가정
- 보고 애플리케이션을 메인 RDS 데이터베이스 인스턴스에 연결하면 오버로드가 발생하고 프로덕션 애플리케이션의 속도가 느려질텐데, 이를 피하고자 함
- 이때 새로운 워크로드에 대한 읽기 전용 복제본을 생성하면 메인 RDS DB 인스턴스와 읽기 전용 복제본 간에 비동기식 복제 발생
- 보고 애플리케이션에서 생성한 읽기 전용 복제본에서 읽기 및 분석 작업 실행
- 이렇게 하면 프로덕션은 영향을 받지 않으니 좋음
- 읽기 전용 복제본은 SELECT 명령에만 사용됨

### RDS Read Replicas Network Cost

![RDS3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS3.png)

- AWS에서는 하나의 가용 영역에서 다른 가용 영역으로 데이터가 이동할 때 비용 발생
- 예외가 존재하는데, 이 예외는 보통 관리형 서비스일 때 나타남
- RDS 읽기 전용 복제본은 관리형 서비스
- 읽기 전용 복제본이 **다른 AZ 상이지만 동일한 리전 내에 있을 때는 비용이 발생하지 않음**
	- 비동기식 복제로 읽기 전용 복제의 복제 트래픽이 하나의 AZ에서 다른 AZ로 넘어가더라도 무료로 이동 가능
- 하지만 서로 다른 리전에 복제본이 존재하는 경우 RDS DB 인스턴스와 읽기 전용 복제본이 여러 리전을 넘나들어야 하기 때문에 네트워크에 대한 복제 비용 발생

## RDS Multi AZ (Disaster Recovery)

![RDS4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS4.png)

- RDS 다중 AZ는 주로 재해 복구에 사용됨
- 위 예시 상에서 애플리케이션은 가용 영역 AZ A에서 읽기와 쓰기를 수행하는 마스터 DB 인스턴스
- 동기식으로 이를 AZ B에 스탠바이 인스턴스로 복제
- 마스터 데이터베이스의 모든 변화를 **동기적으로 복제**하는 것인데, 이는 애플리케이션의 마스터에 쓰이는 변경 사항이 대기 인스턴스에도 그대로 복제된다는 것을 의미
- 즉 **하나의 DNS 이름**을 갖고 애플리케이션 또한 하나의 DNS 이름으로 통신하며 마스터에 문제가 생길 때도 스탠바이 DB에 자동으로 장애 조치가 수행됨
- **가용성을 높일 수 있기** 때문에 다중 AZ라 불림
- 전체 AZ 또는 네트워크가 손실될 때에 대비한 장애 조치이자 마스터 DB의 인스턴스 또는 스토리지에 장애가 발생할 때 스탠바이 DB가 새로운 마스터가 될 수 있도록 함
- 따로 앱에 수동으로 조치를 취할 필요가 없음
	- 자동으로 DB에 연결이 시도되고 장애 조치가 필요하게 될 대도 스탠바이가 마스터로 승격되는 과정이 자동으로 이뤄짐
- 스케일링에 이용되지 않음
	- 스탠바이 데이터베이스는 단지 대기 목적 하나만 수행
	- 누구도 이를 읽거나 쓸 수 없음
- **참고: 읽기 전용 복제본을 다중 AZ로 설정할 수 있음**

## RDS - From Single-AZ to Multo-AZ

![RDS5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS5.png)

### 단일 AZ에서 다중 AZ로 전환이 가능한지?

- 이 작업에는 다운타임이 전혀 없다는 것을 염두에 둬야 함
	- 즉 단일 AZ에서 다중 AZ로 전환할 때 데이터베이스를 중지할 필요가 없음
- 데이터베이스 수정을 클릭하고 다중 AZ 기능을 활성화시키기만 하면 됨
- 이를 통해 RDS DB는 마스터를 갖고 동기식 복제본인 스탠바이 DB를 확보
- 이때 설정을 수정하는 것 외에는 아무 작업도 할 필요가 없음

### 내부적으로 발생하는 일

- 기본 DB의 RDS가 자동으로 스냅샷을 생성
- 이 스냅샷은 새로운 스탠바이 DB에 복원됨
- 스탠바이 DB가 복원되면 두 데이터베이스 간 동기화가 설정되므로 스탠바이 DB가 메인 RDS DB 내용을 모두 수용해 다중 AZ 설정 상태가 됨

## RDS Custom

- RDS에는 기저 운영 체제나 사용자 지정 기능에 액세스할 수 없음
- 그러나 RDS Custom에서는 가능
- RDS Custom에서는 Oracle, Microsoft SQL Server 두 유형의 데이터베이스를 다룸
- RDS Custom 덕분에 이 둘에서 OS 및 DB 사용자 지정 기능에 액세스할 수 있음
- RDS를 통해 AWS에서의 데이터베이스 자동화 설정, 운영, 스케일링의 장점을 모두 챙길 수 있음
- RDS Custom 옵션을 추가하면 커스텀이라는 말 뜻대로 기저 데이터베이스와 운영 체제에 액세스할 수 있게 됨
	- 따라서 내부 설정 구성, 패치 적용, 네이티브 기능 활성화가 가능
	- SSH 또는 SSM 세션 관리자를 사용해 RDS 뒤에 있는 기저 EC2 인스턴스에 액세스 가능
- Amazon RDS Custom을 사용 중이라면 사용자 지정 설정을 적용하고 검색을 추가할 수 있음
- RDS가 수시로 자동화, 유지 관리 또는 스케일링과 같은 작업을 수행하지 않도록 자동화를 꺼두는 것이 좋음
- 기저 EC2 인스턴스에 액세스가 가능하므로 문제가 쉽게 발생할 수 있기 때문에 DB 스냅샷을 만들어 두는 것이 좋음
- RDS vs RDS Custom
	- RDS: DB 전체를 관리, 운영체제와 나머지는 AWS에서 관리하고 사용자는 아무 것도 안 해도 됨
	- RDS Custom: Oracle & Microsoft SQL Server에서만 사용 가능, 기저 운영 체제와 DB에 대한 관리자 권한 전체를 갖게 됨

## RDS - Summary

- 관리형 PostgreSQL, MySQL Oracle, SQL 서버, MariaDB용 및 사용자 지정 RDS가 있음
- Amazon RDS를 사용할 때는 RDS 인스턴스 크기와 EBS 볼륨 유형 및 크기를 프로비저닝해야 함
	- 스토리지 계층에 오토 스케일링 기능이 있어도 프로비저닝해야 함
- 읽기 용량 확장을 위해 읽기 전용 복제본을 지원
	- 프로덕션 데이터베이스에 대해 분석을 실행해야 하는 애플리케이션은 읽기 전용 복제본을 생성하면 효율적
- 고가용성 목적으로 대기 데이터베이스를 다중 AZ에 둘 수 있음
	- 대기 데이터베이스는 재해 발생 대비용이며 쿼리를 수행할 수는 없음
- RDS 데이터베이스 보안은 IAM을 통해 설정할 수 있음
	- 사용자 이름과 비밀번호로 DB에 연결할 수 있게 하거나 일부 사용자에게 IAM 인증을 부여할 수 있음
- 네트워크 보안을 위해 Amazon RDS DB에 보안 그룹을 설정할 수 있음
- 저장 데이터 암호화에는 KMS를 전송 데이터 암호화에는 SSL과 TLS를 사용할 수 있음
- 자동 백업 옵션도 있음
	- 최대 35일까지 지원
	- 최근 35일 내에는 지정 시간 복구 기능을 지원하고 복구 시 새 데이터베이스가 생성됨
- 장기 보존 백업이 필요한 경우에는 수동 DB 스냅샷을 사용하면 됨
- 유지 관리 기능을 예약할 수 있기 때문에 데이터베이스 다운타임이 발생할 수 있음
	- 프로비저닝하거나 AWS가 데이터베이스 엔진을 주기적으로 업데이트하고 기본 EC2 인스턴스에 보안 패치를 실행할 때 발생할 수 있음
- RDS 프록시를 강제하여 RDS에 IAM 인증을 추가하는 기능이 있음
- Secrets Manager와 통합하여 DB 자격 증명을 관리할 수도 있음
- 기본 인스턴스에 액세스하고 사용자 지정하려면 RDS 사용자 지정 옵션을 사용할 수 있음
	- Oracle, SQL 서버 유형의 데이터베이스에서 지원됨
- Amazon RDS는 관계형 데이터베이스를 저장하는 데 활용됨
	- RDBMS와 온라인 트랜잭션 처리 데이터베이스(OLTP)가 포함
	- SQL 쿼리를 실행할 수도 있고 트랜잭션에도 활용됨

# Amazon Aurora

- AWS 고유의 기술
- 오픈소스는 아니지만 PostgreSQL, MySQL과 호환되도록 만듬
	- Aurora 데이터베이스에 호환 가능한 드라이버가 있는데, PostgreSQL, MySQL DB에 연결하면 작동함
- 클라우드에 최적화되어 있고 여러가지 똑똑한 최적화를 통해 RDS의 MySQL보다 5배 높은 성능, RDS의 PostgreSQL보다 3배 높은 성능
- Aurora의 스토리지는 자동으로 확장됨
	- 자동으로는 10GB to 128TB
- 15개의 읽기 전용 복제본을 둘 수 있음
	- MySQL에서는 5개만 가능했음
	- 복제 속도도 훨씬 빠름
- 장애 조치가 즉각적
	- 다중 AZ나 MySQL RDS보다 훨씬 빠름
- 기본적으로 클라우드 네이티브이므로 가용성이 높음
- 비용은 RDS에 비해 20% 정도 높지만 스케일링 측면에서 훨씬 효율적
	- 오히려 비용 절감 가능

## Aurora High Availability and Read Scaling

![RDS6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS6.png)

- Aurora는 세 개의 AZ에 걸쳐 무언가를 기록할 때마다 6개의 사본을 저장
	- 쓰기에는 6개 사본 중 4개에 성공하면 완료
	- 읽기에는 6개 사본 중 3개에 성공해야 완료
	- 자가 복구 과정이 있음
		- 일부 데이터가 손상되거나 문제가 있으면 백엔드에서 P2P 복제를 통한 자가 복구가 진행됨
	- 단일 볼륨에 의존하지 않고 수백 개의 볼륨을 사용
		- 작은 블록 단위로 자가 복구 또는 확장이 일어남
		- 백엔드에서 진행되는 과정이며 리스크를 크게 감소시켜 줌
- 하나의 Aurora 인스턴스(Master)가 쓰기를 받음
- 마스터가 작동하지 않으면 평균 30초 내로 장애 조치가 시작됨
- 마스터 외에 읽기를 제공하는 읽기 전용 복제본을 15개까지 둘 수 있음
	- 복제본을 많이 두고 읽기 워크로드를 스케일링할 수 있음
- 마스터에 문제가 생기면 읽기 전용 복제본 중 하나가 마스터가 되어 대체
- 복제본들은 리전 간 복제를 지원

## Aurora DB Cluster

![RDS7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS7.png)

- 공유 볼륨은 10GB에서 128TB까지 자동으로 확장됨
- 스토리지에 쓰는 것은 마스터에만 가능
- 마스터가 바뀌거나 장애 조치가 실행될 수 있으므로 Aurora에서는 **Writer 엔드포인트**를 제공
	- Writer 엔드포인트는 DNS 이름으로 항상 마스터를 가리킴
- 따라서 장애 조치 후에도 클라이언트는 라이터 엔드포인트와 상호작용 하게되며 올바른 인스턴스로 자동 리다이렉트됨
- 읽기 전용 복제본을 15개까지 생성 가능하며 자동 스케일링을 설정해 항상 적절한 수의 읽기 전용 복제본이 존재하도록 할 수 있음
	- 자동 스케일링이 켜져 있는 경우 앱 입장에서는 복제본이 어딨고, URL이 뭔지 파악하기 어려울 수도 있는데, **Reader 엔드포인트**가 이 역할을 함
	- 리더 엔드포인트는 라이터 엔드포인트와 같은 기능을 함
		- 연결 로드 밸런싱에 도움
		- 모든 읽기 전용 복제본과 자동으로 연결됨
	- 따라서 클라이언트가 리더 엔드포인트에 연결될 때마다 읽기 전용 복제본 중 하나로 연결됨
	- **로드 밸런싱이 Statement 레벨이 아닌 Connection 레벨에서 일어난다는 사실을 꼭 기억**

## Feature of Aurora

- 자동 장애 조치
- 백업 및 복구
- 격리 및 보안
- 산업 규정 준수
- 자동 스케일링을 통한 버튼식 스케일링 
- 제로 다운타임 자동 패치 설치
- 고급 모니터링
- 일상 유지 관리
- 백트랙: 과거 어느 시점의 데이터로도 복원 가능
	- 백업에 의존하지 않고 다른 방법을 사용

## Aurora Replicas - Auto Scaling

![RDS8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS8.png)

- 처음에 세 개의 Aurora 인스턴스가 있음
- 하나는 라이터 엔드포인트를 통해 쓰고 있고 나머지 둘은 리더 엔드포인트를 통해 읽고 있음
- 이제 리더 엔드포인트의 읽기 요청이 매우 많다고 가정
	- Amazon Aurora 데이터베이스의 CPU 사용량이 증가하는 것
- 이런 상황에서 복제본 자동 스케일링을 설정하면 됨
	- 이렇게 하면 Amazon Aurora 복제본이 추가됨
- 이 새로운 복제본을 처리하기 위해 자동으로 리더 엔드포인트가 연장됨
	- 그러면 이 새로운 복제본들이 트래픽을 받게 되고 읽기가 좀 더 분산된 형태로 이루어지게 됨
- 이를 통해 전반적 CPU 사용량 감소를 기대할 수 있음

## Aurora - Custom Endpoints

![RDS9](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS9.png)

- 위 예시와 같은 상황이라고 가정
	- 그러나 이번에는 두 종류의 다른 복제본이 있음
	- db.r3.large가 있고 db.r5.2xlarge도 있음
	- 따라서 일부 복제본은 다른 것보다 크기가 큼
	- 이렇게 하는 이유는 Aurora 인스턴스의 서브셋을 사용자 지정 엔드포인트로 정의하기 위함
- 큰 Aurora 인스턴스 두 개에 사용자 지정 엔드포인트를 정의했다고 가정
	- 이렇게 하는 이유는 이 인스턴스들이 더 강력해서 특정 복제본에서 분석 쿼리를 실행하는 게 더 나을 수도 있기 때문
- 이렇게 사용자 지정 엔드포인트를 정의할 수 있음
- 일반적으로 사용자 지정 엔드포인트를 정의하면 리더 엔드포인트는 사용하지 않음
	- 사라지는 않지만 더 이상 사용하지 않게 됨
- 실제 프로젝트에서는 다양한 종류의 워크로드에 대해 사용자 지정 엔드포인트를 여러 개 만들게 됨
	- 그렇게 해서 쿼리가 Aurora 복제본의 서브셋으로만 향하게 할 수 있음

## Aurora Serverless

![RDS10](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS10.png)

- Aurora 서버리스는 실제 사용량에 기반한 자동 데이터베이스 인스턴스화와 자동 스케일을 가능하게 함
- **비정기적, 간헐적, 또는 예측 불허한 워크로드에 유용**
- 용량 계획을 세울 필요가 전혀 없으며 각 Aurora 인스턴스에 대해 매 초당 비용을 지불
	- 비용 면에서 훨씬 더 효율적

### 원리

- 클라이언트는 Aurora 관리 하위 프록시 플릿과 소통
- 그리고 백엔드에서 워크로드에 기반해 서버리스 방식으로 여러 Aurora 인스턴스가 생성됨
	- 따라서 용량을 미리 확보할 필요가 없음

## Aurora Multi-Master

![RDS11](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS11.png)

- 멀티 마스터(Multi-Master)는 라이터 노드에 대한 즉각적 장애 조치로 라이터 노드에 높은 가용성을 갖추고자 할 때 사용
- 이 경우 Aurora 클러스터의 모든 노드에서 읽기 및 쓰기가 가능
- 기존에는 하나의 쓰기 노드만이 있고 그 노드에 장애가 발생하면 읽기 전용 복제본 하나를 새로운 마스터로 승격시켜야 했음
- 위 예시에는 Aurora 인스턴스 세 개가 있음
	- 모든 인스턴스 간에 복제가 일어나고 있음
	- 공유 스토리지 볼륨이 있고 클라이언트는 다중 데이터베이스 접속이 가능
	- 모든 Aurora 인스턴스에서 쓰기가 가능
	- 하나의 Aurora 인스턴스가 장애를 일으키면 자동으로 장애 조치를 통해 다른 인스턴스로 넘어갈 수 있음
	- 이렇게 라이터 노드에 대한 즉각적인 장애 조치가 가능

## Global Aurora

![RDS12](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS12.png)

- Aurora 리전 간 복제본
	- 재해 복구에 많은 도움이 되며 간단하게 생성이 가능
- 하지만 최근에는 Aurora Global 데이터베이스를 만드는 것을 권장
	- Global Aurora의 경우 모든 쓰기 및 읽기가 진행되는 하나의 기본 리전이 있음
	- 복제 지연이 1초 미만인 보조 읽기 전용 리전을 다섯 개까지 설정할 수 있고 각 보조 리전마다 읽기 전용 복제본을 16개까지 생성 가능
	- 이렇게 하면 세계 각지에 있는 읽기 전용 복제본의 지연 시간을 단축할 수 있음
	- 또한 한 리전의 데이터베이스가 작동 중단될 경우 재해 복구 목적으로 다른 지역을 승격하는데 필요한 RTO 즉 복구 시간 목표는 1분 미만
		- 다른 리전으로 복구하는 데 1분도 걸리지 않는다는 뜻
	- **Aurora Global 데이터베이스에서 리전에 걸쳐 데이터를 복제하는데 걸리는 시간은 평균 1초 미만**
		- 이런 문장이 시험에 보인다면 Global Aurora를 사용하란 단서
- 위 예시에서
	- us-east-1이 기본 리전이고 여기서 애플리케이션이 읽기 및 쓰기를 하게 됨
	- 보조 리전을 eu-west-1로 가정
		- 여기서는 Aurora Global 데이터베이스로 복제가 이루어지고 해당 설정에서 이곳의 애플리케이션은 읽기만이 가능
	- 만약 us-east-1이 장애를 일으키면 장애 조치를 통해 읽기/쓰기 Aurora 클러스터로 승격시켜 eu-west-1로 넘어갈 수 있음

## Aurora Machine Learning

![RDS13](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS13.png)

- Aurora 머신 러닝은 ML 기반 예측을 SQL 인터페이스로 애플리케이션에 적용하는 것
- Aurora와 다양한 AWS 머신 러닝 서비스를 쉽고 최적화된 방식으로 안전하게 통합할 수 있음
- 지원하는 두 서비스
	- 백엔드에서 어떤 머신 러닝 모델이라도 사용할 수 있게 해주는 SageMaker
	- 감정 분석에 사용하는 Amazon Comprehend
	- 서비스를 몰라도 머신 저런 것들이 Aurora와의 통합이 가능하다는 사실만 알면 됨
- Aurora 머신 러닝을 사용하기 위해 머신 러닝 경험은 필요하지 않음
- 활용 예시로는 감지 맞춤 광고, 감정 분석 제품 추천 등이 있으며 모두 Aurora 내에서 가능
- 아키텍처 개념
	- 먼저 Aurora가 AWS의 머신 러닝 서비스와 연결됨
	- 그리고 애플리케이션이 간단한 SQL 쿼리를 실행
		- 예를 들어 추천 제품은 무엇인가? 같은 쿼리
	- 그러면 Aurora가 머신 러닝 서비스로 데이터를 전송
		- 예를 들어 사용자 프로필, 쇼핑 내역 등
	- 머신 러닝 서비스는 Aurora로 직접 예측을 반환
	- 이렇게 해서 예를 들어 빨간색 셔츠와 파란색 바지를 추천
	- 그리고 Aurora는 쿼리에 대한 결과를 애플리케이션에 반환
	- 모두 SQL 쿼리를 통해 이루어지며 매우 유용

# RDS & Aurora Backup and Monitoring
## RDS Backups

- 자동 백업
	- RDS 서비스가 자동으로 매일 데이터베이스 유지 관리 시간에 데이터베이스 전체를 백업한다는 뜻
	- 거기에 더해서 5분마다 트랜잭션 로그도 백업됨
		- 이는 가장 최신 백업이 5분 전임을 뜻함
	- 자동 백업을 사용하면 5분 전 어떤 시점으로도 복구가 가능
	- 자동 백업 보유 기간은 1일에서 35일까지로 설정할 수 있음
	- 이 기능을 비활성화하려면 0으로 설정해서 자동 백업을 비활성화하면 됨
- 수동 DB 스냅샷 생성
	- 수동 데이터베이스 스냅샷
	- 이름에서도 알 수 있듯이 이는 사용자가 수동으로 트리거해야 함
	- 하지만 수동 백업은 원하는 만큼 오랫동안 보유할 수 있다는 장점이 있음
		- 자동 백업은 기간이 지나면 사라지지만 수동으로 만든 DB 스냅샷은 원하는 기간 동안 보유가 가능
- Trick
	- 백업에 대해 비용을 절감할 수 있는 방법이 있음
	- 예를 들어 RDS 데이터베이스가 있는데 매달 2시간만 사용할 예정이라고 가정
		- 이 경우 데이터베이스를 정지하게 되면 정지했음에도 불구하고 스토리지 비용을 지불해야 함
		- 그 대신 2시간 사용 후에 스냅샷을 만들고 원본 데이터베이스는 삭제하는 방법이 있음
			- 스냅샷의 비용은 RDS 데이터베이스 스토리지보다 훨씬 저렴
		- 그리고 데이터베이스를 다시 사용할 때가 되면 스냅샷을 복원해서 사용하면 됨

## Aurora Backups

- 자동 백업
	- 하루에서 35일까지 보유 가능한 자동 백업
		- 비활성화는 불가능
		- RDS에서는 비활성화가 가능하지만 Aurora에서는 불가능
	- 지정 시간 복구 기능
		- 이 기능은 정해진 시간 범위 내의 어느 시점으로도 복구가 가능
- 수동 DB 스냅샷 생성
	- 원하는 기간 보유가 가능
- Aurora와 RDS의 백업은 유사한 부분이 많음

## RDS & Aurora Restore options

- 복원 기능
- RDS 및 Aurora 백업 또는 스냅샷
	- 이를 새로운 데이터베이스로 복원할 수 있음
	- 자동 백업이나 수동 스냅샷을 복원할 때마다 새로운 데이터베이스가 생성됨
- 다른 방법으로는 S3로부터 MySQL RDS 데이터베이스를 복원
	- 기본적인 개념은 온프레미스 데이터베이스의 백업을 만들어서 객체 스토리지인 Amazon S3에 두는 것
- 마지막으로 RDS에는 Amazon S3에서 MySQL를 실행 중인 새로운 RDS 인스턴스로 백업 파일을 복원하는 방법
	- 그러지 않고 MySQL Aurora 클러스터로 복구하고 싶으면 온프레미스 데이터베이스를 외부로 다시 백업하고 Percona XtraBackup이라는 소프트웨어를 사용하면 됨
		- 현재는 이 프로그램만 사용 가능
	- 그러면 Percona Xtrabackup의 백업 파일이 Amazon S3로 전송됨
	- 그리고 백업 파일을 MySQL을 실행 중인 새 Aurora 클러스터로 복원
- 차이점은 RDS MySQL로 복원할 때에는 데이터베이스 백업만 있으면 되고 Aurora MySQL로 복원할 때에는 Percona XtraBackup를 사용해서 백업한 다음 S3로부터 Aurora DB 클러스터로 복원하면 된다는 점

## Aurora Cloning

![RDS14](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS14.png)

- Aurora 데이터베이스 복제 기능
	- 기존의 데이터베이스로부터 새로운 Aurora DB 클러스터를 만들 수 있음
- 프로덕션 Aurora 데이터베이스가 있다고 가정
	- 이를 새로운 Amazon Aurora 데이터베이스로 복제할 수 있음
	- 예를 들어 스테이징 Aurora로 복제해 프로덕션 데이터베이스에 영향을 주지 않고 개발 또는 테스트를 수행할 수 있음
- 실제로 스냅샷을 만들고 복원하는 것보다 복제한 Aurora를 사용하는 편이 더 빠름
- 새 데이터베이스는 원래 데이터와 같은 클러스터 볼륨을 사용
	- 이것이 더 빠른 이유임
- 데이터베이스 복제는 매우 빠르고 비용 면에서 효율적이며 프로덕션 데이터베이스에 영향을 주지 않음
- 가령 프로덕션 데이터베이스를 복제해 스테이징 데이터베이스를 생성하는 것을 보면 알 수 있음

# RDS & Aurora Security

- RDS 및 Aurora 데이터베이스에 저장된 데이터를 암호화할 수 있음
	- 이는 데이터가 볼륨에 암호화된다는 뜻
- KMS를 사용해 마스터와 모든 복제본의 암호화가 이루어지며 이는 데이터베이스를 처음 실행할 때 정의됨
- 어떤 이유에서든 마스터, 즉 주 데이터베이스를 암호화하지 않았다면 읽기 전용 복제본을 암호화할 수 없음
- 암호화 되어있지 않은 기존 데이터베이스를 암호화 하려면 암호화 되지 않은 데이터베이스의 데이터베이스 스냅샷을 가지고 와서 암호화된 데이터베이스 형태로 데이터베이스 스냅샷을 복원해야 함
	- 스냅샷 생성 및 복원 작업을 거쳐야 함
- In-flight encryption
	- 클라이언트와 데이터베이스 간의 전송 중 데이터 암호화
	- RDS 및 Aurora의 각 데이터베이스는 기본적으로 전송 중 데이터 암호화 기능을 갖추고 있음
	- 따라서 클라이언트는 AWS 웹사이트에서 제공하는 AWS의 TLS 루트 인증서를 사용해야 함
- IAM Authentication
	- 데이터베이스 인증
	- RDS와 Aurora이기 때문에 사용자 이름과 패스워드라는 전통적인 조합을 사용할 수도 있음
	- 반면 AWS이기도 하므로 IAM 역할을 사용해서 데이터베이스에 접속할 수도 있음
	- 예를 들어 EC2 인스턴스에 IAM 역할이 있다면 이를 사용해 사용자 이름이나 패스워드 없이 직접 데이터베이스를 인증할 수 있음
		- 이는 AWS 내의 모든 보안 기능을 IAM로 관리하는 데 도움
- Security Groups
	- 보안 그룹을 사용해서 데이터베이스에 대한 네트워크 액세스를 통제할 수도 있음
	- 특정 포트, IP, 보안 그룹을 허용하거나 차단할 수 있음
- No SSH available
	- RDS와 Aurora에는 SSH 액세스가 없음
	- 관리형 서비스가 있기 때문
	- 다만 AWS의 RDS 커스텀 서비스를 사용한다면 예외
- Audit logs can be enabled
	- 감사 로그가 있어서 시간에 따라 RDS 및 Aurora에서 어떤 쿼리가 생성되고 있는지 데이터베이스를 확인하려면 감사 로그 작성을 활성화하면 됨
	- 로그는 시간이 지나면 자동으로 삭제되며 장기간 보관하고 싶다면 AWS에 있는 CloudWatch Logs라는 전용 서비스로 전송해야 함

# Amazon RDS Proxy

![RDS15](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/RDS15.png)

- 완전 관리형 RDS 데이터베이스 프록시를 배포할 수 있음
- RDS 데이터베이스에 바로 액세스하면 되는데 왜 프록시가 필요한지?
	- Amazon RDS 프록시를 사용하면 애플리케이션이 데이터베이스 내에서 데이터베이스 연결 풀을 형성하고 공유할 수 있음
	- 애플리케이션을 RDS 데이터베이스 인스턴스에 일일이 연결하는 대신 프록시에 연결하면 프록시가 하나의 풀에 연결을 모아 RDS 데이터베이스 인스턴스로 가는 연결이 줄어듬
	- 왜 이렇게 하는지?
		- RDS 데이터베이스 인스턴스에 연결이 많은 경우 CPU와 RAM 등 데이터베이스 리소스의 부담을 줄여 데이터베이스 효율성을 향상시킬 수 있고 데이터베이스에 개방된 연결과 시간초과를 최소화할 수 있기 때문
- RDS 프록시는 완전한 서버리스로 오토 스케일링이 가능해 용량을 관리할 필요가 없고 가용성이 높음
- 다중 AZ를 지원
- 가령 RDS 데이터베이스 인스턴스에 장애 조치가 발생하면 기본 인스턴스가 아니라 대기 인스턴스로 실행되며 RDS 프록시 덕분에 RDS와 Aurora의 장애 조치 시간을 66%까지 줄일 수 있음
	- 메인 RDS 데이터베이스 인스턴스에 애플리케이션을 모두 연결하고 장애 조치를 각자 처리하게 하는 대신 장애 조치와 무관한 RDS 프록시에 연결하는 것
	- RDS 프록시가 장애 조치가 발생한 RDS 데이터베이스 인스턴스를 처리하므로 장애 조치 시간이 개선됨
- RDS 프록시는 MySQL, PostgreSQL MariaDB용 RDS를 지원하며 MySQL, PostgreSQL용 Aurora를 지원
- 애플리케이션의 코드를 변경하지 않아도 되고 RDS 데이터베이스 인스턴스나 Aurora 데이터베이스에 연결하는 대신 RDS 프록시에 연결하기만 하면 됨
- 데이터베이스에 IAM 인증을 강제함으로써 IAM 인증을 통해서만 RDS 데이터베이스 인스턴스에 연결하도록 할 수 있음
	- 이때 자격 증명은 AWS Secrets Manager 서비스에 안전하게 저장됨
	- 데이터베이스에 IAM 인증을 강제하고 싶다면 RDS 프록시를 사용
- RDS는 퍼블릭 액세스가 절대로 불가능
	- VPC 내에서만 액세스 가능
	- 인터넷을 통해 RDS 프록시에 연결할 수 없으니 보안이 훌륭
- RDS 프록시를 사용하면 상당히 유용한 서비스
	- 코드 조각을 실행하는 Lambda 함수
		- Lambda 함수는 증식
		- 여러 개가 생성되고 사라지는 속도가 매우 빠름
		- RDS 데이터베이스 인스턴스에 수만 개의 Lambda 함수가 순식간에 발생했다 사라지며 연결을 개방한다고 가정
			- 개방된 연결에 시간초과도 생길 테니 난장판이 될 것임
		- 따라서 RDS 프록시를 사용해서 Lambda 함수의 연결 풀을 생성하면 Lambda 함수가 RDS 프록시를 오버로드
		- RDS 프록시가 풀을 생성하면 RDS 데이터베이스 인스턴스 연결이 줄어 문제가 해결될 것

## Amazon Aurora - Summary

- Aurora는 데이터베이스 엔진인 PostgreSQL, MySQL과 호환되는 API
- 컴퓨팅과 스토리지가 분리된 특별한 서비스
- 스토리지의 경우 기본적으로 데이터를 세 가용 영역에 걸쳐 여섯 개의 복제본에 저장
	- 이 설정은 바꿀 수 없음
	- 가용성이 아주 높음
	- 뿐만 아니라 스토리지에 문제가 발생하면 백그라운드에서 자가 복구 처리
	- 오토 스케일링이 기능이 내장돼 있어 스토리지 확장도 문제 없음
- 컴퓨팅의 경우 클러스터화된 실제 데이터베이스 인스턴스를 여러 가용 영역에 걸쳐 저장할 수도 있음
- 읽기 전용 복제본이 있다면 로드가 증가할 때 오토 스케일링을 통해 용량을 늘릴 수 있음
- 데이터베이스 인스턴스 클러스터가 있으므로 읽기와 쓰기를 위한 사용자 지정 엔드 포인트, 즉 라이터(writer) 엔드 포인트와 리더(reader) 엔드 포인트가 필요
- Aurora는 RDS와 동일한 보안 모니터링, 유지 관리 기능을 가짐
	- 데이터는 결국 같은 엔진을 사용하기 때문
- Aurora의 백업 및 복구 옵션도 알아야 함
- Aurora 추가 기능
	- Aurora Serverless
		- Aurora Serverless는 워크로드가 간헐적이거나 예측할 수 없는 경우 용량 계획을 하지 않아도 되므로 매우 유용
	- Aurora Multi-Master
		- 쓰기 고가용성을 위해 쓰기 장애 조치가 지속해서 필요할 때 Aurora Multi-Master를 사용하면 DB 인스턴스 여러 개가 스토리지에 쓰기를 할 수 있도록 설정할 수 있음
	- Aurora Global
		- 글로벌 데이터베이스를 원할 땐 Aurora Global을 사용
		- 데이터베이스가 복제된 리전마다 최대 16개의 데이터베이스 읽기 전용 인스턴스를 제공하고 리전 간 스토리지 복제에 걸리는 시간은 일반적으로 1초 미만
		- 시험을 위해 기억
		- 기본 리전에 문제가 발생하면 보조 리전을 새 기본 리전으로 승격할 수 있음
	- Aurora Machine Learning
		- Aurora Machine Learning 모듈을 사용하면 SageMaker와 Comprehend를 사용해 Aurora에서 머신 러닝을 수행할 수 있음
	- Aurora Database Cloning
		- 프로덕션 데이터베이스로부터 테스트 데이터베이스나 스테이징 데이터베이스를 만들려면 Aurora Database Cloning 기능을 사용해 기존 클러스터에서 새 Aurora 클러스터를 만들면 됨
		- 스냅샷을 사용해서 복구하는 것보다 훨씬 빠름
- Aurora의 사용 사례는 RDS와 같지만 Aurora는 유지 관리할 게 적고 유연성은 높으며 성능도 더 좋을 뿐만 아니라 내장된 기능도 더 많음
