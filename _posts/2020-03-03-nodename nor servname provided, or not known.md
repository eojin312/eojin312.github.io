---
title: "nodename nor servname provided, or not known"
date: 2020-03-03 18:20:28 -0400
categories: 개인공부
---
aws 에서 ip 를 할당받고 pem 키를 다운받았습니다
근데 aws 와 같이 외부서버로 ssh 접속으를 하려면 긴 명령어를 일일이 치고 접속해야하는데 쉽게 ssh 를 접속할 수 있게 pem 파일을 자동으로 읽어서 접속 진행할 수 있도록 해줬습니다.
pem 키를 복사해서 ssh 파일에 붙여넣고 pem 키의 권한을 변경하고 등등 많은 절차를 진행하고 나서 config 파일에서 Host 키값을 적고 
ssh config 에 등록한 서비스명을 치고 ec2 에 접속하려는데 **nodename nor servname provided, or not known** 이라는 에러가 발생했습니다.
리눅스도 ec2 도 아직 낯설은 저에게 이 에러는 마치 처음부터 꼬인 듯 해보였습니다. 하지만 좌절하지않고 문제점을 하나씩 되짚어보았습니다.
일단 config 파일 내 설정들이 문제인가 봤더니 다행이도 여기서 문제를 해결할 수 있었습니다.

```console
# freelec-springboot-webservice
Host freelec-springboot-webservice
    HostName 15.~~~~~* -> 이게 문제인 것
    User ec2-user
    IdentityFile _/.ssh/freelec-springboot-webservice
```

책에서 HostName 은 aws 에서 부여받은 탄력적 ip 를 붙여넣으라고 되어있었습니다. 탄력적 ip 는 15.~~~ * 이 붙여져있었고
* 를 없애니 정상적으로 해결되었습니다


