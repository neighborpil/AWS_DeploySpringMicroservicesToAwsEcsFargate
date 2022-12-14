# AWS_DeploySpringMicroservicesToAwsEcsFargate

### Instructor Homepage
 - https://github.com/in28minutes/deploy-spring-microservices-to-aws-ecs-fargate


### How to build docker image on m1 macos

```
% docker buildx build --platform=linux/amd64 -t neighborpil/aws-currency-exchange-service-h2:0.0.4-SNAPSHOT .

% docker push neighborpil/aws-currency-exchange-service-h2:0.0.4-SNAPSHOT 

% docker buildx build --platform linux/amd64 -t neighborpil/aws-currency-exchange-service-mysql:0.0.1-SNAPSHOT . 

% docker buildx build --platform linux/amd64 -t neighborpil/aws-currency-conversion-service:0.0.1-SNAPSHOT .

% docker push neighborpil/aws-currency-conversion-service:0.0.1-SNAPSHOT

% docker buildx build --platform linux/amd64 -t neighborpil/aws-currency-exchange-service-h2-xray:0.0.1-SNAPSHOT .

% docker push neighborpil/aws-currency-exchange-service-h2-xray:0.0.1-SNAPSHOT

% docker buildx build --platform=linux/amd64 -t neighborpil/aws-currency-conversion-service-xray:0.0.1-SNAPSHOT .

% docker push neighborpil/aws-currency-conversion-service-xray:0.0.1-SNAPSHOT

```

### 파라미터 저장하는 법
 - System manager - Parameter Store에서 생성하고 저장하는 것이 가능하다

![image](https://user-images.githubusercontent.com/22423285/183383276-9a8f2003-21ed-4230-87b4-872496d6a2e1.png)


### 사용하는 법
 - account-id에는 자동 부여되되는 아이디가 들어가야 한다
 ![image](https://user-images.githubusercontent.com/22423285/183383657-75529cba-5b7a-4c28-bee9-95c9a7bb7837.png)


Environment Variables
SSM URN - arn:aws:ssm:us-east-1:<account-id>:parameter/

/dev/currency-exchange-service/RDS_DB_NAME - exchange_db
/dev/currency-exchange-service/RDS_HOSTNAME
/dev/currency-exchange-service/RDS_PASSWORD
/dev/currency-exchange-service/RDS_PORT - 3306
/dev/currency-exchange-service/RDS_USERNAME - exchange_db_user


 - 실제 사용 하는 곳 : task definition에서 사용
 
arn:aws:ssm:us-east-1:35906889:parameter/dev/currency-exchange-service/RDS_DB_NAME

arn:aws:ssm:us-east-1:35906889:parameter/dev/currency-exchange-service/RDS_HOSTNAME

arn:aws:ssm:us-east-1:35906889:parameter/dev/currency-exchange-service/RDS_PASSWORD

arn:aws:ssm:us-east-1:35906889:parameter/dev/currency-exchange-service/RDS_PORT

arn:aws:ssm:us-east-1:35906889:parameter/dev/currency-exchange-service/RDS_USERNAME


![image](https://user-images.githubusercontent.com/22423285/183384196-b5241389-e682-48a0-a8de-2fd8a427c346.png)


### app mesh
 - 내부망을 위한 프록시이다
 - ecs에서 설정해도 ec2에서 접근 가능하다
 - 추가 요금이 없다
 - fargate에 대해서만 

#### envoy setting options
 - 갱신하게 되면 새로 만들어 주어야 함
 - ENABLE_ENVOY_XRAY_TRACING : 1
 - ENVOY_LOG_LEVEL : trace

#### terminal에서 json 포맷되게 보는 법
```
$ sudo yum install jq
 
$ curl http://json호출주소 | jq
```
 
 
### 로그 통합하는 법
implementation 'org.springframework.cloud:spring-cloud-starter-sleuth'


### cli
$ aws configure 

IAM에서 cli 접근 키 생성 후
아이디설정
키 설정
기본지역 설정

$ aws servicediscovery list-services

$ aws servicediscovery delete-service --id=srv-czklcexmh5jh5p2w


$ aws servicediscovery list-namespaces

$ aws servicediscovery delete-namespace --id=ns-ewrulluidzvfme5x
