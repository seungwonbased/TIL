# AWS CloudFront

![cf1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf1.png)

- CloudFront: CDN (콘텐츠 전송 네트워크)
	- 시험에서 CDN을 본다면 CloudFront를 떠올리면 됨
	- CDN은 웹사이트의 컨텐츠를 서로 다른 엣지 로케이션에 미리 캐싱하여 읽기 성능을 높이는 것
	- 컨텐츠가 네트워크 전체에 캐싱되므로 전세계 사용자들이 낮은 레이턴시로 접근할 수 있어서 사용자 경험을 증대시킬 수 있음
- 현재 CloudFront는 전세계에 있는 총 216개의 엣지 로케이션을 통해 구성이 되어 있음
	- AWS는 지속적으로 이를 추가하고 있음
- 컨텐츠가 전체적으로 분산되어 있으므로 DDoS 공격에서 보호를 받을 수 있음
	- DDoS: 동시에 모든 서버가 공격을 받는 방식
	- CouldFront는 이러한 공격에 대해서 애플리케이션을 보호할 수 있음
- 보안 챕터의 Shield라 불리는 것과, 어플리케이션 방화벽 등이 있음
- AWS의 엣지 지도를 보면, 전세계에 있는 AWS 엣지와 엣지 캐쉬 분포를 볼 수 있음
	- 호주에 위치한 S3 버킷에 웹사이트를 만들었다 하더라도 미국에 있는 사용자는 CloudFront를 이용해 미국에 있는 엣지에 컨텐츠를 요청하게 됨
	- 그럼 CloudFront가 호주에서 이 컨텐츠를 Fetch해서 가져옴
	- 미국의 다른 사용자가 똑같은 컨텐츠를 요청한다면, 이 컨텐츠는 호주에서 출발하지 않고 엣지에서 직접 컨텐츠를 제공받게 됨

## CloudFront - Origins

- CloudFront의 원본 제공 방식은 다음과 같음
- S3 Bucket
	- CloudFront를 통해 파일을 분산하고 캐싱할 수 있게 함
	- 버킷에는 CloudFront만 접근할 수 있게 보장
	- 이를 가능하게 하는 것이 OAC라 불리는, Origin Access Control(원본 접근 제어)로 기존의 OAI를 대체
	- CloudFront를 통해 버킷에 데이터를 보내는 방법도 가능
		- 이를 Ingress라고 함
- Custom Origin (HTTP)
	- 또 다른 원본 제공 방식으로 HTTP 백엔드와 같은 사용자 정의 원본을 쓸 수 있음
	- 어플리케이션 로드 밸런서나 EC2 인스턴스, 또는 S3 웹사이트가 그 예시
	- 단, S3 웹사이트의 경우 **버킷을 활성화해서 정적 웹 사이트로 설정해야 함**
	- 그 외의 다른 HTTP 백엔드도 가능

## CloudFront at High Level

![cf2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf2.png)

- CloudFront의 작업 방식 예시
- 전세계에 퍼져 있는 CloudFront의 엣지가 있고 여기에 연결할 원본이 있음
	- 이는 S3 버킷이나 HTTP 서버가 될 수 있음
- 이제 클라이언트가 연결해서 엣지 로케이션에 HTTP 요청을 보내면, 엣지는 이것이 캐싱되어 있는지 확인
	- 캐싱되어 있지 않을 경우에는, 원본으로 가서 요청 결과를 가져옴
- 결과를 가져오면서, 이를 로컬 캐시에 저장을 해서 다른 클라이언트가 같은 컨텐츠를 같은 엣지에 요청할 시 사용
	- 이러면 원본을 다시 가져올 필요가 없음

## CloudFront - S3 as an Origin

![cf3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf3.png)

- S3를 원본으로 사용한다고 가정
	- AWS 클라우드의 어떤 리전에 있는 S3 버킷이 원본일 것임
