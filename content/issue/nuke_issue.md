---
title: nuke 插件问题
---

## nuke OFX 插件报错无法加载指定模块
deadline 渲染报错如下，有一个 OFX 插件无法加载，原因是vc运行库dll缺失，重新安装对应版本的vcredist解决，这里是vc2013版本  
2025-06-27 00:37:37:  0: STDOUT: [ 0:37.37] ERROR: RSMB5: 'OFXcom.revisionfx.rsmb_regular_v3': unknown command. This is most likely from a corrupt .nk file, or from a missing or unlicensed plug-in. Can't render from that Node