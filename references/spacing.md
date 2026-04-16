# 间距

## 行距

行高 = leading + 文字尺寸。中文黄金行高为字号 1.5–1.75 倍。

```typst
// 小四号（12pt）正文：行高约 20pt
#set par(leading: 0.67em)

// 五号（10.5pt）正文：行高约 17pt
#set text(size: 10.5pt)
#set par(leading: 0.62em)
```

设置 `top-edge` / `bottom-edge` 使文字外框更接近中文习惯：

```typst
#set text(top-edge: "ascender", bottom-edge: "descender")
```

## "行"的视觉定义

行的可见高度受 `text` 的 `top-edge` 和 `bottom-edge` 影响。西文默认用 `cap-height` / `baseline`，中文习惯用 `ascender` / `descender`：

```typst
// 西文习惯（默认）
#set text(top-edge: "cap-height", bottom-edge: "baseline")

// 中文习惯（推荐）
#set text(top-edge: "ascender", bottom-edge: "descender")
```

## 段距

段间距 > 行高，清晰区分段落。不要用空行控制段距。

```typst
#set par(spacing: 0.75em)
// 标题段前段后距
#show heading.where(level: 1): set block(above: 24pt, below: 18pt)
#show heading.where(level: 2): set block(above: 12pt, below: 6pt)
```

⚠️ `#show par: set block(spacing: ..)` 在 Typst 0.12+ 已废弃，用 `#set par(spacing: ..)`。

## Word 行距换算

Word 和 Typst 的行距模型差异很大：

- Word 行距实际是行高，行高 = 文字尺寸 + 行距
- Word "单倍行距"与文字尺寸的比值依赖字体，在 1.14–1.92 间浮动
- Typst 的 `1em` 始终等于当前字号
- Word 有行网格机制，会四舍五入行距

**不要纠结换算公式**。推荐做法：写满一页纸，调整 `leading` 数值，让每页行数与预期一致。

```typst
// 小四号正文，约 28 行/页
#set text(size: 12pt)
#set par(leading: 0.67em)
```

## 两端对齐

```typst
#set par(justify: true)
```

中文方块字必须两端对齐，右侧参差不齐破坏版面。

## 字距

正文用默认。短标题可加宽（≤3pt）：

```typst
#show heading.where(level: 1): set text(tracking: 1.5pt)
```

## 分散对齐

在字符间插入等宽空隙：

```typst
#set text(tracking: 2pt)
[分散对齐]
```

⚠️ 正文不建议使用，仅用于标题等短文本。

## 行内公式与中文间距

Typst 不自动在行内公式与中文间插入间距：

```typst
#show math.equation.where(block: false): it => {
  h(0.25em, weak: true) + it + h(0.25em, weak: true)
}
```

## 代码块两端对齐

Typst 0.13+ 已默认修复。旧版本：

```typst
#show raw.where(block: true): set par(justify: false)
```
