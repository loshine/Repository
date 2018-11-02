# Baseline

一个用于调整文字基线的控件

![baseline](https://i.imgur.com/67VuNOn.png)

Baseline 控件布局行为分为两种情况：

* 如果 child 有 baseline，则根据 child 的 baseline 属性，调整 child 的位置；
* 如果 child 没有 baseline，则根据 child 的 bottom，来调整 child 的位置。

## 属性

* baseline：baseline 数值，必须要有，从顶部算。
* baselineType：bseline 类型，也是必须要有的，目前有两种类型：
    1. alphabetic：对齐字符底部的水平线；
    2. ideographic：对齐表意字符的水平线。

## 示例

```dart
new Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: <Widget>[
    new Baseline(
      baseline: 50.0,
      baselineType: TextBaseline.alphabetic,
      child: new Text(
        'TjTjTj',
        style: new TextStyle(
          fontSize: 20.0,
          textBaseline: TextBaseline.alphabetic,
        ),
      ),
    ),
    new Baseline(
      baseline: 50.0,
      baselineType: TextBaseline.alphabetic,
      child: new Container(
        width: 30.0,
        height: 30.0,
        color: Colors.red,
      ),
    ),
    new Baseline(
      baseline: 50.0,
      baselineType: TextBaseline.alphabetic,
      child: new Text(
        'RyRyRy',
        style: new TextStyle(
          fontSize: 35.0,
          textBaseline: TextBaseline.alphabetic,
        ),
      ),
    ),
  ],
)
```

![baseline-example](https://i.imgur.com/Y255rz2.png)