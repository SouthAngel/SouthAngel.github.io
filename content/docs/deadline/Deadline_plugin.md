---
title: Deadline 插件开发
---
## 插件分类
Dealine 插件使用 Python 编写，Deadline 插件分几个部分
- Application Plugins
- Event Plugins
- Monitor Scripts
- Job Scripts
- Web Serveice Scripts
---
Deadline 所有插件只要遵循一定的规则编写并按照特定文件命名置于 custom 下对应文件夹下，Deadline就能自动识别并分发

## Application Plugins
自定义应用插件的位置 custom\plugins  
应用插件有两种类型
- Simple
- Advanced

Simple就是我们通常理解的把不同帧分到不同机器上，差别在于Advanced类型会在不同帧任务之间保持进程不退出，这种好处主要是减少了每次启动时IO的时间，坏处是某些软件内存管理的不好渲大场景容易崩