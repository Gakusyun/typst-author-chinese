# typst query — 提取元数据

从编译后的文档中提取指定元素。

```
typst query [OPTIONS] <INPUT> <SELECTOR>
```

## 参数

| 参数 | 说明 |
|------|------|
| `<INPUT>` | 输入文件路径，`-` 表示从 stdin 读取 |
| `<SELECTOR>` | CSS 选择器语法，选取要提取的元素 |

## 选项

| 选项 | 说明 |
|------|------|
| `--field <FIELD>` | 仅提取指定字段 |
| `--one` | 期望恰好匹配一个元素 |
| `--format <json\|yaml>` | 输出格式（默认 json） |
| `--pretty` | 美化输出（仅 JSON） |
| `--target <paged\|html>` | 编译目标（默认 paged） |

其余通用选项（`--root`、`--input`、`--font-path`、`--ignore-system-fonts`、`--package-path` 等）与 [compile](compile.md) 相同。

## 常用示例

```bash
# 提取所有标题
typst query doc.typ "<heading>"

# 提取 metadata 元素的 value 字段（stdin 探测）
echo '#metadata(1 + 2) <probe>' | typst query - "<probe>" --field value --one

# 查询指定标签的元素
typst query doc.typ "<my-label>"

# YAML 格式输出
typst query doc.typ "<heading>" --format yaml --pretty

# 提取 figure 的 count 字段
typst query doc.typ "<figure>" --field count --one
```
