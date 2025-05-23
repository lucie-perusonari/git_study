# Git 요약

# 챕터 1 - 개요

## Git의 시스템 개념

### 스냅샷 시스템

- Git의 시스템은 각 커밋마다 파일의 스냅샷을 기록함.
- 모든 파일들을 각각 스냅샷으로 기록함.
  - 변화가 없다면 생략하지만, 그래도 각 버전에 모든 파일 정보가 담김.

### 시스템 구역

- 시스템을 Working, Staging, Repository의 구역으로 나눌 수 있음.
- 파일은 Modified, Staged, Committed의 3가지 상태를 지님.

# 챕터 2 - Git의 기초

```bash
## Local Git Repository에서 
## init 및 commit 하는 방법.

git init
git status

git add .
git mv <source>... <destination>
git rm .

git diff [--cached]

## Commit 관련.
git commit [-a][--amend][-m][--no-edit]
git log [--oneline][--graph]

```

## 저장소(Repository)

- Git 시스템의 관리하에 있는 디렉토리.
- `git init` 커맨드를 통해서 생성 가능함.

#### 변경사항 저장하기

- Git은 저장소 내부의 파일의 변화를 감지함.
  - 새로운 파일이 생긴 경우에는 알아차릴 수 있음.
- `git add` 를 이용해서 파일의 변화를 추적하도록 만들 수 있음.
  - add된 파일은 **Tracked** 상태가 됨.

#### 저장소 상태 확인하기

- `git status`를 이용해서 저장소 상태를 확인함.
- 저장소 내 파일의 **Tracked**, **Modified**, **Staged** 여부를 보여줌.
- 빨간색이면 Unstaged, 초록색이면 Staged

#### 파일 삭제하기

- `git rm`을 이용해서 파일을 삭제할 수 있다.
  - `rm`을 해도 되지만, `git`을 붙이면 git도 추적함.

#### 파일 옮기기 & 이름 바꾸기

- `git mv`을 이용해서 파일을 옮기거나, 이름을 바꿀 수 있다.
  - `mv`를 해도 되지만, `git`을 붙히지 않으면, **새로운 파일이 생성된 것으로 취급됨**
  - 따라서 **파일의 이름 변경 및 이동 같은 경우에는 되도록 `git`을 사용해서 처리하자.**

#### 저장소 수정사항 확인하기

- `git diff`를 이용해서 현재 저장소에서 Unstaged된 변화를 추적
- `git diff --cached`를 이용해서 저장소에 생긴 변화를 추적.

#### 변경사항 반영하기

- `git commit`을 이용하면 ***vi***로 편집이 가능하다.
- `git commit -m`을 이용하면 뒤에 문자열을 적어 곧바로 커밋이 가능하다.
- `git commit -a`을 이용하면 **Tracked** 상태의 파일을 전부 커밋한다.

#### 변경사항 수정하기

- `git commit --amend`를 이용해서 이전 커밋을 수정할 수 있다.
  - `commit`커맨드이므로 `-a`, `-m` 옵션을 이용해서 커밋 메세지를 바꿀 수 있음.
    - `--amend`와 함께 `-m` 옵션을 사용한 경우, 이전 커밋의 메시지도 수정됨.
- 리모트 저장소에 반영하기 위해선 `--force`, `--force-with-lease` 옵션을 사용해야 함.
  - `--force-with-lease`를 사용할 것을 권장함. `push`되지 않은 경우에만 `commit`함.

#### 주의 사항

##### `amend` -->  `push`하는 경우

- 로컬 저장소에서 `amend`를 하면, **리모트 저장소와 분기가 달라짐**
- `amend`하게 되면 **체크섬이 바뀌기 때문**.
- 따라서 반드시 `--force-with-lease` 등으로 맞춰줘야 함.

##### `amend` --> `pull`하는 경우

- 다른 브런치에서 push가 발생하면 `force-with-lease`를 사용하지 못함.
  - 이 경우 먼저 rebase 후에 push해야 함.
  - 자세한 사항은 [rebase]() 장에서 다룸.
- 다른 브런치에서는 git pull을 하면 해당 커밋이 rebase 됨.

##### 다른 계정에 push하는 경우.
- `git config --list`를 살펴보면, 현재 git 저장소의 유저 이름과 메일이 나옴.
- 이것을 올바르게 수정해줘야만 저장소를 찾을 수 있음.

