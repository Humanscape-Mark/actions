# **MDA v2 Server**

<!-- TOC depthFrom:2 -->

## **목차**
- [개발환경](#개발-환경)
- 개발 환경 세팅
  - MacOS 패키지 관리
  - Framework
  - Node.js 버전 관리
  - IDE
  - Repository
- 설치 방법
  - 서버 실행 방법
  - 빌드 방법
- Pull Request
  - dev
  - prod
- Commit

- 배포 방법
  - 개발 배포
  - 운영 배포
  - Hot Fix
- CI/CD

- ENV
  - AWS Secret Manager
  - Github Environment secrets
  - Github Environment variables
  - Github Repository variables

<!-- /TOC -->

## 개발 환경
- Language: Node.js (Typescript) + GraphQL
- IDE: VScode
- Repository: Github
- Framework: NestJS + TypeORM
- Database: MySQL
- Protocol: HTTP, GraphQL, TCP Socket


## **개발 환경 세팅**
### **MacOS 패키지 관리**
Homebrew
> Homebrew는 Apple(또는 Linux 시스템)에서 제공하지 않는 유용한 패키지 관리자를 설치한다. 
> [Homebrew page link]( https://brew.sh/index_ko ) 

- Terminal에 `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`를 입력한다.
- M1 Mac의 경우 설치 후 바로 brew command가 먹히지 않는 이슈가 있다. `eval $(/opt/homebrew/bin/brew shellenv)`를 .zshrc에 설정한다.


### **Framework**
NestJS
> NestJS는 Node.js에 기반을 둔 웹 API 프레임워크로써 Express 또는 Fastify 프레임워크를 래핑하여 동작한다. 데이터베이스, ORM, 설정(Configuration), 유효성 검사 등 수많은 기능을 기본 제공하고 있다. 
> [NestJS 참고 page link]( https://wikidocs.net/148194 )


### **Node.js 버전 관리**
fnm으로 node.js 버전 관리가 가능하므로 fnm 설치 & 설정 후에 node.js 설치
```sh
brew install fnm
```
fnm 환경변수를 ~/.zshrc에 추가
```sh
vim ~/.zshrc
eval "$(fnm env --use-on-cd)"
```
Terminal로 현재 개발 버전에 맞는 node.js 설치
```sh
fnm install 20
```
`node -v` 로 현재 버전을 확인해 보고 20이 아니라면 아래 명령어 실행
```sh
fnm use 20
```
또는, 프로젝트 디렉토리 벗어난 후 재진입 시 자동으로 .node_version 파일의 node 버전이 설정됨


### **IDE**
VScode
> VScode의 정식 명칭은 Visual Studio Code이며 마이크로소프트에서 오픈소스로 개발하고 있고, 다양한 extension을 지원하고 있다. 
> [Visual Studio Code page link]( https://code.visualstudio.com/ )

- 위의 link에서 다운로드 하여 설치 한다.

< VScode에서 사용하면 좋을 extension >
- Project Manager: 여러 프로젝트들을 손쉽게 관리해준다.
- GitLens: 마우스 오버 시 해당라인의 커밋 코멘트를 알 수 있다. 이전/다음 버튼으로 파일의 히스토리를 쉽게 파악 할 수 있다.
- Material Theme: vscode 테마를 손쉽게 설정 할 수 있다.
- Material Icon Theme: 디렉토리안의 파일 아이콘들을 시각적으로 바로 알아볼 수 있도록 설정 할 수 있다. 


### **Repository**
GitHub
> 대표적인 Git 저장소

- 기존의 구성원으로 부터 초대를 받아 계정을 생성한다.
- 계정에 2FA( 이중 인증 절차 )를 필수적으로 적용해야 한다. 우측 상단 아이콘 > Settings > Password and Authentication > Two-factor authentication에서 설정 가능하다.
- 추가적으로 로컬에서 GitHub 연동을 하려면 ssh key를 통해 연동 할 수 있으며, 한번 연동하면 git clone시 비밀번호 입력 절차 없이 clone이 가능하다.
 우측 상단 아이콘 > Settings > SSH and GPG keys > SSH keys의 "New SSH Key"에 먼저 생성한 ssh key를 등록한다. [참고 page link](https://problem-solving.tistory.com/7)


## **설치 방법**
- 레포지토리 클론
    ```sh
    git clone https://github.com/mmtalk-app/mmb-box-admin-server.git
    ```
- `package.json` 내의 의존성 패키지들 일괄 설치
    ```sh
    npm install
    ```

### **서버 실행 방법**
```sh
npm start
```

### **빌드 방법**
```sh
npm run build
```


## **배포 방법**
### **개발 배포**
- Jira에서 이슈 할당
  - 이슈를 만들거나, 만들어져 있는 이슈를 가져온다
  - Sprint, 담당자 지정
- develop에서 브랜치를 생성 (브랜치 이름은 Jira 이슈 이름)
- 작업 완료 후 PR
- dev 브랜치로 merge 하면 자동으로 version bump 후 (+tag 생성) dev에 배포


### **운영 배포**
- 일정량 작업 이후 운영 배포
  - 기간이나 기준은 없음
- main 브랜치로 merge 이후 actions에서 release 필요
- 각 리전 서버 배포는 actions에서 수동으로 진행
  - 각 리전별 운영시간 고려 필수, hotfix는 신중히, 알아서, 잘


### **Hot Fix**
- 바로 수정해야 할 긴급한 문제가 발생했을 때
  - 바로 dev에 commit, 테스트 후 prod로 merge
  - 바로 prod에 commit, 해결 후 dev로 merge
  - prod에서만 테스트 가능한 문제는 최대한 dev에서도 테스트할 수 있게 환경 수정 필요


## **Pull Request**
### **dev**
- PR 이름은 Jira 이슈 이름
- 이슈 1개당 최소 1개의 PR, 한 PR에서 여러 이슈 해결 금지
  - Release할 때 자동 생성되는 작업 내역이 PR 기준으로 작성됨 
- dev 브랜치는 squash merge 기본
- 웬만한 PR은 토끼가 approve 하면 merge
  - 중요한 변경 사항이 있거나, 논의해볼 만한 문제가 있다면 토끼가 approve한 이후에 리뷰어 추가 및 댓글에 추가한 이유 작성 (리뷰어가 중요한 알림만 받을 수 있도록 미리 추가해 놓지 않는다)


### **main**
- main PR 이름은 deploy vX.X.X
- main 브랜치는 commit marge 기본
- develop PR들은 모두 approve 된 내용들이라 리뷰 필요 없음


## **Commit**
- 본인 작업 브랜치로의 commit은 자유롭게
  - 공통 commit naming도 안지켜도 됨
- dev로 merge할 때는 `squash merge`로 깔끔하게
- main으로 merge할 때는 `commit merge`로 작업 내용 보존


## **CI/CD**
- Github action으로 자동 ECS 배포

<table>
<tr><th>Deploy to Amazon ECS</th></tr>
<tr><td valign="top">

<https://github.com/mmtalk-app/mmb-box-admin-server/actions>
</td></tr>
</table>


## **EVN**
- 모든 환경변수는 Naming Rule에 따른 각 국가별 리전의 Secret Manager에 존재
- Gitub actions에 필요한 기본 환경변수는 하기 목록별로 존재
  - Github Secrets
  - variables
  - Repository variables


### **AWS Secret Manager**
prod
  - kr
https://ap-northeast-2.console.aws.amazon.com/secretsmanager/secret?name=box-admin-server-kr&region=ap-northeast-2
  - id
https://ap-southeast-1.console.aws.amazon.com/secretsmanager/secret?name=box-admin-server-id&region=ap-southeast-1
  - vn
https://ap-southeast-1.console.aws.amazon.com/secretsmanager/secret?name=box-admin-server-vn&region=ap-southeast-1
  - us
https://us-east-1.console.aws.amazon.com/secretsmanager/secret?name=box-admin-server-us&region=us-east-1


### **Github Environment secrets**
  - AWS_ACCESS_KEY_ID
  - AWS_SECRET_ACCESS_KEY
  - AWS_ARN


### **Github Environment variables**
  - ENV_PATH


### **Github Repository variables**
  - CLUSTER_NAME
  - ENV_PATH
  - RESOURCE_NAME
