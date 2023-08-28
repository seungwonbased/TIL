# ElastiCache
## Amazon ElastiCache Overview

- ElastiCache는 RDS와 동일한 방식으로 관계형 데이터베이스를 관리할 수 있음
- 일래스티 캐시는 레디스 또는 멤캐시드와 같은 캐시 기술을 관리할 수 있도록 함
- 캐시란?
	- 캐시는 높은 성능과 낮은 지연 시간을 가진 인 메모리 데이터베이스
- 일래스티 캐시를 사용하면 읽기 집약적인 워크로드의 부하를 줄이는데 도움
- 이 개념은 일반적인 쿼리가 캐시 되어 데이터베이스가 매번 쿼리 되지 않는 것이며 캐시는 이러한 쿼리의 결과를 검색할 때 사용할 수 있는 것
	- 애플리케이션의 상태를 Amazon 일래스티 캐시에 저장해 애플리케이션을 무상태로 만들 수 있도록 함
- RDS와 같은 장점을 갖기 때문에 AWS는 동일한 유지 보수를 수행
	- 운영 체제, 패치, 최적화와 설정, 구성, 모니터링, 장애 회복 그리고 백업을 수행
- 일래스티 캐시를 사용할 때 애플리케이션에 관한 몇 가지 코드 변경을 요청할 수도 있음
	- 단순한 활성화가 아니라 캐시를 사용
	- 데이터베이스 쿼리 전과 후에 캐시를 쿼리하도록 애플리케이션을 변경해야 함

## ElastiCache Solution Architecture - DB Cache

![EC1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EC1.png)

- 일래스티 캐시 사용을 위한 아키텍처
- 일래스티 캐시와 RDS 데이터베이스 그리고 애플리케이션이 있고 애플리케이션은 일래스티 캐시를 쿼리함
- 캐시 히트(cache hit): 쿼리가 이미 생성됐는지, 이미 생성되어 일래스티 캐시에 저장됐는지 확인하는 것
	- 이는 일래스티 캐시에서 바로 응답을 얻어서 쿼리하기 위해 데이터베이스로 이동하는 동선을 줄여줌
- 캐시 미스(cache miss): 데이터베이스에서 데이터를 가져와서 데이터베이스에서 읽음
- 동일한 쿼리가 발생하는 다른 애플리케이션이나 인스턴스에서는 데이터를 캐시에 다시 기록하여 다음에는 같은 쿼리로 캐시 히트를 얻도록 함
- 이는 RDS 데이터베이스에서 부하를 줄이는데 도움을 주는데, 데이터를 캐시에 저장하기 때문에 캐시 무효화 전략이 있어야 하며 가장 최근 데이터만 사용하는지 확인해야 함
	- 이것이 캐싱 기술 사용과 연관된 어려움

## ElastiCache Solution Architecture - User Session Store

![EC2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EC2.png)

- 다른 아키텍처는 사용자 세션을 저장해 애플리케이션을 무상태로 만드는 것
- 사용자가 애플리케이션의 모든 계정에 로그인하면 애플리케이션이 일래스티 캐시에 세션 데이터를 기록하는 것
- 사용자가 애플리케이션의 다른 인스턴스로 리디렉션 되면 애플리케이션은 일래스티 캐시에서 직접 세션 캐시를 검색할 수 있음
	- 그래서 사용자는 계속 로그인한 상태로 한 번 더 로그인 할 필요가 없음
- 사용자의 세션 데이터를 일래스티 캐시에 기록해서 애플리케이션을 무상태로 만든 것

## ElastiCache - Redis vs Memcached

- 레디스와 멤캐시드의 차이를 전반적으로 이해하는 것도 중요
- 레디스(Redis)
	- 자동 장애 조치로 다중 AZ를 수행하는 기술이며 읽기 전용 복제본은 읽기 스케일링에 사용되며 가용성이 높음
	- 약간 RDS와 비슷
	- 그리고 지속성으로 인해 데이터 내구성도 있으며 백업과 기능 복원 기능도 있음
- 멤캐시드(Memcached)
	- 데이터 분할에 다중 노드를 사용하고 이를 샤딩(sharding)이라고 함
	- 가용성이 높지 않고 복제도 발생하지 않음
	- 지속적인 캐시가 아님
	- 백업과 복원 기능도 없음
	- 다중 스레드 아키텍처로 몇몇 샤딩과 함께 캐시에서 함께 실행되는 여러 인스턴스가 있음
