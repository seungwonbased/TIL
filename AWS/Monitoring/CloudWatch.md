# Amazon CloudWatch
## Amazon CloudWatch Metrics

- CloudWatch는 AWS의 모든 서비스에 대한 지표를 제공
	- 따라서 계정에서 일어나는 모든 일을 모니터링할 수 있음
- 지표(Metric)는 모니터링할 변수
	- EC2 인스턴스의 지표로는 CPUUtilization, NetworkIn 등
	- Amazon S3의 지표로는 버킷 크기 등
- 지표는 이름공간(namespaces)에 속하므로 각기 다른 이름공간에 저장되며 서비스당 이름공간은 하나
- 지표의 속성으로 측정 기준(Dimension)이 있음
	- CPU 사용률에 대한 지표는 특정 인스턴스 ID나 특정 환경 등과 관련이 있을 것임
	- 지표당 최대 측정 기준은 10개
- 지표는 시간을 기반으로 하므로 타임스탬프가 꼭 있어야 함
- 지표가 많아지면 CloudWatch 대시보드에 추가해 모든 지표를 한 번에 볼 수 있음
- CloudWatch 사용자 지정 지표를 만들 수도 있음
	- AWS의 서비스에서 제공되는 지표만 보는 대신 자체 지표를 만들 수 있음
		- 예를 들어 EC2 인스턴스로부터 메모리 사용량을 추출
	- CloudWatch 사용자 지정 지표의 전형적인 사용 사례

### CloudWatch Metric Streams

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa1.png)

- CloudWatch 지표는 CloudWatch 외부로 스트리밍할 수 있음
- CloudWatch 지표를 원하는 대상으로 지속적으로 스트리밍하면 거의 실시간으로 전송되고 지연 시간도 짧아짐
- Amazon Kinesis Data Firehose가 대상이 될 수 있고, 여기서 원하는 곳으로 전송할 수 있음
- Datadog, Dynatrace, New Relic, Splunk, Sumo Logic 같은 타사 서비스 제공자로 CloudWatch 지표를 직접 전송할 수도 있음
- 작동 원리
	- CloudWatch 지표는 Kinesis Data Firehose로 거의 실시간으로 스트리밍됨
	- 별도의 설정이 필요
	- Kinesis Data Firehose에서 Amazon S3 버킷으로 지표를 보내 Amazon Athena를 사용해 지표를 분석할 수 있음
	- Amazon Redshift를 사용해 지표로 데이터 웨어하우스를 만들거나 Amazon OpenSearch를 사용해 대시보드를 생성하고 지표 분석을 할 수도 있음
	- 모든 이름공간에 속한 모든 지표를 스트리밍하거나 몇몇 이름공간의 지표만 필터링할 수 있음
	- 필터링한 지표의 서브셋만 Kinesis Data Firehose로 보낼 수 있음

## CloudWatch Logs

- CloudWatch Logs는 로그들을 로그 그룹으로 그룹화
- 로그 그룹의 이름으로는 보통 애플리케이션을 나타냄
- 각 로그 그룹 내에는 로그 스트림이 있음
	- 로그 스트림은 애플리케이션 내 인스턴스나 다양한 로그 파일명 또는 컨테이너 등을 나타냄
- 로그 만료일도 정의할 수 있음
	- 로그가 영원히 만료되지 않게 하거나 일정 기간 후 만료되게 할 수 있음
		- CloudWatch Logs 스토리지는 유료기 때문
- 로그는 Amazon S3나 Kinesis Data Streams 및 Firehose Lambda, ElasticSearch 등으로 내보낼 수 있음

### CloudWatch Logs - Sources

- SDK, CloudWatch Logs 에이전트, 통합 CloudWatch 에이전트를 통해 로그를 보낼 수 있음
	- 통합 CloudWatch 에이전트를 통한 로그 전송 방식은 요즘 사장되는 추세
- Elastic Beanstalk는 애플리케이션의 로그를 CloudWatch에 전송
- ECS는 컨테이너의 로그를 CloudWatch에 전송
- Lambda는 함수 자체에서 로그를 보냄
- VPC Flow Logs는 VPC 메타데이터 네트워크 트래픽 로그를 보냄
- API Gateway는 받은 모든 요청을 CloudWatch Logs에 보냄
- CloudTrail은 필터링해 로그를 보냄
- Route53은 모든 DNS 쿼리를 로그로 저장 

### CloudWatch Logs Insights

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa2.png)

