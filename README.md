# PDFium-aardio
PDFium 是 Google 著名开源项目 Chromium 的一部分，也是福昕的技术中比较核心的引擎代码。它比较底层和基础，能够支持 PDF 的阅读、搜索、打印和文档/表单的填写。开发者在此基础上可以开发比较简单的 PDF 应用。  

经过我的实际使用来看，PDFium 解析 PDF 的完整度要好于 Python 的著名项目 `pdfminer `，尤其是解析带签名的合同制式时，PDFMiner 经常丢失内容，PDFium 暂时没发现有此类问题，且中文支持较好。


# 示例:

在 aardio 中导入 fsys.pdfium(PDFium) 扩展库就可以开始使用了，aardio 官方扩展库已收录 fsys.pdfium，不再需要单独下载安装。

PDF 提取文本：

```javascript
import console;
import inet.http;//导入 inet.http 则 fsys.pdfium 支持网络 PDF
import fsys.pdfium;

//打开 PDF 文件
var pdf = fsys.pdfium("https://www.orimi.com/pdf-test.pdf")

//遍历 PDF 所有页面并获取文本，可选用参数 @1 指定开始页面，可选用参数 @2 指定结束页面
for pageNum,textContent in pdf.eachPageText(){
	//只有包含文本内容的 PDF 页才能提取到文本，有些 PDF 页只有图像而文本为空。
	console.log(textContent)
}

console.pause();

```

# 依赖项目:

1. [pdfium - Git at Google (googlesource.com)](https://pdfium.googlesource.com/pdfium/)
2. [bblanchon/pdfium-binaries: 📰 Binary distribution of PDFium (github.com)](https://github.com/bblanchon/pdfium-binaries) ( PDFium 去掉 V8 核心的 DLL 动态库)
	

