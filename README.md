# AWS_Serverless

## 목차

[등장배경 및 소개](#등장배경-및-소개)
[한계점 및 장단점](#한계점-및-장단점)

### 등장배경 및 소개

- 클라우드 컴퓨팅의 역사
  회사들은 자사 서버를 많이 가지고 있음
  서버를 회사에 두면 트래픽 증가에 대한 대비를 계속 해야 되기 때문에 부담감이 커지게 됨
  - 서버 확장의 이슈가 생기게 됨

* Data Center
  보안, 내진, 냉각 (서버를 두는 것이 단점으로 다가옴)

* 예전의 대응 방식
  물리 서버 확장
* 서버 가상화를 통해서 고객에게 임대

> IaaS : infrastucture as a Service

관리 부담에 대한 부담을 줄여주기 위해서 플랫폼 형태로 내놓음

- PaaS : Platform as a Service

Iaas -> PaaS

- Faas : Function as a Service
  함수 단위로 동작하는 서비스
  서버리스가 일반화
  확장성이 좋음 Good
  안정적인 서비스 제공 가능
  확장성, 신속한, 효율적

- 비즈니스 로직에 집중 서버 운영으로 부터 자유로워짐

### 한계점 및 장단점

Fass 에 중점을 두어서 설명

- 장점
  1. 관리비용 감소(인프라관리 감소), 확장성
  2. 빠른 아웃풋
     완성된 인프라 환경이 제공되기에 비즈니스 로딩에 집중이 가능
  3. 경제적
     사용한 만큼의 비용만 지불하게 됨
- 단점

  1. 낮은 호환성, 높은 종속성
     aws, gcp 운영체제, runtime이 정해져 있음(java, ruby, python)
     서버리스라고 해도 물리적인 서버는 존재하기에 한정적
  2. 한정적
     개발 내용에 따라서 기능들이 한계적임
     디버깅, 모니터링의 경우 한계적임
     별도로 구축한 모니터링의 경우 트래픽 처리 속도를 맞춰줘야함
  3. 무상태
     Stateless 변수, 데이터들이 공유가 불가능함
     이용될 때 마다 같은 서버가 제공되는 것이 아님

- 사용처
  스케쥴러, IOT, 배치, 예측불가, 작은, 빠른, 간단, API
  사용패턴이 일정하지 않은 써드파티
  적은 비용으로 많은 트래픽을 감당가능

### Amazon

리테일 사업 -> 판매자 지원 사업 -> 클라우드 컴퓨팅

- Region
  여러 가용 영역들의 집합체
  대륙간 네트워크 통신을 통해 연결
  물리적으로 분리가 되어 있음
  이중네트워크-고가용성, 이중화
  이중화 : 가용 영역 최소 2개 -> 하나의 가용영역의 문제가 생길경우를 대비하여서 하나를 다른 가용영역에 복사
- Availiability Zones
- Data Center

- Edge
  전세계 사람들에게 원하는 데이터를 제공하는 CDN 서비스

* 캐시서버를 이용해서 데이터를 복사하여 전달하고 변경사항이 있을 경우 빠르게 변경 가능

### 어플리케이션 및 아키택쳐 소개

컴퍼런스 참가 시스템

참가신청 화면, 신청내역 화면, 신청결과 화면

API Gateway
Proxy
clout Front
CDN
S3
html,css 정적 호스팅
Dynamo DB
전송할 데이터 적재
SNS
sns 통해 lambda에게 이벤트 호스팅 역할

- 아키텍쳐 -Hosting
- 참가증 로직 Lambda에서 처리하면 시간지연 -> 비동기적으로 SNS(Async) 통해서 작성

### 실습환경 준비

1. docker run -it amazonlinux bash
1. yum update -y
1. yum install python3 -y
1. pip3 install virtualenv
1. yum install which
1. which python3
1. mkdir venv
1. virtualenv -p /usr/bin/python3 py37
1. source py37/bin/activate
1. pip3 install awscli
1. aws configure

https://console.aws.amazon.com/iam/home?region=us-east-2#/home

- 확인 방법
  cat ~/.aws/credentials
- 권한 프로파일 부여
  aws configure --profile s3

### 프런트 앤드 화면 개발

Material Design Lite

- https://getmdl.io/started/index.html

* wget https://raw.githubusercontent.com/google/material-design-lite/mdl-1.x/LICENSE

### 객체 스토리지

버킷 안에 객체들이 무제한으로 적재가 가능
버킷은 -> 100개의 임계치 존재
user별로 버킷을 생성하는 것은 자제

aws s3api list-objects --bucket fastcampus2022

echo 'test' > test.txt

aws s3api put-object --bucket fastcampus2022 --key dir1/test.txt --body ./test.txt

무한히 확장해 나가면서 같은 deps 안에 있다고 보면됨

(mypage)[http://fastcampus2022.s3-website.ap-northeast-2.amazonaws.com/]

Cloud Front 에서 요청을 모두 처리하는 것이 아닌 하나만 처리하도록 하는 것도 중요함

### S3

version이 가장 중요한 -> 우발적인 삭제가 우려 되는 경우가 있음

index.html이 삭제가 되어있다고 가정 -> 버저닝을

서버 엑세스 로깅 -> 트래픽에 대한 요청 처리
객체 수준 로깅 -> 객체가 변경 사항에 대해서 로깅을 처리하는데 이에따른 요금도 더 많이 부과가 될 수 있음

- 이벤트 기능 -> put post 삭제 마커 이벤트 트리거 해서 서비스에서 해당 요청에 대한 처리를 할 수 있음

* 배치 작업 -> 무수히 많은 데이터를 쌓은것을 캐시 -> 메타 데이터, 데이터 일괄적으로 넣는 경우 사용(사용 빈도가 많지 않음)

### CloudFront

세계 각지의 region에 배포 하는 개념

- Origin Domain Name : 캐싱할 대상
  cf와 s3가 여기서 생성 된다고 보면됨
- Origin Path : 경로를 설정하면 여기가 origin Path로 동작
- Restrict Bucket Access : Cloud Front(cf)를 통해서만 접근이 가능하도록 설정
- Grant Read Permissions on Bucket : 모든 유저에게 열어준다는 말이 없음, WKO라는 권한을 가진 유저들에게 모든 권한을 주겠다고 설정이 가능하다.
- cdn을 통해서만 접속이 가능하도록 설정하는 것이 좋음 -> 비용 발생이 가능함
- Viewer Protocol Policy : httys only는 위험
- Field-level Encryption Config : 암호화
- Cached HTTP Methods : 이에 한해서만 캐시가 됨
- Cache Based on Selected Request Headers : 특정 헤더를 통해서 캐싱을 구분지음 -> user_id : 캐싱이 private 유저에게 열리게 됨 none이 일반적
- Object Caching : 캐시 컨트롤 origin 메타 데이터 값을 사용한다는 의미
- Minimum TTL 0초
- Maximum TTL 1년
- 캐시 유지되는 기간 : Cache-Control max-age
- Forward Cookies : ?
- Query String Forwarding and Caching : 캐싱이 되고 나서 index.html 수정 : 캐시 메타 데이터 없기 때문에 default TTL 따라감, cf에는 복제된 index.html을 따라감 (구분자가 달라진다고 생각)
- Smooth Streaming : 온디맨드 방식 비디오 스트리밍
- Restrict Viewer Access
  (Use Signed URLs or
  Signed Cookies) : (https://console.aws.amazon.com/iam/home?region=ap-northeast-2#/security_credentials) 서명된 url 사용하느냐 안하는냐
- Compress Objects Automatically : cf 복제할 때 zip 파일 형태로 적재한다는 의미 : 압축이 될 경우 원래 크기의 1/4 되는 경우도 존재하기 때문에 좋음 : Accept Encoding : gzip 으로 설정이 되어야만 요청이 처리됨
- Lambda Function Associations : 4가지의 람다 함수 추가가능
  4가지의 response에 대하여 이미지 비용을 줄일 수 있음
- Alternate Domain Names(CNAMEs) : 도메인 기능
- Default Root Object : s3의 default root 와 동일
