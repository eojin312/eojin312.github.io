---
title: "Vue.js 를 사용하기 위한 셋팅 과정"     
date: 2021-08-10 22:38:28 -0400
categories: 공부
---
## vue 를 세팅해봅시다!

1. terminal 을 띄우자

2. $ brew install node
   node-sass 를 사용한다면.. node-sass 버전을 확인하고 node 를 설치.. 저 명령어는 최신버전 노드를
설치하는 것 버전 확인은  [https://www.npmjs.com/package/node-sass](https://www.npmjs.com/package/node-sass)
   

3. $ brew install npm
- 모듈을 웹에서 받아서 설치하고 관리해주는 프로그램
    - npm 과 비슷하게 nvm 도 있다
    - npm 은 모듈관리라면, nvm 은 node 버전 관리
    - nvm 은 따로 보강 ..(분명 설치되어있는데 설치안되어있다고 뜸 개빡침;) 

4. intellij terminal 로 이동

5. $ npm install --global yarn (= intellij terminal 에서 yarn install)
- npm 깔았는데, 뜬금없이 yarn 이 뭐하는 새끼인가 싶을텐데.. 이 녀석은 npm 과 같은 새끼다.. 좀 더 유능한 새끼 (찾아보니 facebook 에서 만들었다고 하네.. 오..)
    - 모듈 install 할 때, npm install 해도 되는거고, yarn install 해도 된다. 근데 npm install 할 때 에러가 계속 생겼다 

6. yarn install (package.json (= maven 같은 애 위에서 줄곧 얘기하던 모듈이 여기에 있다.) 을 install 한다.)

$ yarn dev (프로젝트 run 시키는 명령어)
