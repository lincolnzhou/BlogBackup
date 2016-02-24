title: html5学习
tags: [html5]
---
## 新特性
* 视频video
* 音频audio
* 绘图canvas
* 本地离线存储
* 新的特殊内容元素，比如 article、footer、header、nav、section
* 新的表单控件，比如 calendar、date、time、email、url、search

## 视频video
### 基本用法
```
<video src="song.ogg" width="300" height="400"></video> 
```
或者
```
<video width="300" height="400">
  <source src="song.ogg" type="video/ogg">
  <source src="song.mp4" type="video/mp4">
</video> 
```
后面一种是兼容模式，按顺序检测当前浏览器是否支持该播放格式

|属性|描述|
|------|------|
|controls|是否显示控件|
|loop|是否循环播放|
|preload|是否预加载|
|autoplay|是否自动播放|
|width|宽度|
|height|高度|
|src|视频地址|
 
同时video提供DOM操作，可以通过getElementById获取DOM对象
可以触发基本操作及属性，play()，pause()，width，height等

**介绍一个强大的视频插件[videojs](http://videojs.com/)**

## 音频audio
### 基本用法
```
<audio src="song.ogg"></audio> 
```
或者
```
<audio>
  <source src="song.ogg" type="audio/ogg">
  <source src="song.mp3" type="audio/mp4">
</audio> 
```
后面一种是兼容模式，按顺序检测当前浏览器是否支持该播放格式

|属性|描述|
|------|------|
|controls|是否显示控件|
|loop|是否循环播放|
|preload|是否预加载|
|autoplay|是否自动播放|
|src|音频地址|

## 存储
html5在客户端提供了两种存储数据方式:
* localStorage: 没有时间限制的数据存储
* sessionStorage: 针对一个session的数据存储，用户关闭浏览器就会删除


