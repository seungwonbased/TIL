# Software Updates Offloading

- 소프트웨어 업데이트 오프로딩
- EC2에서 작동하는 애플리케이션이 있고, 종종 소프트웨어 업데이트를 배포한다고 가정
	- 컴퓨터에서 패치를 다운로드해 EC2에 설치하는 상황
	- 소프트웨어가 새로 업데이트되면 요청을 많이 받게 됨
	- 그래서 콘텐츠는 네트워킹을 통해 다수에게 배포되는데 비용이 많이 듬
	- 애플리케이션을 변경하거나 아키텍팅을 다시 하는 일 없이 비용과 CPU를 최적화하고 싶음

## Application Current State

![suo](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/suo1.png)

- 애플리케이션의 현재 상태
- 전형적인 ELB와 ASG 개발 애플리케이션이 있는데, 다중 AZ에 걸쳐 작동
- M5 인스턴스는 소프트웨어 업데이트를 배포
- 소프트웨어 업데이트는 모두 Amazon EFS에 위치한다고 가정
- 애플리케이션을 전 세계로 확장하고 CPU 사용률을 줄이면서 비용을 절감하는 방법은 사실 아주 쉬움
	- CloudFront를 상위에 두면 됨

## Easy Way to Fix Things

![suo](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/suo2.png)

- CloudFront가 구세주
- 아키텍처에는 변화가 없음
- 엣지에서 소프트웨어 업데이트 파일은 캐시에 저장됨
	- 업데이트 파일은 변하지 않기 때문
	- 동적이 아니라 정적이라서 절대 바뀌지 않음
- EC2 인스턴스는 서버리스가 아니지만 CloudFront는 서버리스라서 확장함
- ASG가 많이 확장하지 않아 EC2와 네트워크, EFS 비용을 크게 절감하게 됨
- 또한 가용성도 확보할 수 있음
- CloudFront가 기존 애플리케이션의 확장성을 높이고 비용을 절감하는 데 용이하다는 점을 기억
	- 엣지에서 캐싱을 활용하는 정적 콘텐츠가 대부분인 경우
