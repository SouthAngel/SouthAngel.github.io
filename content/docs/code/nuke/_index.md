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
#### 检查重名的 Command
```python
import nuke
toolbar = nuke.toolbar("Nodes")
type(toolbar) is nuke.Menu
gdata = [[toolbar, []]]
fldata = []
while len(gdata)>0:
    cit = gdata.pop()
    if type(cit[0]) is nuke.Menu:
        for mit in cit[0].items():
            patha = cit[1][:]
            patha.append(mit.name())
            gdata.append([mit,patha])
    else:
        fldata.append(cit[1])
# print(fldata)
nmap = {}
for pa in fldata:
    if len(pa) < 1 : continue
    k_ = pa[-1]
    if not k_ : continue
    if k_ not in nmap: nmap[k_] = []
    nmap[k_].append(pa)
for cname in nmap:
    if len(nmap[cname]) < 2 : continue
    print('='*16)
    print(cname)
    print('\n'.join(map(lambda x:'/'.join(x),nmap[cname])))
    print('='*16)
```