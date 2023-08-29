# Solutions Architecture: MyWordPress.com
## Stateful Web App: MyWordPress.com

- 상태 유지 웹 애플리케이션인 MyWordPress.com
- 완전히 확장 가능한 WordPress 웹사이트
- WordPress는 웹사이트를 만드는데 흔히 사용되는 방법으로 아주 인기가 많음
- 사용자 데이터와 블로그 내용 등 모든 게 MySQL 데이터베이스에 저장되야 하고 글로벌하게 확장하고 싶음

## RDS Layer

![mwp](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mwp1.png)

- 첫 번째로 할 일은 RDS가 있는 계층을 생성하는 것
- 이제 백엔드에 RDS가 있는 이러한 아키텍처는 우리에게 매우 익숙
- 다중 AZ이고 모든 EC2 인스턴스에 걸쳐 적용됨

## Scaling with Aurora: Multi AZ & Read Replicas

![mwp](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mwp2.png)

- 만약 정말로 크게 확장하고 싶다면 이 계층을 오로라 MySQL로 교체
	- 다중 AZ, 읽기 전용 복제본, 원하면 글로벌 데이터베이스까지 적용할 수 있음
	- 오로라를 사용함으로써 연산을 줄일 수 있음

## Storing Images with EBS

![mwp](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mwp3.png)

- 하나의 EC2 인스턴스가 있고 하나의 EBS 볼륨이 연결되어 있는 단일 AZ의 아주 단순한 솔루션 아키텍처
- 일단 로드 밸런서에 연결되어 있음
	- 사용자는 로드 밸런서로 이미지를 보내고 싶음
- 그러면 그 이미지는 EBS까지 도달
	- 그리고 EBS에 저장됨
- EC2 인스턴스가 하나일 때는 정말 작동
	- 이미지를 불러올 때도 EBS 볼륨으로부터 이미지를 읽어들여서 사용자에게 보냄

![mwp](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mwp4.png)

- 그러나 확장하기 시작하면 문제가 생김
- 이제 두 개의 EC2 인스턴스와 두 개의 AZ가 있음
	- 그리고 각각의 EC2 인스턴스는 각각의 EBS 볼륨을 가지고 있음
- 한 인스턴스로 이미지를 보내면 해당 EBS 볼륨에 저장됨
	- 이미지를 불러오고 싶다면 읽어들일 수 있음
- 그러나 흔한 오류는 이미지를 읽어들이기 위해 다른 쪽으로 갈 때 발생
	- 그러면 같은 EBS 볼륨이 아니기 때문에 이미지가 없고 이미지에 접근할 수 없음
- EBS 볼륨의 단점은 하나의 인스턴스만 있을 때는 아주 잘 작동하지만 다중 AZ 또는 다중 인스턴스로 확장을 시작하면 문제가 생기기 시작

## Storing Images with EFS

![mwp](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mwp5.png)

- 이를 해결하려면 EFS를 사용할 수 있음
- 완전히 동일한 아키텍처를 사용
	- 하지만 이번엔 EFS 즉 네트워크 파일 시스템 드라이브를 사용
- EFS는 NFS
- EFS는 탄력적인 네트워크 인터페이스를 위해 각각의 AZ에 ENI를 생성
	- 이 ENI는 EFS 드라이브에 접근하는 모든 EC2에 사용할 수 있음
- 여기에서 정말 좋은 것은 스토리지가 모든 인스턴스에게 공유된다는 점
- 즉 M5 인스턴스로 이미지를 보내면 ENI를 거쳐 EFS로 전달되고 이미지가 EFS에 저장됨
- 이미지를 불러오고 싶다면 아래쪽으로 가서 ENI를 거쳐 EFS로부터 읽어들임
- 이것은 가용 영역이나 인스턴스 숫자에 관계없이 모든 인스턴스가 동일한 파일에 접근하도록 허용하기 위해 다수의 EC2 인스턴스에 걸쳐 웹사이트 스토리지를 확장하는 아주 흔한 방법

## EBS vs EFS

- EBS: 단일 인스턴스 애플리케이션에서 아주 잘 작동
- EFS: 다중 AZ에 걸친 분산 애플리케이션을 만들 수 있음
- 비용 관점에서 볼 때 EBS가 EFS 보다 저렴
	- 그러나 EFS를 사용할 때 많은 이점을 얻을 수 있음