- 다음으로 중요한 것은 지표 필터와 인사이트 정의
- CloudWatch Logs에서 필터 표현식을 쓸 수 있음
	- 예를 들어 로그 내 특정 IP를 찾을 수 있음
	- 특정 IP가 찍힌 로그를 찾거나 또는 'ERROR'라는 문구를 가진 모든 로그를 찾을 수 있음
- 이 지표 필터를 통해 출현 빈도를 계산해 지표를 만들 수 있음
	- 그렇게 만들어진 지표는 CloudWatch 경보로 연동할 수 있음
- 또 다른 멋진 기능은 CloudWatch Logs Insights
- 이 기능을 통해 로그를 쿼리하고 이 쿼리를 대시보드에 바로 추가할 수 있음
	- 자주 쓰이는 쿼리들도 AWS에서 추가해 놓음
	- 아주 사용하기 간편한 쿼리 언어

### CloudWatch Logs - S3 Export

- CloudWatch에서 S3로 보낼 때 내보내기가 가능해질 때까지 최대 12시간은 걸릴 수 있음
- API 호출은 CreateExportTask
- 시간이 따로 설정돼 있어 실시간이 아님

### CloudWatch Logs Subscriptions

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa3.png)

- CloudWatch Logs에서 로그를 스트림하고 싶다면 구독 필터를 사용해야 함
- 구독 필터란 CloudWatch Logs 상단에 적용하여 이를 목적지로 보내는 필터를 말함
	- 목적지의 예를 들자면 Lambda 함수가 있음
	- 직접 정의하든 AWS 제공을 쓰든 Amazon ES에 데이터를 보낼 때 씀
- Kinesis Data Firehose는 Amazon S3에 근 실시간 전송을 할 때 사용
	- CloudWatch에서 S3로 내보내기한 것보다 훨씬 빠른 방법
	- Kinesis Data Streams로 Kinesis Data Firehose에 데이터를 보낼 수도 있음
- 그 외에도 KDF, KDA EC2, Lambda 등이 있음

## CloudWatch Logs Aggregation Multi-Account & Multi Region

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa4.png)

- CloudWatch Logs로 여러 계정과 리전간 로그를 집계할 수 있음
	- 여러 계정을 생성할 수 있음
	- 예를 들어 구독 필터를 가진 리전1에 계정A가 있다면 공통 계정을 통해 Kinesis Data Streams로 보내짐
		- 계정B와 리전2, 3도 마찬가지

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa5.png)

- 모든 로그를 모아 Kinesis Data Streams 그리고 Data Firehose 이후에는 Amazon S3로 보낼 수 있는 것

### CloudWatch Logs for EC2

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa6.png)

- CloudWatch 에이전트를 이용해서 EC2 인스턴스에서 로그와 지표를 받아 CloudWatch에서 표시할 수 있는 방법
- EC2 인스턴스에서 CloudWatch로는 기본적으로 어떤 로그도 옮겨지지 않음
	- 그러려면 EC2 인스턴스에 에이전트라는 작은 프로그램을 실행시켜 원하는 로그 파일을 푸시해야 함
	- CloudWatch Logs 에이전트가 EC2 인스턴스에서 작동해 CloudWatch Logs로 로그를 보내는 것
- 그러려면 EC2 인스턴스에 로그를 보낼 수 있게 해주는 IAM 역할이 있어야 함
- 또한 이 에이전트는 온프레미스 환경에서도 셋업될 수 있음
	- 따라서 VM-ware 같은 가상 서버에서 온프레미스 환경으로 서비스를 제공할 수 있음
	- 같은 에이전트를 설치하면 CloudWatch Logs로 로그를 보낼 수도 있음

#### CloudWatch Logs Agent & Unified Agent

- CloudWatch에는 두 가지 에이전트가 있음
	- 둘 다 EC2 인스턴스나 온프레미스 서버 같은 가상 서버를 위한 것
- CloudWatch Logs Agent
	- 좀 더 오래된 CloudWatch Logs 에이전트
	- CloudWatch Logs로 로그만 보냄
	- 
- CloudWatch Unified Agent
	- 좀 더 최근에 나온 통합 CloudWatch 에이전트
	- 반면 통합 에이전트는 프로세스나 RAM 같은 추가적인 시스템 단계 지표를 수집
	- 그리고 CloudWatch Logs에 로그를 보냄
	- 지표와 로그를 둘 다 사용하기 때문에 통합 에이전트인 것
	- 이 에이전트가 더 나음
	- 또한 이전 버전에는 없는 기능인 SSM Parameter Store를 이용해 에이전트를 쉽게 구성할 수 있음
	- 모든 통합 에이전트를 대상으로 중앙 집중식 환경 구성을 할 수 있음

