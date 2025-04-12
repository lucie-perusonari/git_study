# Git 요약

[Git 요약.](./summary.md)

# 개요

- 코드 동기화와 버전 관리를 위한 툴.
- Github와는 다름.
- Git 자체적으로 버전 관리를 하며, 굳이 Github가 없더라도 그 자체를 버전 관리용으로 사용하는 것이 가능하다.
- 만일 로컬 서버에서 git만을 이용해서 협업을 하고 싶다면, 해당 서버에 git을 깔아 놓고 사용하면 됨.

# 챕터 1

## 왜 사용하는가?

- 버전 관리(Version Control)를 위해서 사용한다.
- 버전 관리 시스템(VCS, Version Control System)은 파일 변화를 시간에 따라 기록했다가 나중에 특정 시점의 버전을 다시 꺼내올 수 있는 시스템이다.
- 근본적으로 Git은 로컬에 변경 데이터를 모두 저장하기 때문에, 서버의 영향을 받지 않는다.

### 파일 관리 시스템의 종류

- RCS(Revision Control System)는 로컬에서 버전 관리를 하는 용도.
- CVCS(Centralized Version Control System)는 서버 단위에서 버전 관리를 하는 용도.
- DVCS(Distributed Version Control System)은 아예 파일 자체에 버전을 모두 명시하는 방식으로 버전을 관리함.

## Git과 여타 VCS의 차이

- 델타 기반 버전 관리는 각 파일의 변화점을 기록한다.
  - 즉, 처음의 파일 원본에 변화점을 덧붙이는 식으로 버전이 관리된다.
  - 따라서 이 논리에 따르자면 변경점만 저장되기 때문에 원본 소실에 취약함.
- 반면 Git은 스냅샷을 이용해서 변화점을 기록한다.
  - 파일 그 자체를 통째로 찍어버리기 때문에 각 버전은 모두 고유하게 존재한다.
  - Git은 체크섬(Hash)를 이용한다. 체크섬을 이용해 파일의 변화 여부를 감지한다.

## 중요한 특징

- Git은 데이터를 오직 추가하기만 한다. 잘못된 commit이 있지 않은 이상 기록이 지워지는 일은 존재하지 않는다.
- Git은 파일을 '***Committed***, ***Modified***, ***Staged***' 이렇게 세 가지 상태로 관리한다.
  - '**Committed**'란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다.
  - '**Modified**'는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다.
  - '**Staged**'란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.
- **Git 디렉토리, 워킹 트리, Staging Area**의 상호작용 구조로 되어있다.

### Git의 시스템 구조  

  ![areas.png](./img/areas.png)

- Git 디렉토리를 clone해서 워킹 트리를 만든다.
- 워킹 트리를 수정한다. ⇒ **Modified**
- 워킹 트리의 작업을 Staging Area에 저장한다. ⇒ **Staged**
- Staging Area의 데이터를 Commit해서 Git 디렉토리에 반영한다. ⇒ **Committed**
  - 이 때 Git 디렉토리가 어디에 있느냐에 따라서 다른 브런치에 영향을 미칠 수도 있다.

## 설정 방법

- <https://git-scm.com/book/ko/v2/시작하기-Git-최초-설정>
- Git 시스템 전체, 현재 Git 사용자, 특정 저장소에 설정을 할 수 있다.
  - 범위가 좁아질 수록 우선권이 높다.
  - Git 시스템 전체(가장 낮음) < 현재 Git 사용자 < 특정 저장소(가장 높음)
- 편집기 설정.
  - VS Code를 사용할 것이므로 해당 문서를 찾아서 따로 설정해줘야 한다.

## 도움말 보기

- 여차하면 git [커맨드] -h로 명령어를 확인해보자.

# 챕터 2

## 저장소 만들기

- 특정 디렉토리에 들어가서 "***git init***"을 한다.
  - ***“.git”*** 이라는 디렉토리가 생성되고, 이곳이 바로 Git들이 저장 장소가 된다.
- 이렇게 해서 결과적으로 생성되는 것은 일종의 Repository이다.
  - ***.git/ 리포지토리 이름 /…***
- Repository는 저장소 혹은 프로젝트라고 지칭된다.

## **수정하고 저장소에 저장하기**

- 워킹 디렉토리의 모든 파일은 '**Tracked(관리대상임)**'와 '**Untracked(관리대상이 아님)**'로 나눈다.
- Tracked 파일은 이미 스냅샷에 포함돼 있던 파일이다.
  - Tracked 파일은 '**Unmodified, Modified, Staged**' 상태 중 하나이다.
  - 이는 Staging area와 관련해서 생각해봐야 한다.
    - **Unmodified => (수정) => Modified => (Commit에 추가) => Staged**'

### **파일의 상태 확인하기**

```bash
## 긴 버전
$ git status
On branch develop
Your branch is up to date with 'origin/develop'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        build.gradle
        gradle/
        gradlew
        gradlew.bat
        settings.gradle
        src/
        
## 짧은 버전
## 아직 추적하지 않는 새 파일 앞에는 ?? 표시가 붙는다. 
## Staged 상태로 추가한 파일 중 새로 생성한 파일 앞에는 A 표시가, 
## 수정한 파일 앞에는 M 표시가 붙는다.

$ git status -s
A  build.gradle
A  gradle/wrapper/gradle-wrapper.jar
A  gradle/wrapper/gradle-wrapper.properties
A  gradlew
A  gradlew.bat
A  settings.gradle
A  src/main/java/com/example/demo/PerusonariDemoApplication.java
A  src/main/resources/application.properties
A  src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
?? .gitignore
```

- 위 명령어를 통해서 어떤 파일이 git에 의해 관리되고 있는 지를 확인할 수 있다.
- 파일들의 관리 내역만을 표시해줄 뿐 실제 수정 사항까지는 표시해주지 않는다.

### 파일 추가하기

