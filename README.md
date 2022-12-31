# PDFium-aardio
PDFium 是 Google 著名开源项目 Chromium 的一部分，也是福昕的技术中比较核心的引擎代码。它比较底层和基础，能够支持 PDF 的阅读、搜索、打印和文档/表单的填写。开发者在此基础上可以开发比较简单的 PDF 应用。  

经过我的实际使用来看，PDFium 解析 PDF 的完整度要好于 Python 的著名项目 `pdfminer `，尤其是解析带签名的合同制式时，PDFMiner 经常丢失内容，PDFium 暂时没发现有此类问题，且中文支持较好。


# 示例:

在 aardio 中导入 fsys.pdfium(PDFium) 扩展库就可以开始使用了，aardio 官方扩展库已收录 fsys.pdfium，不再需要单独下载安装。

载入并显示 PDF

```javascript
import win.ui;
/*DSG{{*/
var winform = win.form(text="PDF 简单绘图")
winform.add(
plus={cls="plus";left=11;top=8;right=742;bottom=453;db=1;dl=1;dr=1;dt=1;repeat="scale";z=1}
)
/*}}*/

//打开 PDF
import inet.http;
import fsys.pdfium;
var pdf = fsys.pdfium("https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf")

//显示 PDF，自动支持 plus 控件的缩放模式
pdf.pageNum = 1;
winform.plus.background = pdf.asBitmap();

winform.show();
win.loopMessage();

```

提取树形目录

```javascript
//加载目录
var bm = pdf.extractBookmarks()
mainForm.treeview.insertItem( bm.asTree() )

//树视图改变当前节点触发此事件
mainForm.treeview.onSelChanged = function(hItem,data,nmTreeView){
	if(data){
		pdf.pageNum = data.pageIndex;//设置当前页码，起始页码为 1
		
		//wb 为 web.view 对象
		wb.go("/a.pdf#page="+data.pageIndex)  
		mainForm.editPageNum.text = data.pageIndex; 
	} 	
}
```

![](screenshots/screenshot.png)

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

合并 PDF
```javascript
import fsys.pdfium;
var pdf = fsys.pdfium("/a.pdf");

//导入另外一个 PDF，参数 @1 也可以是另外的 fsys.pdfium 对象。
pdf.importPages("/b.pdf",,"1-7,6,9");//导入 1-7,6,9 页，省略页码参数则导入所有页面

//保存 PDF
pdf.save("/c.pdf");
```

# 依赖项目:

1. [pdfium - Git at Google (googlesource.com)](https://pdfium.googlesource.com/pdfium/)
2. [bblanchon/pdfium-binaries: 📰 Binary distribution of PDFium (github.com)](https://github.com/bblanchon/pdfium-binaries) ( PDFium 去掉 V8 核心的 DLL 动态库)
	

