---
title: 포크한 레포지토리 업데이트하기
date: 2025-09-25
categories: [Git]
tags: [Git, Github]
description: 포크해서 포스팅을 이미 하고 있었는데 블로그 테마의 업데이트를 해야하는 경우에 어떻게 해볼 수 있는지를 다룹니다.
toc: false
comments: false
math: true
mermaid: false
---

# 문제 상황

저는 이 블로그를 [cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)라는 곳에서 포크해서 쓰고 있습니다. 그런데 어느날 새로운 업데이트가 나왔고 main 브랜치가 46 커밋 앞에 있고 16개 정도 쯤 블로그 테마의 레포 브랜치보다 뒤에 있다고 하더라구요.

![상황 설명](/assets/post_assets/branch.png)

여기서 기능 업데이트를 반영하려면 무조건 지금 했던 커밋들을 취소하거나, jekyll-theme-chirpy 쪽으로 제 커밋을 반영하도록 Pull Request를 열어야 하는 상황입니다. 후자는 말이 안되죠.  

# 해결 방법

우선은 새로운 브랜치를 만듭니다. 저는 dev라는 브랜치를 만들었습니다.

이 브랜치는 Github 홈페이지의 제 레포지토리 화면에서 만드는 게 아니라, 로컬 환경에서 만들어야 합니다. 예를 들어 다음과 같습니다. 제 블로그 레포지토리를 로컬에 클론하고, 로컬에서 클론된 폴더로 이동 후 `git checkout -b dev`를 실행하여 `dev`라는 로컬 브랜치를 만듭니다. 여기까지 하면 다음과 같습니다.

![로컬 브랜치](/assets/post_assets/local-branch.png)

이후엔 `git remote add upstream git@github.com:cotes2020/jekyll-theme-chirpy.git`을 실행합니다.

만약 `dev`라는 브랜치를 Github 홈페이지에서 바로 만드는 경우는 다음과 같습니다.

![nono](/assets/post_assets/notthis.png)

저의 경우는 뭣모르고 처음에 이렇게 했습니다. 이렇게 하고 Github 페이지에서 `dev` 브랜치로 전환 후 Discard 46 commits. 를 클릭해서 `dev`브랜치에 대해 클론한 초기 상태로 돌려놓은 걸 끝냈었습니다. 이렇게 되면 로컬에서 `dev`브랜치를 만들어서 upstream에 제 Github의 `dev` 브랜치를 연결해야 합니다.  

현재 원격(Github)의 `dev`브랜치는 `cotes2020/jekyll-theme-chirpy`의 상태와 같습니다. 처음부터 로컬의 `dev` 브랜치를 만들어서 `cotes2020/jekyll-theme-chirpy`를 upstream으로 등록하고 merge를 했으면 굳이 이렇게 두 번 연결하는 상황은 없었을 것 같습니다.

# 사용한 커맨드 정리

## `git branch -a`

로컬과 (연결된 원격이 존재할 경우) 원격 레포지토리에 존재하는 모든 브랜치를 보여주며, 현재 브랜치도 화살표로 보여줍니다.

## `git checkout -b [branch]`

`-b` 옵션을 붙이면 해당 인자값을 이름으로 가지는 브랜치가 새로 만들어지며 그 브랜치로 이동합니다.

## `git remote add [alias] [remote-url]`

현재 로컬 브랜치에 원격 저장소를 연결합니다. 저의 경우엔 원격 저장소 URL이 `git@github.com:cotes2020/jekyll-theme-chirpy.git`이었는데, 이게 너무 길다보니 alias를 붙이게 됩니다. 보통 이 상황에서 alias는 `upstream`로 지정하는 것이 관행입니다.

{% capture callout_content %}

`origin`, `master`, `main`과 같은 이름을 종종 보게 됩니다. 우선 `origin`은 제 레포지토리를 말합니다. 가령 이 블로그 레포지토리의 `origin`은 `https://github.com/dlwoals256/dlwoals256.github.io`가 됩니다. 즉, alias 입니다. 다시 말해,

```

git push origin main
git push https://github.com/dlwoals256/dlwoals256.github.io main

```

위 두 명령은 같습니다.

`master`과 `main`은 레포지토리의 중심 브랜치, 기본 브랜치를 말합니다. 두 개는 서로 같은 의미이며, 비교적 최근에 `master`에서 `main`이라는 이름으로 바뀌었다보니, 좀 옛날 레포지토리는 `master`라는 이름을 기본 브랜치로 쓰는 레포지토리가 많습니다. 현재는 main을 기본 브랜치로 쓰는 것이 표준입니다.  

여담이지만, `master`라는 이름이 주종관계를 나타내는 것 같다 하여 바뀌었다고 합니다.

{% endcapture %}

{% include callout.html title="`master` 브랜치, `main` 브랜치, `origin`?" type="note" content=callout_content %}

{% capture callout_content %}

`fetch`와 `pull`은 무슨 차이가 있을까요? 

{% endcapture %}

{% include callout.html title="`fetch`와 `pull`의 차이? (+`rebase`)" type="note" content=callout_content %}
