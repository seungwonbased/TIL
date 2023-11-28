# IaC

- 코드를 통해 인프라를 관리하고 프로비저닝하는 것
- 자동화 및 관리의 용이성을 증가시키는 개념

## 특징

1. 자동화 및 일관성
2. 버전 관리
3. 검증 및 테스트
4. 확장성 및 유연성

## 구현

1. 프로비저닝 도구 사용
2. 선언적 언어 사용
3. 코드 리뷰 및 테스트
4. 지속적 통합 및 배포

# 테라폼 구성 요소

- [Terraform](https://www.terraform.io/)은 코드로 인프라스트럭처를 관리하기 위한 오픈 소스 인프라스트럭처 자동화 도구
- Terraform 코드는 다양한 구성 요소를 사용하여 작성되며, 각 구성 요소는 특정 목적을 가지고 있음

### 1. **Provider:**
- **용도:** 클라우드 서비스 제공업체나 인프라스트럭처 플랫폼을 지칭하는데 사용됨
- **예시:**
  ```terraform
  provider "aws" {
    region = "us-west-2"
  }
  ```

### 2. **Data Source:**
- **용도:** 외부 리소스의 정보를 가져오는 데 사용됨
- **예시:**
  ```terraform
  data "aws_ami" "latest_amazon_linux" {
    most_recent = true
    owners      = ["amazon"]

    filter {
      name   = "name"
      values = ["amzn2-ami-hvm-*-x86_64-gp2"]
    }
  }
  ```

### 3. **Variables:**
- **용도:** 값의 동적인 부분을 변수로 정의하여 코드를 재사용하고 매개변수화
- **예시:**
  ```terraform
  variable "instance_type" {
    description = "The type of EC2 instance"
    type        = string
    default     = "t2.micro"
  }
  ```

### 4. **Resource:**
- **용도:** 인프라 리소스를 생성하고 관리
- **예시:**
  ```terraform
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = var.instance_type
  }
  ```

### 5. **Input:**
- **용도:** 사용자에게 입력을 받을 때 사용되는 변수로, 인터랙티브하게 값 입력을 받을 수 있음
- **예시:**
  ```terraform
  variable "region" {
    description = "AWS region"
    type        = string
    default     = "us-west-2"
  }
  ```

### 6. **Locals:**
- **용도:** 지역적인 변수를 정의하여 코드의 가독성을 높이거나 중복을 방지
- **예시:**
  ```terraform
  locals {
    common_tags = {
      Name        = "example-instance"
      Environment = "production"
    }
  }
  ```

### 7. **Output:**
- **용도:** 특정 값을 출력하여 다른 모듈이나 외부에서 사용할 수 있게 함
- **예시:**
  ```terraform
  output "instance_ip" {
    description = "The public IP address of the instance"
    value       = aws_instance.example.public_ip
  }
  ```

### State 및 Backend:

- **State:**
  - **용도**
	  - Terraform은 자체적으로 관리하는 `terraform.tfstate` 파일에 인프라 상태를 저장하고 추적
	  - 이 파일은 현재 인프라 상태를 기록하고 변경 사항을 추적하는 데 사용됨

- **Backend:**
  - **용도** 
	  - State 파일을 저장하는 위치로, 로컬 파일 시스템, AWS S3, Azure Storage 등 다양한 백엔드에 저장됨
	  - 백엔드는 협업 및 상태 공유를 가능케 하고, 팀 전체에서 동시에 작업할 수 있도록 함


# Terraform CLI 명령어 및 주요 옵션

### 1. `terraform init`

- **용도:** Terraform 환경을 초기화하고, 필요한 플러그인을 다운로드
- **옵션:**
  - `-backend-config`: 백엔드 구성을 지정
  - `-input=false`: 사용자 입력을 비활성화

### 2. `terraform plan`

- **용도:** Terraform 코드를 평가하여 어떻게 인프라가 변경될지 미리 확인
- **옵션:**
  - `-out`: 실행 계획을 파일로 저장

### 3. `terraform apply`

- **용도:** Terraform 코드를 실행하여 인프라를 생성 또는 업데이트
- **옵션:**
  - `-auto-approve`: 사용자 확인을 건너뛰고 자동으로 승인
  - `-lock=false`: 잠금을 무시
  - `-var`: 변수의 값을 변경

### 4. `terraform destroy`

- **용도:** Terraform 코드에 정의된 인프라를 삭제
- **옵션:**
  - `-auto-approve`: 사용자 확인을 건너뛰고 자동으로 승인

### 5. `terraform fmt`

- **용도:** Terraform 코드의 형식을 정리하고 일관된 스타일을 유지

### 6. `terraform graph`

- **용도:** 인프라 코드 간의 관계를 시각화하기 위해 그래프 형식으로 출력

### 7. `terraform console`

- **용도:** 인터랙티브한 콘솔 환경을 제공하여 Terraform 표현식을 평가하고 테스트

### 8. `terraform show`

- **용도:** Terraform 실행 계획 또는 상태 파일의 내용을 표시

### 9. `terraform validate`

- **용도:** Terraform 코드를 유효성 검사하여 구문 오류를 확인

### 10. `terraform state`

- **용도:** Terraform 상태 파일과 상호 작용하는 다양한 하위 명령을 제공 (예: `terraform state list`, `terraform state show`, 등)
 
# 메타인수 (Meta-arguments)
    
- 메타인수(Meta-arguments)는 Terraform에서 리소스 블록이나 모듈과 함께 사용되어 동작을 제어하거나 인프라 프로비저닝 프로세스에 영향을 주는 특수 인수

### 1. `depends_on`:

- **용도:**
	- 리소스 간의 종속성을 지정
	- 특정 리소스가 다른 리소스보다 먼저 생성되거나 업데이트되도록 지정할 수 있음

```terraform
resource "aws_instance" "web" {
  # ...
}

resource "aws_security_group" "example" {
  # ...
}

# aws_instance가 aws_security_group에 종속성을 갖도록 지정
depends_on = [aws_security_group.example]
```

### 2. `count`:

- **용도:** 
	- 조건이나 변수에 따라 생성되는 리소스 인스턴스의 수를 설정하여 인프라를 동적으로 제어

```terraform
resource "aws_instance" "example" {
  count = var.instance_count
  # ...
}
```

### 3. `for_each`:

- **용도:** 
	- 맵이나 문자열 집합을 기반으로 리소스의 여러 인스턴스를 생성
	- 각 인스턴스는 고유한 키-값 쌍으로 생성됨

```terraform
variable "instance_amis" {
  type = map
  default = {
    us-west-2 = "ami-abc123"
    us-east-1 = "ami-def456"
  }
}

resource "aws_instance" "example" {
  for_each = var.instance_amis

  ami           = each.value
  instance_type = "t2.micro"
}
```

### 4. `lifecycle`:

- **용도:** 
	- 리소스 업데이트, 교체, 삭제를 관리하기 위한 수명 주기 규칙을 정의

```terraform
resource "aws_instance" "example" {
  # ...

  lifecycle {
    create_before_destroy = true
    prevent_destroy      = false
  }
}
```

### 5. `provider`:

- **용도:** 
	- 리소스에 대한 공급자 구성을 지정
	- 특정 공급자나 버전을 선택할 수 있음

```terraform
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  # ...
}
```

### 6. `provisioner`:

- **용도:** 
	- 생성 후 리소스에 대해 수행할 작업을 정의
	- 스크립트 실행, 명령 실행 등이 가능

```terraform
resource "aws_instance" "example" {
  # ...

  provisioner "local-exec" {
    command = "echo 'Resource provisioned'"
  }
}
```

### 7. `connection`:

- **용도:** 
	- 리소스에 대한 연결 세부 정보를 정의하여 원격 실행이나 파일 전송을 활성화

```terraform
resource "aws_instance" "example" {
  # ...

  connection {
    type     = "ssh"
    user     = "ubuntu"
    private_key = file("~/.ssh/id_rsa")
  }
}
```

### 8. `variable`:

- **용도:** 
	- Terraform 실행 중에 제공할 수 있는 입력 변수를 선언

```terraform
variable "region" {
  description = "AWS region"
  type        = string
  default     = "us-west-2"
}
```

### 9. `output`:

- **용도:** 
	- Terraform 실행 후 표시할 수 있는 출력 값을 선언

```terraform
output "instance_ip" {
  description = "The public IP address of the instance"
  value       = aws_instance.example.public_ip
}
```

### 10. `locals`:

- **용도:** 
	- 구성 파일 내에서 사용할 수 있는 로컬 값을 정의

```terraform
locals {
  common_tags = {
    Name        = "example-instance"
    Environment = "production"
  }
}
```

# 변수 유형
### 기본 유형(Primitive Types):

1. **`string` (문자열):**
   - 예시: `"hello"`
   - 설명: 유니코드 문자의 시퀀스로 텍스트를 나타냄

2. **`number` (숫자):**
   - 예시: `15`, `6.283185`
   - 설명: 정수 및 소수점 숫자를 나타냄

3. **`bool` (부울):**
   - 예시: `true`, `false`
   - 설명: 논리적인 참과 거짓을 나타내며 조건부 논리에 사용됨

### 복합 유형(Complex Types):

4. **`list` (또는 `tuple`, 목록 또는 튜플):**
   - 예시: `["us-west-1a", "us-west-1c"]`
   - 설명: 여러 값을 포함하는 순서 있는 목록으로 0부터 시작하는 연속된 정수로 요소를 식별

5. **`set` (집합):**
   - 예시: `{"apple", "banana", "orange"}`
   - 설명: 중복되지 않은 값의 컬렉션으로, 순서나 부가적인 식별자가 없음

6. **`map` (또는 `object`, 맵 또는 객체):**
   - 예시: `{name = "Mabel", age = 52}`
   - 설명: 명명된 레이블에 의해 식별되는 값의 그룹다.

# lifecycle 메타인수에 설정할 수 있는 값과 설명

- `lifecycle` 메타인수는 Terraform에서 리소스의 생명 주기(lifecycle)를 제어하는 데 사용됨
- 각각의 옵션은 리소스가 생성, 업데이트, 교체, 삭제되는 방식을 조절

### 1. `create_before_destroy`:

- **값:** `true` 또는 `false` (기본값은 `false`)
- **설명:** 
	- 새 리소스를 만들고 나서 이전 리소스를 삭제하기 전에 새로운 리소스를 생성
	- 이렇게 하면 downtime이 최소화될 수 있음

```hcl
lifecycle {
  create_before_destroy = true
}
```

### 2. `prevent_destroy`:

- **값:** `true` 또는 `false` (기본값은 `false`)
- **설명:** 리소스를 삭제하는 것을 방지하고 실수로 삭제되는 것을 방지

```hcl
lifecycle {
  prevent_destroy = true
}
```

### 3. `ignore_changes`:

- **값:** 리스트 형태의 속성 이름
- **설명:** 특정 속성의 변경을 무시하고 업데이트를 건너뛸 수 있습니다.

```hcl
lifecycle {
  ignore_changes = ["example_attribute"]
}
```

### 4. `replace_triggered_by`:

- **값:** 리소스 종류와 이름
- **설명:** 특정 리소스의 변경이 리소스 교체를 트리거하는 경우에 사용됨

```hcl
lifecycle {
  replace_triggered_by = ["aws_instance.example_replacement"]
}
```


# 변수값 설정 방법
### 1. **변수 선언과 함께 기본 값 설정:**

```hcl
variable "instance_count" {
  description = "Number of instances to create"
  type        = number
  default     = 2
}
```

- 위 예제에서 `default` 키워드를 사용하여 변수에 기본 값을 할당
- 이 값은 사용자가 입력하지 않은 경우에 사용됨

### 2. **환경 변수 사용:**

- Terraform은 환경 변수를 통해 변수 값을 설정하는 기능을 제공
- 변수 이름을 `TF_VAR_` 접두어와 함께 사용하여 환경 변수를 설정

```bash
export TF_VAR_instance_count=3
terraform apply
```

### 3. **tfvars 파일 사용:**

- `.tfvars` 파일을 생성하여 변수 값을 설정할 수 있음
- 기본적으로 Terraform은 `terraform.tfvars` 및 `terraform.tfvars.json` 파일을 자동으로 로드

**terraform.tfvars:**
```hcl
instance_count = 4
```

### 4. **변수 파일 명시적으로 사용:**

- `terraform apply` 또는 `terraform plan` 명령어를 실행할 때 `-var-file` 플래그를 사용하여 변수 파일을 명시적으로 지정할 수 있음

```bash
terraform apply -var-file="custom-vars.tfvars"
```

### 5. **커맨드 라인에서 직접 변수 지정:**

- `terraform apply` 또는 `terraform plan` 명령어를 실행할 때 `-var` 플래그를 사용하여 직접 변수 값을 지정할 수 있음

```bash
terraform apply -var="instance_count=5"
```

### 6. **인터랙티브 모드에서 변수 지정:**

- `terraform apply` 또는 `terraform plan` 명령어를 실행할 때 변수의 값을 인터랙티브하게 입력받을 수 있음

```bash
terraform apply -var="instance_count=5"
```

# Semantic Versioning

- Semantic Versioning, 또는 SemVer는 소프트웨어 버전을 명명하고 관리하기 위한 규칙의 체계
- 이 체계는 간결하며 의미 있게 버전을 할당하여 버전 간의 호환성을 유지하고 업데이트에 대한 이해를 도움
- Semantic Versioning은 주로 오픈 소스 소프트웨어 및 다양한 프로젝트에서 사용됨

> 형태: `MAJOR.MINOR.PATCH`

1. **MAJOR (주 버전):**
   - **호환성이 없는** API 변경이 있을 때 증가
   - 새로운 기능이 추가되었을 때 증가
   - 업데이트에 따라 기존 기능이 삭제되었을 때 증가

2. **MINOR (부 버전):**
   - **하위 호환성을 유지**하면서 기능이 추가되었을 때 증가
   - MAJOR 버전이 증가하지 않았다면, 새로운 기능이나 기능의 향상이 있을 때 증가

3. **PATCH (수정 버전):**
   - **기존 기능의 하위 호환성을 유지하면서 버그 수정**이 이루어졌을 때 증가
   - 새로운 기능이나 기능의 향상이 없는 일반적인 수정이 있을 때 증가