- 그리고 전세계의 엣지 로케이션이 있음, 예시를 LA로 가정
- LA 엣지에 접근하는 사용자는 LA 엣지가 직접 컨텐츠를 제공하게 됨
- 하지만 먼저 내부 AWS망을 통해 S3 버킷 원본을 받아옴
- 이 버킷은 OAS로 보호받으며, S3 버킷 정책에 의해서만 수정할 수 있음
- CloudFront과 엣지 로케이션을 이용해, 특정 리전에 속한 S3 버킷을 전세계의 엣지 로케이션으로 분산시킬 수 있음

## CloudFront vs S3 Cross Region Replication

- CloudFront는 전세계의 엣지 네트워크를 이용
	- 216개의 엣지 로케이션에 하루 동안 파일들이 캐싱됨
- 특히 이는 전세계를 대상으로 한 정적 컨텐츠를 사용하고자 할 때 용이
- CRR(교차 리전 복제)은 조금 다름
	- 복제를 원하는 각 리전에 이 설정이 되어 있어야 함
	- 즉 전세계를 대상으로 한 것은 아님
	- 그리고 파일은 거의 실시간으로 갱신됨
	- 즉 캐싱이 되지 않고, 또한 읽기 전용으로만 설정 가능
	- 이런 것은 일부 리전을 대상으로 동적 컨텐츠를 낮은 지연 시간으로 제공하고자 할 때 유용
- CloudFront는 전세계에 걸친 컨텐츠 전송 네트워크이며, 반면 S3 교차 리전 복제는 다른 리전으로의 버킷 복제

## CloudFront - ALB or EC2 as an Origin

![cf4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf4.png)

- CloudFront는 사용자 지정 HTTP 백엔드에도 접근할 수 있음
	- 여기에는 EC2 인스턴스나 어플리케이션 로드 밸런서도 포함됨
- 예시 1
	- EC2 인스턴스 통해 HTTP 백엔드를 개발한다고 가정
	- 그리고 사용자들이 CloudFront를 통해 접근하길 원함
		- 이러면 사용자들은 CloudFront의 엣지 로케이션에 접근할 것이고, 어플리케이션이 EC2 인스턴스에 요청을 보낼 것임
	- 따라서 엣지 로케이션, EC2 인스턴스 모두 퍼블릭으로 설정되어야 함
		- 그렇지 않으면, CloudFront에는 가상 프라이빗 클라우드가 없기 때문에 EC2 인스턴스에 접근할 수 없게 됨
	- 엣지 로케이션의 모든 공용 IP가 EC2에 접근할 수 있도록 하는 보안 그룹 역시 설정해줘야 함
- 예시 2
	- 어플리케이션 로드 밸런서를 퍼블릭으로 설정
	- 반면 백엔드의 EC2 인스턴스는 프라이빗 설정을 사용해도 문제 없음
		- 로드 밸런서와 인스턴스 간에 가상 프라이빗 네트워크가 설정되어 있기 때문
		- 단순히 EC2 인스턴스의 보안 그룹이 로드 밸런서를 허용하게끔 설정해주면 됨
	- 사용자는 엣지 로케이션으로 접근하기 때문에, 연결이 맺어질 수 있도록 어플리케이션의 공용 IP가 로드 밸런서의 보안 그룹 정책에 허용이 되어 있어야 함

## CloudFront Geo Restriction

- CloudFront의 지리적 제한 기능을 이용하면 사용자들의 지역에 따라 여러분들의 배포 객체 접근을 제한할 수 있음
- 접근이 가능한 국가 목록을 만들거나, 반대로 접근이 불가능한 국가 목록을 만들어서 설정할 수 있음
- 이 국가는 서드 파티 지역 DB에서 설정한 것으로, 사용자의 IP가 어떤 국가에 해당하는지를 확인할 수 있음
- 사용 사례: 컨텐츠 저작권법으로 인한 제한
- CloudFront의 Geographic restrictions 메뉴로 들어가보면 허용 목록이나 제한 목록을 지정할 수 있음
	- 미국이나 인도에서 접근한 사용자들만 제 파일에 접근할 수 있게 할 수 있음
	- Save changes를 눌러 저장하면, 설정이 완료됨

## CloudFront - Pricing

![cf5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf5.png)

- CloundFront 엣지 로케이션은 전 세계에 고루 분포해 있음
	- 전역에 퍼져 있으므로 엣지 로케이션마다 데이터 전송 비용이 다름