```bash
## 파일들을 staged
$ git add *

The following paths are ignored by one of your .gitignore files:
HELP.md
build
hint: Use -f if you really want to add them.
hint: Disable this message with "git config advice.addIgnoredFile false"

## 이스케이프 문자인 LF가 git에서 관리를 시작한 이후에 
## 다음 번에 접근했을 때 CRLF로 바뀔 것이라는 경고.
warning: in the working copy of 'build.gradle', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'gradle/wrapper/gradle-wrapper.properties', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'gradlew', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'settings.gradle', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'src/main/java/com/example/demo/PerusonariDemoApplication.java', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'src/main/resources/application.properties', LF will be replaced by CRLF the next time Git touches it
warning: in the working copy of 'src/test/java/com/example/demo/PerusonariDemoApplicationTests.java', LF will be replaced by CRLF the next time Git touches it

## 제대로 staged되었는지 확인.
$ git status

On branch develop
Your branch is up to date with 'origin/develop'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   build.gradle
        new file:   gradle/wrapper/gradle-wrapper.jar
        new file:   gradle/wrapper/gradle-wrapper.properties
        new file:   gradlew
        new file:   gradlew.bat
        new file:   settings.gradle
        new file:   src/main/java/com/example/demo/PerusonariDemoApplication.java
        new file:   src/main/resources/application.properties
        new file:   src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
```

- **git add는 Untracked 파일을 Staged 상태로 만든다.**
- 또한 modified 상태인 파일도 staged 상태로 만들어준다.
  - 모든 변경 사항은 먼저 git add를 통해 staging area를 거쳐야 한다.
  - 그리고 **git은 이런 과정을 통해서 staged된 파일들만 인식**한다.
    - &rarr; 즉, 유저가 파일을 git add를 해주고 이후에 수정이 있다면, 다시 git add를 해줘야 한다.
      - 그렇지 않다면 가장 마지막에 git add를 한 파일의 내용이 commit된다.

### **파일 무시하기**

```bash
$ cat .gitignore ## .gitignore파일을 만듬.
*.[oa] ## 무시하고픈 형태의 파일 이름 구조를 적음.
*~
```

- .gitignore는 기본적으로 프로젝트를 생성하게 되면 어느 정도 설정이 되어있다.
- 그래서 우선은 넘어가도록 한다.

### 수정 사항 확인하기

```bash
## 현재 로컬 파일과 staged Area를 비교.
$ git diff
warning: in the working copy of 'src/test/java/com/example/demo/PerusonariDemoApplicationTests.java', LF will be replaced by CRLF the next time Git touches it
diff --git a/src/test/java/com/example/demo/PerusonariDemoApplicationTests.java b/src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
index 799c8ba..5cb4ca9 100644
--- a/src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
+++ b/src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
@@ -10,4 +10,9 @@ class PerusonariDemoApplicationTests {
        void contextLoads() {
        }

## 추가한 내용이 나온다.
+       @Test
+       void helloWorld(){
+               System.out.println("hello world");
+       }
+
 }

## staged Area와 Repository를 비교한다.
## 실제로는 이것보다 훨씬 많은 수정 사항이 한꺼번에 출력됨.
## -r 옵션은 원활한 출력을 위해 사용한다.
$ git diff --cached -r
diff --git a/build.gradle b/build.gradle
new file mode 100644
index 0000000..fe715c4
--- /dev/null
+++ b/build.gradle
@@ -0,0 +1,28 @@
+plugins {
+       id 'java'
+       id 'org.springframework.boot' version '3.3.2'
+       id 'io.spring.dependency-management' version '1.1.6'
+}
+
+group = 'com.example'
+version = '0.0.1-SNAPSHOT'
+
+java {
+       toolchain {
+               languageVersion = JavaLanguageVersion.of(17)
+       }
+}
+
+repositories {
+       mavenCentral()
:

```

- `git diff`는 Patch처럼 어떤 라인을 추가했고 삭제했는지가 궁금할 때 사용한다.
- 단 `git diff` 는 Unstaged 상태인 것들만 보여준다. statged 상태인 것을 보려면 `git diff --cahced` 를 사용해야한다.

### 커밋하기

```bash
$ git commit

## 이후 이하와 같은 데이터가 에디터에서 출력됨.
## 커밋 메세지를 적으면 된다.
## 단 주석 부분은 무시되고, 또 commit 메세지가 존재하지 않으면 
## commit은 abort(중단) 된다.
## 마지막으로 파일을 저장 후 종료하면 git이 commit을 진행한다. 

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch develop
# Your branch is up to date with 'origin/develop'.
#
# Changes to be committed:
# new file:   build.gradle
# new file:   gradle/wrapper/gradle-wrapper.jar
# new file:   gradle/wrapper/gradle-wrapper.properties
# new file:   gradlew
# new file:   gradlew.bat
# new file:   settings.gradle
# new file:   src/main/java/com/example/demo/PerusonariDemoApplication.java
# new file:   src/main/resources/application.properties
# new file:   src/test/java/com/example/demo/PerusonariDemoApplicationTests.java
#
# Untracked files:
# .gitignore
#

## 이하와 같이 커밋 메세지를 인라인으로도 처리 가능.
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
 
 
## -a 옵션을 사용하면 tracked 중인 파일을 모두 자동으로 commit한다.
no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```

### 삭제하기

```bash
## tracked 중인 경우. 파일을 완전히 삭제.
$ git rm unnessary.md
rm 'unnessary.md'

## staging area에서만 삭제.
$ git add unnessary.md
$ git rm --cached unnessary.md
rm 'unnessary.md'

```

- Git에서 파일을 제거하려면 `git rm` 명령으로 Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제하는 것) 커밋해야 한다.
- 이 명령은 워킹 디렉토리에 있는 파일도 삭제하기 때문에 **실제로 파일도 지워진다.**

### 파일명 수정하기

```bash
## mv 명령어로 이름 바꾸기.
$ git add unnessary.md
$ git mv unnessary.md modified.md

## 실제로는 mv를 했을 때 아래와 같이 코드가 실행.
$ mv README.md README
$ git rm README.md
$ git add README

## 파일을 git 이외의 에디터로 변경한 경우.
$ git status -s
RD unnessary.md -> modified.md ## 이렇게 mv명령어로 modified.md로 바꾼 파일을
A  modified_md.md ## 에디터에서 modified_md.md 파일로 바꿨더니 새로운 파일이 생성된 것처럼 취급된다.
A  "unnessary copy.md"
?? .gitignore

```

- 어떤 도구로 이름을 바꿔도 상관없다.
- 중요한 것은 이름을 변경하고 나서 꼭 rm/add 명령을 실행해야 한다는 것 뿐이다.
- 실제로 git 이외의 에디터로 파일의 이름을 바꾸는 경우에는, git이  현실적으로 git이 추적하는

