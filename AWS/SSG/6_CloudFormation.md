# AWS CloudFormation 🌟

- AWS 리소스를 모델링하고 설정하여 리소스 관리 시간을 줄이고 AWS에서 실행되는 애플리케이션에 더 많은 시간을 사용하도록 해주는 IaC 기반의 구성 조정 서비스
- 필요한 모든 AWS 리소스를 설명하는 템플릿을 생성하면 CloudFormation이 해당 리소스의 프로비저닝과 구성을 담당
- AWS 리소스를 개별적으로 생성하고 구성할 필요가 없으며 어떤 것이 무엇에 의존하는지 파악할 필요가 없음
- 클라우드 환경 내 모든 인프라 리소스르 설명하고 프로비저닝할 수 있도록 공통 언어(JSON, YAML)를 통해 코드 기반 인프라를 구성하고 프로비저닝할 수 있음
- CloudFormation에는 추가 요금이 없으며 애플리케이션 실행에 필요한 AWS 리소스에 대해서만 요금을 지불하면 됨

## 특징
### 인프라 관리 간소화

- CloudFormation 템플릿을 사용하여 **스택(Stack)**을 생성하면 CloudFormation에서 Auto Scaling 그룹, 로드 밸런서 및 데이터베이스를 자동으로 프로비저닝함
- 스택 생성 후 AWS 리소스가 실행되면 필요 시 손쉽게 스택을 통해 리소스를 수정, 삭제할 수 있음
- AWS의 리소스를 스택 단위로 손쉽게 관리할 수 있음

### 신속하게 인프라를 복제

- 애플리케이션의 가용성을 높이기 위해 AWS CloudFormation을 이용하여 여러 리전에 복제
- 기존 템플릿을 재사용하여 리소스를 일관되고 반복적으로 설정할 수 있음

### 인프라 변경 사항을 쉽게 제어, 추적할 수 있음

- 인프라의 프로비저닝을 위한 리소스와 해당 설정이 템플릿에 저장
- 템플릿은 텍스트 형태로 저장되며, 버전 관리 기능을 통해 이전과의 차이점을 추적하여 인프라 변경 사항을 추적할 수 있음
- 인프라의 프로비저닝 중 문제가 발생된다면 언제든지 인프라에 대한 변경 사항을 되돌리기 위해 이전 버전의 템플릿을 사용할 수 있음

## 작동 방식

