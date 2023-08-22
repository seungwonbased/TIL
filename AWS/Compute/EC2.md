# EC2 (Elastic Computing Cloud)

- Amazon EC2는 클라우드에서 컴퓨팅 파워의 규모를 자유자재로 변경할 수 있는 웹 서비스
- **IaaS (Infra as a Service)**
- 64000 EBS IOPS를 달성하려면 **Nitro-based EC2**를 프로비저닝해야 함
  - Non-nitro EC2 사용 시 최대 32000 EBS IOPS
- EC2 인스턴스를 다시 시작하면 Public IP가 변경될 수 있음
  - **Elastic IP (탄력적 IP)**를 사용해 EC2 인스턴스에 고정 Public IPv4를 할당
  - 기본적으로 모든 AWS 계정은 지역당 탄력적 IP 주소를 5개로 제한
- EC2 인스턴스를 동일한 AZ에 배치해 데이터 전송 비용을 줄임
- **EC2 Hibernate**는 인스턴스 메모리(RAM)의 내용을 Amazon EBS 루트 볼륨에 저장
- **VM Import/Export**를 사용해 가상 머신 이미지를 가져오고 Amazon EC2 AMI로 변환하여 EC2 인스턴스를 시작

## EC2 Instance types

| Instance class |    사용 목적    |                                                     사용 예시                                                     |
| :------------: | :-------------: | :---------------------------------------------------------------------------------------------------------------: |
|      T, M      |    일반 목적    |                        웹 서버, 마이크로 서비스, 소규모 DB, 가상 데스크탑, 개발 환경 구축                         |
|       C        |  컴퓨팅 최적화  |                      고성능 컴퓨팅, 일괄 처리, 게임 서버, 과학적 모델링, CPU 기반 기계 학습                       |
|    R, X, Z     |  메모리 최적화  |                                  인메모리 캐시, 고성능 DB, 실시간 빅데이터 분석                                   |
|    F, G, P     |   가속 컴퓨팅   |                           고성능 GPU, 그래픽 집약적 애플리케이션, 기계 학습, 음석 인식                            |
|    D, H, I     | 스토리지 최적화 | EC2 인스턴스 스토리지, 높은 I/O 성능, HDFS, MapReduce file system, Spark, Hadoop, Redshift, Kafka, Elastic Search |

## EC2 시작 유형 (EC2 Launch Type)

### On-demand

- 사용한 만큼 지불
- 실행하는 인스턴스에 따라 시간 또는 초당 컴퓨팅 파워로 측정된 가격을 지불
- 약정이 필요 없음
- 장기적인 수요 예측이 힘들거나 유연하게 EC2를 사용하고 싶을 때 사용

#### On-demand 권장 소비자

- 선불 결제 또는 장기 약정 없이 Amazon EC2의 저렴한 비용과 유연성을 선호하는 사용자
- 중단할 수 없는 단기, 예측할 수 없는 워크로드가 있는 애플리케이션
- Amazon EC2에서 처음으로 개발 또는 테스트 중인 애플리케이션

### 예약 인스턴스 (Reserved Instance - RI Pricing)

- 미리 일정 기간(1 ~ 3년)을 약정해서 쓰는 방식 (선납금 납부)
- 최대 75% 저렴
- 수요 예측이 확실할 때 사용

### Reserved Standard

### Reserved Convertible

#### Reserved Instance 권장 소비자

- 수요가 꾸준한 애플리케이션
- 예약 용량이 필요한 애플리케이션
- 총 컴퓨팅 비용 절감을 위해 1년 또는 3년 동안 EC2를 사용하기로 약정할 수 있는 사용자

### Spot Instnace

- 경매 형식으로 시장에 남는 인스턴스를 구매해서 쓰는 방식
- On-demand 요금에 비해 최대 90% 저렴
- AWS가 임의로 인스턴스를 회수해갈 수 있음
  - 반환 시간 예측 불가능
  - 인스턴스 수요가 적어지면 다시 인스턴스를 할당받아 애플리케이션을 다시 구동 가능

#### Spot Instance 원리

![SpotInstance](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/SpotInstance.png)

- 원하는 지정 가격을 정하고, 이 지정 가격보다 비싸지면 반납해서 인스턴스를 중지시키고, 가격이 싸지면 다시 인스턴스를 사용하는 원리

#### Spot Instnce 권장 소비자

- 시작 및 종료 시간이 자유로운 응용 애플리케이션
- 빅데이터 처리, ML 등 많은 인스턴스가 필요한 작업에 사용
- 컴퓨팅 가격이 매우 저렴해야만 수익이 나는 애플리케이션
- 대량의 서버 용량 추가로 긴급히 컴퓨팅 파워가 필요한 사용자

### 전용 호스트 (Dedicated Host)

- EC2 인스턴스 용량을 갖춘 물리적 서버를 대여
- 보안적인 이유, 라이센스에 따른 이유, 퍼포먼스를 위한 이유 등으로 사용됨

#### Dedicated Host 권장 소비자

- EC2에서 Microsoft 및 Oracle 같은 공급업체의 적격 소프트웨어 라이센스를 사용할 경우
- 기존의 물리적 서버에서 EC2를 사용할 경우

## EC2 요금 정책

### 가격 순서

- Spot < Reserved < On-demand < Dedicated
- EC2의 가격 모델은 EBS와는 별도로 청구된다 (EBS는 사용한 만큼 지불)
- 기타 데이터 통신 등의 비용은 별도로 청구
  - 이때, AWS는 AWS 바깥으로 나가는 트래픽에만 요금 부과
    - 수십 TB 파일을 업로드할 땐 무료
    - 해당 파일을 다운로드 할 땐 요금 청구
- Reserved Instance는 20개, vCPU On-demand 표준 인스턴스 1,152개, vCPU Spot Instance 1,440개로 제한
