# GridView

一个滚动的多列列表

## 构造方法

其默认构造方法如下：

```dart
GridView({
  Key key,
  Axis scrollDirection = Axis.vertical,
  bool reverse = false,
  ScrollController controller,
  bool primary,
  ScrollPhysics physics,
  bool shrinkWrap = false,
  EdgeInsetsGeometry padding,
  @required this.gridDelegate,
  bool addAutomaticKeepAlives = true,
  bool addRepaintBoundaries = true,
  double cacheExtent,
  List<Widget> children = const <Widget>[],
})
```

同时也提供了如下额外的四种构造方法，方便开发者使用。

```dart
GridView.builder
GridView.custom
GridView.count
GridView.extent
```

## 属性解析

* `scrollDirection`：滚动的方向，有垂直和水平两种，默认为垂直方向（Axis.vertical）。
* `reverse`：默认是从上或者左向下或者右滚动的，这个属性控制是否反向，默认值为 false，不反向滚动。
* `controller`：控制 child 滚动时候的位置。
* `primary`：是否是与父节点的 PrimaryScrollController 所关联的主滚动视图。
* `physics`：滚动的视图如何响应用户的输入。
* `shrinkWrap`：滚动方向的滚动视图内容是否应该由正在查看的内容所决定。
* `padding`：四周的空白区域。
* `gridDelegate`：控制 GridView 中子节点布局的 delegate。
* `cacheExtent`：缓存区域。