## 리모트 저장소

```bash
## Remote Repository를 
## clone, add, pull, push하는 방법.

git clone <url>

git remote [-v]
git remote add <name> <url>
git remote remove <name>

git pull <remote> <branch>
git fetch <remote> <branch>

git push <remote> <branch> [--force-with-lease]
```

#### 리모트 저장소 복제하기

- `git clone` 명령어를 통해서 현재 디렉토리에 기존의 저장소를 불러올 수 있다.

#### 리모트 만들기

- `git remote add`로 원격 저장소의 이름을 추가할 수 있다
  - 그러나 리모트를 등록했을 뿐. 저장소가 활성화된 것은 아니다.

#### 리모트에서 저장소 로드하기

- `git fetch origin`으로 원격 저장소의 모든 브런치를 가져올 수 있다
  - **로컬에 없는 브런치가 원격에 있다면,** `fetch`**를 해줘야 함.**

# 챕터 3 - 브랜치

```bash
## Local Repository에서 Branch를 사용하는 방법.
## Branch는 현재 Commit을 기준으로 만들어진다.
git branch [<name>][-vv][-d][-r]
git checkout <commit> [-b <branch>]

git merge [<branch>][<branch>]
git rebase [--onto] [--continue]

## Remote Repository에서 Branch를 사용하는 방법.
git pull <remote> <branch>
git fetch <remote> <branch>

git push <remote> <branch> [--force-with-lease]

## Squash를 사용해서 커밋을 정리하는 방법.
git commit --fixup=[(amend|reword):]<commit>
git rebase --autosquash

```
## 브랜치의 기본
- 브랜치는 커밋의 분기(Branch)를 의미한다.

### 커밋
- 저장소는 커밋의 연쇄로 구성 됨.
- 커밋의 현재 위치를 `HEAD`라고 함.
- 특정 지점의 커밋을 식별하기 위해 `tag` 혹은 `branch`를 사용함.

### 태그.
- 특정 커밋의 위치를 알리는 용도.

### 브랜치
- 특정 분기(Branch)를 가리키는 요소.
  - 근본적으로 커밋의 포인터이다.

## 브랜치 병합.
-  `merge`, `rebase` 등으로 브랜치를 합칠 수 있다.
   -  그러나 두 브랜치가 같은 파일을 수정하는 경우 충돌이 발생한다.
-  리모트와 로컬을 동기화하는 과정에서 많은 문제가 발생한다.

### Merge
- 공통 조상을 기본으로 두고, 두 브랜치를 합친다.
  - merge commit은 merge된 두 브랜치를 가리킨다.
  - => 이를 3-way merge라고 표현한다.

### Rebase
- 공통 조상의 위치에서 sub-branch의 diff를 저장한다. 그 뒤 main-branch에 덮어씌운다.
- 깔끔한 커밋 이력 관리를 위해서 사용한다.

### Pull & Push
- `pull`, `push` 등으로 리모트와 동기화할 수 있다.
  - 이 과정에서 현재 커밋의 체크썸이 사용된다.
  - 만일 체크썸이 바뀌었다면, 충돌이 발생한다.

### Squash  
- 커밋 시에 `--fixup` 옵션을 사용해서, 주 커밋과 수정 커밋을 구분할 수 있다.
  - 브랜치의 수정이 모두 끝났을 때 `--autosquash`를 이용하여 rebase한다.
  - 이미 `--fixup`으로 표시해뒀으므로 알아보기 쉽다.

### 충돌 해결 전략.
- **push된 커밋에는 amend를 사용하지 않는 것이 최선이다.** 
  - `force-with-lease` 옵션 등을 이용해서 push하는 방법은 좋지 않다.
- 그러므로 깔끔한 커밋 이력을 위해 다른 방법을 사용한다.
  - 먼저 `--fixup` 옵션으로 fixup 커밋을 생성한다.
  - 그렇게 생성된 fixup commit에 amend한다.
  - push하기 전에 rebase 리베이스기점 --auto-squash를 이용해서 커밋을 정리한다.
    - (* 그러나 이렇게 하더라도 rebase시에 rebase된 커밋에 문제가 생김. => 그렇다면 remote에 대해서 rebase하면 되는 거잖아?)