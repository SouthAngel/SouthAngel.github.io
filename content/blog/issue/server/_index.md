---
title: 服务器有关的问题
---

## Mac OS 自动在共享存储上生成一大堆 .DS_Store
```zsh
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

## 思源笔记忘记授权码
docker搭建的服务器应用忘记了授权码，去数据目录下找 conf/conf.json 文件，搜索 accessAuthCode 字段  
```bash
grep 'accessAuthCode' workspace/conf/conf.json
# "accessAuthCode": "111111",
```