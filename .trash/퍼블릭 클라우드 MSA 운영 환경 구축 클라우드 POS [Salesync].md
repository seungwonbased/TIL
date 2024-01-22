# 퍼블릭 클라우드 MSA 운영 환경 구축 클라우드 POS [Salesync]
# 목차

# 4. Micro Service Architecture
## 4.1. MSA 설계
### 4.1.1. 두 가지 원칙에 기반하여 설계

- 기술적 요구를 기반으로 서비스의 경계를 정하는 것은 안티 패턴
    - 기술적 요구가 아닌 “비즈니스 기능”을 중심으로 구성
- 너무 일찍 세분화해서는 안 됨
    - 초기부터 폭발적인 수준으로 세분화되면 높은 복잡성을 감당할 수 없어질 수 있음

### 4.1.2. Domain-Driven Development

![](https://github.com/ssg-salesync/.github/tree/main/assets/event1.jpeg)

![](https://github.com/ssg-salesync/.github/tree/main/assets/event2.jpeg)

- 마이크로 서비스가 얼마나 “마이크로”해야 하는지 기준이 애매했던 상태에서, DDD 기법의 Event Storming을 통해 도메인 정의
- 하위 도메인의 주요 집합체를 발견하고 시스템에 존재하는 콘텍스트의 경계를 식별하기 위해 진행

## 4.2. 마이크로 서비스

- Store Service
- Item Service
- Order Service
- Sale Service
- Dashboard Service
- Consulting Service

## 4.3. Backend-for-Frontend

## 4.4. Kubernetes 내부 API 통신

## 4.5. Kafka 비동기 이벤트 처리

# 5. Infrastructure
## 5.1. 유일한 개발 환경: Docker
- Docker를 유일한 종속성으로 설정
    - 특정 버전 혹은 OS에 대한 요구사항 제거를 위해 컨테이너화된 개발자 워크스페이스 구축
- 개발 데이터베이스는 마이크로 서비스별로 RDS의 스키마를 나눠 설정
    - e.g., dev-stores, dev-items, …

## 5.2. Public Cloud: Amazon Web Service

![](https://github.com/ssg-salesync/.github/tree/main/assets/aws.png)

### 5.2.1. Salesync 인프라 설계 기초 정보

### 5.2.2. Salesync 인프라 설계 전략

1. 데이터 백업, 복구 전략
2. 무정지 운영 (Zero Downtime Operations) 전략
3. DR (Disaster Recovery) 전략
4. IAM 관리 전략

[📃 Salesync 인프라 전략 상세 보기](https://github.com/ssg-salesync/.github/blob/main/strategy.md)

### 5.2.3. 코드형 인프라 (Infrastructure as a Code)

- Terraform을 사용해 인프라의 재현성, 확장성 증대
- Terraform 코드를 모듈화해서 작성

## 5.3. 컨테이너 오케스트레이션: Kubernetes

![](https://github.com/ssg-salesync/.github/tree/main/assets/k8s.png)

- 컨테이너 환경의 예측 가능하고 격리된 시스템 구축
- 컨테이너화된 애플리케이션의 오케스트레이션을 목적으로 AWS EKS 기반 클러스터 구축
    - 컨테이너의 배포, 확장, 관찰, 관리
    - 배포의 롤아웃, 롤백
    - 시크릿 관리, 로드 밸런싱, 트래픽 관리 등

### 5.3.1. Scaling Out (Cluster Autoscaling)

- Kubernetes의 Cluster Autoscaler 모듈 사용해 Autoscaling Group의 Desired capacity 변경
- 장점
    - 자동 스케일링
- 단점
    - 새 인스턴스가 생성되는 시간이 느림 → 급격한 트래픽에 즉각적으로 반응하지는 않음
    - 내부 네트워크 트래픽 증가

### 5.3.2. Scaling Up

- 로그 파이프라인 애플리케이션이 높은 메모리 사용량을 필요로 해 수직 확장이 필요해짐
- 수동으로 노드의 스펙 변경
    - e.g., t3.medium → t3.large
- 장점
    - 계산해보니 더 큰 노드를 운영하는 것이 비용 효율적일 때가 있음
    - 관리 복잡성 감소
- 단점
    - 수동 스케일링

### 5.3.3. Ingress

- 외부에서 BFF, 앞단 API로 들어오는 트래픽을 라우팅
- API Gateway의 관리형 기능은 필요하지 않아 Ingress 사용
- 인그레스를 관리하는 인그레스 컨트롤러 구현체는 ALB Controller 사용

### 5.3.4. 클러스터 리소스 모니터링