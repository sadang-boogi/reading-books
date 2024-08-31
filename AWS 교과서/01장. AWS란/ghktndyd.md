# 1장. AWS란

IT 자원 구축시 보통 온프레미스, 클라우드 방식이 있다.

> - 온프레미스(on-premiss): 사용자가 공간, 자원 등 모든 것을 자체적으로 구축하고 운영하는 방식
> - 클라우드(cloud): 인터넷 구간 어딘가에 눈에 보이지 않는 형태로 구성된 IT 자원의 집합
> - 클라우드 컴퓨팅(cloud computing): 인터넷을 통해 IT 자원 요구에 따라 온디맨드 방식으로 IT 자원을 제공하고, 사용한만큼만 비용을 지불하는 서비스

여기서 온디맨드 방식은 "클라우드가 즉시 제공하는 방식" 정도로 이해해도 될 것 같다.

- **클라우드 컴퓨팅의 장점**

1. 민첩성
2. 탄력성
3. 비용 절감

**클라우드 컴퓨팅 유형**

1. IaaS(Infrastructure as as Service)
   클라우드 공급자가 서버/스토리지/네트워킹, 가상화 영역을 제공하고 나머지는 사용자가 직접 관리하는 구조

2. PaaS(Platform as a Service)
   플랫폼 형태로 제공된다. 클라우드 사용자가 애플리케이션, 데이터 영역만 관리하면 되고, 나머지는 클라우드 공급자가 관리하는 구조

3. SaaS(Software as as Service)
   소프트웨어 영역까지 클라우드 공급자가 관리 및 제공하는 유형이다.

- 온프레미스가 재료 구매하고 조리까지 직접해서 먹는 방식
- IaaS는 밀키트 사서 조리만 해서 먹는 방식
- PaaS는 음식점이라는 플랫폼에서 음식을 배달시켜 먹는 방식
- SaaS는 출장 뷔페 불러서 먹고 싶은 거를 먹기만 하면 되는 방식

**클라우드 구축 모델**

- 퍼블릭 클라우드: AWS 같은 서비스들, 사용자가 클라우드 자원을 소유하지 않고 필요할 때 공급받아서 사용하는 것 (온디맨드 형식)
- 프라이빗 클라우드: 온프레미스 환경이다. 자원 소유권이 사용자에게 있음
- 하이브리드 클라우드: 두 개 혼합

---

AWS

- 리전: 데이터 센터가 집합된 물리적 위치 (지역)
- 가용 영역: 리전 내에 구성된 하나 이상의 개별 데이터 센터

- AWS 컴퓨팅
    - Amazon Elastic Compute Cloud(**EC2**)

- AWS 네트워킹 및 컨텐츠 전송
    - Amazon Virtual Private Cloud(**VPC**)
    - Amazon **CloudFront**
    - Amazon **Route53**

- AWS 스토리지
    - Amazon Simple Storage Servie (**S3**)
    - Amazon Elastic File System (**EFS**)
    - Amazon Elastic Block Store (**EBS**)

- AWS 데이터베이스
    - Amazon Relational Database Service (**RDS**)
    - Amazon **Aurora**
    - Amazon **DynamoDB**

- AWS 보안 자격 증명 및 규격 준수
    - AWS Identity & Access Management (**IAM**)


