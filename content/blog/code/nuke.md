### Nuke 代码片段
#### 检查重名 nuke 节点
```python
import sys,os
import hashlib
import nuke

gmap = {}
for each in nuke.pluginPath():
    if not os.path.isdir(each): continue
    for ldi in os.listdir(each):
        spn = os.path.splitext(ldi)
        if spn[1] not in ('.nk','.gizmo'): continue
        cmp = os.path.join(each, ldi)
        if not os.path.isfile(cmp): continue
        nnk = spn[0]
        if nnk not in gmap: gmap[nnk] = []
        gmap[nnk].append(cmp)
for each in list(filter(lambda x:len(gmap[x])<2, gmap)):
    del gmap[each]
for each in gmap:
    print('='*16)
    print(each)
    for eachf in gmap[each]:
        hobj = hashlib.md5()
        with open(eachf, 'rb') as _f:
            hobj.update(_f.read())
        print(eachf, hobj.hexdigest())
```