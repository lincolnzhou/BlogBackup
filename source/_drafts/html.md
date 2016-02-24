title: html
tags: [html]
---
## 啥是HTML？
HTML是用来描述网页的一种语言

> * HTML是超文本标记语言(Hyper Text Markup Language)
> * HTML不是编程语言，是标记语言
> * 标记语言是一套标记标签
> *  HTML使用**标记标签**来描述网页

<!-- more -->

## HTML元素
是指从开始标签(start tag)到结束标签(end tag)的所有代码

```html
<p>这是我的第一个网页</p>
```

|开始标签|标签内容|结束标签|
|------|------|--------|
|&lt;p&gt;<p>|这是我的第一个网页|&lt;/p&gt;

注：这儿的p标签是怎么显示在Table里的？哈哈，Markdown中不能直接使用html，所以必须转义

HTML标签最好是闭合的，虽然浏览器显示会没有问题，为了考虑兼容或者与其他代码冲突，建议书写时标签闭合

## HTML属性
属性总是以键/值对形式出现，比如: name="value"，必须有引号

公用属性

|属性|值|描述|
|----|-----|-----|
|class|classname|规定元素的类名|
|id|id|规定元素唯一id|
|style|style_definition|规定元素的内联样式(inner style)|
|title|text|规定元素的额外信息(可在工具提升时显示)|

## HTML标题
标题是通过`<h1>`—`<h6>`等标签进行定义的

h1字号最大

## HTML注释
代码如下：

```html
<!-- 这是一段注释 -->
```

## HTML段落
HTML段落是通过<p>定义的

p标签里面的内容，多个空格算一个空格


## HTML文本格式化
HTMl可定义很多格式化输出的元素，比如：粗体和斜体

### 文本格式化标签
|标签|描述|效果
|----|----|
|&lt;b&gt;|粗体(Bold)|<b>这是一个粗体</b>
|&lt;big&gt;|大号字|<b>这是一个粗体</b>
|&lt;small&gt;|小号字|<small>小号字体</small>
|&lt;em&gt;|着重文字|<p>一个普通的p标签<em>哈哈</em></p>
|&lt;i&gt;|斜体|<p>一个普通的p标签<i>哈哈</i></p>
|&lt;strong&gt;|加重语气|<p>一个普通的p标签<strong>哈哈</strong></p>
|&lt;sub&gt;|下标|<p>一段文字<sup>123</sup></p>
|&lt;sup&gt;|上标|<p>一段文字<sup>123</sup></p>
|&lt;ins&gt;|插入字|<p>一段文字<sup>123</sup></p>
|&lt;del&gt;|删除字|<p>一段文字<del>123</del></p>

### 计算机输出标签
|标签|描述|效果
|----|----|
|&lt;code&gt;|定义计算机代码|<code>console.log("123")</code>
|&lt;kbd&gt;|定义键盘码|<kbd>abcdefg</kbd>
|&lt;samp&gt;|计算机代码样本|<samp>Sample Text</samp>
|&lt;tt&gt;|定义打字机代码|<tt>Teletype text</tt>
|&lt;var&gt;|定义变量|<var>Computer Variable</var>
|&lt;pre&gt;|定义预格式文本|<pre>哈哈</pre>

### 引用、术语定义
|标签|描述|效果
|----|----|
|&lt;abbr&gt;|定义缩写(abbreviation)|The <abbr title="People's Republic of China">PRC</abbr> was founded in 1949
|&lt;acronym&gt;|定义首字母缩写|<acronym title="World Wide Web">WWW</acronym>
|&lt;address&gt;|定义地址|<address>Written by <a href="mailto:webmaster@example.com">Donald Duck</a>.<br> Visit us at:<br> Example.com<br> Box 564, Disneyland<br>USA</address>
|&lt;bdo&gt;|定义文字方向|<bdo dir="rtl">Teletype text</bdo>
|&lt;blockquote&gt;|定义长引用|<blockquote>这是一个长引用</blockquote>
|&lt;q&gt;|定义短引用|<q>这是一个短引用</q>
|&lt;cite&gt;|定义引用、引证|<cite>这是一个短引用</cite>