![](https://i.imgur.com/YtDoZlu.png)

## 구성 요소
### 템플릿

- 명세
- JSON 또는 YAML 형식의 텍스트 파일
- AWS 리소스 구축을 위한 청사진으로 사용

```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "A sample template",
    "Resources": {
        "MyEC2Instance": {
            "Type": "AWS::EC2::Instance",
            "Properties": {
                "ImageId": "ami-0ff8a91507f77f867",
                "InstanceType": "t2.micro",
                "KeyName": "testkey",
                "BlockDeviceMappings": [
                    {
                        "DeviceName": "/dev/sdm",
                        "Ebs": {
                            "VolumeType": "io1",
                            "Iops": 200,
                            "DeleteOnTermination": false,
                            "VolumeSize": 20
                        }
                    }
                ]
            }
        }
    }
}
```

### 스택

- 명세를 통해 생성된 리소스들의 관리 단위
- CloudFormation을 사용할 경우 스택이라는 하나의 단위로 관련 리소스를 관리
- 스택을 생성, 업데이트, 삭제하여 리소스 모음을 생성, 업데이트 및 삭제
- 스택의 모든 리소스는 스택의 템플릿으로 정의

### 변경 세트 (Change Set)

- 스택에서 실행 중인 리소스를 변경해야 하는 경우 스택을 업데이트
- 리소스를 변경하기 전에 제안된 변경 사항이 요약된 변경 세트를 생성할 수 있음
- 변경 세트를 사용하면 변경 사항을 구현하기 전에 해당 변경이 실행 중인 리소스, 특히 중요 리소스에 미치는 영향을 확인할 수 있음

## 템플릿 구조

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: Make a VPC 1

Resources:
  ToturialVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 172.0.0.0/16
      EnableDnsHostnames: true

  InternetGateway:
    Type: AWS::EC2::InternetGateway

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref ToturialVPC
      InternetGatewayId: !Ref InternetGateway

  PublicSubnet01:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref ToturialVPC
      CidrBlock: 172.0.0.0/24
      AvailabilityZone: !Select 
        - '0'
        - !GetAZs ''

  PrivateSubnet01:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref ToturialVPC
      CidrBlock: 172.0.1.0/24
      AvailabilityZone: !Select 
        - '0'
        - !GetAZs ''

  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref ToturialVPC

  PublicRoute:
    Type: AWS::EC2::Route
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

  PublicSubnetRouteTableAssociation1:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet01
      RouteTableId: !Ref PublicRouteTable

  PrivateRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref ToturialVPC

  PrivateSubnetRouteTableAssociation1:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PrivateSubnet01
      RouteTableId: !Ref PrivateRouteTable

Outputs:
  VPC:
    Description: Toturial VPC ID
    Value: !Ref ToturialVPC
  AZ1:
    Description: Availability Zone 1
    Value: !GetAtt 
      - PublicSubnet01
      - AvailabilityZone
```

### Ref 내장 함수

- 지정된 파라미터 또는 리소스에 대한 정보를 반환
- 전체 함수 이름의 구문
	- Ref: logicalName
	- Ref: IntrinsicFunction
- 짧은 형식의 구문
	- !Ref logicalName
	- !Ref IntrinsicFunction

### Select 내장 함수

- 인덱스별 객체 목록에서 객체 하나를 반환

### GetAZs 내장 함수

- 지정된 리전의 가용 영역을 알파벳 순으로 나열하는 배열 반환
- 빈 문자열을 지정하면 AWS::Region을 지정하는 것과 같음

### GetAtt 내장 함수
- 리소스의 속성을 반환

### AWS::Region

- 가상 파라미터
	- CloudFormation에서 사전 정의된 파라미터
- 포괄 리소스를 생성하는 리전을 나타내는 문자열을 반환
- 현재 CloudFormation을 구동하는 리전이 반환됨

## CloudFormation 헬퍼 스크립트

- CloudFormation으로 생성한 EC2 인스턴스에 소프트웨어를 설치하고 서비스를 시작하는데 사용할 수 있도록 제공하는 Python 스크립트

### aws-cfn-bootstrap 

- AWS에서 제공하는 유틸리티 패키지로, EC2 인스턴스를 프로비저닝하고 초기화하기 위한 도구

#### 주요 구성 요소
- cfn-init
	- EC2 인스턴스의 초기 설정을 관리
	- 리소스 메타데이터를 검색 및 해석하고, 패키지를 설치하고, 파일을 생성하고, 서비스를 시작하는데 사용
- cfn-signal
	- EC2 인스턴스가 성공적으로 초기화되었음을 CloudFormation 스택에 알림
	- 필수 리소스나 애플리케이션이 준비될 때 스택에서 다른 리소스를 동기화할 수 있도록 CreationPolicy 또는 WaitCondition에서 신호를 전송하는데 사용 
- cfn-get-metadata
	- EC2 인스턴스의 CloudFormation 스택 메타데이터를 검색하는 데 사용
	- 스택에서 사용 가능한 메타데이터를 검색할 때 유용하며, 다른 명령에 전달해 활용할 수 있음
- cfn-hup
	- EC2 인스턴스에서 실행되며, 주기적으로 CloudFormation 스택의 변경 사항을 감지하고 업데이트된 메타데이터를 검색하여 인스턴스를 업데이트
	- 변경 사항을 실시간으로 반영하려는 스택에서 사용

### AWS::CloudFormation::Init

- CloudFormation 템플릿에서 사용되는 리소스 유형 중 하나
- cfn-init 헬퍼 스크립트에 대한 구성 작업을 정의
- EC2 인스턴스를 생성할 때 초기 설정, 패키지 설치, 파일 다운로드, 명령 실행 및 다른 구성 단계를 선언적으로 정의
- cfn-init 헬퍼 스크립트는 패키지, 그룹, 사용자, 소스, 파일, 명령 및 서비스 순으로 구성(config) 섹션을 처리
	- packages
		- 사전 패키징된 애플리케이션 및 구성 요소를 다운로드 및 설치
	- groups
		- Linux/UNIX 사용자 그룹을 생성하고 그룹 ID를 할당 (Windows 시스템에는 지원하지 않음)
	- users
		- Linux/UNIX 사용자를 생성 (Windows 시스템에는 지원하지 않음)
	- sources
		- 아카이브 파일을 다운로드한 후 EC2 인스턴스의 대상 디렉터리에 압축을 해제
	- files
		- EC2 인스턴스에서 파일을 생성
	- commands
		- EC2 인스턴스에서 명령을 실행
	- services
		- 인스턴스가 실행될 때 활성화되거나 비활성화될 서비스를 정의 (Linux 시스템에서는 sysvinit 또는 systemd를 사용해 지원
		- Windows 시스템에서는 Windows 서비스 관리자를 사용해 지원

## CloudFormation Helper Script

- AWS에서 제공하는 유틸리티 패키지로, EC2 인스턴스를 프로비저닝하고 초기화하기 위한 도구