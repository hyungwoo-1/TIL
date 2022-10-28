## GIT

---

> ### git restore

* working directory에서 수정한 파일을 수정 전으로 되돌리기

* 이미 버전 관리가 되고 있는 파일만 되돌리기 가능

* git retore를 통해 되돌리면, 해당 내용을 복원할 수 없으니 주의

* git retore {파일 이름}

* [참고] git 2.23.0버전 이전에는 git checkout -- {파일이름}



![](GIT%20advanced_assets/2022-10-28-09-23-08-image.png)

![](GIT%20advanced_assets/2022-10-28-09-23-24-image.png)

![](GIT%20advanced_assets/2022-10-28-09-23-38-image.png)



---

## staging Area 작업 단계 되돌리기

---

> ### 개요

* git add를 잘못 한 경우

* staging Area에 반영된 파일을 working directory로 되될리기

* root-commit 여부에 따라 두가지 명령어로 나뉨
  
  * root-commit이 없는 경우 : git rm--cached
  
  * root-commit이 있는 경우 : git retore --staged



> ### git restore --staged

* "the contents are restored from HEAD"

* root-commit이 있는 경우 사용(git 저장소에 한 개 이상의 커밋이 있는 경우)

* git restore --staged {파일이름}



---

### Repository 작업 단계 되돌리기

---

> ### git commit --amend

* 커밋을 완료한 파일을 staging area로 되돌리기

* 상황 별로 두 가지 기능으로 나뉨
  
  * staging area에 새로 올라온 내용이 없다면, 직전 커밋의 메세지만 수정
  
  * staging area에 새로 올라온 내용이 있다면, 직전 커밋을 덮어쓰기

* 이전 커밋을 완전히 고쳐서 새 커밋으로 변경하므로, 이전 커밋은 일어나지 않은 일이 되며 히스토리에도 남지 않음을 주의할 것!



---

## Git branch & merge

---

> ### branch

* branch는 나뭇가지라는 뜻으로, 여러 갈래로 작업 공간을 나누어 독립적으로 작업할 수 있도록 도와주는 Git의 도구



> ### 장점

1. 브랜치는 독립공간을 형성하기 때문에 원본에 대해 안전함

2. 하나의 작업은 하나의 브랜치로 나누어 진행되므로 체계적인 개발이 가능함

3. Git은 브랜치를 만드는 속도가 굉장히 빠르고, 적은 용량을 소모함



> ### Git branch

* 조회
  
  * git branch # 로컬 저장소의 브랜치 목록확인
  
  * git branch -r # 원격 저장소의 브랜치 목록 확인

* 생성
  
  * git branch {브랜치 이름} # 새로운 브랜치 생성
  
  * git branch {브랜치 이름} {커밋 ID} # 특정 커밋 기준으로 브랜치 생성

* 삭제
  
  * git branch -d{브랜치 이름} # 병합된 브랜치만 삭제 가능
  
  * git branch -D{브랜치 이름} # 강제 삭제



> ### Git switch

* 현재 브랜치에서 다른 브랜치로 이동하는 명령어

* git switch {브랜치 이름} # 다른 브랜치로 이동

* git switch -c {브랜치 이름} # 브랜치를 새로 생성 및 이동

* git switch -c {브랜치 이름} {커밋 ID} # 특정 커밋 기준으로 브랜치 생성 및 이동

* switch하기 전에, 해당 브랜치의 변경 사항을 반드시 커밋 해야함을 주의 할 것!
  
  * 다른 브랜치에서 파일을 만드록 커밋 하지 않은 상태에서 switch를 하면 브랜치를 이동했음에도 불구하고 해당 파일이 그대로 남아있게 됨



---

## Git merge

---

> ### git merge

* 분기된 브랜치들을 하나로 합치는 명령어

* master 브랜치가 상용이므로, 주로 master 브랜치에 병합

* git merge {합칠 브랜치 이름} {master} 선택된 상태에서 
  
  * 병합하기 전에 브랜치를 합치려고 하는, 즉 메인 브랜치로 switch 해야함
  
  * 병합에는 세 종류가 존재
    
    1. Fast-Forward
    
    2. 3-way Merge
    
    3. Merge Conflict



> ### Fast-Forward

* 마치 빨리감기처럼 브랜치가 가리키는 커밋을 앞으로 이동시키는 방법

* (master) $ git merge hotfix



> ### 3-way Merge

* 각 브랜치의 커밋 두 개와 공통 조상 하나를 사용하여 커밋하는 방법

