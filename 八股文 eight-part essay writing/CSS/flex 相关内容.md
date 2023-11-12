---
title: flex 相关内容
url: https://www.yuque.com/wcywxq/mxunh7/kwodx6
---

<a name="kXWxz"></a>

## flex 的两个轴

- 水平的主轴 (main axis)
- 垂直的交叉轴 (cross axis) <a name="wQfyt"></a>

## flex-flow

flex-direction 和 flex-wrap 的集合。默认值：`row nowrap` <a name="Hd5wl"></a>

## align-content

用于控制多行项目的对齐方式，如果项目只有一行则不会起作用 <a name="q8mb6"></a>

## order

用于决定项目的排列顺序，数值越小，项目排列越靠前。默认为 0 <a name="It1wp"></a>

## flex-grow

用于决定项目在有剩余空间的情况下是否放大。默认为 0，不放大
:::warning
即使设置了固定宽度，也会放大
::: <a name="GB8va"></a>

## flex-shrink

用于决定项目在空间不足时是否缩小。默认为 1，即空间不足时一起等比例缩小
:::warning
即使设置了固定宽度，也会缩小
::: <a name="P9Nk4"></a>

## flex-basics

用于设置项目宽度。默认为 auto，即项目会保持默认宽度，或者以 width 为自身宽度，然而如果设置了 flex-basis，权重就会比 width 属性高，因此会覆盖 width 属性 <a name="eBWLC"></a>

## flex

flex 属性是 flex-grow、flex-shrink 与 flex-basis 三个属性的简写，用于定义项目放大、缩小比例与宽度。默认为 `0 1 auto` <a name="lEssa"></a>

## align-self

表示继承父容器的 align-items 属性。如果没有父元素，则默认为 stretch，用于让个别项目拥有与其它项目不同的对齐方式，各个值的表现与父容器的 align-items 属性完全一致
