# Transform

进行矩阵变换的 widget

## 构造方法

```dart
const Transform({
  Key key,
  @required this.transform,
  this.origin,
  this.alignment,
  this.transformHitTests = true,
  Widget child,
})
```

Transform 也提供下面三种构造函数：

```dart
Transform.rotate
Transform.translate
Transform.scale
```

## 属性

* `transform`: 一个4x4的矩阵。不难发现，其他平台的变换矩阵也都是四阶的。一些复合操作，仅靠三维是不够的，必须采用额外的一维来补充，感兴趣的同学可以自行搜索了解
* `origin`: 旋转点，相对于左上角顶点的偏移。默认旋转点事左上角顶点
* `alignment`: 对齐方式
* `transformHitTests`: 点击区域是否也做相应的改变

## 示例

```dart
Center(
  child: Transform(
    transform: Matrix4.rotationZ(0.3),
    child: Container(
      color: Colors.blue,
      width: 100.0,
      height: 100.0,
    ),
  ),
)
```