## HTML样式
1. 编写css文件
`<link href="style.css">`
2. <head>标签中编写`<style></style>`标签内容
3. 在标签上直接添加style属性
```html
<p style="margin-left:10px;"></p>
```

## HTML表格
通过`<table>`标签来定义，`<th>`定义表头，`<tr>`定义表格数据行，`<td>`定义表格数据，`<caption` 定义表标题，`<thead>`定义页眉(包括表头)，`<tbody>`定义主体，`<tfoot>`定义页脚

完整实例：
```html
<table>
  <caption>表标题</caption>
  <thead>
    <tr>
      <td>属性</td>
	</tr>
  </thead>
  <tbody>
    <tr>
      <td>p</td>
    </tr>
  </tbody>
  <tfoot></tfoot>
</table>
```

## HTML列表
列表是可以嵌套的
### 无序列表
就是没有序号的一项列表啦，每一项前面是黑点标记(可用CSS修改样式的)。由`<ul>`定义，每一项用`<li>`定义
```html
<ul>
  <li>雪碧</li>
  <li>可乐</li>
</ul>
```
<ul>
  <li>雪碧</li>
  <li>可乐</li>
</ul>

### 有序列表
每一项前面有数字来标记，使用`<ol>`定义，每一项用`<li>`定义

```html
<ol>
  <li>雪碧</li>
  <li>可乐</li>
</ol>
```
<ol>
  <li>雪碧</li>
  <li>可乐</li>
</ol>

### 定义列表
定义列表不仅仅是一项列表，而是列表和注释的组合
使用`<dl>` 定义，每一项数据用`<dt>`定义，每一项数据定义用`<dd>`
```
<dl>
  <dt>雪碧</dt>
  <dd>雪碧是.....组成的</dd>
  <dt>可乐</dt>
  <dd>可乐是.....组成的</dd>
</dl>
```
<dl>
  <dt>雪碧</dt>
  <dd>雪碧是.....组成的</dd>
  <dt>可乐</dt>
  <dd>可乐是.....组成的</dd>
</dl>

## HTML块
### 块元素
通常会用新行来开始和结束
例如：div，p，h1，ul，table等

### 内联元素
通常不会用新行来开始
例如：span，b，td，a，img等

### div
块级元素，作为组合其他HTMl元素的容器

### span
内联元素，作为文本的容器，特别是文字描述

## HTML布局
> 让我想起以前才开始学习.net的时候，使用的Table布局，那页面惨不忍睹

div＋css来写，后面来着重写这部分

## HTML表单
HTML 表单用于搜集不同类型的用户输入

页面中使用`<form>`标签定义

form需要定义如下属性
action(提交地址)
method(提交方式get或者post)
enctype(编码方式）
* application/x-www-form-urlencoded
* multipart/form-data
* text/plain

###  input标签
type
* button
* text
* radio
* submit
* file
* image
* hidden
* reset
* checkbox
* password
* email

### textarea
控制多行文本输入

### label
label标签是为input标签标记的
将for的值与需绑定元素的id相同即可

```
<label for="username">Username</label>
<input id="username" type="text" placeholder="Please input username" />
```

当触发Username标签时，id为username的input也会相应聚焦，如果时radio直接是选中

### select
用于下拉选择多项

### button

## HTMl颜色
颜色是由红、绿、蓝三色组合
颜色是由十六进制符号组成的，这些符号由红色、绿色、蓝色的值组成(RGB)
每个颜色的最小值为0(#00)，最大值为255(#FF)


黑色 #000000 rgb(0,0,0)
红色 #FF0000 rgb(255,0,0)
绿色 #00FF00 rgb(0,255,0)
蓝色 #0000FF rgb(0,0,255)
白色 #FFFFFF rgb(255,255,255)

## <!DOCTYPE>
只是一个声明，网页使用的哪个版本

**html5申明**
<!DOCTYPE html>

## HTML头部元素
`<head>`标签定义

包括：meta、link、style、script




