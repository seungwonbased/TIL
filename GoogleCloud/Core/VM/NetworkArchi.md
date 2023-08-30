# 일반적인 네트워크 설계

# 가용성 (Increased availability with multiple zones)

![near](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/near1.png)
![](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/)

<aside>
💡 가용성 (Availability): 시스템이나 서비스가 사용 가능한 정도를 나타내는 지표, 서비스가 중단되지 않고 유지보수가 영향을 미치지 않으며 복원력이 좋을수록 가용성이 높음

</aside>

- 단일 리전의 여러 영역에 리소스를 배치하여 다양한 유형의 인프라, 하드웨어 및 소프트웨어 장애로부터 격리
- 애플리케이션의 가용성을 높여야 하는 경우, 두 가상 머신을 서로 다른 영역에 배치하되 서브네트워크는 같아야함
- 단일 서브네트워크를 사용하면 이에 대한 방화벽 규칙을 만들 수 있음
- VM을 단일 서브넷의 서로 다른 영역에 할당하면 보안을 더 복잡히 하지 않으면서 가용성을 높일 수 있음
- 리전의 관리형 인스턴스 그룹에는 동일한 리전의 여러 영역의 인스턴스가 포함되어 있어 가용성이 높음

# 전역화 (Globalization with multiple regions)

![near](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/near2.png)

- 여러 리전에 리소스를 배치하여 더 높은 수준의 장애 독립성
- HTTP 부하 분산기와 같은 전역 부하 분산기를 사용하는 경우 사용자와 가장 가까운 리전으로 트래픽을 라우팅 가능
    - 사용자의 지연 시간이 단축되고 프로젝트의 네트워크 트래픽 비용 절감

# Cloud NAT provides internet access to private instances

![near](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/near3.png)

- 일반적인 보안 권장사항에 따라 가능하면 VM 인스턴스에 내부 IP 주소만 할당하는 것이 좋음
- Cloud NAT는 Google의 관리형 네트워크 주소 변환 서비스
    - 애플리케이션을 인스턴스 공개 IP 주소 없이 프로비저닝 가능
    - 인스턴스의 인터넷 액세스 제어 가능, 효율적인 방식으로 허용
        - 비공개 인스턴스가 인터넷에 액세스하여 업데이트, 패치, 구성 관리 등을 이용 가능
- Cloud NAT는 인바운드 NAT를 구현하지 않음
    - 즉, VPC 네트워크 외부의 호스트는 Cloud NAT 게이트웨이 이면의 비공개 인스턴스에 직접 액세스 불가능
    - 이를 통해 VPC 네트워크를 격리하고 안전하게 유지

![near](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/near4.png)

- 내부 IP 주소만 있는 VM 인스턴스가 Google API 및 서비스의 외부 IP 주소에 연결할 수 있도록 비공개 Google 액세스를 사용 설정 해야함
    - 예를 들어 비공개 VM 인스턴스가 Cloud Storage 버킷에 액세스해야 하는 경우 비공개 Google 액세스를 사용 설정 해야함
- 서브넷별로 비공개 Google 액세스를 사용 설정
    - 이 예시에서는 VM A1의 비공개 Google 액세스가 사용되고 있으므로 Google API 및 서비스에 액세스할 수 있고 외부 IP 주소가 없어도 됨
- 비공개 Google 액세스는 외부 IP 주소가 있는 인스턴스에는 영향 X
    - 그건 원래 Google API 및 서비스 액세스 가능