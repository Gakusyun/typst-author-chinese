---
name: typst-author-chinese
description: 生成符合中文排版习惯的 Typst（.typ）代码，编辑、调试 Typst 文档与项目，回答 Typst 语法与参考问题。当用户处理 .typ 文件、涉及 Typst 文档创建/编辑/调试/编译/格式化/模板/包的使用，或者涉及中文排版、中文学术论文、中文书籍排版、CTeX 替代、中文字体配置、中文标点处理等场景时，均应使用本 skill。也适用于任何 Typst 语法参考和项目构建需求。即使用户没有明确提到 Typst，当提到"中文论文排版"、"中文字体配置"、"首行缩进"、"标点挤压"等关键词，也应使用本 skill。
---

# typst-author skill（中文排版适配版）

## 最小中文文档

```typst
#set document(title: "我的文档", author: "作者姓名")
#set page(numbering: "1")
#set text(lang: "zh", font: ("New Computer Modern", "SimSun"))
#set par(first-line-indent: (amount: 2em, all: true), justify: true, spacing: 0.75em)
#title[我的文档]

= 第一章
这是一段中文正文。Typst 对中文排版有良好的原生支持。

== 第一节
中文排版需要注意以下要点：字体选择、标点挤压、首行缩进以及行距设置。
```

## 工作流程

1. **新手入门**：使用最小中文文档模板，阅读 [docs/tutorial/writing-in-typst.md](docs/tutorial/writing-in-typst.md) 入门
2. **语法熟悉**：对照 [docs/reference/syntax.md](docs/reference/syntax.md) 和 [docs/reference/scripting.md](docs/reference/scripting.md) 确认语法
3. **样式配置**：参考 [docs/reference/styling.md](docs/reference/styling.md) 完成全局样式，参考 [docs/guides/page-setup.md](docs/guides/page-setup.md) 调整页面设置
4. **中文排版**：查阅 [references/README.md](references/README.md) 索引，按需深入具体章节

### CLI 命令参考

所有 `typst` 子命令的详细用法见 [references/cli/README.md](references/cli/README.md)。

## 中文排版核心要点

1. **语言**：`#set text(lang: "zh")` — 开启标点挤压和中文断行
2. **字体**：先执行 `typst fonts` 确认可用字体，只用确认存在的。优先：宋体 `SimSun`/`Source Han Serif SC`，黑体 `SimHei`/`Source Han Sans SC`，楷体 `KaiTi`，西文 `New Computer Modern`/`Times New Roman`。更多 CLI 用法见 [references/cli/README.md](references/cli/README.md)
3. **首行缩进**：`#set par(first-line-indent: (amount: 2em, all: true))` — `all: true` 确保标题后首段也缩进
4. **行距段距**：`#set par(leading: 0.75em, spacing: 1em)` 好看的排版需要短间距大于行间距。
5. **常用包**：`cuti` 0.4.0（伪粗体）、`pointless-size` 0.1.2（中文字号）。标题编号用内置 `numbering()` 即可（支持 `1.1`、`一、`、`第一章` 等模式）

→ 详细用法和完整模板请查阅 [references/README.md](references/README.md)

## 操作规范

1. **优先查本地文档**再生成代码。训练数据可能过时。
2. 生成/编辑 `.typ` 后执行**格式化检查**（见下方）。
3. 可用 `typst compile` 验证编译。CLI 详细用法参阅 [references/cli/README.md](references/cli/README.md)。

### 探测不确定行为

文档无法确定运行时行为时，用 stdin 探测：

```
echo '#metadata(1 + 2) <probe>' | typst query - "<probe>" --field value --one
```

参见 [docs/reference/introspection/query.md](docs/reference/introspection/query.md) 和 [docs/reference/introspection/metadata.md](docs/reference/introspection/metadata.md)。

### 格式化检查

1. `typstyle` 不可用则跳过
2. 每次修改 `.typ` 后运行 `typstyle --check <file>`
3. 仅对当前编辑的代码使用 `typstyle -i <file>` 格式化
4. 格式化影响未修改代码时停下来询问

## Quick syntax

| 概念 | 语法 | 注意 |
|------|------|------|
| 数组 | `(a, b)` | 圆括号，没有元组 |
| 字典 | `(k: v)` | 带冒号的圆括号 |
| 内容块 | `[markup]` | 方括号 |
| `#` 用法 | 标记模式加 `#`，代码模式不加 | `#figure(image(...))` 不双加 `#` |

### set vs show

- `set`：配置可选参数（`#set text(font: ..)`）
- `show`：选定元素变换（`#show heading: set text(navy)`）

## 常见错误

- `[]` 表示数组（应用 `()`）
- `arr[0]`（应用 `arr.at(0)`）
- 标记块漏 `#`（如 `[numbering(...)]` 应 `[#numbering(...)]`）
- 代码块多加 `#`（如参数列表中）
- `**双星号**` 加粗（Typst 用 `*单星号*`）
- 中文标点在语法结构中（术语列表 `/ 术语：描述` 的 `：` 应为 `:`）
- `prod`（不存在的符号，应用 `product`）
- `#show par: set block(spacing: ..)`（已废弃，用 `#set par(spacing: ..)`）
- 猜测字体名而不执行 `typst fonts` 验证
- 忘记 `lang: "zh"` 或 `first-line-indent: (amount: 2em, all: true)`

## 高级功能

- 文档标题：`#title[标题]`（Typst 0.12+）
- 上下文：`context { ... }` — 页眉页脚、计数器
- 目录：`#outline(title: [目录], indent: auto)`
- 日期：`datetime.today().display("[year]年[month]月[day]日")`
- 多文件：`#include "file.typ"`
- 数学/可视化：`docs/reference/math/`、`docs/reference/visualize/`

## 故障排除

- **字体缺失**：用 `typst fonts` 确认，移除不存在的字体
- **variable fonts 警告**：不影响编译，可换静态字体版本
- **包未找到**：核实 Typst Universe 包名和版本号
- **"expected content"**：标记处用了代码，用 `#{ }` 包裹
- **"unknown variable"**：检查拼写和导入
