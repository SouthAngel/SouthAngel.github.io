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
Deadline 所有插件只要遵循一定的规则编写并按照特定文件命名置于 custom 下对应文件夹下（必要文件是主py文件以及插件的.param配置文件），Deadline就能自动识别并分发

## Application Plugins
自定义应用插件的位置 custom\plugins  
应用插件有两种类型
- Simple
- Advanced

Simple就是我们通常理解的把不同帧分到不同机器上，差别在于Advanced类型会在不同帧任务之间保持进程不退出，这种好处主要是减少了每次启动时IO的时间，坏处是某些软件内存管理的不好渲大场景容易崩

### 主py文件
{{% details title="Plugin.py" closed="true" %}}
```python {filename=Plugin.py}
#coding=utf-8
#!/usr/bin/env python3
#Author:PengCheng
import sys,os
from Deadline.Plugins import DeadlinePlugin,PluginType

# Deadline 要求的接口结构
def GetDeadlinePlugin():
    return PHoudiniPlugin()

def CleanupDeadlinePlugin(deadlinePlugin):
    deadlinePlugin.Cleanup()

# 必须从 DeadlinePlugin 继承
class PHoudiniPlugin(DeadlinePlugin):
    def __init__(self):
        super().__init__()
        # .net 风格回调
        self.InitializeProcessCallback += self.InitializeProcess # 初始化进程
        self.RenderExecutableCallback += self.RenderExecutable # 返回要调用的Houdini程序路径，这里是hython
        self.RenderArgumentCallback += self.RenderArgument # 拼自己的命令行

    # Deadline 要求的接口结构，uninitialize 调用
    def Cleanup(self):
        for stdoutHandler in self.StdoutHandlers:
            del stdoutHandler.HandleCallback
        del self.InitializeProcessCallback
        del self.RenderExecutableCallback
        del self.RenderArgumentCallback
    
    def InitializeProcess(self):
        self.SingleFramesOnly = False
        # 插件的类型
        self.PluginType = PluginType.Simple
        '''...'''

    def RenderExecutable(self):
        version = self.GetPluginInfoEntryWithDefault( "HVersion", "19.5.805" )
        app_path = f'Houdini {version}/bin/hython.exe'
        return app_path

    def RenderArgument(self):
        # 需要准备一个自定义的渲染脚本，并配置对应的命令行调用，可以根据需要在这一步做一些准备环境之类的操作
        arguments = ['render.py','-start_frame',self.GetStartFrame(),'-end_frame',self.GetEndFrame()]
        return ' '.join(arguments)
```
{{% /details %}}

### 配置文件
{{% details title="Plugin.param" closed="true" %}}

```txt {filename="Plugin.param"}
[About]
Type=label
Label=About
Category=About Plugin
CategoryOrder=-1
Default=Custom plugin for houdini execute
Description=Not configurable

[ConcurrentTasks]
Type=label
Label=ConcurrentTasks
Category=About Plugin
CategoryOrder=-1
Index=0
Default=True
Description=Not configurable

```
{{% /details %}}

## Event Plugins