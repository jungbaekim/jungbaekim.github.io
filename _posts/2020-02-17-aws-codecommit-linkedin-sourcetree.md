---
layout: post
title:  "AWS CodeCommit Repository를 SourceTree와 연동하기"
date:   2020-02-17 03:59:26 +0900
categories: jekyll update
---


# AWS CodeCommit Repository를 SourceTree와 연동하기

## CodeCommit과 연동할 IAM User 생성
Root 계정으로 모든 설정을 관리하는것은 보안상 위험이 따른다.
따라서 CodeCommit과 연동하는 [별도의 계정을 만들어](https://console.aws.amazon.com/iam/home?#/users$new?step=details){: target="blank"} SourceTree와 연동을 하도록 한다.

---

![사용자 생성 예시](https://drive.google.com/uc?id=1RS74FLzLGplCcjK0cnnCAUGgAZ2BmfuR)

* 사용자이름은 원하는대로 넣어주고

* 엑세스 유형은 위 예시와 같이 프로그래밍 방식 엑세스를 선택해준다.

---

## 생성한 User의 권한을 부여

하단의 다음을 눌러 새롭게 생성된 사용자의 권한을 설정해 주어야 하는데, [기존 정책 직접 연결]을 선택하고, 정책필터에서 CodeCommit을 입력 후 AWSCodeCommitPowerUser를 선택하고 하단의 다음을 눌러 사용자를 생성한다.

---

![유저 권한 설정 예시](https://drive.google.com/uc?id=1KeUB3E0Z5uNtzgQrmKePsUJIXYlnQasr)

---

* 이후 입력은 선택사항이니 패스해도 무관

사용자 생성을 완료 하면 아래와 같은 화면이 출력된다.

![사용자 생성 후 예시](https://drive.google.com/uc?id=1-HK1JP-MJv6YFESgu8TLZJPUzzubhtdw)


엑세스 키 ID와 비밀 엑세스 키가 담겨있는 .csv파일은 지금이 아니면 다시다운로드 받을 수 없다.
만약 엑세스키와 비밀키를 분실하게 될 경우 사용자를 새로 만들어야 할 수 있으나 .csv파일은 민감한 정보이니 다운받아서 보관할지는 선택에 맡기겠다.

** AWS CLI 설치는 차후 포스팅

## 로컬환경에서 AWS User 설정

터미널을 열어 `$ aws configure`를 입력 후

순서대로 노출 되는  
`AWS Access Key ID`,  
`AWS Secret Access Key`  
에는 사용자 생성에서 나온 값을 Copy&Paste 해주고,

`Default region name`  
에는 CodeCommit을 생성한 Region (예: `ap-northeast-2`{서울})을 넣어주고,  
`Default output format`은  
`json`으로 입력한다.

입력을 완료 하였으면 IAM -> 사용자 탭에서 방금 생성한 사용자를 클릭 후 상단의 [보안 자격 증명]탭을 선택  
AWS CodeCommit에 대한 HTTPS Git 자격 증명 섹션의 [자격 증명 생성]을 클릭한다.

![보안 자격 증명 생성 예시](https://drive.google.com/uc?id=1XfW5oe8TPKLkDP_721Ali1azOybZ3SVQ)


사용자 생성과 마찬가지로 .csv 파일을 다운로드 할 수있는 옵션이 있는데 역시나 사용자의 선택사항에 맡기도록 하겠다.


terminal 로 돌아와 repository를 clone 하면 Username, Password를 입력 하는 화면이 나오고, 그곳에 방금 생성한 credential값 을 입력해주면 된다.


---

## SourceTree에 AWS User 설정

지금까지는 AWS CodeCommit의 저장소를 로컬로 clone하는 방법이었고, 이제 SourceTree에 저장소를 연동해 보도록 하겠다.


SourceTree의 global 설정을 수정하면 다른 Repository에서도 동일한 설정이 적용되기때문에 필자의 경우 가급적 저장소 마다 각각의 설정을 이용하는 편이다.

하지만 global 설정을 원하는 분을 위해 잠깐 언급하자면,
terminal 창에서

`$ git config --global credential.helper "!aws codecommit credential-helper $@"`  
`$ git config --global credential.UseHttpPath true`  
를 차례대로 입력 해주거나,  
Finder 에서 Command + Shift + G 를 누른 후 ~/.gitconfig (숨겨진 파일 보기 옵션이 On 상태여야 함) 을 직접 열고 기존 설정 값 하단에  
>[credential]  
>    helper = !aws codecommit credential-helper $@  
>    UseHttpPath = true  

를 추가 해주면 된다.
