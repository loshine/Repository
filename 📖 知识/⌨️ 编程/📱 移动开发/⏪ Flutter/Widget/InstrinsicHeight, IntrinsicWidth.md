# IntrinsicHeight, IntrinsicWidth

## IntrinsicHeight

将高度不受限制的控件调整到固定的大小。

![intrinsic-height](https://i.imgur.com/7RiLHzP.png)

## IntrinsicWidth

将宽度不受限制的控件调整到固定的大小

* stepHeight: 不是 null 的时候, child 的高度是 stepHeight 的倍数;当 stepHeight 小于最小高度或 null 的时候, child 的高度取最大高度;
* stepWidth: 不是 null 的时候, child 的宽度将会是 stepWidth 的倍数;当 stepWidth 值比 child 最小宽度小或为 null 的时候, child 的宽度是最小宽度;

![intrinsic-width](https://i.imgur.com/UDfuzAh.png)