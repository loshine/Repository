# ConstrainedBox

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