## 커밋 히스토리 조회하기

### **커밋 히스토리 조회하기**

```bash
## git log를 통해서 현재 브런치의 커밋 목록을 확인하는 예시.
$ git log
commit afb50b6798f919d39526f59cb18d4894486279ce (HEAD -> 2024-10-01)
Author: perusonari <perusonari@gmail.com>
Date:   Wed Oct 2 00:21:08 2024 +0900

    아무튼 커밋함.

commit 13095b60f24bc096d1995804a86a3d22717d05f5 (tag: 2024-10-01-lw, tag: 2024-10-01)
Author: perusonari <perusonari@gmail.com>
Date:   Sat Sep 28 15:33:17 2024 +0900

    변경된 파일을 저장
    변경된 파일에 내용이 누락되어서 이를 수정함.
    앗차차 파일 명이 또 잘못됐네?

commit 3cd7c6994a3fc05ace04c0792acafc7d16c84fbc (tag: 2024-09-28)
Author: perusonari <perusonari@gmail.com>
Date:   Sat Sep 28 14:18:08 2024 +0900

    test commit

commit 45fa144321262862eb318f3c79a0b331279b0876
Author: perusonari <perusonari@gmail.com>
Date:   Thu Sep 26 22:53:18 2024 +0900

    Please enter the commit message for your changes. Lines starting
    with '#' will be ignored, and an empty message aborts the commit.

    On branch develop
    Your branch is up to date with 'origin/develop'.

    Changes to be committed:
            new file:   build.gradle
            new file:   gradle/wrapper/gradle-wrapper.jar
            new file:   gradle/wrapper/gradle-wrapper.properties
            new file:   gradlew
            new file:   gradlew.bat
            new file:   settings.gradle
            new file:   src/main/java/com/example/demo/PerusonariDemoApplication.java
            new file:   src/main/resources/application.properties
            new file:   src/test/java/com/example/demo/PerusonariDemoApplicationTests.java

    Untracked files:
            .gitignore

commit 94116383ae8525fb93cda21d6c06dd65bb095f87
Author: Jae_Sung_Bak <161293432+personari@users.noreply.github.com>
Date:   Thu Sep 26 19:56:54 2024 +0900

    Create README.md

## -p 옵션을 이용해서 커밋을 단 하나만 조회하는 예시.
$ git log -p -1
commit afb50b6798f919d39526f59cb18d4894486279ce (HEAD -> 2024-10-01)
Author: perusonari <perusonari@gmail.com>
Date:   Wed Oct 2 00:21:08 2024 +0900

    아무튼 커밋함.

diff --git a/modified.md b/modified.md
deleted file mode 100644
index d84012f..0000000
--- a/modified.md
+++ /dev/null
@@ -1 +0,0 @@
-modified
\ No newline at end of file
diff --git a/modified_md.md b/modified_md.md
deleted file mode 100644
index e69de29..0000000

## --pretty 옵션을 이용해 간단하게 체크섬과 커밋을 조회.
$ git log --pretty=oneline
afb50b6798f919d39526f59cb18d4894486279ce (HEAD -> 2024-10-01) 아무튼 커밋함.
13095b60f24bc096d1995804a86a3d22717d05f5 (tag: 2024-10-01-lw, tag: 2024-10-01) 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
3cd7c6994a3fc05ace04c0792acafc7d16c84fbc (tag: 2024-09-28) test commit
45fa144321262862eb318f3c79a0b331279b0876 Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
94116383ae8525fb93cda21d6c06dd65bb095f87 Create README.md

```

- 그 밖에도 다양한 옵션이 존재한다. 아래의 문서를 참조할 것.
  - <https://git-scm.com/book/ko/v2/Git의-기초-커밋-히스토리-조회하기>

## 되돌리기

### 되돌리기

```bash

## 나는 modified_md.md를 modified.md로 수정했다.
## 그에 따라 commit에는 modified_md.md가 저장되어 있는데, 이를 modified.md로 수정할 것이다.
$ git add modified.md
$ git status
On branch develop
Your branch is ahead of 'origin/develop' by 3 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   modified.md

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    modified_md.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        
## 수정 사항을 반영한 이후에 아래와 같이 amend해서 이전 커밋을 수정한다.        
## 이전 커밋 코멘트 또한 변경이 가능하다.
$ git commit --amend        
```

- Github Desktop에서는 amend를 사용시 이하와 같은 경고문을 출력한다.

> Are you sure you want to amend?
>
>
> At the end of the amend flow, GitHub Desktop will enable you to force push the branch to update the upstream branch. Force pushing will alter the history on the remote and potentially cause problems for others collaborating on this branch.
>
> - amend는 force push를 해야만 적용이 되는데, **해당 commit이 포함된 브런치를 사용하고 있는 사람들에게 영향이 가기 때문**에 위와 같은 메세지가 출력됨.
> - 따라서 amend는 본인이 새로 작업한 내용에만 사용하는 것이 바람직하다.

## 리모트 저장소

### **리모트 저장소**

- 원격 저장소란 소리. Git을 로컬에만 저장하지 않고 원격의 저장소를 관리할 수 있다.
  - 로컬 저장소가 remote라고 표현될 수도 있음.
- 하나의 저장소에서 여러 개의 리모트 저장소를 지니는 것이 가능하다.
  - 즉, 하나의 로컬 저장소 안에 여러 저장소들을 가져오는 것도 가능하다는 것.

```bash
## 일반적으로 git을 clone 해왔을 때의 예시.
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin ## git을 clone 하는 경우에 origin이라는 리모트 저장소가 자동으로 등록.

## remote 저장소가 여러 개인 경우의 예시.
$ cd grit
$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)`
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```

### 리모트 저장소 추가하기

```bash
## 원래 상태.
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)

## origin만 존재함.
$ git remote
origin

## https://github.com/paulboone/ticgit라는 url에 단축이름 pb로 리모트를 추가.
## 현재 로컬에 리모트 이름을 등록했을 뿐, 데이터가 가져와진 것은 아니다.
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)

