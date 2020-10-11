# position

（static、relative、fixed、absolute）

## static

1. **static** 是默认值。表示没有定位，或者说不算具有定位属性。
2. 如果元素 **position** 属性值为 **static**（或者未设 **position** 属性），该元素出现在正常的流中（忽略 **top**, **bottom**, **left**, **right** 或者 **z-index** 声明）。

## relative

1. relative** 生成相对定位的元素，相对于其正常位置进行定位。
2. 相对定位完成的过程如下：

- 首先按默认方式（**static**）生成一个元素(并且元素像层一样浮动了起来)。
- 然后相对于以前的位置移动，移动的方向和幅度由 **left**、**right**、**top**、**bottom** 属性确定，偏移前的位置保留不动。

## absolute

1. **absolute** 生成绝对定位的元素。
2. 绝对定位的元素使用 **left**、**right**、**top**、**bottom** 属性相对于其最接近的一个具有定位属性的父元素进行绝对定位。
3. 如果不存在这样的父元素，则相对于 **body** 元素，即相对于浏览器窗口。
4. **让标题元素相对于它的父容器做绝对定位（注意父容器 position 要设置为 relative）！！！！！！！！！**