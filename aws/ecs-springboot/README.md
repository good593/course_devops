---
style: |
  img {
    display: block;
    float: none;
    margin-left: auto;
    margin-right: auto;
  }
marp: true
paginate: true
---
# [AWS CodePipeline 요금](https://aws.amazon.com/ko/codepipeline/pricing/?icmpid=docs_console_unmapped)
AWS CodePipeline에는 선결제 금액이나 약정이 없습니다.
- `V1 유형 파이프라인`: 활성 파이프라인 (30일 이상 존재하고 해당 월에 코드 변경이 한 번 이상 실행되는 파이프라인)당 월 1.00 USD가 부과됩니다. 파이프라인 생성 후 처음 30일 동안은 무료입니다.
- `V2 유형 파이프라인`: 작업 실행 분당 0.002 USD가 부과됩니다. 수동 승인 및 사용자 지정 작업 유형을 제외한 모든 작업 유형에 대해 요금이 부과됩니다. 

---
### AWS 프리 티어
CodePipeline은 AWS 프리 티어의 일부로 신규 및 기존 고객에게 다음을 제공합니다.
- `V1 유형 파이프라인`: V1 유형 파이프라인에 대한 매월 1개의 무료 활성 파이프라인
- `V2 유형 파이프라인`: 매월 무료 작업 실행 시간 100분 무료 작업 실행. 사용하지 않은 시간(분)은 다음 달로 이월되지 않습니다.

### 추가 요금
Amazon S3에 파이프라인 아티팩트를 저장하고 액세스하는 경우와 파이프라인에 연결한 기타 AWS 및 서드 파티 서비스에서 작업을 트리거하는 경우 추가 요금이 발생할 수 있습니다.

---
# Github Code

---
### 단계1: Springboot 프로젝트 생성
![alt text](image.png)

---
### 단계2: Springboot 테스트 
```shell
http://localhost:8080/api/v1/hello

http://localhost:8080/api/v1/message/goodjob!!!
```
![alt text](image-1.png)

---
### 단계3: Dockerfile 추가 
![alt text](image-2.png)

---
### 단계4: docker build 테스트 
```shell
docker build -f ./aws/ecs-springboot/Dockerfile -t ecs-springboot:latest .
docker images
```
![alt text](image-3.png)

---
### 단계5: buildspec.yml 추가 
![alt text](image-4.png) 

---
### 단계6: Github 배포 
![alt text](image-9.png)

---
# Amazon Elastic Container Registry

---
### 단계1: Amazon Elastic Container Registry
![alt text](image-5.png)

---
### 단계2: Amazon Elastic Container Registry > Setting
- `buildspec.yml`에 정의된 이름으로 작성

![alt text](image-7.png)

---
### 단계3: Amazon Elastic Container Registry > Create
![w:700](image-6.png)

---
# CodeBuild

---
### 단계1: Create connection
![alt text](image-8.png)

---
![alt text](image-10.png)

---
![alt text](image-11.png)

---
### 단계2: connection 확인 
![alt text](image-12.png)

---
### 단계3: Create build project
![alt text](image-13.png) 

---
- Project configuration
![alt text](image-14.png)

---
- Connect to Github
![alt text](image-15.png)

---
![alt text](image-16.png)

---
- Environment
```shell
# Role name
codebuild-ecs-springboot-build-service-role
```
![bg right w:600](image-17.png)

---
- Buildspec
![alt text](image-18.png)

---
- Create build project
![bg right w:600](image-19.png)

---
### 단계4: build role > add Permission
![alt text](image-21.png)

---
```shell
# add Permission
AmazonEC2ContainerRegistryPowerUser
```
![alt text](image-22.png)

---
![alt text](image-23.png)


---
### 단계5: Start build
![alt text](image-20.png)

---
- Start build > Succeeded

![alt text](image-24.png)

---
### 단계6: Amazon Elastic Container Registry 확인 
![alt text](image-25.png) 

---
# Amazon Elastic Container Service

---
### 단계1: Create Cluster
![alt text](image-26.png)

---
- Cluster configuration

![alt text](image-27.png)

---
- Infrastructure

![alt text](image-28.png)

---
- 생성 확인 

![alt text](image-29.png)

---
### 단계2: Create new task definition
![alt text](image-30.png)

---
- Task definition configuration

![alt text](image-31.png)

---
- Infrastructure requirements

![bg right w:600](image-32.png)

---
- copy > Amazon Elastic Container Registry URI

![alt text](image-33.png)

---
- Container 
```shell
# buildspec.yml에 정의된 이름으로 생성 
ecs-springboot-container
```
![w:800](image-34.png)

---
- Logging

![alt text](image-35.png)

---
- HealthCheck
```shell
CMD-SHELL,curl -f http://localhost:8080/api/v1/hello || exit 1
```
![w:700](image-36.png)

---
- Create

![w:700](image-37.png)

---
- 결과 확인 

![alt text](image-38.png)

---
### 단계3: Create Service
- Cluster 선택 

![alt text](image-40.png)

---
- Create Service

![alt text](image-39.png)

---
- Environment

![alt text](image-41.png)

---
- Deployment configuration

![w:700](image-42.png)

---
- Create 

![w:700](image-43.png)

---
# API 테스트 

---
### 단계1: Public IP 확인 
- Service 선택 

![alt text](image-45.png)

---
- Task 선택 

![alt text](image-44.png)

---
- Public IP 확인 

![alt text](image-46.png)

---
### 단계2: Security Group > Add Inbound rule
![alt text](image-47.png)

---
### 단계3: API 테스트 
```shell
[Public IP]:8080/api/v1/hello
```
![alt text](image-48.png)

---
# CodePipeline

---
### 단계1: Create pipeline
![alt text](image-49.png)

---
### 단계2: Choose pipeline settings

![w:700](image-50.png)

---
- Next

![alt text](image-51.png)

---
### 단계3: Source

![bg right w:600](image-52.png)

---
- Next

![alt text](image-53.png)

---
### 단계4: Add build stage

![bg right w:600](image-54.png)

---
### 단계5: Add deploy stage
![bg right w:600](image-55.png)


---










---
# 참고문서
- https://github.com/kodedge-swapneel/springboot-aws-deploy

















