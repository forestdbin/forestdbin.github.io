---
title: pyenv：Python版本管理
tags:
  - python
  - linux
  - env
date: 2024-04-29 23:05:37
---

## [pyenv](https://github.com/pyenv/pyenv 'Python Version Management') - Python Version Management

## 安装方式一

```bash
$ curl https://pyenv.run | bash
# 使用此方式也会安装pyenv-doctor，pyenv-update和pyenv-virtualenv plugins

# 修改~/.bashrc或者~/.profile如下
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
# command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)" # init virtualenv plugin

# RESTART SHELL
```

## 安装方式二

```bash
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv

$ cd ~/.pyenv && src/configure && make -C src # optional
```

## 安装必要的包（用于构建Python）

```bash
$ sudo apt install build-essential libssl-dev zlib1g-dev libbz2-dev \
    libreadline-dev libsqlite3-dev curl libncursesw5-dev xz-utils tk-dev \
    libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

## `pyenv`使用

```bash
$ pyenv install 3.12.1

$ pyenv global/local/shell
$ pyenv version/versions
```

## [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv)插件

```bash
# 安装
$ git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv

# 使用
$ pyenv virtualenvs
$ pyenv virtualenv
$ pyenv activate
$ pyenv deactivate
$ pyenv virtualenv-delete
```

## [pyenv-virtualenvwrapper](https://github.com/pyenv/pyenv-virtualenvwrapper)插件（可选）

```bash
# 安装
$ git clone https://github.com/pyenv/pyenv-virtualenvwrapper.git $(pyenv root)/plugins/pyenv-virtualenvwrapper

# 使用
$ pyenv virtualenvwrapper
$ pyenv virtualenvwrapper_lazy
```

使用`pyvenv`而不是`virtualenv`：
```bash
export PYENV_VIRTUALENVWRAPPER_PREFER_PYVENV="true"
```

### [virtualenvwrapper](https://github.com/python-virtualenvwrapper/virtualenvwrapper)

[virtualenvwrapper文档](https://virtualenvwrapper.readthedocs.io/en/latest/)。`virtualenvwrapper`可以提供`mkvirtualenv`，`lsvirtualenv`，`workon`这样的命令，以方便使用虚拟环境。

## 一点说明

一般一个系统上只安装一个版本的`Python`。有了`pyenv`就可以方便地安装多个版本，并在不同版本间切换。

早期的Python只能装一组包（一个环境），难免会出现包版本冲突的情况。于是发展出了`virtualenv`这的方案，它可以创建多个虚拟环境，这样就可以在不同的环境中安装不同版本的包。

在`virtualenv`的基础上，它的一个子集成为了后来Python内置库——`venv`模块（`pyvenv`包）。

`virtualenvwrapper`提供了方便使用虚拟环境的命令。
