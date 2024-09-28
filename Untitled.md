在 Excel 中要根据 `1 sheet` 中的项目编号 (C 列) 从 `source sheet` 中匹配相同的项目编号，并将 `source sheet` 中对应的累计收入 (Q 列) 填入 `1 sheet` 中的某个单元格 (例如 K 列)，可以通过 `VLOOKUP` 函数或 `XLOOKUP` 函数来实现（如果使用的是 Excel 365 或 2021）。

### 方法 1：使用 `VLOOKUP` 函数

1. 假设 `1 sheet` 中项目编号在 `C` 列，收款核对的结果要回填到 `K` 列。
2. `source sheet` 中项目编号在 `C` 列，累计收入在 `Q` 列。

在 `1 sheet` 的 `K2` 单元格中输入以下公式（假设数据从第 2 行开始）：

excel

`=IFERROR(VLOOKUP(C2, source!C:Q, 15, FALSE), 0)`

- `C2`：当前 `1 sheet` 中的项目编号。
- `source!C:Q`：表示 `source sheet` 的 C 到 Q 列数据范围（C 列是项目编号，Q 列是累计收入）。
- `15`：表示从 `source sheet` 的 C 列开始，第 15 列就是累计收入所在的 Q 列。
- `FALSE`：表示精确匹配。
- `IFERROR`：用来处理如果没有匹配的情况，返回 0。

将公式下拉填充至所有相关行即可。

### 方法 2：使用 `XLOOKUP` 函数（Excel 365 或 2021 版本）

`XLOOKUP` 更简单，并且不需要指定列数。公式如下：

excel

`=IFERROR(XLOOKUP(C2, source!C:C, source!Q:Q, 0), 0)`

- `C2`：当前 `1 sheet` 中的项目编号。
- `source!C:C`：表示 `source sheet` 中的项目编号列。
- `source!Q:Q`：表示 `source sheet` 中的累计收入列。
- `0`：表示如果没有找到匹配的值，返回 0。
- `IFERROR`：处理错误情况。

### 步骤

1. 打开 `1 sheet`。
2. 在 `K2` 单元格中输入上述公式。
3. 将公式往下拖动，填充到其他行。

这样就可以自动从 `source sheet` 中查找并填充累计收入了。