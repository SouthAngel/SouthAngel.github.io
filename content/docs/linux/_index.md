---
title: Linux
---

## Linux命令速记

### sed

```bash
# -n 静默
# -i 修改文件内容,原文件加后缀备份
# -r/-E 扩展正则表达式
# acdips 操作符
# a向上插入行
# c取代行
# d删除行
# i向下插入行
# p打印行
# s替换
sed '3d' # 删除第三行
sed '2,8ab' # 第二行到第八行每行下面添加一个b
sed '/client/d' # 搜索定位包含client的行，执行删除操作
# 向后引用
sed 's/\(test..\)\(ac..\)/\1_\2/'
```
### top
- PID
- USER
- PR priority值，优先级，系统自动分配
- NI nice值，优先级，用户可以指定，与PR共同决定了进程的实际优先级
- VIRT 虚拟内存
- RES 物理内存
- SHR 共享内存
- S 进程状态，R=运行,S=睡眠,I=空闲,D=不可中断睡眠,T=跟踪,Z=僵尸进程
- %CPU
- %MEM
- TIME+ 进程使用的CPU时间总计
- COMMAND