* (master) $ git merge hotfix



> ### merge conflict

* 두 브랜치에서 같은 부분을 수정한 경우, git이 어느 브랜치의 내용으로 작성해야 하는지 판단하지 못하여 충돌이 발생했을 때 이를 해결하며 병합하는 방법

* 보통 같은 파일의 같은 부분을 수정했을 때 자주 발생함

* (master) $ git merge hotfix

* conflict 해결후 add  => commit => 끝



---

## Git worknow

---

> ### 개요

* Branch아 원격 저장소를 이용해 협업을 하는 두 가지 방법
  
  1. 원격 저장소 소유권이 있는 경우 => Shared repository model
  
  2. 원격 저장소 소유권이 없는 경우 => Fork % pull model



---

## Shared repository model

---

> ### 개요

* 원격 저장소가 자신의 소유이거나 Collaborator로 등록되어 있는 경우

* master 브랜치에 직접 개발하는 것이 아니라, 기능별로 브랜치를 따로 만들어 개발

* Pull Request를 사용하여 팀원 간 변경 내용에 대한 소통 진행



> ### 따라하기

* clone받기

* 자신이 작업할 기능에 대한 브랜치를 각자 생성하고 기능을 구현

* 기능구현이 완료되면, 본인 브랜치에 push (master X)

* 원격 저장소에 각 변경사항 반영

* Pull Request를 통해 브랜치를 master에 반영해달라는 요청을 보냄

* 병합이 완료되면 브랜치는 불필요하므로 원격 저장소에서 삭제

* master로 브랜치를 변경하고

* 병합으로 변경된 master의 내용을 pull

* 개발 완료한 브랜치는 삭제 (한 사이클 종료)

* 새 기능 추가를 위해 새로운 브랜치를 생성하고 지금까지의 과정을 반복



---

## Fork & Pull model

---

> ### 개요

* 오픈소스 프로젝트와 같이, 자신의 소유가 아닌 원격 저장소인 경우

* 원본 원격 저장소를 그대로 내 원격 저장소에 복제 (이러한 행위를 Fork라고 함)

* 기능 완성 후 복제한 내 원격 저장소에 Push

* 이후 Pull Request를 통해 원본 원격 저장소에 반영될 수 있도록 요청함



> ### 따라하기

* 소유권이 없는 원격 저장소를 fork를 통해 내 원격 저장소로 복제

* 이후에 로컬 저장소와 원본 원격 저장소를 동기화 하기 위해 연결(도중에 업데이트 할 경우 pull 을 받을 용도)

* 사용자는 자신이 작업할 기능에 대한 브랜치를 생성하고, 그 안에서 기능을 구현

* 기능 구현이 완료되면, 복제 원격 저장세오 해당 브랜치를 push

* 복제 원격 저장소에 브랜치가 반영됨

* Pull Request를 통해 origin의 브랜치를 uipstream에 반영해달라는 요청을 보냄

* upstream에 브랜치가 병합되면 origin

*  

* 병합으로 인

* 새로운 기능 추가를 위해 새로운 브랜치를 생성하며 위 과정을 반복



---

## [참고] PR 자세히 알아보기

---



---

## Git Branch 전략

---

> ### git-flow

* 2010년 vincent driessen이 제안한 git 브랜치 전략

* 아래와 같이 5개의 브랜치로 나누어 소스코드를 관리
  
  * master: 제품으로 출시될 수 있는 브랜치
  
  * develop : 다음 출시 버전을 개발하는 브랜치
  
  * feature : 기능을 개발하는 브랜치
  
  * release : 이번 출시 버전을 준비하는 브랜치
  
  * hotfix : 출시 버전에서 발생한 버그를 수정하는 브랜치

* 대규모 프로젝트에 적합한 브랜치 전략



> ### git-hub-flow

* 복잡한 git-flow를 개선하여 github에서 사용하는 방식

* Pull Request 기능을 사용하도록 권장, 병합후 배포가 자동



> ### gitlab-flow

* github-flow의 배포 이슈를 보완하기 위해 gitlab에서 사용하는 방식

* master 브랜치와 production 브랜치 사이에 pre-production 브랜치를 두어 개발 내용을 바로 반영하지 않고, 배포 시기를 조절함



> ### 정리

* 결국 어떤 브랜치 전략을 사용할 것 인지는 팀에서 정하는 문제

* 소개된 git, github, gitlab 브랜치 전략이 아닌 우리팀 고유의 브랜치 전략도 가능

* 브랜치를 자주 생성하는 것을 강력히 권장


