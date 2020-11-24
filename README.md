# 基于 https://github.com/mozilla/pdfjs-dist 版本【2.3.200】
* 高版本里面的语法有些浏览器不支持，因此降级
* 说明： 源码不支持获取rotate参数，但是做pdf手签时，需要旋转角度的参数，因此复制了一份源码，自己修改了。
##  yarn add pdfjs-dist
## v1.0.0
* 基于pdfjs-dist重新创建
* 添加了旋转角度存储在sessionStorage,本地编辑器打开pdf，整体旋转之后，解析时需要获取rotate,手签需要；
* 注释了pdf.worker.js里面的Sign代码，以便展示签章

## 使用方式

https://mozilla.github.io/pdf.js/examples/index.html#interactive-examples

```javascript
  import * as PDFJS from 'dpdf-dist';
   const loadingTask = PDFJS.getDocument('pdfurl');
    loadingTask.promise.then((pdf) => {
      renderPdf(pdf, defaultWidth, pageNumber);
    }, (reason) => {
      console.error('错误', reason);
    });

    const renderPdf = (pdf, defaultWidth, pageNum) => {
    pdf.getPage(pageNum).then((page) => {
      // const viewport = page.getViewport({ scale: 1 });
      // const scale = defaultWidth / viewport.width;
      const defaultViewport = page.getViewport({ scale: 1 });
      const canvas = pdfCanvas.current;
      const context = canvas.getContext('2d');
      canvas.height = defaultViewport.height;
      canvas.width = defaultViewport.width;

      const renderContext = {
        canvasContext: context,
        viewport: defaultViewport
      };
      const renderTask = page.render(renderContext);
      renderTask.promise.then(() => {
        console.log('Page rendered');
      });
    });
  }
```                                                              