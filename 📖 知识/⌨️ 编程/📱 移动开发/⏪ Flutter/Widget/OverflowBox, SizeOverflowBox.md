# OverflowBox, SizedOverflowBox

## OverflowBox

允许 child 比自身大的控件

### 属性

* alignment: 对齐方式。
* minWidth: 允许child的最小宽度。如果child宽度小于这个值，则按照最小宽度进行显示。
* maxWidth: 允许child的最大宽度。如果child宽度大于这个值，则按照最大宽度进行展示。
* minHeight: 允许child的最小高度。如果child高度小于这个值，则按照最小高度进行显示。
* maxHeight: 允许child的最大高度。如果child高度大于这个值，则按照最大高度进行展示。

### 示例

```dart
Container(
  color: Colors.green,
  width: 200.0,
  height: 200.0,
  padding: const EdgeInsets.all(5.0),
  child: OverflowBox(
    alignment: Alignment.topLeft,
    maxWidth: 300.0,
    maxHeight: 500.0,
    child: Container(
      color: Color(0x33FF00FF),
      width: 400.0,
      height: 400.0,
    ),
  ),
)
```

![overflow-box](https://i.imgur.com/0MnsOHQ.png)

## SizedOverflowBox

SizedBox 与 OverflowBox的结合体。

### 属性

* `size`: 固定的尺寸
* `alignment`: 对齐方式