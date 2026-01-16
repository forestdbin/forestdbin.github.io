---
title: Windows激活本地admin
date: 2026-01-16 13:48:18
tags:
  - misc
  - windows
categories:
---

传统的组策略（group policy）
```
gpupdate /force
```

dsregcmd: diagnose and manage device registration with Microsoft Entra ID (formerly Azure AD)

PRT: Primary Refresh Token
```
dsregcmd /status

dsregcmd /refreshprttoken
```

检查账号信息
```
whoami /groups

whoami /priv
```

常规操作：重启；重新登录；重连网络；重启windows explorer。
