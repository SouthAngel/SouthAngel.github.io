### Houdini 代码片段
#### 格式化 path name
```vex
string name,path,path_seg[];
int feat;
path = s@path;
path = replace(path,"namespace:","");
path_seg = split(path,"/");
name = path_seg[len(path_seg)-1];
feat = match("*piece[09]*",name)?1:0;
s@path = path;
s@name = name;
i@feat = feat;
```
