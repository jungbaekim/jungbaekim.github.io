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

*사용자이름은 원하는대로 넣어주고

*엑세스 유형은 위 예시와 같이 프로그래밍 방식 엑세스를 선택해준다.

---

하단의 다음을 눌러 새롭게 생성된 사용자의 권한을 설정해 주어야 하는데, [기존 정책 직접 연결]을 선택하고, 정책필터에서 CodeCommit을 입력 후 AWSCodeCommitPowerUser를 선택하고 하단의 다음을 눌러 사용자를 생성한다.

---

![유저 권한 설정 예시](https://drive.google.com/uc?id=1KeUB3E0Z5uNtzgQrmKePsUJIXYlnQasr)

---

*이후 입력은 선택사항이니 패스해도 무관
