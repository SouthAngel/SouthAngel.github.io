---
title: dealine 插件开发
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