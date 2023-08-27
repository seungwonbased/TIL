# DNS

- DNS: Domain Name System
	- 사람에게 친숙한 호스트 이름을 대상 서버 IP 주소로 번역
	- 웹 브라우저에 www.google.com을 입력하면 IP 주소를 주고 웹 브라우저가 이면에서 여기에 접근하여 구글로부터 데이터를 얻음
- DNS의 계층적 이름 구조
	- .com
	- example.com
	- www.example.com 또는 api.example.com

## DNS 용어

![rt1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt1.png)

- 도메인 레지스트라 (Registrar)
	- 도메인 이름을 등록하는 곳
	- 아마존 Route 53 또는 GoDaddy가 될 수도 있고 온라인에서 찾을 수 있는 다른 레지스트라들도 있음
- DNS Records: A, AAAA, CNAME, NS, ...
- 존 파일 (Zone File)
	- 모든 DNS 레코드를 포함
	- 호스트 이름과 IP 또는 주소를 일치시키는 방법
- 이름 서버 (Name Server)
	- DNS 쿼리를 실제로 해결하는 서버
- 최상위 도메인 (TLD, Top Level Domain)
	- .com, .us, .in, .gov, .org 등
- 2단계 도메인 (SLD, Second Level Domain)
	- amazon.com, google.com 등
	- 단어 사이에 '.'이 있는 것을 볼 수 있음
- FQDN 또는 전체 주소 도메인 이름 (Fully Qualified Domain Name)
	- 예시: http://api.www.example.com.
		- 마지막 .을 루트라고 함
		- 전체 도메인 이름의 루트
		- www.example.com이 서브 도메인
		- api.www.example.com이 도메인 이름
		- HTTP 부분은 사용하기를 원하는 프로토콜

## DNS의 동작 

![rt2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt2.png)

- 공인 IP: 9.10.11.12
- 도메인 이름 example.com을 이용해서 접근하고자 함
- example.com이란 도메인 이름을 DNS용 서버를 등록해야 함
- 웹 브라우저가 example.com에 접근하기 위해서는 로컬 DNS 서버에 물어볼 것
	- 'example.com을 아십니까?'
	- 로컬 DNS 서버는 보통 회사에 의해 할당되고 관리됨
	- 또는 인터넷 서비스 제공자에 동적으로 할당됨
- 로컬 DNS 서버가 이 쿼리를 전에 본 적이 없다면 먼저 ICANN에 의해 관리되는 루트 DNS 서버에 물어볼 것
	- ICANN 조직에 'example.com을 아십니까?'라고 물어봄
- 가장 먼저 요청되는 서버인 루트 DNS 서버는 '본 적 없어요, 그러나 .com은 알고 있습니다'라고 함
	- .com은 이름 서버(NS) 레코드로 공인 IP 1.2.3.4로 가보라고 알려줌
- 로컬 DNS 서비스는 '좋아요, 최상단 도메인에 물어보겠습니다'라고 함
	- 따라서 1.2.3.4.에 있는 .com 도메인 서버에게 쿼리의 답을 요청할 것
	- IANA에 의해 관리되는 또 다른 도메인
- TLD DNS 서버에 example.com을 다시 물어보게 됨
	- 'example.com을 아십니까?'
- DNS 서버는 example.com이 무엇인지 몰라서 쿼리에 당장 답할 수가 없음
	- 그러나 example.com이라는 서버는 알고 있음
	- 5.6.7.8에 있음
		- 다음 질문을 할 공용 IP
- 로컬 DNS 서버는 최종 서버로 서브도메인의 DNS 서버
	- 이는 도메인 이름 레지스트라에 의해 관리되는 서버
	- 예를 들어 아마존 Route 53 등
- 로컬 DNS 서버는 'example.com을 아십니까?'라고 물어보고 SLD DNS 서버에는 example.com에 대한 항목이 있음
	- 'example.com이 무엇인지 알고 있죠'라고 하고 example.com은 A 레코드이고 이것의 결과로 IP 9.10.11.12를 얻음
	- DNS 서버가 이제 답을 알고 있음
- DNS 서버에 반복적으로 물어보며 가장 구체적인 것을 찾음
- 만약 누군가가 다시 example.com을 물어본다면 바로 답변을 줄 수 있음
	- 로컬 DNS 서버는 답변을 웹 브라우저에 보내고 브라우저는 답변을 받음

# Amazon Route 53

![rt3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt3.png)

- Route53: 고가용성, 확장성을 갖춘, 완전히 관리되며 권한있는 DNS
	- 권한이 있다는 것이 무슨 뜻?
		- 고객인 사용자가 DNS 레코드를 업데이트할 수 있다는 것
		- 즉, DNS에 대한 완전히 제어할 수 있음
