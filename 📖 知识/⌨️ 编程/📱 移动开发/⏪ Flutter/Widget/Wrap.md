# Wrap

可以实现多行 Row 或者多列 Column 的效果, 类似前端的 Flexbox

## 属性

* `direction`: 主轴（mainAxis）的方向，默认为水平。
* `alignment`: 主轴方向上的对齐方式，默认为 start。
* `spacing`: 主轴方向上的间距。
* `runAlignment`: run 的对齐方式。run 可以理解为新的行或者列，如果是水平方向布局的话，run 可以理解为新的一行。
* `runSpacing`: run的间距。
* `crossAxisAlignment`: 交叉轴（crossAxis）方向上的对齐方式。
* `textDirection`: 文本方向。
* `verticalDirection`: 定义了 children 摆放顺序，默认是 down，见 Flex 相关属性介绍

## 示例

```dart
Wrap(
  spacing: 8.0, // gap between adjacent chips
  runSpacing: 4.0, // gap between lines
  children: <Widget>[
    Chip(
      avatar: CircleAvatar(
          backgroundColor: Colors.blue.shade900, child: new Text('AH', style: TextStyle(fontSize: 10.0),)),
      label: Text('Hamilton'),
    ),
    Chip(
      avatar: CircleAvatar(
          backgroundColor: Colors.blue.shade900, child: new Text('ML', style: TextStyle(fontSize: 10.0),)),
      label: Text('Lafayette'),
    ),
    Chip(
      avatar: CircleAvatar(
          backgroundColor: Colors.blue.shade900, child: new Text('HM', style: TextStyle(fontSize: 10.0),)),
      label: Text('Mulligan'),
    ),
    Chip(
      avatar: CircleAvatar(
          backgroundColor: Colors.blue.shade900, child: new Text('JL', style: TextStyle(fontSize: 10.0),)),
      label: Text('Laurens'),
    ),
  ],
)
```

![wrap](https://i.imgur.com/LiVL5ZX.png)