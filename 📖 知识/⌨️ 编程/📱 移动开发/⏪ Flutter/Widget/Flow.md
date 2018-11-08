# Flow

一个实现流式布局算法的控件。

## 示例

```dart
const width = 80.0;
const height = 60.0;

Flow(
  delegate: TestFlowDelegate(margin: EdgeInsets.fromLTRB(10.0, 10.0, 10.0, 10.0)),
  children: <Widget>[
    new Container(width: width, height: height, color: Colors.yellow,),
    new Container(width: width, height: height, color: Colors.green,),
    new Container(width: width, height: height, color: Colors.red,),
    new Container(width: width, height: height, color: Colors.black,),
    new Container(width: width, height: height, color: Colors.blue,),
    new Container(width: width, height: height, color: Colors.lightGreenAccent,),
  ],
)

class TestFlowDelegate extends FlowDelegate {
  EdgeInsets margin = EdgeInsets.zero;

  TestFlowDelegate({this.margin});
  @override
  void paintChildren(FlowPaintingContext context) {
    var x = margin.left;
    var y = margin.top;
    for (int i = 0; i < context.childCount; i++) {
      var w = context.getChildSize(i).width + x + margin.right;
      if (w < context.size.width) {
        context.paintChild(i,
            transform: new Matrix4.translationValues(
                x, y, 0.0));
        x = w + margin.left;
      } else {
        x = margin.left;
        y += context.getChildSize(i).height + margin.top + margin.bottom;
        context.paintChild(i,
            transform: new Matrix4.translationValues(
                x, y, 0.0));
        x += context.getChildSize(i).width + margin.left + margin.right;
      }
    }
  }

  @override
  bool shouldRepaint(FlowDelegate oldDelegate) {
    return oldDelegate != this;
  }
}
```

![flow](https://i.imgur.com/iT5MemW.png)

## delegate

Flow 最重要的属性就是 delegate 了，它决定了 children 的排列方式。

它包含如下几个方法：

* `getConstraintsForChild`: 设置每个 child 的布局约束条件，会覆盖已有的
* `getSize`: 设置Flow的尺寸
* `paintChildren`: child 的绘制控制代码，可以调整尺寸位置，写起来比较的繁琐
* `shouldRepaint`: 是否需要重绘
* `shouldRelayout`: 是否需要重新布局
