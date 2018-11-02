# Align, Center

## Align

设置 child 的对齐方式，例如居中、居左居右等，并根据 child 尺寸调节自身尺寸。

```dart
const Align({
    Key key,
    this.alignment: Alignment.center,
    this.widthFactor,
    this.heightFactor,
    Widget child
  })
```

### 属性解析

* alignment: 可以使用定义好的`Alignment.center`等9个值，或者`Alignment(1.0, 0.5)`代表居右高于底部 1/4 处。
* widthFactor: 宽度因子，如果设置的话，Align 的宽度就是 child 的宽度乘以这个值，不能为负数。
* heightFactor: 高度因子，如果设置的话，Align 的高度就是 child 的高度乘以这个值，不能为负数。

## Center

child 会默认居中，并且尽量占满 parent 的大小。