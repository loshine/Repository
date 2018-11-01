# Column, Row

最常见的布局模式之一是垂直或水平排列 widget。您可以使用行（Row）水平排列 widget，并使用列（Column）垂直排列 widget。

## 对齐 widgets

您可以控制行或列如何使用`mainAxisAlignment`和`crossAxisAlignment`属性来对齐其子项。

对于行（Row）来说，主轴是水平方向，横轴垂直方向。对于列（Column）来说，主轴垂直方向，横轴水平方向。

![column-row](https://i.imgur.com/SdhKhsL.png)

## 调整 widget

使用 Expanded 可以在 Column 或 Row 中调整其子 widget 的弹性系数。

```dart
class Expanded extends Flexible {
  /// Creates a widget that expands a child of a [Row], [Column], or [Flex]
  /// expand to fill the available space in the main axis.
  const Expanded({
    Key key,
    int flex = 1,
    @required Widget child,
  }) : super(key: key, flex: flex, fit: FlexFit.tight, child: child);
}
```

## 聚集 widgets

默认情况下，行或列沿着其主轴会尽可能占用尽可能多的空间，但如果要将子控件紧密聚集在一起，可以将`mainAxisSize`设置为`MainAxisSize.min`。