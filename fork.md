
## fork 및 pr 절차.

``` bash
	## 0. git 및 gh는 기본적으로 디렉터리 바탕임.
	## 따라서 원하는 리포지토리의 내부에서 작업해야 함.

	## 1. fork할 repository 가져오기.
	git clone https://github.com/jhs512/p-14009-mission-1.git
	cd ./p-14009-mission-1

 	## 2. fork하기. 
	## 대화형으로 진행 됨.  
	gh repo fork

	## 3. fork한 repo의 default repo를 설정해줌.
	## 대화형으로 진행 됨. 
	gh repo set-default

	## 4. 현재 레포에서 pr을 생성함.
	gh pr create --title "[BE8]박재성 : 과제 제출합니다" --body "확인해주세요"

set-default

```
