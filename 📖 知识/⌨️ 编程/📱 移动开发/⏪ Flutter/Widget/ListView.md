# ListView

滚动的列表控件

## 构造函数

```dart
ListView({
  Key key,
  Axis scrollDirection = Axis.vertical,
  bool reverse = false,
  ScrollController controller,
  bool primary,
  ScrollPhysics physics,
  bool shrinkWrap = false,
  EdgeInsetsGeometry padding,
  this.itemExtent,
  bool addAutomaticKeepAlives = true,
  bool addRepaintBoundaries = true,
  double cacheExtent,
  List<Widget> children = const <Widget>[],
})
```

同时也提供了如下额外的三种构造方法，方便开发者使用。

```dart
ListView.builder
ListView.separated
ListView.custom
```

## 示例

```dart
ListView(
  shrinkWrap: true,
  padding: EdgeInsets.all(20.0),
  children: <Widget>[
    Text('I\'m dedicating every day to you'),
    Text('Domestic life was never quite my style'),
    Text('When you smile, you knock me out, I fall apart'),
    Text('And I thought I was so smart'),
  ],
)
```

```dart
ListView.builder(
  itemCount: 1000,
  itemBuilder: (context, index) {
    return ListTile(
      title: Text("$index"),
    );
  },
)
```