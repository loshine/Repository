# FittedBox, AspectRatio, ConstrainedBox

## FittedBox

调整子控件缩放显示模式的控件。

```dart
const FittedBox({
Key key,
this.fit: BoxFit.contain,
this.alignment: Alignment.center,
Widget child,
})
```

### 属性解析

#### fit

缩放方式，，默认的属性是`BoxFit.contain`

![scale-type](https://i.imgur.com/3rg3foi.png)

## AspectRatio

控制子控件宽高比的控件。

```dart
const AspectRatio({
Key key,
@required this.aspectRatio,
Widget child
})
```

## ConstrainedBox

添加额外的限制条件（constraints）到 child 上的控件

```dart
ConstrainedBox({
Key key,
@required this.constraints,
Widget child
})
```

## 示例

```dart
new ConstrainedBox(
  constraints: const BoxConstraints(
    minWidth: 100.0,
    minHeight: 100.0,
    maxWidth: 150.0,
    maxHeight: 150.0,
  ),
  child: new Container(
    width: 200.0,
    height: 200.0,
    color: Colors.red,
  ),
);
```