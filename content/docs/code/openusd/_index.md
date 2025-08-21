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
from pxr import Geom
stage.DefinePrim('/root/obj')
stage.OverridePrim('/root/obj')
Geom.Xform.Define('/root/obj')
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
from pxr import Usd,Geom
Geom.ModelAPI.Apply(prim)
```