title: Javascript复制到剪切板
date: 2014-04-17 16:43:17
tags: [Javascript, utils]
categories: diary
keywords: Javascript,clipboardData,复制,剪切板
---

### 代码
```js
/**
 * 将特定的字符串复制到剪切板
 * @param {String} str 待复制的字符串
 */
function doCopy(str) {
  if (window.clipboardData && window.clipboardData.setData) {
    // for IE

    window.clipboardData.setData("Text", str);
  } else {
    // for no-IE

    var elem = document.createElement('elem');
    elem.style = 'width:0;height:0;';
    elem.innerHTML = str;
    document.body.appendChild(elem);

    var selection = window.getSelection();
    var range = document.createRange();
    range.selectNodeContents(elem);
    selection.removeAllRanges();
    selection.addRange(range);
    var ret = document.execCommand('Copy');
    selection.removeAllRanges();

    // 删除辅助元素（可选）
    elem.parentNode.removeChild(elem);
  }
}
```

### 说明
只是简单的实现，还可以扩展，以方便满足不同需求的调用。  
另外，仅测试chrome和IE，还没深入研究是否可以取代[flash方案](https://github.com/zeroclipboard/zeroclipboard)