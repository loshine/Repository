# 概述

随意点开一个 Widget，就会发现，基本都会有一个参数`Key`

> Flutter 是受 React 启发的，所以 Virtual Dom 的 diff 算法也有参考。在 diff 的过程中如果节点有 Key 来比较的话，能够最大程度重用已有的节点，除了这一点这个 Key 也用在很多其他的地方。总之，这里我们可以知道 key 能够提高性能，所以每个 Widget 都会构建方法都会有一个 key 的参数可选，贯穿着整个框架。

通常情况下，我们不需要去传递这个Key，因为 framework 会在内部自处理它，来区分不同的 Widgets

## ObjectKey 和 ValueKey

这两个 Key 可以用来对组件进行区分

## GlobalKey

GlobalKey 的作用是从父控件和跨子 Widget 来传递状态。

```dart
class _MyHomePageState extends State<MyHomePage> {
  final globalKey = new GlobalKey<ScaffoldState>();

  void _incrementCounter() {
    globalKey.currentState.showSnackBar(SnackBar(content: Text('I am context from Scaffold')));
  }

 @override
  Widget build(BuildContext context) {
    return new Scaffold(
        key: globalKey,
      //...
      );
  }
}
```