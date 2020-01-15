---
layout: post
title:  "Django pyenv, virtualenv 가상환경 설정하기"
comments: true
description: "Django pyenv"
author: SeungHyeon Tak
date:   2020-01-15 11:22:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django pyenv"
---
## Django pyenv, virtualenv 가상환경 설정

설정 os 

* Mac
* Ubuntu

### 설치하기

#### MAC

Mac은 homebrew를 우선 설치해서 pyenv의 설치를 돕도록 한다.

```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)”
```

homebrew로 pyenv와 pyenv-virtaulenv 설치

```bash
$ brew install pyenv
$ brew install pyenv-virtualenv
```

#### Linux

```bash
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

설치가 완료되었다면 pyenv --version 을 통해서 해당 버전을 알 수 있다.


#### pyenv 관련 설정

위의 설치가 성공되었다면 vim을 통해 확인할것이 있다.

지금 zsh을 쓰고 있기때문에 zsh로 설명 하겠다.

터미널에서 `vim ~/.zshrc`로 접속한다.

맥과 리눅스 둘다 똑같은 명령어다.

```bash
export PATH="$HOME/.pyenv/bin/:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

터미널을 통해 열었는데 맨 밑에 해당 코드가 없으면 입력해주길 바란다. (있는데 또 입력하면 안됨)

이후 vim을 종료 하고

아래의 명령어를 입력해준다.

#### Mac

```bash
$ export PYENV_ROOT=/usr/local/var/pyenv

$ if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi

$ if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

#### Linux

```bash
$ export PATH="$HOME/.pyenv/bin:$PATH"

$ eval "$(pyenv init -)"

$ eval "$(pyenv virtualenv-init -)"
```

#### pyenv 설치

지금 설치할 버전은 python 3.6.4 버전이다.

```bash
$ pyenv install 3.6.4
```

#### 가상환경 이름 생성하기

```bash
$ pyenv virtualenv 3.6.4 EX
```

* 3.6.4 부분에는 python version을 입력
* EX부분에는 사용자가 입력한 가상환경 이름을 입력하면 된다.(아무 이름이나 본인이 알수 있을 만한걸로 적으면 됨)

#### 가상환경 지정하기

가상환경을 생성하고 특정 디렉토리를 가상환경으로 지정해야한다.

```bash
$ pyenv local EX
```

해당 작업이 끝나면 터미널 앞부분에 (EX)가 생겼을것이다.

이렇게 되면 가상환경 지정에 성공하게 된것이다.

#### 가상환경 삭제

```bash
$ pyenv uninstall EX
```

만약 가상환경을 잘못 만들었다면 위의 명령어로 지워줄 수 도 있다.

#### 가상환경 해제

```bash
$ pyenv deactivate
```

가상환경을 지정했으면 해제 할 수도 있다.