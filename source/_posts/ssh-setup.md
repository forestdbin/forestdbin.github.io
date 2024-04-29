---
title: SSH相关的设置
tags:
  - ssh
  - linux
date: 2024-04-29 21:00:01
---

## 在Ubuntu主机上安装SSH服务器

```bash
$ sudo apt-get install openssh-server
```

## 公钥认证

参见[What is SSH Public Key Authentication?](https://www.ssh.com/academy/ssh/public-key-authentication)。

### ssh目录

```bash
# Windows
## system
C:\ProgramData\ssh\
## user
C:\Users\Foo\.ssh\

# Linux
## system
/etc/ssh/
## user
/home/foo/.ssh/
```

### key文件

```bash
# private key
id_rsa
# public key
id_rsa.pub
```

### 用`ssh-keygen`生成client的key pair

### 对public key授权

可以在client端执行`ssh-copy-id`命令；也可以在server端直接编辑`~/.ssh/authorized_keys`文件，加入client的public key。

### WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

以前连接过的server端的key文件更新了，或者IP变化了，会出现这种情况。

1. 可以用`ssh-keygen -R`从`known_hosts`移除server的fingerprint。
2. 然后用`ssh-keyscan`将新的fingerprint加入。
3. 也可以直接修改`known_hosts`。
