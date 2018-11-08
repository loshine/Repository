# ListBody

按给定的轴方向，按照顺序排列子节点，通常配合 ListView 或 Column 使用。

在主轴上，子节点按照顺序进行布局，在交叉轴上，子节点尺寸会被拉伸，以适应交叉轴的区域。

在主轴上，给予子节点的空间必须是不受限制的（unlimited），使得子节点可以全部被容纳，ListBody不会去裁剪或者缩放其子节点。

## 属性

* `mainAxis`: 排列的主轴方向
* `reverse`: 是否反向

## 示例

```dart
Flex(
  direction: Axis.vertical,
  children: <Widget>[
    ListBody(
      mainAxis: Axis.vertical,
      reverse: false,
      children: <Widget>[
        Container(color: Colors.red, width: 50.0, height: 50.0,),
        Container(color: Colors.yellow, width: 50.0, height: 50.0,),
        Container(color: Colors.green, width: 50.0, height: 50.0,),
        Container(color: Colors.blue, width: 50.0, height: 50.0,),
        Container(color: Colors.black, width: 50.0, height: 50.0,),
      ],
  )],
)
```