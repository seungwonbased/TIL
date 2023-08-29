# Amazon MQ

- SQS와 SNS는 AWS의 독점 기술이기 때문에 클라우드 네이티브 서비스
	- 각자 사용하는 API 세트가 따로 있음
- 온프레미스에서 기존 애플리케이션을 실행하는 경우 개방형 프로토콜인 MQTT, AMQP, STOMP, WSS Openwire 등을 사용하면 됨
- 그리고 애플리케이션을 클라우드에 마이그레이션하는 경우에 SQS, SNS 프로토콜 혹은 API를 사용하기 위해 애플리케이션을 다시 구축하고 싶지 않고 MQTT, AMQP 등과 같은 기존에 쓰던 프로토콜을 사용하고 싶을 수 있는데, 이때 Amazon MQ를 쓰면 됨
- MQ는 상당히 간단한 서비스로 RabbitMQ와 ActiveMQ 두 가지 기술을 위한 관리형 메시지 브로커 서비스
- RabbitMQ와 ActiveMQ는 온프레미스 기술로 앞서 설명했던 개방형 프로토콜 액세스를 제공
- Amazon MQ를 이용하면 해당 브로커의 관리형 버전을 클라우드에서 사용할 수 있음
- 특징
	- 무한 확장이 가능한 SQS나 SNS처럼 확장성이 크지는 않음
		- Amazon MQ는 서버에서 실행되므로 서버 문제가 있을 수 있기 때문
	- 고가용성을 위해 장애 조치와 함께 다중 AZ 설정을 실행할 수 있음
	- SQS처럼 보이는 대기열 기능과 SNS처럼 보이는 주제 기능을 단일 브로커의 일부로 제공

## Amazon MQ High Availability

![mq](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mq1.png)

- us-east-1라는 리전에 us-east-1a와 us-east-1b 두 개의 가용 영역이 있다고 가정
- 영역 하나는 활성 상태 그리고 또 다른 영역은 대기 상태
- 이제 두 영역에 각각 활성, 대기 상태인 Amazon MQ 브로커를 추가
- 장애 조치 실행을 위해 백엔드 스토리지에 Amazon EFS도 정의해야 함
	- EFS는 네트워크 파일 시스템으로 다중 가용 영역에 마운트할 수 있음
- 이렇게 설정하면 장애 조치가 일어날 때마다 대기 상태 영역 역시 Amazon EFS에 마운트되므로 첫 번째 활성 대기열과 동일한 데이터를 가질 수 있고 따라서 장애 조치도 올바르게 실행됨
- 클라이언트가 Amazon MQ 브로커와 통신해서 장애 조치가 실행되는 경우에도 Amazon EFS 덕분에 데이터가 저장됨
- 이 내용은 시험에 나오니 꼭 기억

