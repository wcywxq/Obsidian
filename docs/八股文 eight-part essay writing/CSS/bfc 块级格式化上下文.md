---
title: bfc 块级格式化上下文
url: https://www.yuque.com/wcywxq/mxunh7/px9dg8
---

<a name="m2clJ"></a>

## 概念

bfc 是块级格式化上下文，根据盒模型可知，每个元素都被定义为一个矩形盒子，然而盒子的布局会受到尺寸、定位、盒子的子元素或兄弟元素、视窗的尺寸等因素来决定。因此浏览器会有一个计算的过程，计算的规则是由一个叫做视觉格式化模型的东西所定义的，bfc 就是来自这个概念，它是 css 视觉渲染的一部分，用于决定块级盒的布局以及浮动相互影响范围的一个区域。 <a name="W7FFb"></a>

## bfc 的特性

1. 块级元素会在垂直方向一个接一个的排列，和文档流的排列方式一致
2. 在 bfc 中上下相邻的两个容器的 margin 会重叠，创建新的 bfc 可以避免外边距重叠
3. 计算 bfc 的高度时，需要计算浮动元素的高度
4. bfc 区域不会与浮动的容器发生重叠
5. bfc 是独立的容器，容器内部元素不会影响外部元素
6. 每个元素的左 margin 值和容器的左 border 相接触 <a name="mvHho"></a>

## 创建 bfc 的方式

- 绝对定位的元素(position 为 absolute 或 fixed)
- 行内块元素，即 display 为 inline-block
- overflow 值不为 visible
