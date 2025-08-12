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
#### LOP python node 根据 JSON 指认材质
```python
node = hou.pwd()
stage = node.editableStage()
jfpath = hou.parm('mat_info').eval()
mnamespace = hou.parm('namespace').eval()
material_parent = hou.parm('material_parent').eval()
import json
import pxr.UsdShade
with open(jfpath) as _f:
    jdata = json.load(_f)
mamap = {}
name_rep = '/'if not mnamespace else ('/'+mnamespace+'_')
matpath_prefix = material_parent if material_parent.endswith('/') else (material_parent+'/')
for i in range(0,len(jdata),3):
    mamap[jdata[i].replace('|',name_rep)] = matpath_prefix+jdata[i+2]
for me,ma in mamap.items():
    ume = stage.GetPrimAtPath(me) or None
    uma = stage.GetPrimAtPath(ma) or None
    if not ume or not uma:
        print(['error bind material',me,ume,ma,uma])
        continue
    bind_ok = pxr.UsdShade.MaterialBindingAPI(ume).Bind(pxr.UsdShade.Material(uma))
    tsh = me
    # break
```