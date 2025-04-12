# Git 요약

# 챕터 1 - 개요

## Git의 시스템 개념

### 스냅샷 시스템

- Git의 시스템은 각 커밋마다 파일의 스냅샷을 기록함.
- 모든 파일들을 각각 스냅샷으로 기록함.
  - 변화가 없다면 생략하지만, 그래도 각 버전에 모든 파일 정보가 담김.
  -
  - [페이지](./other.md)

### 시스템 구역

- 시스템을 Working, Staging, Repository의 구역으로 나눌 수 있음.
- 파일은 Modified, Staged, Committed의 3가지 상태를 지님.

# 챕터 2 - Git의 기초

```bash
git init
git status

git add *.
git mv
git rm .

git diff [--cached]
git commit [-a][--amend][-m]
```

## 저장소(Repository)

- Git 시스템의 관리하에 있는 디렉토리.
- `git init` 커맨드를 통해서 생성 가능함.

### 변경사항 저장하기

- Git은 저장소 내부의 파일의 변화를 감지함.
  - 새로운 파일이 생긴 경우에는 알아차릴 수 있음.
- `git add` 를 이용해서 파일의 변화를 추적하도록 만들 수 있음.
  - add된 파일은 **Tracked** 상태가 됨.

### 저장소 상태 확인하기

- `git status`를 이용해서 저장소 상태를 확인함.
- 저장소 내 파일의 **Tracked**, **Modified**, **Staged** 여부를 보여줌.
- 빨간색이면 Unstaged, 초록색이면 Staged

### 파일 삭제하기

- `git rm`을 이용해서 파일을 삭제할 수 있다.
  - `rm`을 해도 되지만, `git`을 붙이면 git도 추적함.

### 파일 옮기기 & 이름 바꾸기

- `git mv`을 이용해서 파일을 옮기거나, 이름을 바꿀 수 있다.
  - `mv`를 해도 되지만, `git`을 붙히지 않으면, **새로운 파일이 생성된 것으로 취급됨**
  - 따라서 파일의 이름 변경 및 이동 같은 경우에는 되도록 `git`을 사용해서 처리하자.

### 저장소 수정사항 확인하기

- `git diff`를 이용해서 현재 저장소에서 Unstaged된 변화를 추적
- `git diff --cached`를 이용해서 저장소에 생긴 변화를 추적.

### 변경사항 반영하기

- `git commit`을 이용하면 ***vi***로 편집이 가능하다.
- `git commit -m`을 이용하면 뒤에 문자열을 적어 곧바로 커밋이 가능하다.
- `git commit -a`을 이용하면 **Tracked** 상태의 파일을 전부 커밋한다.

### 변경사항 수정하기

- `git commit --amend`를 이용해서 이전 커밋을 수정할 수 있다.
  - `commit`커맨드이므로 `-a`, `-m` 옵션을 이용해서 커밋 메세지를 바꿀 수 있음.
    - `--amend`와 함께 `-m` 옵션을 사용한 경우, 이전 커밋의 메시지도 수정됨.
- 리모트 저장소에 반영하기 위해선 `--force`, `--force-with-lease` 옵션을 사용해야 함.
  - `--force-with-lease`를 사용할 것을 권장함. `push`되지 않은 경우에만 `commit`함.

#### :warning: 주의 사항

##### `amend` 후 `push`할 시

- 로컬 저장소에서 `amend`를 하면, **리모트 저장소와 분기가 달라짐**
- `amend`하게 되면 **체크섬이 바뀌기 때문**.
- 따라서 반드시 `--force-with-lease` 등으로 맞춰줘야 함.

#### `amend` 후 `pull`할 시 주의사항
  
## 리모트 저장소

```bash
git clone https://github.com/lucie-perusonari/git_study

git remote
git remote add origin https://github.com/lucie-perusonari/git_study

git pull origin main
git fetch origin main
git push origin main
```

### 리모트 저장소 복제하기

- `git clone` 명령어를 통해서 현재 디렉토리에 기존의 저장소를 불러올 수 있다.
- `git clone`으로 저장소를 불러오면 그 리모트는 origin이란 이름으로 등록된다.

### 리모트 만들기

- `git remote add`로 원격 저장소의 이름을 추가할 수 있다
  - 그러나 리모트를 등록했을 뿐. 저장소가 활성화된 것은 아니다.

### 리모트에서 저장소 로드하기

- `git fetch origin`으로 원격 저장소의 모든 브런치를 가져올 수 있다
  -

# 챕터 3 - 브랜치

```bash

git branch



```

- 별도의 수정.
- amend를 방해함.
- 새로 추가하고픈 데이터
