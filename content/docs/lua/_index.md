---
title: lua
---
## lua 基础
### 循环
### 字符串
```lua
string.gsub("goto 11 line", "1", "32") --> return goto 32 line
```
### Table
```lua
var = {"string","join"}
table.concat(var) --> return stringjoin
```
### 协程
### 元表
## 调用脚本
### load
load 是个偏底层的函数，接受一个读取器函数（类似于迭代器），load 会多次调用读取器，直到读取器返回nil，loadfile 和 loadstring 基于 load 实现。用户层面一般很少直接使用 load
### loadstring
载入而不执行，接受一段代码字符串，返回一个可执行函数
### loadfile
载入而不执行，接受一个文件路径，读取文件内容，把文件内容编译成一个函数，返回一个可执行函数
### dofile
载入并执行，参照loadfile，每次调用都会重新加载文件
### require
载入并执行，参照dofile，只执行一次，第二次加载不重复执行
## lua 调用 C
lua使用一个状态机对象lua_State与C函数沟通，每次调用函数lua会创建一个虚拟栈，lua解释器通过多次压栈传入不同的参数，同理C也通过压栈方式向解释器传递返回值
```C
#include "stdio.h"
#include "string.h"
#include "lua.h"
#include "lauxlib.h"
#include "lualib.h"
static int test_func1(lua_State* L){
    char* teststr = luaL_checklstring(L,1,NULL)
    char* teststr2 = luaL_checklstring(L,2,NULL)
    char strout[1024] = {0};
    strcat(strout, teststr);
    strcat(strout, "_join_");
    strcat(strout, teststr2);
    lua_pushstring(L, strout);
    return 1;
}
```
C语言按约定编译动态链接库，将动态库置于lua可识别的模块路径中可以直接require
```C
// C
static const luaL_Reg TestLib[] = {
	{"test_func1",test_func1},
	{NULL,NULL}
};
int luaopen_TestLib(lua_State* L) {
	luaL_newlib(L, TestLib);
	return 1;
}
```
```lua
-- lua
testlib = require("TestLib")
outvar = testlib.test_func1("apple","banana")
print(outvar)
```