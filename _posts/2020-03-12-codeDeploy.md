---
title: "terminal codeDeploy 설치"
date: 2020-03-12 16:20:28 -0400
categories: 개인공부
---

```console
$ aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2
```

성공하면 이런 메시지가 뜬다

```console
download: s3://aws-codedeploy-ap-northeast-2/latest/install to ./install
```

install 파일에 실행권한 추가

```console
$ chmod +x ./install
```

그 상태 그대로 설치 진행하기

```console
$ sudo ./install auto

I, INFO -- : Starting Ruby version check.
I, INFO -- : Starting update check.
I, INFO -- : Attempting to automatically detect supported package manager type for system...
I, INFO -- : Checking AWS_REGION environment variable for region information...
I, INFO -- : Checking EC2 metadata service for region information...
I, INFO -- : Downloading version file from bucket aws-codedeploy-ap-northeast-2 and key latest/VERSION...
~~중략~~
Installed:
  codedeploy-agent.noarch 0:1.0-1.1597                                                                              

Complete!
```

Agent 정상적인 실행하는지 확인 
```console
$ sudo service codedeploy-agent status
The AWS CodeDeploy agent is running as PID {숫자}
```







