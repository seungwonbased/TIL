- 서버에 대한 고민 없이 애플리케이션을 구축하고 실행
- 인프라, 운영체제, 런타임과 같은 것들을 관리할 필요가 없음
- 오토 스케일링, 고가용성 제공, 종량제 결제 모델 제공

![](https://i.imgur.com/jsPytYS.png)

# AWS Lambda

- PaaS
- 이벤트에 대한 응답으로 코드를 실행
- 트리거될 때만 필요한 컴퓨팅 리소스를 사용해 코드를 실행
- 최대 10GB 메모리, 동시 실행 한도 최대 3000회, 컨테이너 이미지 지원, 100ms에서 1ms로 실행 시간 과금 세분화
- 일반적인 Lambda의 구현

![](https://i.imgur.com/FqJLnVA.png)

## Lambda Function 뜯어보기

```python
import json

def lambda_handler(event, context):
    return {
        'StatusCode': 200,
        'body': json.dumps('Hello World!')
    }
```

- Handler() Function
	- 호출 시 실행되는 함수
- Event Object
	- 람다 함수 호출 중에 전송된 데이터
- Context Object
	- 런타임 정보와 상호 작용할 수 있는 메서드 (요청 ID, 로그 그룹 등
- Lambda 콘솔에서 _코드 > 런타임 설정에 들어가면
	- 파일_이름.함수_이름의 형태로 핸들러가 정의되어 있음
		- 바꿀 수 있음

## Lambda Layer

![](https://i.imgur.com/L2YnXRK.png)

- 공통적으로 써야하는 라이브러리와 같은 것들을 Layer에 구성할 수 있음

### Lambda Layer 특징

- 손쉬운 코드 공유
	- 한 번 Layer에 업로드하면 어떤 Lambda Function에서든 쉽게 참조 가능
- 공통 구성 요소를 ZIP 파일에 넣고 업로드
- 포함될 수 있는 것
	- Dependencies
	- Training Data
	- Configuration File
	- 등등
- 할당량
	- 함수 당 최대 5개 Layer 사용 가능
	- 함수 + 모든 Layer의 압축되지 않은 총 크기 < 250mb
- Layer들은 함수 실행 환경에서 /opt 디렉터리로 추출

### Lambda Layer 장점

- 배포 패키지를 작게 유지할 수 있음
- 책임 분리를 가능하게 하고 비즈니스 로직을 빠르게 반복 가능
- 빌트인된 안전한 공유 환경 지원
- 환경을 커스터마이징 가능

# Serverless Framework

- https://myanjini.tistory.com/entry/Serverless-Framework-1
- https://myanjini.tistory.com/entry/Serverless-Framework-2
- https://myanjini.tistory.com/entry/Serverless-Framework-3-%EC%8B%A4%ED%96%89-%ED%99%98%EA%B2%BD-%EC%A0%9C%ED%95%9C-%EC%84%A4%EC%A0%95
- https://myanjini.tistory.com/entry/Serverless-Framework-4-%EB%9E%8C%EB%8B%A4-%ED%95%A8%EC%88%98-%EC%8B%A4%ED%96%89%EC%97%90-%ED%95%84%EC%9A%94%ED%95%9C-%EA%B6%8C%ED%95%9C-%EC%84%A4%EC%A0%95
- https://myanjini.tistory.com/entry/Serverless-Framework-5-%EB%9E%8C%EB%8B%A4-%ED%95%A8%EC%88%98-%EC%8B%A4%ED%96%89%EC%97%90-%ED%95%84%EC%9A%94%ED%95%9C-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95
- https://myanjini.tistory.com/entry/Serverless-Framework-6-%EC%97%85%EB%A1%9C%EB%93%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80%EC%9D%98-%EC%8D%B8%EB%84%A4%EC%9D%BC-%EC%9E%90%EB%8F%99-%EC%83%9D%EC%84%B1
	- 업로드 이미지의 썸네일 자동 생성