- 여기서 기억해야 할 것
	- 레디스는 고가용성과 백업 읽기 전용 복제본 등이 있음
	- 멤캐시드는 데이터를 손실할 수 없는 단순한 분산 캐시
		- 가용성이 높지 않고 백업과 복원 기능도 없음

## ElastiCache - Cache Security

![EC3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EC3.png)

- 일래스티 캐시의 모든 캐시는 IAM 인증을 지원하지 않음
- 일래스티 캐시에서 정의할 IAM 정책은 AWS API 수준 보안에만 사용됨
	- 즉 캐시 생성, 캐시 삭제 같은 종류의 작업을 의미
	- 그러나 캐시 내의 모든 작업은 IAM을 사용하지 않음
- Redis AUTH
	- 레디스를 인증하려면 레디스 AUTH를 사용하여 레디스 클러스터를 생성할 때 비밀번호나 토큰를 설정할 수 있음
		- 이렇게 하면 캐시에 들어갈 때 비밀번호를 사용할 수 있음
		- 이는 캐시에 사용할 수 있는 보안 그룹에 대한 추가적인 수준의 보안
	- 전송 중 암호화를 위해 SSL 보안을 지원할 수 있음
- Memcached
	- 멤캐시드는 좀 더 높은 수준인 SASL 기반 인증을 지원
		- 상당히 고급이라 다른 종류의 인증 메커니즘
- 캐시의 보안
	- EC2 인스턴스에는 자체 보안 그룹도 레디스에 액세스할 수 있는 자체 보안 그룹이 있음
		- 그래서 일래스티 캐시를 사용하여 보안 그룹 수준의 보안을 수행할 수 있음
	- 다음으로 전송 중 암호화를 위해 SSL 암호화가 있음
	- 인증을 위해 일래스티 캐시에서 레디스 종류의 캐시를 사용할 때 레디스 AUTH를 가질 수 얻을 수 있음

## Patterns for ElastiCache

![EC4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EC4.png)

- 일래스티 캐시에 데이터를 불러오는 패턴에는 세 가지가 있음
- Lazy Loading
	- 레이지 로딩으로 모든 읽기 데이터가 캐시되고 데이터가 캐시에서 최신성을 잃을 수 있음
	- 레이지 로딩의 전략
		- 일래스티 캐시에 캐시가 히트하면 애플리케이션이 캐시로부터 데이터를 받음
		- 만일 캐시 미스가 있으면 데이터베이스에서 데이터를 읽고 캐시에 씀
		- 캐시 히트가 없을 때만 발생하기 때문에 레이지 로딩이라고 함
		- 그런 다음 데이터를 Amazon 일래스티 캐시에 불러옴
- Write Through
	- 라이트 스루는 데이터가 오래된 데이터가 없는 데이터베이스에 기록될 때마다 캐시에 데이터를 추가하거나 업데이트하는 것
- Session Store
	- 일래스티 캐시를 세션 저장소로 쓸 수 있고 Time To Live(TTL) 속성으로 세션을 만료시킬 수 있음

## ElastiCache - Redis Use Case

![EC5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EC5.png)

- **레디스의 사용 사례**
- 시험을 위해 알아둬야할 게임 리더보드 생성
	- 게임의 어느 시점에서든 누가 1위고 누가 2위고 3위인지 가려내는 개념
	- 그래서 레디스에는 고유성과 요소 순서를 모두 보장하는 정렬된 집합(Redis Sorted Sets)이라는 기능이 있음
		- 요소가 추가될 때마다 실시간으로 순위가 매겨진 다음 올바른 순서로 추가
		- 레디스 클러스터가 있는 경우 실시간으로 1위, 2위, 3위 플레이어가 있는 실시간 리더보드를 생성한다는 개념
	- 모든 레디스 캐시는 동일한 리더보드를 사용할 수 있음
		- 즉 레디스로 Amazon 일래스티 캐시와 통신할 때 클라이언트는 이 실시간 리더보드에 액세스할 수 있고 애플리케이션 측에서 이 기능을 프로그래밍할 필요가 없음
		- 실시간 리더보드에 액세스하기 위해 레디스의 정렬된 집합을 활용할 수 있음