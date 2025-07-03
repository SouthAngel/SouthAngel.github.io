---
title: cmake
---

## 生成表达式（Generator expressions）
生成表达式在build时才会被执行
### 基本用法
```
 $<condition:true_string>
 $<IF:condition,true_string,false_string>
```
### 条件语句
```
$<AND:conditions>
$<OR:conditions>
$<NOT:condition>
```
### 字符串对比
```
$<STREQUAL:string1,string2>
$<EQUAL:value1,value2>
```
### CONFIG
```
$<CONOFIG>
$<CONFIG:Debug>
例：
如果是Debug编译,则追加_d后缀，输出 output_d 否则输出 output
output$<$<CONFIG:Debug>:_d>
```
### 测试生成表达式
```
add_custom_target(genexdebug COMMAND ${CMAKE_COMMAND} -E echo "$<CONFIG>")

cmake --build . --target genexdebug
```
### 常用的方法就是上述几个
更详细的用法可以参考 [cmake文档](https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html#configuration-expressions)