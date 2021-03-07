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
