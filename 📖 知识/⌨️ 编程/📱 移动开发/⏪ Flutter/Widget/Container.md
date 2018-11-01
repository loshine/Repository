# Container

Container 是一个结合了绘制（painting）、定位（positioning）以及尺寸（sizing）widget 的 widget。

![container](https://i.imgur.com/pBgjG7I.png)

```dart
Container({
    Key key,
    this.alignment,
    this.padding,
    Color color,
    Decoration decoration,
    this.foregroundDecoration,
    double width,
    double height,
    BoxConstraints constraints,
    this.margin,
    this.transform,
    this.child,
  })
```

## 属性解析

* **key**：Container 唯一标识符，用于查找更新。
* **alignment**：控制 child 的对齐方式，如果 container 或者 container 父节点尺寸大于 child 的尺寸，这个属性设置会起作用，有很多种对齐方式。
* **padding**：decoration 内部的空白区域，如果有 child 的话，child 位于 padding 内部。
* **color**：用来设置 container 背景色，如果 foregroundDecoration 设置的话，可能会遮盖 color 效果。
* **decoration**：绘制在 child 后面的装饰，设置了 decoration 的话，就不能设置 color 属性，否则会报错，此时应该在 decoration 中进行颜色的设置。
* **foregroundDecoration**：绘制在 child 前面的装饰。
* **width**：container 的宽度，设置为`double.infinity`可以强制在宽度上撑满，不设置，则根据 child 和父节点两者一起布局。
* **height**：container 的高度，设置为`double.infinity`可以强制在高度上撑满。
* **constraints**：添加到 child 上额外的约束条件。
* **margin**：围绕在 decoration 和 child 之外的空白区域，不属于内容区域。
* **transform**：设置 container 的变换矩阵，类型为 Matrix4。
* **child**：container 中的内容 widget。

## 例子

```dart
new Container(
  constraints: new BoxConstraints.expand(
    height:Theme.of(context).textTheme.display1.fontSize * 1.1 + 200.0,
  ),
  decoration: new BoxDecoration(
    border: new Border.all(width: 2.0, color: Colors.red),
    color: Colors.grey,
    borderRadius: new BorderRadius.all(new Radius.circular(20.0)),
    image: new DecorationImage(
      image: new NetworkImage('http://h.hiphotos.baidu.com/zhidao/wh%3D450%2C600/sign=0d023672312ac65c67506e77cec29e27/9f2f070828381f30dea167bbad014c086e06f06c.jpg'),
      centerSlice: new Rect.fromLTRB(270.0, 180.0, 1360.0, 730.0),
    ),
  ),
  padding: const EdgeInsets.all(8.0),
  alignment: Alignment.center,
  child: new Text('Hello World',
    style: Theme.of(context).textTheme.display1.copyWith(color: Colors.black)),
  transform: new Matrix4.rotationZ(0.3),
)
```