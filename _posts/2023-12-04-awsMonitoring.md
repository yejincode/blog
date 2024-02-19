---
layout: post  
title: `AWS를 통한 모니터링`
author: `송예진`
categories: `기술세미나`
banner:
  image: `썸네일로 넣고 싶은 이미지 링크`
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  tags: [`AWS`, `모니터링`, `키워드를`, `다음과 같이`, `넣으시면`, `태그가`, `생성됩니다.`]
---

안녕하세요,
저는 이번에 AWS를 통한 모니터링을 주제로 기술 세미나를 진행했습니다.

여러 프로젝트를 통해서 AWS에 서비스를 배포한 경험이 있으신 분들이 계실텐데요, 실제 서비스를 배포한다면 단순히 배포에서 끝나는 것이 아니라 이 후 모니터링을 통해 장애나 서비스 중단 등의 문제가 발생하는지 확인해보아야 합니다. 
이번 주제에서는 AWS가 제공하는 서비스를 통해, 모니터링을 어떻게 할 수 있는지에 대해 알아보겠습니다. 

## 1. AWS
- Amazon Web Services
- 아마존(Amazon)에서 제공하는 클라우드 서비스로, 네트워킹을 기반으로 가상 컴퓨터와 스토리지, 네트워크 인프라 등 다양한 서비스를 제공합니다.

AWS에서는 300개가 넘는 다양한 서비스를 제공하는데요, 이 중 가장 많이 쓰이는 서비스들이 무엇이 있는지에 대해도 간단히 살펴보겠습니다. 

## 2. AWS가 제공하는 다양한 서비스
### Amazon EC2 (Elastic Compute Cloud)
- 가상 서버를 제공하며, 사용자는 필요에 따라 가상 서버를 프로비저닝하고 실행할 수 있습니다.
- EC2는 유연하고 확장 가능한 클라우드 인프라를 제공하여 다양한 애플리케이션을 실행하고 관리할 수 있으며, Auto Scale을 통해 트래픽이 증가, 감소할 때 이에 대응하여 인스턴스의 수를 동적으로 조정할 수 있습니다. 
### Amazon S3 (Simple Storage Service)
- 객체 스토리지 서비스로, 대규모의 데이터를 안전하게 저장하고 검색할 수 있습니다.
- 데이터는 버킷이라는 컨테이너에 저장하며, 각 객체는 고유한 키로 식별됩니다. 
### Amazon RDS (Relational Database Service)
- 관리형 관계형 데이터베이스 서비스로, MySQL, PostgreSQL, Oracle 등을 제공합니다.

배포 경험이 있으시다면, 다들 한번 쯤 들어봤을 법한 서비스들이네요 !
다양한 서비스를 제공해주는만큼, 모니터링을 위한 서비스도 있습니다.

## 3. 모니터링에 필요한 AWS 서비스들
### Amazon CloudWatch
- AWS 리소스와 AWS에서 실시간으로 실행 중인 애플리케이션을 모니터링 하는 서비스
- 지표(metric)를 감시해 알림을 보내거나 임계값을 위반한 경우 모니터링 중인 리소스를 자동으로 변경하는 경보를 생성할 수 있습니다.

   - 지표(Metric) : 모니터링할 변수 (ex. 특정 EC2 인스턴스의 CPU 사용량, 네트워크 입출력, 로그 등)
   - 경보 : 생성해둔 단일 지표를 감시합니다.
   - 로그 : Amazon EC2과 같은 시스템과 애플리케이션 및 사용자 지정 로그 파일을 모니터링, 저장 및 액세스 해주는 기능을 제공합니다.
 
기본적으로 EC2 인스턴스, EBS 볼륨, RDS DB 인스턴스과 같은 많이 쓰이는 서비스에서 이러한 리소스에 대한 지표를 제공해주기 때문에 기본 제공되는 지표들은 따로 설정해주지 않고도 쉽게 사용할 수 있습니다. 
경보를 생성할 때, 지표에 대해 설정해둔 임계값을 초과했을 때에 대한 대처 방식 등을 설정할 수 있습니다. 
예를 들어 CPU 사용량이 지표라면 경보 설정을 할 때 50%를 임계값으로 설정해놓고, 초과했을 경우 이메일, 슬랙 등을 통해 알람을 받거나, lambda 등의 다른 서비스를 사용하여 원하는 방식으로 알람을 받을 수 있습니다. 

### Amazon SNS(Simple Notification Service)
- 구독 중인 Service 또는 사용자(Client)에 메시지 전달, 전송을 조정 및 관리 하는 웹서비스
- 만들어진 주제(Topic)에 대해 구독을 생성하고 이를 통해 구독되어있는 Email 주소들에 대해 메시지를 전송할 수 있습니다.
- 하나의 주제에 대해 다양한 대상(이메일, SMS, AWS lambda 함수 등)이 구독할 수 있습니다. 

### Amazon Lambda
- AWS에서 제공하는 서버리스 컴퓨팅 플랫폼
- 코드를 계속 실행시키기 보다는 특정한 시기에만 실행시키는 경우에 Lambda를 사용하면 유용합니다.

예를 들어 앞선 SNS 토픽을 람다가 구독한다고 했을 때, SNS 토픽을 통해 람다가 알림을 받았을 경우, 람다를 통해 이메일, 슬랙 등에 전송할 메시지를 처리할 수 있습니다. 

지금까지는 AWS의 서비스들을 살펴봤습니다. 
이제는 위의 서비스들을 활용하여 간단하게 AWS 모니터링 아키텍처를 구축해보겠습니다.

특정 EC2 인스턴스에서 log를 지표로 두어서 모니터링 하다가 경보 상태가 되면 SNS를 통해 이메일에 알람이 가도록 구현하겠습니다.

먼저 CloudWatch를 사용하기 위해 EC2에서 CloudWatch에 액세스 할 권한을 줍니다. 
리소스에 대한 권한을 담당하는 IAM 서비스를 통해 권한을 부여할 수 있습니다. 지금 보고 계신 이미지와 같이 신뢰할 엔터티 유형으로 AWS 서비스를 선택하고 사용사례로 EC2를 선택하여 EC2가 작업을 수행할 수 있도록 합니다. 
![image](https://github.com/yejincode/blog/assets/69861207/6ff394a2-1615-490e-8882-0e9556373ab7)

다음 단계는 권한 추가 입니다. EC2가 어떤 권한을 가질지 선택하면 되는데요, Cloudwatch에 액세스 해야하므로 cloudwatchfullaccess 라는 권한 정책을 선택합니다. 
![image](https://github.com/yejincode/blog/assets/69861207/e39b0b99-b0bf-4618-8d22-c1d9514e9d23)

생성이 된다면 다음과 같이 cloudwatch가 생성된 것을 볼 수 있습니다. 
![image](https://github.com/yejincode/blog/assets/69861207/71205bcd-7c74-4a54-9dc7-07306e9da32b)














