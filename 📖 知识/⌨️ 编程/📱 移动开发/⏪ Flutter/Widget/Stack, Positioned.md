# Stack, Positioned

## Stack

类似 FrameLayout 的 widget。

```dart
Stack({
  Key key,
  this.alignment = AlignmentDirectional.topStart,
  this.textDirection,
  this.fit = StackFit.loose,
  this.overflow = Overflow.clip,
  List<Widget> children = const <Widget>[],
})
```

### 属性解析

#### alignment

如何对齐 Stack 中未定位和部分定位的子项。

#### fit

如何调整 Stack 的未定位子项的大小。

## Positioned

通过子控件的`top`,`right`,`bottom`和`left`属性将它们定位在 Stack 控件不同位置处。

```dart
const Positioned({
  Key key,
  this.left,
  this.top,
  this.right,
  this.bottom,
  this.width,
  this.height,
  @required Widget child,
})
```