---
layout: post
title:  "Django_pyenv-virtualenv를 사용한 개발환경 구성"
comments: true
description: "Django, pyenv-virtualenv"
author: SeungHyeon Tak
date:   2019-10-15 23:40:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django, pyenv-virtualenv, python"
---
#### Django - pyenv-virtualenv

#### 설치 전 필요 과정
##### mac OS
<br>
> `homebrew` 설치 <br>
> homebrew는 macOS를 위한 패키지 관리자이다. <br>
> 설치 명령어를 터미널에 입력한다.

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

<br>
> `pyenv-virtualenv` install <br>
> brew를 이용해 pyenv와 pyenv-virtualenv를 설치 <br>

```
$ brew install pyenv
$ brew install pyenv-virtualenv

//(밑의 부분에서 zsh를 사용할 경우 ~/.bash_profile 부분을 ~/.zshrc 로 변경)
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
```

<br>
> pyenv용 virtualenv 설치하기 <br>
> virtualenv는 프로젝트마다 독립된 환경(python 버전과 module 설치상태)을 구성할 수 있게 해 주는 툴입니다. <br>

```
$ brew install pyenv-virtualenv
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```

> pyenv와 virtualenv로 가상환경을 제어

```
$ pyenv install 3.7.4
$ pyenv virtualenv <python version> <virtualenv-name>  # 가상환경 생성
$ pyenv activate <virtualenv-name>  # 가상환경 activate
$ pyenv deactivate  # 가상환경 deactivate
$ pyenv uninstall <virtualenv-name> #가상환경 삭제
```

> 프로젝트 디렉토리에 가상환경 `autoenv` 적용하기 <br>
> autoenv는 특정 디렉터리에 진입하면 해당 디렉터리에서 필요한 가상환경을 자동으로 activate 해주는 툴입니다. <br>

```
$ cd 디렉토리이름 # 프로젝트 디렉터리로 이동
$ pyenv local 가상환경 이름 # 이전에 생성한 RC라는 가상환경을 해당 디렉터리에 설정하면 끝!

```
*****

##### Ubuntu
<br>
> 패키지 관리자 없데이트 및 설치된 패키지 업데이트

```
$ sudo apt update
$ sudo apt dist-upgrade

$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
// 만약, 설치가 안된다면 `sudo apt install git curl` 명령어로 git rhk curl 패키지를 설치해야함
```

(작성중 ..)
