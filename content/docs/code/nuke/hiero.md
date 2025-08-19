### Hiero 代码片段
#### 获取当前选中的片段
```python
hiero.ui.getTimelineEditor(hiero.ui.activeSequence()).selection()
```
#### 为片段设置颜色
```python
trackitem.source().binItem().setColor(color)
```
#### 设置Tag
```python
tag = hiero.core.Tag('tagA')
tag.setIcon('icons:status:/TagBlocked.png')
trackitem.addTag(tag)
```