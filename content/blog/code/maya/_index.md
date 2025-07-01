### Maya 代码片段
#### 断掉所有材质连接
其它环节给回的abc文件带了多余的SG分组信息
```python
import maya.cmds as cmds

def foo():
    sels = cmds.ls(sl=1)
    mes = cmds.listRelatives(ad=1,type='mesh')
    for me in mes:
        lcs = cmds.listConnections(me,s=0,d=1,c=1,p=1)
        for i in range(0,len(lcs),2):
            if 'instObjGroups.objectGroups[' not in lcs[i]: continue
            cmds.disconnectAttr(lcs[i],lcs[i+1])
        print(lcs)
    tsh = mes
    print(tsh)
foo()
```

#### 找回曲线层级
特效输出的树状线层级缺失，根据曲线起始点位置及曲线方向找回 branch 层级
```python
#coding=utf-8
import maya.cmds as cmds
import maya.api.OpenMaya as mom

if 'gmap' not in globals():
    gmap = {}

def gpmap():
    print(2)
    dmap = {}
    sell = mom.MGlobal.getActiveSelectionList()
    for i in range(sell.length()):
        dp_ = sell.getDagPath(i)
        cvo = mom.MFnNurbsCurve(dp_)
        dmap[dp_.fullPathName()] = {'cv':cvo,'dp':dp_,'p1':cvo.cvPosition(0),'p2':cvo.cvPosition(cvo.numCVs-1)}
    pmap = {}
    for eacp in dmap:
        for cmpp in dmap:
            if eacp == cmpp: continue
            tol = 0.01
            if dmap[eacp]['p1'].distanceTo(dmap[cmpp]['p1']) <= tol : continue
            # dmap[cmpp]['cv'].isPointOnCurve(dmap[eacp]['p1'])
            if dmap[cmpp]['cv'].isPointOnCurve(dmap[eacp]['p1'], tol):

                pmap[eacp] = cmpp
                break
    tsh = [len(dmap),len(pmap)]
    print(tsh)
    return pmap

def foo():
    print(1)
    pmap = gpmap()
    rmap = {}
    for eacp in pmap:
        itt = [eacp]
        oset = set([eacp])
        while itt[-1] in pmap:
            itt.append(pmap[itt[-1]])
            print([x.split('|')[-1] for x in itt])
            if itt[-1] in oset: break
            oset.add(itt[-1])
        rmap[eacp] = list(reversed(itt))
    for eacp in rmap:
        lv = 'level_%d'%(len(rmap[eacp]))
        if not cmds.ls('|'+lv): cmds.createNode('transform',n=lv)
        print(lv)
        cmds.parent(eacp,'|'+lv)
    tsh = rmap
    print(tsh)


foo()
```