## origin과 pb가 출력되는 것을 확인할 수 있음.
## 현재 ticgit이라는 저장소 안에 origin과 pb라는 이름의 리모트가 동시에 존재하고 있는 것임.
$ git remote
origin
pb

## 리모트 pb에서 데이터를 가져옴.
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
```

- git remote add는 해당 리모트를 등록할 뿐이다.
  - 브런치라던지, 구체적인 데이터는 전혀 가져와지지 않는다.
- git fetch remote를 해준 이후에, 리모트의 모든 브런치를 가져올 수 있다.
- 다만 일반적으로는 하나의 저장소당 하나의 리모트만 가져오는 것이 바람직해보인다.
  - 브런치명이 충돌하는 등의 문제가 있다.

### **리모트 저장소를 Pull 하거나 Fetch 하기**

```bash
## 특정 리모트 저장소에서 데이터를 가져오기.
## 그러나 이 명령어는 리모트 저장소의 데이터를 가져올 뿐, merge시키지는 않는다.
git fetch <remote>

## 따라서 pull명령어를 사용해서 로컬 수정 사항과 merge시켜줄 수 있다.
git pull <remote>

## 만일 업스트림(리모트의 원본)에 수정 사항을 반영하고 싶다면 push 명령어를 사용한다.
## 이 때 리모트의 이름과 브런치 이름을 적어준다.
git push origin master

```

### 리모트 저장소 살펴보기

```bash
## fetch하면 어디로 가는지, push하면 어디로 가는지.
## head 브런치는 뭔지, 어떤 브런치가 리모트에 존재하는지.
## git pull, git push의 명령어가 어떤 결과로 이어지는지 알려준다.
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
    
