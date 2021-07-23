---
title: Media Queries
date: 2018-02-18 18:11:46
updated:
tags: CSS
categories: CSS
cover: https://titangene.github.io/images/cover/css.png
---
## Media Queries 
### Media Queries使用方法
```
@media 媒体类型and （媒体特性）{你的样式}
```
注意：使用Media Queries必须要使用“@media”开头，然后指定媒体类型（也可以称为设备类型），随后是指定媒体特性（也可以称之为设备特性）。 媒体特性的书写方式和样式的书写方式非常相似，主要分为两个部分，第一个部分指的是媒体特性，第二部分为媒体特性所指定的值，而且这两个部分之间使用冒号分隔。例如：`(max-width: 480px)`

#### 媒体特性
1. 最大宽度“max-width”(max-width 表示最大即小于等于)是媒体特性中最常用的一个特性，其意思是指媒体类型小于或等于指定的宽度时，样式生效。如：
```
// 当屏幕小于或等于480px时,页面中的广告区块（.ads）都将被隐藏。
@media screen and (max-width:480px){
 .ads {
   display:none;
  }
}
```

2. 最小宽度“min-width”(min-width 表示最小即大于等于)与“max-width”相反，指的是媒体类型大于或等于指定宽度时，样式生效。
```
// 当屏幕大于或等于900px时，容器“.wrapper”的宽度为980px。
@media screen and (min-width:900px){
  .wrapper{width: 980px;}
}
```

3. 多个媒体特性使用关键词"and"将多个媒体特性结合在一起。也就是说，一个Media Query中可以包含0到多个表达式，表达式又可以包含0到多个关键字，以及一种媒体类型。
```
// 当屏幕在600px~900px之间时，body的背景色渲染为“#f5f5f5”
@media screen and (min-width:600px) and (max-width:900px){
  body {background-color:#f5f5f5;}
}
```
4. 在智能设备上，例如iPhone、iPad等，还可以根据屏幕设备的尺寸来设置相应的样式（或者调用相应的样式文件）。同样的，对于屏幕设备同样可以使用“min/max”对应参数，如“min-device-width”或者“max-device-width”。
```
// “iphone.css”样式适用于最大设备宽度为480px
<link rel="stylesheet" media="screen and (max-device-width:480px)" href="iphone.css" />
```
5. 使用关键词“not”是用来排除某种制定的媒体类型，也就是用来排除符合表达式的设备。换句话说，not关键词表示对后面的表达式执行取反操作，如：
```
// 样式代码将被使用在除打印设备和设备宽度小于1200px下所有设备中。
@media not print and (max-width: 1200px){样式代码}
```
6. only用来指定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器。其实only很多时候是用来对那些不支持Media Query但却支持Media Type的设备隐藏样式表的。其主要有：支持媒体特性的设备，正常调用样式，此时就当only不存在；表示不支持媒体特性但又支持媒体类型的设备，这样就会不读样式，因为其先会读取only而不是screen；另外不支持Media Queries的浏览器，不论是否支持only，样式都不会被采用。如:
```
<linkrel="stylesheet" media="only screen and (max-device-width:240px)" href="android240.css" />
```
在Media Query中如果没有明确指定Media Type，那么其默认为all，如：
```
<linkrel="stylesheet" media="(min-width:701px) and (max-width:900px)" href="mediu.css" />
```
另外在样式中，还可以使用多条语句来将同一个样式应用于不同的媒体类型和媒体特性中，指定方式如下所示。
```
// style.css样式被用在宽度小于或等于480px的手持设备上，或者被用于屏幕宽度大于或等于960px的设备上。
<linkrel="stylesheet" type="text/css" href="style.css" media="handheld and (max-width:480px), screen and (min-width:960px)" />
```