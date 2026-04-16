# 表格

简要说明：面向中文 Typst 用户，汇总常见表格问题与模式，并给出最常用的原生 Typst 实现示例。

## 基本语法
简单表格通常指定列数，提供表头并按行写入数据。可以通过列宽表达式实现自适应或固定宽度。
```typst
#table(
  columns: 3,
  // stroke: none, // 如需去边框，请取消注释
  table.header([Header1], [Header2], [Header3]),
  [数据11], [数据12], [数据13],
  [数据21], [数据22], [数据23],
)
```
- Key:
- columns: 列数或带单位的宽度表达式，例如 `(1fr, 1fr, 1fr)`，也可书写 3 表示三列。
- table.header(): 第一行为表头。
- 行数据按列顺序写入，每个单元格用逗号分隔。
⚠️ 提示：需要跨列或跨行时，可使用 table.cell(colspan: n, …) / table.cell(rowspan: n, …) 等参数。

## 三线表
问题：在中文学术论文中，常用的三线表风格，通过隐藏边框并仅用横线完成。
```typst
#table(
  columns: 3,
  stroke: none,
  table.hline(),
  table.header([a], [b], [c]),
  table.hline(stroke: 0.5pt),
  [d], [e], [f],
  [g], [h], [i],
  table.hline(),
)
```
- 说明：先隐藏外边框，再通过 table.hline() 画线实现三线表效果。

## 续表
问题：跨页时需要在新页表头前显示续表提示。
```typst
#let xubiao = state("xubiao")
#set table(stroke: (x, y) => { if y == 0 {none} else {1pt} })
#show table: it => xubiao.update(false) + it

#table(
  columns: 3,
  table.header(
    table.cell(colspan: 3, {
      context if xubiao.get() {
        align(right)[续表]
      } else {
        v(-0.9em)
        xubiao.update(true)
      }
    }),
    [标题1], [标题2], [标题3],
  ),
  ..for i in range(5){
    ([11111], [22222], [33333],)
  }
)
```
- 说明：通过状态控制续表提示的显示与隐藏。

## 斜线表头
问题：表头单元格绘制对角线以分割信息。
```typst
#table(
  columns: 2,
  table.cell(inset: 0pt, {
    place(top + left, metadata("__th"))
    place(bottom + right, metadata("__th"))
    context {
      let (p1, p2) = query(metadata.where(value: "__th")).slice(-2).map(e => e.location().position())
      let dx = p2.at("x") - p1.at("x")
      let dy = p2.at("y") - p1.at("y")
      line(end: (dx, dy))
    }
  }),
  [111\ 1], [22222],
  [3], [4],
)
```
- 说明：通过元数据测量单元格，绘制对角线实现斜线区域。

⚠️ 提示：对角线的尺寸需要随单元格大小动态调整，某些场景可能需要额外的布局技巧。

## 单元格对齐
问题：实现左/中/右对齐，以及等宽列的控制。
```typst
文本左对齐#h(1fr)文本居中#h(1fr)文本右对齐

左#h(1fr)居中#h(1fr)右对齐

#place[左#h(1fr)右对齐]#align(center)[居中]
```
- 说明：结合 h(1fr) 和 place/align 可实现灵活对齐。

## 圆角表格
问题：需要圆角外观的表格。
```typst
-- #set page(width: 10cm, height: auto)
#let stroke = stroke(2pt + gradient.linear(..color.map.plasma))
#show table: it => block(stroke: stroke, radius: 2em, clip: true, it)

#table(
  columns: (1fr,) * 5,
  rows: 3em,
  stroke: stroke,
  align: (x, y) => {
    if (calc.odd(x + y)) {
      left + top
    } else {
      right + bottom
    }
  },
  [A], [B], [C], [D], [E],
  [F], [G], [H], [I], [J],
  [K], [L], [M], [N], [O],
  [P], [Q], [R], [S], [T],
  [U], [V], [W], [X], [Y],
  [Z],
)
```
- 说明：通过圆角块与 clip 设置实现圆角外观，表格内部对齐通过自定义 align 来实现。

## 自动列宽
问题：列宽根据内容自适应。
```typst
#let choices(..the-choices, gutter: 1em) = layout(available => {
  let cell-width = calc.max(..the-choices.pos().map(it => measure(it).width))
  let ratio = ((available.width + gutter).to-absolute().pt()) / (cell-width + gutter).to-absolute().pt()
  let n = calc.min(4, calc.floor(ratio))
  if n == 3 { n = 2 }

  grid(
    columns: (1fr,) * n,
    gutter: gutter,
    ..the-choices
  )
})

#set page(width: 20em, margin: 1em, height: auto)

= 每行一项共四行
#choices[春江潮水连海平！！！！][B][C][D]

= 每行两项共两行
#choices[春江潮水连海平][B][C][D]

= 每行四项共一行
#choices[春江潮][B][C][D]
```
⚠️ 提示：通过 layout/grid 机制实现自适应列宽，避免固定宽度造成内容裁切。

## 数组转置
问题：将数据矩阵转置以适配表格输出。
```typst
#show: columns.with(2)
#let data = range(8).map(x => [#x]).chunks(4) // 2x4 矩阵
#table(columns: 4, ..data.flatten())
#colbreak()

+#table(columns: 2, ..array.zip(..data).flatten())
```
- 说明：通过数据变换实现转置效果，最后再将扁平化数据写入表格。

⚠️ 提示：对大型数据集，考虑先在内存中构造转置结果再渲染。
