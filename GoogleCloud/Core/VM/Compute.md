# Compute Engine

# Google Cloud의 다양한 컴퓨팅 처리 옵션

![cpn](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/cpen.png)

# `Compute Engine`이란?

- 기본적으로 물리적 서버이며 다양한 구성으로 Google Cloud 환경 내부에서 실행됨
- 사전 정의된 머신 유형과 커스텀 머신 유형이 있어 원하는 메모리 용량과 CPU 크기를 선택 가능
- 디스크 유형 선택 가능
    - SSD, HDD와 같은 영구 디스크
    - Cloud Storage와 같은 저장소
    - 위 옵션을 혼합할 수도 있음
- 네트워킹 인터페이스 구성
- Linux와 Windows 머신을 조합하여 실행 가능

# 옵션

- Compute Engine은 여러 머신 유형을 제공
    - 머신이 요구사항을 충족하지 않으면 나만의 머신을 맞춤설정할 수도 있음

## Compute

- 네트워크 처리량에 영향
- 네트워크가 CPU 코어당 2Gbit/sec로 확장
    - 예외적으로 CPU가 2개 또는 4개인 인스턴스의 대역폭은 최대 10Gbit/sec
- vCPU, 즉 가상 CPIU는 사용 가능한 CPU 플랫폼 중 하나에서 싱글 하드웨어 하이퍼 쓰레드로 구현됨

## Storage

- 표준 HDD, SSD, 로컬 SSD라는 세 가지 옵션
- 기본적으로 회전식 표준 HDD 또는 플래시 메모리 SSD 중 결정
    - 이 두 옵션은 영구 디스크 선택 시 디스크 크기 측면에서 동일한 용량 제공
    - **SSD는 표준 디스크보다 달러당 IOPS를 더 많이 제공하도록 설계되어 있어 비용 대비 성능이 좋음**
    - 인스턴스당 최대 257TB까지 크기를 조절할 수 있음
    - 디스크의 성능은 할당되는 각 GB 공간만큼 확장됨
- 로컬 SSD는 물리적 하드웨어에 연결되므로 SSD 영구 디스크보다 훨씬 더 처리량이 많고 지연 시간이 짧음
    - 그러나 로컬 SSD에 저장하는 데이터는 인스턴스를 중지하거나 삭제할 때까지만 유지됨
    - 일반적으로 로컬 SSD는 램디스크를 만드는 경우와 같이 스왑 디스크로 사용됨
    - 더 많은 용량이 필요한 경우 로컬 SSD에 저장할 수 있음
        - 375GB의 개별 로컬 SSD 파티션이 최대 8개 포함된 인스턴스를 만들어 각 인스턴스에서 총 3TB의 로컬 SSD 공간을 얻을 수 있음

## Networking

- 기본, 커스텀 네트워크
- 인바운드/아웃바운드 방화벽 규칙
    - IP based
    - Instance/group tag
- 지역 HTTPS 로드 밸런싱
- 전역 네트워크 로드 밸런싱
    - Does not require pre-warming
- 글로벌과 멀티 리전 서브넷