- Route 53 역시 도메인 이름 레지스트라
- Route 53의 리소스 관련 상태 확인을 확인할 수 있음
- 100% SLA 가용성을 제공하는 유일한 AWS 서비스
- 왜 Route 53이라고 하는지?
	- 53은 DNS 서비스, 즉, 이름에서 사용되는 전통적인 DNS 포트

## Route 53 - Records

- Route 53에서 여러 DNS 레코드를 정의하고 레코드를 통해 특정 도메인으로 라우팅하는 방법을 정의
- 라우팅 정책: Route 53이 쿼리에 응답하는 방식
- TTL: DNS 리졸버(resolver)에서 레코드가 캐싱 되는 시간
- Route 53에서 지원하는 DNS 레코드 종류는 많은데 반드시 알아야 하는 것은 A, AAAA, CNAME, 그리고 NS

### Record types

- A
	- 호스트 이름과 IPv4 IP를 매핑
	- 예를 들어 example.com은 1.2.3.4로 바로 연결됨
-  AAAA
	- A와 비슷한 아이디어로, 호스트 이름을 IPv6 주소에 매핑
- CNAME
	- 호스트 이름을 다른 호스트 이름과 매핑
	- 물론 대상 호스트 이름은 A나 AAAA 레코드가 될 수 있음
	- Route 53에서 DNS 이름 공간 또는 Zone Apex의 상위 노드에 대한 CNAMES를 생성할 수 없음
	- 예를 들어 example.com에 CNAME을 만들 수는 없지만 www.example.com에 대한 CNAME 레코드는 만들 수 있음
- NS
	- 호스팅 존의 이름 서버
	- 서버의 DNS 이름 또는 IP 주소로 호스팅 존에 대한 DNS 쿼리에 응답할 수 있음
	- 또한 트래픽이 도메인으로 라우팅 되는 방식을 제어

## Route 53 - Hosted Zones

- 호스팅 존: 레코드의 컨테이너
- 도메인과 서브도메인으로 가는 트래픽의 라우팅 방식을 정의

### Hosted Zones types

- 퍼블릭 호스팅 존
	- 퍼블릭 도메인 이름을 살 때마다 mypublicdomain.com이 퍼블릭 도메인 이름이라면 퍼블릭 호스팅 존을 만들 수 있음
	- 퍼블릭 존은 쿼리에 도메인 이름 application1.mypublicdomainname.com의 IP가 무엇인지 알 수 있음
- 프라이빗 호스팅 존
	- 공개되지 않는 도메인 이름을 지원
	- 가상 프라이빗 클라우드(VPC)만이 URL을 리졸브 할 수 있음
	- application1.company.internal 같은 경우에 사용
	- 사기업에는 때때로 회사 네트워크 내에서만 접근할 수 있는 URL이 있음
	- 이면에는 프라이빗 DNS 레코드가 있음
- AWS에서 만드는 어떤 호스팅 존이든 월에 50센트를 지불해야 함

### Public vs Private Hosted Zones

![rt4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt4.png)

- 퍼블릭 호스팅 존은 공개된 클라이언트로부터 온 쿼리에 응답할 수 있음
	- 웹 브라우저에서 example.com을 요청하면 IP를 반환
- 프라이빗 호스팅 존의 경우 해당 VPC에서만 동작
	- 비공개 도메인 이름의 프라이빗 리소스를 식별할 수 있게 함
	- 예를 들어 EC2 인스턴스 1개가 있고 webapp.example.internal을 식별하고자 함
		- 또 다른 EC2 인스턴스에서는 api.example.internal을 식별하기 원하고 데이터베이스에서는 db.example.internal을 식별하고자 함
		- 프라이빗 호스팅 존에 등록하려고 하는데 첫 번째 EC2 인스턴스가 api.example.internal을 요청하는 경우 프라이빗 호스팅 존은 프라이빗 IP 10.0.0.10이라는 답을 갖고 있음
		- EC2 인스턴스는 데이터베이스에 연결이 필요할 수도 있는 두 번째 EC2 인스턴스에 연결
		- database.example.internal가 무엇인지 물어보면 프라이빗 호스팅 존은 프라이빗 IP를 알려줌
- 퍼블릭 호스팅 존은 프라이빗 호스팅 존과 똑같이 동작하지만 퍼블릭 호스팅 존은 누구든 여러분의 레코드를 쿼리 할 수 있음
	- 퍼블릭 레코드를 위한 호스팅 존
- 프라이빗 호스팅 존은 오직 프라이빗 리소스, 예컨대, VPC에서만 쿼리 할 수 있음

## Route 53 - Records TTL (Time to Live)

![rt5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt5.png)

- 클라이언트가 DNS route 53와 웹 서버에 접속한다고 가정
- myapp.example.com에서 DNS 요청을 보내면 DNS로부터 회신을 받음
- 회신 내용: A 레코드와 IP 주소 그리고 TTL이 있으며 TTL은 300초
	- TTL은 클라이언트에게 이 결과를 캐시하도록 요청
