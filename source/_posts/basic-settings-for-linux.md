---
title: Basic Settings for Linux
date: 2025-06-04 21:47:38
tags:
  - linux
  - env
categories:
---

## apt

- apt-get update
- git (config)
- vim
- tree
- mlocate (updatedb)

```bash
sudo apt-get update
sudo apt-get install build-essential git vim tree plocate
```

## bash

- bashmy
- bashrc

```bash
git clone https://github.com/forestdbin/code-snippet.git
```

## vim

- update-alternatives
- vimrc

```bash
sudo update-alternatives --config editor
```

## sudo

```bash
# visudo
%sudo ALL=(ALL:ALL) NOPASSWD:ALL
```

## vmtools
