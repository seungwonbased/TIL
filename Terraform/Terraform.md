- 공급자의 고유한 기능을 활용하여 각 공급자에 특정한 코드를 작성할 수 있도록 하는 동시에 모든 공급자에 대해 동일한 언어, 도구 세트 및 IaC 방식을 사용할 수 있도록 하는 것
- 확장자가 .tf인 파일에 HCL로 작성
	- 선언적 언어로 원하는 인프라를 설명하는 것을 목표

# terraform init

- 테라폼 코드를 스캔하고, 사용 중인 공급자를 파악하고, 해당 코드를 다운로드
	- .terraform 폴더에 다운로드

# terraform plan

- 인프라를 변경하기 전에 테라폼이 수행할 작업을 확인
- 더하기 기호가 있는 항목은 생성
- 빼기 기호가 있는 항목은 삭제
- 물결표 기호가 있는 항목은 수정

# terraform apply

- 동일한 계획 출력을 표시하고, 실제로 이 계획을 진행할 것인지 확인하라는 메시지를 표시

# 입력 변수 정의

```terraform
variable "NAME" {
	[CONFIG ...]
}
```

## CONFIG

- description
	- 변수가 어떻게 사용되는 지 문서화
- default
	- 값이 전달되지 않을 때 사용하는 기본 값
- type
	- type 제약조건을 적용
		- string, number, bool, list, map, set, object, 등
- validation
	- 입력 변수에 대한 유효성 검사
- sensitive
	- 민감 정보인 경우, plan 또는 apply를 실행할 때 기록하지 않음

## 입력 변수에 값 지정하는 방법

### 1. 기본값이 설정되어 있지 않은 입력 변수의 값을 정의하지 않은 상태에서 terraform plan or apply

- 입력 변수를 입력하도록 프롬프트가 제공됨

### 2. -var 옵션을 이용해서 변수 값을 정의

> terraform plan **-var "service_port=8080"**

### 3. TF_VAR_<변수 이름> 라는 환경 변수를 정의해 변수 값을 정의

> export TF_VAR_service_port=8080

> terraform plan

### 4. terraform.tfvars 파일에 변수 = 값 형태를 정의

## 입력 변수의 기본값 설정

```terraform
variable "service_port" {
  description = "Service Port for HTTP Request"
  type        = number
  default     = 8080
}
```

## 변수의 값을 참조하는 방법

> var.<변수 이름>

- 문자열 내부에서는
> "${var.<변수 이름>}"

## 출력 변수 정의

```terraform
output "NAME" {
  value = <VALUE>
  [CONFIG ...]
}
```

- description
	- 출력 변수 설명
- sensitive
	- plan 또는 apply가 끝날 때 출력을 기록하지 않음 (민감 정보 보호)
- depends_on
	- 종속 관계를 명시적으로 설정

# Web Server 클러스터 배포
## 1. Launch Configuration 정의

- ASG에서 EC2 인스턴스를 정의하는 방법 지정
- aws_launch_configuration 리소스를 사용
	- aws_instance 리소스와 거의 동일한 파라미터를 사용
	- tags와 user_data_replace_on_change 파라미터를 지원하지 않음
		- tags
			- aws_autoscailing_group 리소스에서 처리
		- user_data_replace_on_change
			- ASG는 기본적으로 새 인스턴스를 시작하므로 필요하지 않음
	- 파라미터 중 다음 두 개는 다른 이름을 가짐
		- ami -> image_id
		- vpc_security_group_ids -> security_groups

### 수명 주기 설정

- create_before_destroy를 true로 설정하면 테라폼은 리소스를 교체하는 순서를 반전해 먼저 대체 리소스를 생성한 다음 (이전 리소스를 가리키는 모든 참조를 새로운 리소스를 가리키도록 업데이트한 후) 이전 리소스를 삭제

```terraform
lifecycle {
  create_before_destroy = true
}
```

## 2. ASG 정의

- aws_autoscailing_group 리소스로 정의

## 3. 데이터 소스를 이용해 서브넷 정보를 조회
### Data Source

- 테라폼을 실행할 때마다 공급자에서 가져오는 읽기 전용 정보
- 공급자의 API를 호출해서 해당 데이터를 나머지 테라폼 코드에서 사용할 수 있도록 하는 방법
- 예) AWS 공급자의 경우, VPC 데이터, Subnet 데이터, AMI ID, IP 주소 범위, 현재 사용자의 ID 등

```terraform
data "<PROVIDER>_<TYPE>" "<NAME>" {
  [CONFIG ...]
}
```

- PROVIDER
	- 공급자 이름 (예: aws)
- TYPE
	- 데이터 소스 유형 (예: vpc)
- NAME
	- 데이터 소스를 참조할 때 사용할 식별자
- CONFIG
	- 데이터 소스와 관련된 인수

## 4. Load Balancer 배포

- ALB 생성
	- aws_lb
- 로드 밸런서 리스너 정의
	- aws_lb_listener
- 보안 그룹 추가
	- 로드 밸런서에 80 포트로 들어오는 것을 허용하고, 모든 포트로 나가는 것을 허용
- 로드 밸런서에 보안 그룹을 적용
- 로드 밸런서에 타겟 그룹을 정의
	- aws_lb_target_group
- ASG에 타겟 그룹을 지정
- 리스너 규칙 추가

