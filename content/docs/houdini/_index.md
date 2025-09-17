---
title: Houdini
---

## LPE 光路表达式
全称是 Light Path Expression  
光路表达式是描述光线在光源和物体中间来回反弹的过程表达式，主要用于拆AOV层，光路表达式和正则表达式类似，可以想象一下，一次渲染发射出上千条光线追踪射线，每条线与光路径表达式做匹配，匹配正确的留下，错误的丢弃，大概就是这么个原理  
光路表达式有一定的规则，但各家渲染器的具体实现有一些差异，这里以arnold渲染器为例介绍

---

arnold渲染器的光路表达式遵循典型的光线追踪的计算过程，即光线从摄像机发出，撞击到物体表面，根据表面性质反向寻找光源，可以描述为  
<font size=5>camera > surface > bouncing > light</font>  
  
光路表达式匹配规则与正则类似，. 代表匹配一次 + 代表匹配多次 * 代表零次或多次，C 代表相机， L 代表光源，那么一个完整匹配所有光源路径的表达式就是  
<font size=5>C.\*L</font>  
如果使用 D 代表 Diffuse 漫反射 则所有 diffuse aov 提取可以写成  
<font size=5>CD\*L</font>  
我们可能还想加入反射的贡献  
<font size=5>CD[DS]\*L</font>  

*更多高级用法参阅[Arnold文档](https://help.autodesk.com/view/ARNOL/ENU/?guid=arnold_user_guide_ac_output_aovs_ac_expression_aovs_html)*

### Event
符号|事件|描述
-|-|-
C|Camera|相机
L|Light|光源
O|Object emission|物体自发光
B|Background emission|背景自发光
A|Albedo|材质固有色
### Scattering Event
符号|事件|描述
-|-|-
D|Diffuse|漫反射
\<RD\>|Diffuse reflection|
\<TD\>|Diffuse transmission|
S|Specular|高光
\<RS\>|Specular reflection|
\<TS\>|Specular transmission|
V|Volume|体积

### 常见的分层 LPE 表示方法
分层|表达式|-
-|-|-
RGBA|C.*|
diffuse|C\<RD\>.*|
specular|C<RS\[^'coat']\>.*|
transmission|C\<TS\>.*|
diffuse_direct|C\<RD\>L|
diffuse_indirect|C\<RD\>[DSVOB].*|