## 좀 더 복잡한 형태의 remote 예시.
## 현재 존재하는 브런치들이 어떤 상태인지 표시해준다.
## 또한 git pull, git push를 하는 순간 각 브런치들이 어떤 식으로 바뀔지 알려준다.
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in remotes/origin)
    issue-45                         new (next fetch will store in remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch                     (up to date)
    markdown-strip                 pushes to markdown-strip                 (up to date)
    master                         pushes to master                         (up to date)    
```

### **리모트 저장소 이름을 바꾸거나 리모트 저장소를 삭제하기**

```bash
## 리모트 저장소의 이름을 변경하는 예시.
## 로컬에서 사용하는 리모트 저장소의 별명을 바꾼다. 
$ git remote rename pb paul
$ git remote
origin
paul

## 리모트 저장소를 삭제하는 예시.
## 로컬에 존재하던 리모트 저장소 및 관련 브런치가 모두 사라진다.
$ git remote remove paul
$ git remote
origin
```

## **태그**

### **태그 조회하기**

- 태그는 커밋 단위로 저장됨. 즉 중요한 커밋을 표시하기 위해 사용함.
  - 여기서 커밋이 git 저장 기록 상에서 특정 지점임을 이해해야 함.
  - 따라서 커밋에 태그를 단다는 뜻은 **현재 작업물의 버전을 표시**해주는 것과 같음.
- 따라서 일반적으로 버전 릴리즈할 때 태그를 달아서 처리함.
  - github에서도 이러한 특징의 연장선으로 tag에서 릴리즈를 만드는 기능이 있음.

```bash
## 현재 저장소의 태그를 출력.
$ git tag
v0.1
v1.3

## 특정 태그를 골라서 불러올 수 있음.
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```

### **태그 붙이기**

- Git의 태그는 ***Lightweight*** 태그와 ***Annotated*** 태그로 두 종류가 있다.

### **Annotated 태그**

```bash
## 공식 문서의 예제.
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4

## 실제 사용 예시.
## 태그의 종류, 이름, 코멘트를 설정한다.
$ git tag -a 2024-10-01 -m "today's update"

## 태그를 조회하는 예시.
## 태그의 코멘트는 물론, 커밋한 내역들과 변경 사항 또한 보인다.
$ git show 2024-10-01
tag 2024-10-01
Tagger: perusonari <perusonari@gmail.com>
Date:   Tue Oct 1 23:10:44 2024 +0900

today's update

commit 13095b60f24bc096d1995804a86a3d22717d05f5 (HEAD -> develop, tag: 2024-10-01)
Author: perusonari <perusonari@gmail.com>
Date:   Sat Sep 28 15:33:17 2024 +0900

    변경된 파일을 저장
    변경된 파일에 내용이 누락되어서 이를 수정함.
    앗차차 파일 명이 또 잘못됐네?

diff --git a/modified.md b/modified.md
new file mode 100644
index 0000000..d84012f
--- /dev/null
+++ b/modified.md
@@ -0,0 +1 @@
+modified
\ No newline at end of file
diff --git a/unnessary.md b/modified_md.md
similarity index 100%
rename from unnessary.md
rename to modified_md.md
diff --git a/modified_modified.md b/modified_modified.md
new file mode 100644
index 0000000..d84012f
--- /dev/null
+++ b/modified_modified.md
@@ -0,0 +1 @@
+modified
\ No newline at end of file
diff --git a/unnessary copy.md b/unnessary copy.md
new file mode 100644
index 0000000..e69de29

```

### **Lightweight 태그**

```bash
## 공식 문서의 예제.
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw ## 추가된 태그.
v1.5

## 실제 사용 예시.
## lightweight 태그는 별명만 지정한다.
$ git tag 2024-10-01-lw

$ git tag
2024-10-01
2024-10-01-lw

## 커밋된 내용이 표시되는 것은 다른 태그와 동일하다.
## 그러나 태그를 단 사람과 태그가 달린 시간은 정확히 표시되지 않는다.
$ git show 2024-10-01-lw
commit 13095b60f24bc096d1995804a86a3d22717d05f5 (HEAD -> develop, tag: 2024-10-01-lw, tag: 2024-10-01)
Author: perusonari <perusonari@gmail.com>
Date:   Sat Sep 28 15:33:17 2024 +0900

    변경된 파일을 저장
    변경된 파일에 내용이 누락되어서 이를 수정함.
    앗차차 파일 명이 또 잘못됐네?

diff --git a/modified.md b/modified.md
new file mode 100644
index 0000000..d84012f
--- /dev/null
+++ b/modified.md
@@ -0,0 +1 @@
+modified
\ No newline at end of file
diff --git a/unnessary.md b/modified_md.md
similarity index 100%
rename from unnessary.md
rename to modified_md.md
diff --git a/modified_modified.md b/modified_modified.md
new file mode 100644
index 0000000..d84012f
--- /dev/null
+++ b/modified_modified.md
@@ -0,0 +1 @@
+modified
\ No newline at end of file
diff --git a/unnessary copy.md b/unnessary copy.md
new file mode 100644
index 0000000..e69de29
```

### 나중에 태그하기

```bash
## 커밋 히스토리를 확인한다.
$ git log --pretty=oneline
13095b60f24bc096d1995804a86a3d22717d05f5 (HEAD -> develop, tag: 2024-10-01-lw, tag: 2024-10-01) 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
3cd7c6994a3fc05ace04c0792acafc7d16c84fbc test commit
45fa144321262862eb318f3c79a0b331279b0876 Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
94116383ae8525fb93cda21d6c06dd65bb095f87 Create README.md

## 이 때 태그는 기존에 존재하지 않는 태그이다.
## 또한 태그가 커밋 단위로 적용된다는 것을 알 수 있다.
$ git tag -a 2024-09-28 3cd7c6

## 새로 추가된 태그를 확인.
$ git tag
2024-09-28
2024-10-01
2024-10-01-lw

## 새로 추가된 태그의 내용을 확인.
$ git show 2024-09-28
tag 2024-09-28
Tagger: perusonari <perusonari@gmail.com>
Date:   Tue Oct 1 23:42:56 2024 +0900

2024-09-28에 대한 태그.

commit 3cd7c6994a3fc05ace04c0792acafc7d16c84fbc (tag: 2024-09-28)
Author: perusonari <perusonari@gmail.com>
Date:   Sat Sep 28 14:18:08 2024 +0900

    test commit

diff --git a/unnessary.md b/unnessary.md
new file mode 100644
index 0000000..e69de29

```

### **태그 공유하기**

```bash
## 먼저 리모트 상의 저장소를 불러와서 로컬의 저장소와 merge시켜줌.
$ git pull
Merge made by the 'ort' strategy.

## 리모트 상의 저장소에 push.
$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 561 bytes | 561.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/perusonari/perusonari.git
   719650b..7388f74  develop -> develop

## 리모트 상에 태그를 push.
## 커밋과 태그는 별도의 개념이기 때문에 리모트에 따로 태그를 push해줘야함.
$ git push origin 2024-10-01
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 166 bytes | 166.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/perusonari/perusonari.git
 * [new tag]         2024-10-01 -> 2024-10-01
 
## 로컬에 저장된 모든 태그를 push하는 예시.
$ git push origin --tags
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 183 bytes | 183.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/perusonari/perusonari.git
 * [new tag]         2024-09-28 -> 2024-09-28
 * [new tag]         2024-10-01-lw -> 2024-10-01-lw

```

- 리모트가 만일 origin이 아닌 경우 어떤 일이 벌어질지에 대해서도 생각해봐야할 것 같음.

### **태그를 Checkout 하기**

```bash
## 2024-10-01으로 태그된 커밋을 체크아웃.
## detached HEAD 상태가 되었다고 언급 됨.
## => 기존 git의 흐름에서 독립해서 완전히 별개의 흐름에 들어서게 된 것임.
## 그에 따라서 기존 깃에 영향이 가지 않음.
$ git checkout 2024-10-01
Note: switching to '2024-10-01'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 13095b6 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
D       modified.md
D       modified_md.md

## 그러나 위와 같은 경우 커밋을 관리하기 까다로우므로 -b 옵션을 이용해서
## 아래와 같이 새로운 브런치를 생성한 뒤 작업하는 것이 적절함.
$ git checkout -b 2024-10-01 2024-10-01
Switched to a new branch '2024-10-01'

```

## 명령어 별명 만들기

```bash
## global 설정을 통해 alias를 설정해줌.
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status

## 실제로 사용하는 예시.
$ git config --global alias.unstage 'reset HEAD --'

## 아래의 명령어는 동일하다.
$ git unstage fileA
$ git reset HEAD -- fileA
```

## **요약**

```bash
## 저장소 관련.

 ## 저장소 생성하기. 해당 디렉토리를 저장소로 지정함.
 $ git init
 
 ## 저장소 상태 확인하기
 $ git status
 $ git status -s ## 짧은 버전.
 
 ## 현재 변경 사항을 tracked 상태로 바꾸고, staged area에 저장.
 $ git add .
 
 ## 수정 사항 확인. 원활한 출력을 위해 -r 옵션을 사용.
 $ git diff -r ## working directory => staged area
 $ git diff --cached -r ## staged area => repository
 
 ## 커밋하기.
 $ git commit ## 일반적인 커밋.
 $ git commit -m 'added new benchmarks' ## 인라인으로 커밋 메세지 작성.
 $ git commit -a -m 'added new benchmarks' ## 현재 tracked 상태인 파일을 모두 커밋.

## 파일 삭제 및 변경 사항 확인

 ## 파일 삭제하기.
 $ git rm unnessary.md ## 현재 working directory에서 삭제.
 $ git rm --cached unnessary.md ## 현재 staged area에서 삭제.
 
 ## commit 내역을 조회. 
 $ git log ## commit과 상세 내역을 조회.
 $ git log -p -1 ## 가장 최근에 한 commit을 조회.
 $ git log --pretty=oneline ## commit을 간략하게 조회.
 
 ## 가장 최근의 커밋 수정하기. 
 $ git commit --amend 

## 리모트 저장소 관련.
## 리모트 저장소는 Repository이다. 리모트 저장소의 내부에는 여러 개의 브런치가 있다.

 ## 리모트 저장소를 복사해오기.
 $ git clone https://github.com/schacon/ticgit
 
 ## 해당 저장소의 리모트 저장소를 조회
 $ git remote
 $ git remote -v ## -v 옵션을 이용해 단축 이름과 url 동시에 보기.
 
 ## 리모트 저장소 이름을 추가하기. (*실제로 데이터까지 들어오진 않음.)
 $ git remote add pb https://github.com/paulboone/ticgit
 
 ## 특정 리모트 저장소에서 데이터를 가져오기.
 $ git fetch pb ## pb라는 별명의 리모트 저장소의 데이터만 동기화.
 $ git pull pb ## pb라는 별명의 리모트 저장소의 데이터를 로컬 데이터에 merge
 
 ## 만일 업스트림(리모트의 원본)에 수정 사항을 반영하고 싶다면 push 명령어를 사용한다.
 ## 이 때 리모트의 이름과 브런치 이름을 적어준다.
 $ git push origin master
 
 ## 좀 더 복잡한 형태의 remote 예시.
 ## 현재 존재하는 브런치들과 git pull, git push를 하는 순간 각 브런치들이 어떤 식으로 바뀔지 알려준다.
 ## 일반적으로 리모트 저장소의 기본 이름은 origin임
 $ git remote show origin

## 태그 관리

 ## 태그 확인.
 $ git tag ## 태그 확인하기.
 $ git tag -l "v1.8.5*" ## 조건부로 태그 확인하기.
 
 ## 태그 만들기.
 ## 태그의 종류, 이름, 코멘트를 설정한다.
 $ git tag -a 2024-10-01 -m "today's update" ## annotated 태그.
 $ git tag 2024-10-01-lw ## lightweight 태그. 
 $ git tag -a 2024-09-28 3cd7c6 ## 나중에 lightweight 태그를 달아주는 예시. 뒤의 값은 체크섬.
 
 ## 태그를 조회하는 예시.
 ## 태그의 코멘트는 물론, 커밋한 내역들과 변경 사항 또한 보인다.
 $ git show 2024-10-01 
 
 ## 태그 푸시하기.
 $ git push ## 태그를 푸시하기 전에는 반드시 데이터를 푸시해줄 것.
 $ git push origin 2024-10-01 ## origin 리모트에 태그를 푸시.
 $ git push origin --tags ## 현재 저장소에 존재하는 모든 태그를 푸시.
 
 ## 태그 체크아웃.
 $ git checkout 2024-10-01 ## 현재 저장소에 존재하는 태그를 체크아웃.
 $ git checkout -b 2024-10-01 2024-10-01 ## 브런치를 생성 후 태그를 체크아웃 하는 예시.

```

# 챕터 3

## **브랜치란 무엇인가**

- **코드를 통째로 복사하고 나서 원래 코드와는 상관없이 독립적으로 개발.** 그리고 이 단위를 **“브랜치”라고 칭한다.**
- Git은 브랜치를 만들어 작업하고 나중에 Merge 하는 방법을 권장한다.
- 최초 커밋을 제외한 나머지 커밋은 이전 커밋 포인터가 적어도 하나씩 있고 브랜치를 합친 Merge 커밋 같은 경우에는 이전 커밋 포인터가 여러 개 있다.
- **이 때 참조 중인 커밋을 HEAD라고 표현한다.**

### 깃의 데이터 저장 방식

![commit-and-tree.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/commit-and-tree.png)

- **커밋 개체의 구조**
  - 각 파일의 blob파일
  - 파일과 디렉토리 구조가 들어있는 트리 개체.
  - 메타데이터와 루트 트리를 가리키는 포인터가 담긴 커밋 개체 하나이다.
- ⇒ 각 파일의 blob을 전부 생성한 다음에 트리 개체에 담아서 표현하고, 그 뒤 커밋 개체가 이를 참조하는 주소와 메타데이터를 포함.

![commits-and-parents.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/commits-and-parents.png)

- 각 커밋 개체는 이전에 존재하던 커밋 개체를 참조한다.
- Git의 브랜치는 커밋 사이를 가볍게 이동할 수 있는 어떤 포인터 같은 것이다.
  - 정확하게 말해서 브랜치는 특정 시점의 커밋을 가리키는 포인터이다.
  - 기본적으로 develop 혹은 master와 같은 기본 브랜치가 있고, 이 브랜치는 최신의 commit을 참조한다.
  - 브랜치를 만들게 되면 최신의 commit을 참조하는 새로운 포인터가 생성된다.
- 그러나 브랜치와 commit은 기본적으로 다르다.
  - commit은 브랜치와 별개로 존재할 수 있고, 실질적으로 스냅샷을 저장하고 있다.
  - 반면 브랜치는 commit이 존재해야만 생성할 수 있으며, HEAD가 참조할 수 있게 commit의 주소를 제공한다.

### **새 브랜치 생성하기**

![head-to-master.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/head-to-master.png)

```bash
## 브런치를 만든다.
## 그러나 브런치를 만들더라도 위와 같이 HEAD는 바뀌지 않는다.
$ git branch testing
```

### **브랜치 이동하기**

![head-to-testing.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/head-to-testing.png)

```bash
## testing이라는 이름의 브런치로 이동.
$ git checkout testing

## vim을 사용하는 순간 vim 에디터로 넘어가게 되니깐 주의할 것.
$ vim test.rb

## vim으로 작성한 파일을 저장해줌.
$ git add test.rb

## vim으로 만들어낸 파일을 바로 commit함.
$ git commit -a -m 'made a change'

## 다시 원래 develop 브런치로 복귀.
$ git checkout develop
```

- **브랜치를 이동하면 워킹 디렉토리의 파일이 변경된다**
- 브랜치를 이동한다는 말은 지정해놓은 commit으로 이동한다는 뜻이다.
  - 따라서 해당 commit의 스냅샷으로 워킹 디렉토리의 데이터가 바뀌는 것이다.

### 갈라지는 브랜치

![advance-master (1).png](Git%2010d95de8ee3a800787ceff9a6b6c5496/advance-master_(1).png)

- 각각 develop 및 testing 브랜치에서 commit을 했으므로 위와 같은 형태가 된다.

```bash
## 아래에 브랜치가 어떤 형태로 구성되어있는지를 보여줌.
## *이 커밋이고, |가 분기의 역할을 함.
$ git log --oneline --decorate --graph --all
* 1a248de (HEAD -> testing) modified more at testing
* 0c624ab test commit
| * 8ab532c (develop) add file to develop
|/
*   7388f74 (origin/develop, origin/HEAD) Merge branch 'develop' of https://github.com/perusonari/perusonari into develop
|\
| * 719650b 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함.
* | 13095b6 (tag: 2024-10-01-lw, tag: 2024-10-01) 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
|/
* 3cd7c69 (tag: 2024-09-28) test commit
* 45fa144 Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
* 9411638 Create README.md

## develop 브랜치로 이동.
$ git checkout develop
Switched to branch 'develop'
Your branch is ahead of 'origin/develop' by 1 commit.
  (use "git push" to publish your local commits)

## develop 브랜치에 testing을 merge
$ git merge testing
Merge made by the 'ort' strategy.
 test.rb | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 test.rb

## merge이후 변화한 그래프를 확인.
$ git log --graph --oneline
*   2b7bac1 (HEAD -> develop) Merge branch 'testing' into develop
|\
| * 1a248de (testing) modified more at testing
| * 0c624ab test commit
* | 8ab532c add file to develop
|/
*   7388f74 (origin/develop, origin/HEAD) Merge branch 'develop' of https://github.com/perusonari/perusonari into develop
|\
| * 719650b 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함.
* | 13095b6 (tag: 2024-10-01-lw, tag: 2024-10-01) 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
|/
* 3cd7c69 (tag: 2024-09-28) test commit
* 45fa144 Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
* 9411638 Create README.md
****
```

## **브랜치와 Merge 의 기초**

### **브랜치와 Merge 의 기초**

![basic-branching-6.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/basic-branching-6.png)

- 위와 같이 commit의 분기가 갈라진 상황일 때, master와 iss53을 merge하면?
  - ⇒ master와 iss의 최신 commit인 C4와 C5를 합치게 된다.
    - 그러나 C4와 C5는 공통 조상인 C2를 지니고 있다.
    - ⇒ 그에 따라서 공통 조상인 C2에 C4와 C5를 커밋하는 식으로 머지가 이루어짐(3-way merge)

### **충돌의 기초**

```bash
## testing을 develop에 merge하는데 실패 함. 
$ git merge testing
Auto-merging modified_md.md
CONFLICT (content): Merge conflict in modified_md.md
Automatic merge failed; fix conflicts and then commit the result.

## Unmerged paths가 존재. >> modified_md.md 
$ git status
On branch develop
Your branch is ahead of 'origin/develop' by 5 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   modified_md.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore

no changes added to commit (use "git add" and/or "git commit -a")

## bash에 MERGING이라는 단어가 보임.
Koitsu@DESKTOP-0PDGSVH MINGW64 ~/perusonari (develop|MERGING)

## mergetool을 사용하기로 했을 경우. 편집을 
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc codecompare smerge emerge vimdiff nvimdiff
Merging:
modified_md.md

Normal merge conflict for 'modified_md.md':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (vimdiff):
4 files to edit

<<<<<<< HEAD ## 아래가 merge 받는 브런치의 파일 상태.
"it's develop"
=======
"it's testing"
>>>>>>> testing ## 위가 merge 되는 브런치의 파일 상태.

## 상태를 확인해보면 confilct가 해소된 것을 확인할 수 있다.
$ git status
On branch develop
Your branch is ahead of 'origin/develop' by 5 commits.
  (use "git push" to publish your local commits)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        new file:   .gitignore
        modified:   modified_md.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        modified_md.md.orig

## merge commit하기
$ git commit -a -m "merge commit"
[develop 8407eeb] merge commit

## 이후에 확인해보면 merge가 잘 된 것을 확인할 수 있다.
*   8407eeb (HEAD -> develop) merge commit
|\
| * 2c881ee (testing) gitignore 추가
| * 9ad30eb modified_md을 수정
* | ff4c99c 메인 브런치 커밋
* | 2b7bac1 Merge branch 'testing' into develop
|\|
| * 1a248de modified more at testing
| * 0c624ab test commit
* | 8ab532c add file to develop
|/
*   7388f74 (origin/develop, origin/HEAD) Merge branch 'develop' of https://github.com/perusonari/perusonari into develop
|\
| * 719650b 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함.
* | 13095b6 (tag: 2024-10-01-lw, tag: 2024-10-01) 변경된 파일을 저장 변경된 파일에 내용이 누락되어서 이를 수정함. 앗차차 파일 명이 또 잘못됐네?
|/
* 3cd7c69 (tag: 2024-09-28) test commit
* 45fa144 Please enter the commit message for your changes. Lines starting with '#' will be ignored, and an empty message aborts the commit.
* 9411638 Create README.md

```

- 3-way Merge가 실패할 때도 존재한다.
  - 두 브런치가 동일한 파일을 수정했을 때 발생.
  - 이 경우 HEAD 브런치(merge 받는 브런치)와 MERGE 되는 브런치가 존재한다.
  - ⇒ HEAD와 merge 되는 브런치를 비교해서 보여준다 따라서 둘 중 하나를 선택하거나 적절하게 수정해줘야 한다.
- merge도 일종의 커밋이다. 따라서 merge로 생성된 커밋을 참조하게 된다.

## 브랜치 관리

### 브랜치 관리

```bash
$ git branch
* develop
  testing

## 브런치의 마지막 커밋을 확인.   
$ git branch -v
* develop 8407eeb [ahead 8] merge commit
  testing 2c881ee gitignore 추가  

## develop 브런치에서 봤을 때 merged된 브런치를 조회함.
## *이 없다면 삭제해도 무방한 상태.  
Koitsu@DESKTOP-0PDGSVH MINGW64 ~/perusonari (develop)
$ git branch --merged
* develop
  testing

## 반면 testing 브런치에서 merged와 no-merged 옵션을 실행해보면 
## develop이 아직 merged되지 않았고, 자신만 merged된 상태임을 확인할 수 있다.
Koitsu@DESKTOP-0PDGSVH MINGW64 ~/perusonari (testing)
$ git branch --merged
* testing

$ git branch --no-merged
  develop
  
## 작업이 끝나 불필요한 testing 브런치를 삭제한다.  
Koitsu@DESKTOP-0PDGSVH MINGW64 ~/perusonari (develop)
$ git branch -d testing
Deleted branch testing (was 2c881ee).
  

```

## **브랜치 워크플로**

### **브랜치 워크플로**

- 브런치를 나눠서 어떤 이득을 볼 수 있을까?
- 브랜치 워크플로를 잘 설정하면 보다 안정적인 관리가 가능.

### **Long-Running 브랜치**

![lr-branches-1.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/lr-branches-1.png)

- master, develop, topic으로 브런치 종류를 3단계로 분리한다.
  - master는 가장 안정화된 버전.
  - develop은 안정화 중이거나 테스트 중인 버전.
  - topic은 상세 기능 개발에 사용한다.
- develop및 topic 브런치가 앞길을 닦아놓으면 master 브런치가 조금씩 따라가는 식임.

## **리모트 브랜치**

### **리모트 브랜치**

```bash
## 브런치 생성.
$ git branch issue

## 브런치 생성 확인.
$ git branch
* develop
  issue

## 리모트 origin에 로컬 브런치 issue를 push.
## 이 과정에서 리모트 브런치가 생성됨.
$ git push origin issue
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote:
remote: Create a pull request for 'issue' on GitHub by visiting:
remote:      https://github.com/perusonari/perusonari/pull/new/issue
remote:
To https://github.com/perusonari/perusonari.git
 * [new branch]      issue -> issue
 

## 리모트 origin에 대한 정보를 조회.
## develop 브런치와 issue 브런치가 현재 tracked상태임을 확인할 수 있음.
## 또한 리모트의 HEAD 브런치(디폴트)가 develop임을 확인할 수 있다.
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/perusonari/perusonari.git
  Push  URL: https://github.com/perusonari/perusonari.git
  HEAD branch: develop
  Remote branches:
    develop tracked
    issue   tracked
  Local branches configured for 'git pull':
    develop merges with remote develop
    issue   merges with remote issue
  Local refs configured for 'git push':
    develop pushes to develop (up to date)
    issue   pushes to issue   (up to date) 
    
## 리모트 브런치와 로컬 브런치의 차이점을 확인하기 위해 issue 브런치를 삭제.
$ git branch -d issue
Deleted branch issue (was 902592d).

## 로컬 브런치의 상태. 
$ git branch
* develop

## 리모트 브런치의 상태. issue가 삭제되었지만 여전히 존재함.
$ git branch -r
  origin/HEAD -> origin/develop
  origin/develop
  origin/issue

## 아래와 같이 fetch를 하더라도 로컬 브런치는 변하지 않는 것을 확인할 수 있음.
$ git fetch origin

## 리모트 브랜치를 가져오는 예시(단축 버전).
$ git checkout issue
Switched to a new branch 'issue'
branch 'issue' set up to track 'origin/issue'.

## 리모트 브랜치를 가져오는 예시 (원래 버전.)
$ git checkout --track origin/issue
Switched to a new branch 'issue'
branch 'issue' set up to track 'origin/issue'.

## 리모트 브랜치를 로컬에 가져왔다면 리모트 브랜치를 추적할 수 있어야한다.
## 추적 여부는 -vv로 확인할 수 있다.
$ git branch -vv
  develop 902592d [origin/develop] 중간 저장
* issue   902592d [origin/issue] 중간 저장

```

- 리모트 상에 존재하는 브랜치.
- 로컬의 브런치와 대비되는 개념이다.
  - 로컬의 브런치를 리모트에 추가할 수 있다.
  - 리모트의 브런치를 로컬에 가져오는 것도 가능하다.
  - 이 때 리모트는 여럿일 수 있음을 감안할 것.
    - 그러나 일반적인 상황은 아님.
- git은 리모트 상의 브런치와 로컬 상의 브런치를 엄격하게 구분한다.
- 리모트 개념에 대해서 명백하게 짚고 넘어가야한다.
  - 하나의 git 저장소는 여러 개의 리모트를 가질 수 있다.
    - 그러나 git 저장소는 “하나의 프로젝트”를 전제로 만든다.
    - 서로 다른 프로젝트를 리모트에 등록하고 브런치에 등록하는 것도 문제가 없겠지만, 이는 부적절할 것.

## Rebase 하기

### Rebase의 기초

![basic-rebase-3.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/basic-rebase-3.png)

- Reapply commits on top of another base tip
  - 다른 베이스의 끝에 commit들을 재 적용한다.
  - 간단하게 말해서 base가 된 커밋을 시작으로 해서, 그 부분부터 다른 커밋을 적용한다는 뜻.
- Rebase는 해당 브런치들을 순서대로 patch하는 식으로 동작한다.
  - 즉, C2로 돌아가서, C4를 최상위 작업에 패치해버리는 것이다.
  - 변경 사항이 C2, C3에 모두 적용된다.
- Rebase는 보통 리모트 브랜치에 커밋을 깔끔하게 적용하고 싶을 때 사용한다.
  - 근본적으로 커밋 히스토리를 정리하기 위함.

### **Rebase 활용**

![interesting-rebase-2.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/interesting-rebase-2.png)

- $ git rebase --onto master server client
- 위와 같은 상황에서 C8, C9를 반영하고 싶을 때에 —onto 옵션을 사용한다.
  - 저렇게 되면 C3의 하위 커밋들이 모두 master쪽에 반영 됨.

### Rebase 의 위험성

![perils-of-rebasing-4.png](Git%2010d95de8ee3a800787ceff9a6b6c5496/perils-of-rebasing-4.png)

- 공개 저장소에 push한 커밋은 절대로 rebase하면 안 된다.
  - 기본으로 rebase해서 생긴 commit은 원본과는 다름.
  - 그렇기에 다른 사람이 그 커밋을 사용하고 있다면, rebase했을 때, 그 사람은 커밋 히스토리에 동일한 커밋이 2개 존재하게 됨.
- 위 그림은 rebase를 했을 시에 발생하는 안 좋은 상황을 묘사하고 있음.
  - C4와 C4’가 동시에 존재함을 확인할 수 있음.

### **Rebase 한 것을 다시 Rebase 하기**

![perils-of-rebasing-5 (1).png](Git%2010d95de8ee3a800787ceff9a6b6c5496/perils-of-rebasing-5_(1).png)

- 역으로 이 상황에서 공개 저장소의 C4’를 써서 리베이스 하는 것도 가능하다.
  - 현재 브랜치에만 포함된 커밋을 결정한다. (C2, C3, C4, C6, C7)
  - Merge 커밋이 아닌 것을 결정한다. (C2, C3, C4)
  - 이 중 merge할 브랜치에 덮어쓰이지 않은 커밋을 결정한다.
    - ⇒ C2, C3. C4는 C4’와 동일한 Patch이므로 생략.
- 결과적으로 위와 같이 복잡하던 커밋 히스토리가 정리된다.
