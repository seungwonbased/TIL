# AWS Global Infra

## AWS Region

- 데이터 센터 클러스터가 있는 전세계의 물리적 위치

| AWS Region                | Code           |
| ------------------------- | -------------- |
| US East (N. Virginia)     | us-east-1      |
| US East (Ohio)            | us-east-2      |
| US West (N. California)   | us-west-1      |
| US West (Oregon)          | us-west-2      |
| Africa (Cape Town)        | af-south-1     |
| Asia Pacific (Hong Kong)  | ap-east-1      |
| Asia Pacific (Mumbai)     | ap-south-1     |
| Asia Pacific (Osaka)      | ap-northeast-3 |
| Asia Pacific (Seoul)      | ap-northeast-2 |
| Asia Pacific (Singapore)  | ap-southeast-1 |
| Asia Pacific (Sydney)     | ap-southeast-2 |
| Asia Pacific (Tokyo)      | ap-northeast-1 |
| Canada (Central)          | ca-central-1   |
| Europe (Frankfurt)        | eu-central-1   |
| Europe (Ireland)          | eu-west-1      |
| Europe (London)           | eu-west-2      |
| Europe (Milan)            | eu-south-1     |
| Europe (Paris)            | eu-west-3      |
| Europe (Stockholm)        | eu-north-1     |
| Middle East (Bahrain)     | me-south-1     |
| South America (São Paulo) | sa-east-1      |

- EC2, ELB, S3, Lambda 등과 같은 대부분의 AWS 서비스를 사용하려면 먼저 Region을 선택해야 함
- IAM, AWS Organizations, Route 53, CloudFront, WAF 등과 같은 글로벌 AWS 서비스에 대해서는 Region을 선택할 수 없음
- 각 AWS 리전은 지리적 영역 내에서 물리적으로 분리된 여러 개의 격리된 AZ로 구성됨

## AZ (Availability Zone, 가용 영역)
- 중복 전원, 네트워킹 및 연결 기능을 갖춘 하나 이상의 개별 데이터 센터
- AWS Region의 모든 AZ는 고대역폭, 짧은 지연 시간 네트워킹으로 상호 연결됨
- 고객은 고가용성, 확장성, 내결함성 및 짧은 지연 시간을 위해 동일한 지역의 여러 AZ에 애플리케이션을 배포함
- 한 지역의 AZ는 일반적으로 3개이고, 최소는 2개, 최대는 6개
    - 고가용성을 위해 같은 리전이어도 다른 AZ에 인스턴스를 배치할 수 있음