##### CloudWatch Unified Agent - Metrics

- 에이전트를 EC2 인스턴스나 Linux 서버에 설치하면 지표를 수집할 수 있음
- CPU
	- 훨씬 세분화된 수준으로 CPU 지표를 가져올 수 있음
	- active, guest, idle, system user, steal 등
	- 다 외울 필요는 없고, 세부 지표에 이런 게 있다고 알아두면 됨
- Disk
	- 디스크 지표는 free, use, total
	- 디스크 IO에는 writes, reads, bytes, iops
- RAM
	- free, inactive, used, total, cached
- Netstat
	- 넷 상태에는 TCP와 UDP 연결, 넷 패킷, 바이트 수 등
- Processes
	- 프로세스에서 얻을 수 있는 지표는 total, dead, bloqued idle, running, sleep
- Swap Space
	- 메모리처럼 쓰는 디스크인 스와프 공간에서는 free, used, used %를 볼 수 있음
- **여기서의 핵심은 통합 CloudWatch 에이전트가 더 세부적이고 많은 지표를 수집한다는 것**
	- 기본 EC2 인스턴스 모니터링에 비하면
		- EC2에서 곧장 디스크, CPU, 스와프나 메모리가 아닌 네트워크에 대한 지표를 얻을 수 있긴 하지만 상당히 포괄적인 지표만 얻을 수 있음
	- 세부 지표를 얻고 싶다면 통합 CloudWatch 에이전트를 이용하면 됨

## CloudWatch Alarms

- CloudWatch 경보
- 경보는 지표에서 알림을 트리거할 때 사용됨
- sampling, %, max, min 등의 다양한 옵션을 추가해 복잡한 경보를 정의할 수도 있음
- 경보의 세 상태
	- OK: 트리거되지 않았음
	- INSUFFICIENT_DATA: 상태를 결정할 데이터가 부족함
	- ALARM: 임계값이 위반되어 알림이 보내지는 상태
- 기간은 경보가 지표를 평가하는 기간
	- 짧게 설정할 수도 길게 설정할 수도 있음
	- 고해상도 사용자 지정 지표에도 적용될 수 있는데 10초, 30초 또는 60초의 배수로 설정될 수 있음

### CloudWatch Alarm Targets

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa7.png)

- 경보의 주 대상은 세 개
- EC2
	- EC2 인스턴스의 동작들
	- 인스턴스를 멈추거나, 삭제하거나 재시작하거나, 복구하는 등의 동작
- Auto Scaling
	- 오토 스케일링 동작의 트리거
	- 스케일 아웃과 스케일 인 등
- SNS
	- SNS 서비스에 알림을 보내는 것
	- 예를 들어 SNS 서비스를 람다 함수로 연결해 람다 함수를 통해 위반된 경보에 우리가 원하는 작업을 수행할 수 있는 것

### EC2 Instance Recovery

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa8.png)

- EC2 인스턴스 복구
- EC2 VM 점검을 위한 상태 점검이 있고, 하드웨어 점검을 위한 시스템 상태 점검도 있음
	- 이 두 점검으로 CloudWatch 경보를 만들 수 있음
- 특정 EC2 인스턴스를 모니터링하다가 경보가 위반됐을 경우, EC2 인스턴스 복구를 실행해 EC2 인스턴스를 다른 호스트로 옮기는 등의 작업을 할 수 있음
- 복구할 때는 동일한 사설, 공인, 탄력적 IP 주소와 동일한 메타데이터 동일한 인스턴스 배치 그룹을 가지게 됨
- 또한 SNS 주제에 경보를 보내 EC2 인스턴스가 복구되었다는 사실을 인지하게 할 수도 있음

### CloudWatch Alarm: Good to Know

![cwa](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cwa9.png)

- CloudWatch 경보의 유용한 기능 중 CloudWatch Logs 지표 필터에 기반해 경보를 생성하는 기능이 있음
- CloudWatch Logs는 경보에 연결된 지표 필터를 가질 수 있음
	- 때문에 특정 단어를 포함한 인스턴스가 지나치게 많이 생기면 알림을 보낼 수 있고 Amazon SNS에 메시지를 보낼 수 있음
- 경보 알림을 시험해 보고 싶다면 set-alarm-state라는 CLI 호출을 사용하면 됨
	- 이것은 특정 임계값이 도달하지 않아도 경보를 트리거하고 싶을 때 유용
	- 본인의 인프라 내에서 경보가 올바르게 트리거되어 제대로 된 결과를 내는지 여부를 알 수 있음
