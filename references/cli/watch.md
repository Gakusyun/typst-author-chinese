# typst watch — 监听编译

监听输入文件变化，自动重新编译。

```
typst watch [OPTIONS] <INPUT> [OUTPUT]
```

## 参数

与 `compile` 相同：`<INPUT>` 输入路径（`-` 读 stdin），`[OUTPUT]` 输出路径（`-` 写 stdout）。多页输出模板同 `compile`。

## 选项

继承 `compile` 的全部通用选项（`--root`、`--input`、`--font-path`、`--pages`、`--pdf-standard`、`--deps`、`--jobs`、`--features` 等），另加以下独有选项：

| 选项 | 说明 |
|------|------|
| `--no-serve` | 禁用 HTML 导出的内置 HTTP 服务器 |
| `--no-reload` | 禁用 HTML 导出的自动刷新脚本注入 |
| `--port <PORT>` | HTML 服务端口（默认 3000–3005 中首个空闲端口） |

## 常用示例

```bash
# 监听并自动编译为 PDF
typst watch doc.typ

# 监听 HTML 输出，浏览器自动刷新
typst watch doc.typ output.html --open

# 指定端口
typst watch doc.typ output.html --port 8080

# 使用自定义字体目录监听
typst watch --font-path ./fonts doc.typ
```
