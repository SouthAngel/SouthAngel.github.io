---
title : OpenUSD
---

### Stage open or create
```python
from pxr import Usd
Usd.Stage.Open('testa.usd')
Usd.Stage.CreateNew('testa.usd')
Usd.Stage.CreateInMemory()
```
### Stage save or export
```python
stage.Export('testa.usd')
stage.Save()
```
### Add prim
```python
from pxr import UsdGeom
stage.DefinePrim('/root/obj')
stage.OverridePrim('/root/obj')
UsdGeom.Xform.Define(stage,'/root/obj')
```
### Access stage or prim
```python
obj.GetPrimAtPath(path)
obj.GetObjectAtPath(path)
obj.GetPropertyAtPath(path)
obj.GetAttributeAtPath(path)
obj.GetRelationshipAtPath(path)
```
### Apply API
```python
UsdGeom.ModelAPI.Apply(prim)
```
### Variants
```python
prim = stage.GetPrimAtPath(path)
vset = prim.GetVariantSets().AddVariantSet('variantSetA')
vset.SetVariantSelection('A_2')
with vset.GetVariantEditContext():
    # change prim content
    pass
```
### Payload
```python
stage = Usd.Stage.CreateNew('testout.usd')
prim = stage.DefinePrim('/root/objA')
prim.GetPayloads().AddPayload('./ref.usd','/objRef')
mprim = UsdGeom.ModelAPI.Apply(prim)
mprim.SetExtentsHint(mprim.ComputeExtentsHint(UsdGeom.BBoxCache(1,[])))
```
### Xform animation
```python
from pxr import Gf
xprim = UsdGeom.Xform.Define(stage, objpath)
op = xprim.AddTranslateOp(opSuffix='Translate')
op.Set(time=1,value=Gf.Vec3f(1.2,0.2,0.1))
op.Set(time=10,value=Gf.Vec3f(14.2,12.1,0.1))
```
### SubLayer
```python
layer.subLayerPaths.append(usdpath)
```