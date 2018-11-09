# Table

表格布局控件

## 属性

* `columnWidths`: 设置每一列的宽度
* `defaultColumnWidth`: 默认的每一列宽度值，默认情况下均分
* `textDirection`: 文字方向，一般无需考虑
* `border`: 表格边框
* `defaultVerticalAlignment`: 每一个 cell 的垂直方向的 alignment
* `textBaseline`: defaultVerticalAlignment 为 baseline 的时候，会用到这个属性

## 示例

```dart
Table(
  columnWidths: const <int, TableColumnWidth>{
    0: FixedColumnWidth(50.0),
    1: FixedColumnWidth(100.0),
    2: FixedColumnWidth(50.0),
    3: FixedColumnWidth(100.0),
  },
  border: TableBorder.all(color: Colors.red, width: 1.0, style: BorderStyle.solid),
  children: const <TableRow>[
    TableRow(
      children: <Widget>[
        Text('A1'),
        Text('B1'),
        Text('C1'),
        Text('D1'),
      ],
    ),
    TableRow(
      children: <Widget>[
        Text('A2'),
        Text('B2'),
        Text('C2'),
        Text('D2'),
      ],
    ),
    TableRow(
      children: <Widget>[
        Text('A3'),
        Text('B3'),
        Text('C3'),
        Text('D3'),
      ],
    ),
  ],
)
```

![table](https://i.imgur.com/ek9NBnP.png)