- 300초 동안 클라이언트는 결과를 캐시
	- 다시 말해, 클라이언트가 재요청을 보내거나 같은 호스트 이름으로 접속할 경우 클라이언트는 DNS 시스템에게 쿼리를 보내지 않아도 된다는 의미
	- 이미 답변을 캐시에 저장했기 때문에 답을 알고 있기 때문
- 웹 서버에 접속이 가능하며 HTTP 요청 및 회신을 보낼 수 있음

### 예시: 두 가지의 극단적인 경우

- TTL을 24시간으로 높게 설정한다면 Route 53의 트래픽은 현저히 적을 것
	- 결과가 24시간 동안 캐시될 테니 클라이언트는 요청을 적게 보낼 것
	- 하지만 클라이언트가 오래된 레코드를 받을 가능성도 있음
	- 따라서 만약 레코드를 바꾸고자 한다면 모든 클라이언트들이 새 레코드를 캐시에 저장할 때까지 24시간을 기다려야 한다는 뜻
- 반대로 TTL을 60초 정도로 짧게 설정한다면 DNS에는 트래픽의 양이 많아져서 비용이 많이 듬
	- Route 53에 들어오는 요청의 양에 따라 요금이 책정되기 때문
	- 하지만 오래된 레코드의 보관 시간은 짧아짐
	- 따라서 레코드 변경이 빨라짐
	- 레코드 변경 전반이 더욱 편리
- 어떤 TTL 설정이 더 적합할지는 상황에 따라 달라짐
	- 레코드를 변경하려는 경우 예를 들어 TTL을 24시간으로 늦춘 다음 모든 클라이언트가 느린 새 TTL을 가지고 있다는 점을 확인한 후, 레코드 값을 바꿔서 모두에게 업데이트가 되면 TTL을 올리는 식

## CNAME vs Alias

- 로드 밸런서나 CloudFront 등 AWS의 리소스를 사용하는 경우 호스트 이름이 노출되고, 보유한 도메인에 호스트 이름을 매핑하고자 할 수도 있음
- 예시: myapp.mydomain.com에 이 로드 밸런서를 매핑하는 경우
	- 두 가지 옵션이 있음
		- 첫 번째는 CNAME 레코드
			- CNAME은 호스트 이름이 다른 호스트 이름으로 향하도록 할 수 있음
			- 예를 들어 app.mydomain.com이 blabla.anything.com으로 향하는 식
			- 이건 루트 도메인 이름이 아닌 경우에만 가능해서 mydomain.com 앞에 뭔가 붙어야 함
		- 두 번재는 별칭 레코드 (Alias)
			- 이건 Route 53에 한정되지만 호스트 이름이 특정 AWS 리소스로 향하도록 할 수 있음
			- 예를 들어 app.mydomain.com이 blabla.amazonaws.com를 향할 수 있음
			- 별칭 레코드는 루트 및 비루트 도메인 모두에 작동
			- mydomain.com을 별칭으로 사용해 AWS 리소스로 향하도록 할 수 있기 때문에, 아주 유용
			- 별칭의 장점으로는 무료이고 자체적으로 상태 확인이 가능하다는 점이 있음

### Alias Records

![rt6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/rt6.png)

- 별칭 레코드는 AWS의 리소스에만 매핑이 되어 있음
- 예를 들어 Route 53에서 example.com을 A 레코드의 별칭 레코드로 하고 그 값은 로드 밸런서의 DNS 이름을 지정하려 한다고 가정
	- 이건 DNS의 확장 기능으로 시중의 모든 DNS에서 가능
	- 만약 기반 ALB에서 IP가 바뀌면 별칭 레코드는 이걸 바로 인식할 것임
	- CNAME과 달리, 별칭 레코드는 Zone Apex라는 DNS 네임스페이스의 상위 노드로 사용될 수 있음
		- example.com에도 별칭 레코드를 쓸 수 있는 것
- AWS 리소스를 위한 별칭 레코드의 타입은 항상 A 또는 AAAA인데 리소스는 IPv4나 IPv6 중 하나
- 별칭 레코드를 사용하면 TTL을 설정할 수 없음
	- Route 53에 의해 자동으로 설정됨

#### Alias Records Targets

- 일래스틱 로드 밸런서(Elastic Load Balancer)
- CloudFront 배포도 가능
- API Gateway
- 일래스틱 빈스톡 환경
- S3 웹사이트도 가능
- VPC 인터페이스 엔드포인트
- Global Accelerator 가속기
- 동일 호스트 존의 Route 53
- 등등
- **EC2의 DNS 이름에 대해서는 별칭 레코드를 설정할 수 없음**

-