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

$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev

// pyenv
// .bash_profile -> .zshrc (*만약 zsh을 사용할 경우임!!)
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv

$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile

// 변경한 ~/.bash_profile을 적용하기 위해 source명령을 실행해주고 pyenv설치가 잘 됐는지 확인해본다.

$ source ~/.bash_profile
$ pyenv versions
// system (set by /home/nelp/.pyenv/version)

//이제 원하는 python버전을 설치하고 사용
$ pyenv install 3.5.2
//Downloading Python-3.5.2.tar.xz...
//-> https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tar.xz
//Installing Python-3.5.2...
//patching file Lib/venv/scripts/posix/activate.fish
//Installed Python-3.5.2 to /home/nelp/.pyenv/versions/3.5.2

$ pyenv versions
// system (set by /home/nelp/.pyenv/version)
//  3.5.2

$ pyenv shell 3.5.2
$ python
Python 3.5.2 (default, Jun  4 2017, 05:30:18)
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.

// pyenv-virtualenv
$ git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
$ source ~/.bash_profile

$ pyenv virtualenv 3.5.2 test-env
$ pyenv versions
//  system
// 3.5.2
//  3.5.2/envs/test-env
//  test-env (set by PYENV_VERSION environment variable)

$ pyenv activate test-env
//(test-env) $ pyenv versions
//  system
//  3.5.2
//  3.5.2/envs/test-env
* test-env (set by PYENV_VERSION environment variable)
// (test-env) $ python -V
// Python 3.5.2

$ pyenv deactivate //pyenv 삭제

// autoenv
// autoenv를 설정하면 특정 디렉토리로 이동했을 때 자동으로 특정 가상환경으로 activate 되도록 할 수 있다.

$ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
$ echo 'source ~/.autoenv/activate.sh' >> ~/.bash_profile
$ source ~/.bash_profile

$ mkdir test-dir
$ cd test-dir
$ touch .env
$ echo "pyenv activate test-env" > .env

$ cd test-dir/
```

<br>

이렇게 pyenv / pyenv-virtualenv 설치하는법을 알아보았다.
