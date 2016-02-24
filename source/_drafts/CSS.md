title: CSS
tags: [CSS]
---
CSS指层叠样式表(Cascading Style Sheets)，主要定义如何显示HTML元素

## 语法
CSS规则由两个部分组成：**选择器**、**一条或者多条声明**
```
selector {declaration1; declaration2; ... declarationN}
```
选择器就是需要改变样式的HTML元素

每条声明由属性和值组成，格式为：property : value;

## 值不同的写法和单位
红色可以用red来表示，也可以用十六进制表示：#ff0000
```
p { color : #ff0000}
```

为了节约字节，也可以使用缩写
```
p { color : #ff}
```

也可以使用RGB来表示
```
p { color : rgb(255, 0, 0) }
p { color : rgb(100%, 0％, 0％) }
```
注：当使用RGB百分比来表示时，必须有％，即使是0；其他当值为0时，单位可以省略；

## 高级用法
### 选择器分组
对选择器进行分组，这样被分组的选择器共享相同的声明，需要用逗号将被分组的选择器分开，比如：h1-h6标题颜色定义为绿色
```
h1,h2,h3,h4,h5,h6 {
    color : green;
}
```


