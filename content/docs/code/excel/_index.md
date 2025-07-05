---
---
#### 批量导出excel中的图片
```js
function exportImage(){
  let outw = 2386;
  let outh = 1342;
  let exc = 0;
  for (pic of ActiveSheet.Shapes){
    if (pic.Type != msoPicture) { continue; }
    let lce = pic.TopLeftCell.Offset(0,-1);
    let rce = pic.TopLeftCell.Offset(0,1);
    if ((lce+rce)==="") { continue; }
    let outpath = ThisWorkbook.Path + '/' +lce+'_'+rce+'.png'
    let tmpw = pic.Width;
    let tmph = pic.Height;
    pic.Width = outw;
    pic.Height = outh;
    pic.CopyPicture();
    let ct = ActiveSheet.ChartObjects().Add(0,0,outw,outh).Chart
    ct.Paste();
    ct.Export(outpath, 'png');
    exc += 1;
    ct.Parent.Delete()
    pic.Width = tmpw;
    pic.Height = tmph;
    let ab = "c";
  } 
  MsgBox('Export End Count '+exc);
}
```