# 선언 블록

- provider
	- 클라우드 서비스 또는 인프라스트럭처를 제공하는 공급자를 정의
- resource
	- 프로바이더가 제공하는 실제 자원을 정의    
- variable
	- 변수를 정의 (외부에서 입력)
- output
	- 결과를 출력 
- data
	- 리소스의 정보
- terraform
	- 테라폼 구성을 정의
- local
	- 지역 변수를 정의

## Semantic Versioning

> version: major.minor.patch

- major
	- 내부 동작의 API가 변경 또는 삭제되거나 하위 호환이 되지 않는 버전
- minor
	- 신규 기능이 추가되거나 개선되고 하위 호환이 가능한 버전 
- patch
	- 버그 수정 또는 일부 기능이 개선된 하위 호환이 가능한 버전

### 버전 제약 구문

```terraform
terraform {
  required_version = "~> 1.3.0"
}
```

- = 또는 연산자 없음
	- 지정된 버전만 허용
- !=
	- 지정된 버전을 제외
- >, >=, <, <=
	- 지정한 버전과 비교해서 조건에 맞는 경우 허용
- ~>
	- 지정한 버전에서 가장 자리수가 낮은 구성 요소만 증가하는 것을 허용
	- ~> x.y → x.y ~ x.(y+n)
	- ~> x.y.z → x.y.z ~ x.y.(z+n)

### 프로바이더 버전

> 프로바이더 버전 사용법: https://registry.terraform.io/providers/hashicorp/aws/latest

- 테라폼 v0.13 이후부터 required_providers에 정의

```terraform
terraform {
  required_version = ">= 1.0.0"
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "~> 4.2.0"
    }
  }
}
```

## Backend Block

- 테라폼 실행 시 저장된 상태 파일의 저장 위치를 선언
- 상태 파일
	- 코드로 관리되는 리소스를 저장한 파일
	- 리소스 탐색 및 추적에 사용
- 백엔드
	- 상태 파일이 저장되어 있는 곳
- 작업자들이 테라폼 상태 파일을 공유하기 위해서는 외부 백엔드 저장소가 필요
- 민감한 데이터가 포함되어 있을 수 있으므로 접근 제어가 필요
- 기본적으로 활성화되는 백엔드는 local
	- 작업자의 로컬 환경에 저장하고 관리
- 테라폼을 실행하는 동안 .terraform.tfstate.lock.info 파일이 생성되면서 해당 상태를 동시에 사용하지 못하도록 잠금 처리
- -lock=false 옵션을 추가하면 잠금 무시됨
- 새로운 백엔드로 전환
	- -migrate-state
		- init 시 상태 파일 생성됨
	- -reconfigure
		- apply 시 상태 파일 생성됨

## 리소스 선언

```terraform
resource "<프로바이더>_<리소스 유형>" "<이름>" {
  <인수> = <값>
}
```

- 의존 관계가 없는 경우 테라폼은 리소스 생성을 병렬로 처리

### 리소스 구성에서 인수와 속성 참조

- 인수 (Argument)
	- 리소스 생성 시 사용자가 선언하는 값
- 속성 (Attribute)
	- 사용자가 설정하는 것은 불가능하지만 리소스 생성 이후 획득 가능한 리소스 고유값

#### 리소스 참조
- <프로바이더 이름>\_<프로바이더가 제공하는 리소스 유형>.<이름>.<인수>
- <프로바이더 이름>\_<프로바이더가 제공하는 리소스 유형>.<이름>.<속성>

## 생명주기 Meta Argument

- 기본 생명주기를 의도적으로 변경할 때 사용

### create_before_destroy

- Boolean
- 리소스 수정 시 신규 리소스를 우선 생성하고 기존 리소스를 삭제

### prevent_destroy

- Boolean
- 해당 리소스를 삭제하려 할 때 명시적으로 거부

### ignore_changes

- List
- 리소스 요소에 선언된 인수의 변경 사항을 테라폼 실행 시 무시

### preconditions

- 리스트 요소에 선언된 인수의 조건을 검증

### postcondition

- plan과 apply 이후의 결과를 속성 값으로 검증

## Data Source

- 테라폼으로 정의되지 않는 외부 리소스 또는 저장된 정보를 테라폼 내에서 참조할 때 사용

### Data Source 정의

```terraform
data "<프로바이더>_<리소스 유형>" "<이름>" {
  <인수> = <값>
}
```

## 입력 변수
### 선언

```terraform
variable "<이름>" {
  <인수> = <값>
}
```

### 민감 정보 변수

- sensitive = true
- 설정하면 로그에 민감 정보가 남지 않음

### variable vs locals

- variable은 외부에서 사용자가 입력하는 값
	- 외부에서 제공되는 값을 나타내고, 모듈 간에 데이터를 전달하는 데 사용
- locals는 테라폼 내에서 정의하는 값
	- `locals` 블록은 Terraform 코드 내에서 임시로 사용되는 값을 정의하는 데 사용
	- 이것은 주로 코드의 가독성을 향상시키고 중복을 방지하기 위해 사용
	- 같은 이름의 로컬 변수를 중복해서 선언 불가능

### for

- 복합 형식 값의 형태를 변환
- List -> 값 또는 인덱스와 값을 반환
- Map -> 키 또는 키와 값을 반환
- Set -> 키와 값에 대해 반환