- 위 사진이 요금표
	- 엣지 로케이션이 위치한 대륙이나 지리 위치에 따라 각기 다른 요금이 책정되어 있음
- 도표의 숫자를 보면 멕시코, 미국, 캐나다의 경우 첫 10TB는 1GB당 0.085달러가 듬
	- 하지만 인도의 엣지 로케이션은 같은 조건에서 전송된 데이터에 대해 1GB당 두 배인 0.17달러가 듬
- CloudFront에서 더 많은 데이터가 전송될수록 비용은 낮아짐
- 왼쪽에서 오른쪽으로 갈수록 비용이 비싸짐

## CloudFront - Price Classes

![cf6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf6.png)

![cf7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf7.png)

- 비용 절감을 위해 여러분의 CloudFront를 분산할 전 세계 엣지 로케이션 수를 줄이는 방법이 있음
- 세 가지 가격 등급이 있음
1. Price Class All
	- 모든 리전을 사용하며 최상의 성능을 제공하는 Price Class All은 비용이 다소 많이 듬
	- 왜냐하면 인도 엣지 로케이션이 미국 엣지 로케이션보다 비싸기 때문
2. Price Class 200
	- 대부분의 리전을 사용할 수 있지만 가장 비싼 리전은 제외됨
3. Price Class 100
	- 가장 저렴한 리전만 사용할 수 있음
- Price Class 100은 북미와 유럽 엣지 로케이션을 제공
- Price Class 200는 일부 리전이 추가됨
- Price Class All은 전 세계 모든 엣지 로케이션을 사용

## CloudFront - Cache Invalidations

![cf8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cf8.png)

- CloudFront의 캐시 무효화
- CloudFront에는 항상 백엔드 오리진이 있는데, CloudFront 엣지 로케이션은 백엔드 오리진을 업데이트할 때 업데이트 사항을 모름
- 캐시의 타임 투 리브(TTL)가 만료되면 백엔드 오리진으로부터 업데이트된 콘텐츠를 받게됨
	- 하지만 이런 동작을 원하지 않을 수도 있음
	- 최대한 빨리 새 콘텐츠를 받고 싶기 때문
- 전체 또는 일부의 캐시를 강제로 새로고침해서 캐시에 있는 타임 투 리브(TTL)를 모두 제거할 수 있음
- 이를 위해 CloudFront 무효화를 실행해야 함
- 이때 특정 파일 경로를 전달해야 함
- \*로 시작하는 파일이나 /images/\*와 같은 특정 경로를 무효화할 수 있음
- 작동 원리 예시
	- 여러 엣지 로케이션 CloudFront를 분산하고 각 엣지 로케이션이 가진 고유의 캐시에는 index.html 파일과 오리진인 S3 버킷에서 가져온 이미지가 있다고 가정
	- 이 파일들의 타임 투 리브(TTL)를 하루로 설정할 수 있음
		- 즉, 하루 24시간 안에 엣지 로케이션이 이 파일들을 캐시로 다시 가져옴
	- 관리자는 S3 버킷에 이 파일들을 업데이트할 것임
		- 이미지를 추가하거나 바꾸고 index.html 파일도 수정
	- 또 해당 업데이트 사항이 CloudFront 사용자에게 최대한 빨리 반영되길 원함
	- 이럴 땐 두 가지 경로를 무효화하면 됨
		- 첫 번째는 특정 파일을 무효화하는 /index.html
		- 두 번째로 무효화할 경로는 엣지 로케이션의 캐시에 있는 모든 이미지를 지우는 /images/\*
	- CloudFront는 캐시에서 이 두 파일을 무효화하도록 엣지 로케이션에 지시할 거고 엣지 로케이션은 캐시로부터 파일을 지울 것임
	- 이후에 사용자가 index.html 파일을 요청
		- 특정 엣지 로케이션에 CloudFront가 요청을 보내면 더 이상 캐시에 해당 파일이 없다는 걸 알 테고 그럼 엣지 로케이션은 오리진에 요청해서 업데이트된 새로운 index.html 파일을 받음
