# PDFium-aardio
PDFium 是 Google 著名开源项目 Chromium 的一部分，这部分代码就是福昕的技术中比较核心的引擎代码，它比较底层和基础，能够支持PDF的阅读、搜索、打印和文档/表单的填写。开发者可以在此基础上开发出比较简单的PDF应用。  

经过我的实际使用经验来看,PDFium 解析 PDF 的完整度要好于python的著名项目`pdfminer ` ,尤其是解析带签名的合同制式时,pdfminer经常丢失内容.PDFium暂时没发现有此类问题,且对中文支持友好.


# 示例:

载入pdf

```javascript
var pdf = fsys.pdfium("test.pdf")
```

提取树形目录

```javascript
//加载目录
var bm = pdf.extractBookmarks()
mainForm.treeview.insertItem( bm.asTree() )

//改变当前节点
mainForm.treeview.onSelChanged = function(hItem,data,nmTreeView){
	if(data){
		pdf.pageNum = data.pageIndex;//设置当前页码，起始页码为 1
		
        //wb 为 web.view 对象
		wb.go("/FoxitPDF_SDK20_Guide.pdf#page="+data.pageIndex)  
		mainForm.editPageNum.text = data.pageIndex; 
	} 	
}
```

提取某页文本

```javascript
pdf.pageNum = 8; //设置当前页码，起始页码为 1

var text = pdf.extractText();`
```

遍历某页文本块,带坐标数据
```javascript
import console
reader.pageNum = 8; //设置当前页码，起始页码为 1
for left,top,right,bottom,text in reader.eachTextRect(){
    console.log(left,top,right,bottom,text)
}
```

#### 依赖项目:

1. [pdfium - Git at Google (googlesource.com)](https://pdfium.googlesource.com/pdfium/)
2. [bblanchon/pdfium-binaries: 📰 Binary distribution of PDFium (github.com)](https://github.com/bblanchon/pdfium-binaries) (pdfium去掉v8核心的预编译动态库)

![](screenshot.png)

