---
title: HTML常用标签
date: 2020-02-23T17:08:51+08:00
lastmod: 2020-02-23T17:08:51+08:00
author: spc
cover: /img/food4.jpg
categories: ["html"]
tags: ["常用标签"]
# showcase: true
draft: false
---

a 、img 、table 标签的用法

<!--more-->

# HTML常用标签

## a标签
属性
* href  (hyper reference)
* target
* download
* rel=noopener

a标签作用
* 跳转到网页、锚点、邮箱和电话

为了防止`window.opener`被滥用，在使用`targrt=_blank`时需要加上`rel=noopener`  

    <a href="www.baidu.com" target="_blank" rel="noopener" >


属性href取值
* 网址
* 路径 
* id=xxx（锚点）
* 伪协议 ：javascript、mailto、tel

属性target取值
* 内置名字 
  *  _blank (new tab)
  *  _top (用顶部打开)
  *  _parent （上一级打开）
  *  _self （自己打开，默认）
* 其他命名 
  * window的name (new tab's name)
  * iframe的name

属性download

* 作用 ：不打开只下载
* 不是所用的浏览器都支持，尤其是手机浏览器可能不支持

# iframe标签
内嵌标签

# table标签
* 相关样式
  * table-layout
  * border-collapse 
  * border-spacing

border-collapse: collapse;  
因为默认是没合并有空隙，默认样式很丑所以用collapse

```html
<!-- tr:table row 
th:table head一列 
td:table data -->
    <thead>
        <tr>
            <th colspan="2">The table header</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The table body</td>
            <td>with two columns</td>
        </tr>
    </tbody>
    <tfoot>
    </tfoot>
</table>
```

# img标签
* 作用
  * 发出get请求，展示图片
* 属性
  * alt/height/width/src(source)
  * 不要同时height/width，除非有规定，不然是破坏原本比例
* 事件
  * onload/onerror
* 响应式
  * max-width:100%
* [可替换元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)

# form标签
* 作用
  * 发送get和post请求，然后刷新页面
* 属性
  * action (提交到的响应页面)
  * autocomplete 
  * method (get and post)
  * target (与a标签target)
* 事件 onsubmit

```xml
<form action="" method="get" >
  <div>
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required>
  </div>
  <div>
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required>
  </div>
  <div>
    <input type="submit" value="提交">
  </div>
</form>

<!-- button能有额外功能，例如加个<br> -->
<input type="submit" value="提交">
<button type="submit" >提交</button>
```
## input标签
* 作用：输入内容
* 属性：button/checkbox/file/hidden/text/submit/...
* 事件：onchange/onfocus/onblur


注意事项
* 一般不监听input的click事件
* form里面的input要有name
* form里面要发一个submit才能触发submit事件