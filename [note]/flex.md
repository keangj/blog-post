---
title: flex
date: 2018-05-12 01:48:03
tags:
---
来源http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

##### flex-direction属性

`flex-direction`属性决定主轴的方向（即项目的排列方向）。

``` css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

它可能有4个值。

- `row`（默认值）：主轴为水平方向，起点在左端。
- `row-reverse`：主轴为水平方向，起点在右端。
- `column`：主轴为垂直方向，起点在上沿。
- `column-reverse`：主轴为垂直方向，起点在下沿。

##### flex-wrap属性

默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

``` css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
nowrap（默认）：不换行。
wrap：换行，第一行在上方。
wrap-reverse：换行，第一行在下方。