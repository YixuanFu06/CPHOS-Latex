# CPHOS LaTeX 模板使用文档

CPHOS 物理竞赛联考 LaTeX 模板，用于排版参考答案与评分标准文档。

**编译命令：**
```bash
xelatex example-problem.tex
```

---

## 目录

1. [快速开始](#快速开始)
2. [文档类选项](#文档类选项)
3. [题目与解答环境](#题目与解答环境)
4. [评分标准系统](#评分标准系统)
5. [页眉页脚设置](#页眉页脚设置)
6. [图片编号系统](#图片编号系统)
7. [参考文献环境](#参考文献环境)
8. [附录环境](#附录环境)
9. [联系方式与版权](#联系方式与版权)
10. [答题卡模板](#答题卡模板)
11. [其他命令](#其他命令)

---

## 快速开始

```latex
\documentclass{cphos}

\cphostitle{第XX届CPHOS物理竞赛联考}
\cphossubtitle{理论试题参考答案及评分标准}

\begin{document}
\maketitlepage

\begin{problem}{一}{25}{电磁感应}
    \begin{problemstatement}
        题目内容...
    \end{problemstatement}
    
    \begin{solution}
        解答内容...
        \begin{equation}
            E = mc^2 \eqtagscore{1}{3}
        \end{equation}
    \end{solution}
    
    \scoring{25}
\end{problem}

\end{document}
```

---

## 文档类选项

```latex
\documentclass[选项]{cphos}
```

| 选项 | 说明 |
|------|------|
| `answer` | 参考答案及评分标准模式（**默认**），显示完整解答 |
| `exam` | 试题模式，隐藏 `solution` 环境内的所有内容 |
| `nowatermark` | 不显示背景水印 |

**示例：**
```latex
\documentclass[exam]{cphos}           % 试题模式
\documentclass[answer,nowatermark]{cphos}  % 答案模式，无水印
```

---

## 题目与解答环境

### `problem` 环境（核心）

定义一道完整的题目。

```latex
\begin{problem}{题号}{总分}{标题}
    ...
\end{problem}
```

**参数说明：**
- `题号`：中文题号（一、二、三...）
- `总分`：该题满分分值
- `标题`：题目名称

### `problemstatement` 环境

题干内容区域，自动设置首行缩进。

```latex
\begin{problemstatement}
    题目描述文本...
\end{problemstatement}
```

### `solution` 环境

解答区域。在 `exam` 模式下自动隐藏。

```latex
\begin{solution}
    由能量守恒...
\end{solution}
```

### 小问编号命令

| 命令 | 用途 | 输出示例 |
|------|------|----------|
| `\pmark{A}` | Part 分标签 | **Part A** |
| `\subq{1}` | 大问编号 | （1） |
| `\subsubq{i}` | 子问编号 | （i） |
| `\subsubsubq{a}` | 子子问编号 | （a） |

---

## 评分标准系统

### 声明式评分命令

在解答中标记各部分分值，最后由 `\scoring` 自动汇总生成评分标准。

#### 层级声明命令

| 命令 | 用途 | 示例 |
|------|------|------|
| `\scorePart{A}{10}` | 声明 Part A 共 10 分 | — |
| `\scorepart{1}{8}` | 声明第（1）问共 8 分 | — |
| `\scoresubpart{i}{4}` | 声明第（i）子问共 4 分 | — |
| `\scoresubsubpart{a}{2}` | 声明第（a）子子问共 2 分 | — |

#### 计分命令

| 命令 | 用途 | 示例 |
|------|------|------|
| `\eqtagscore{编号}{分值}` | 公式编号并计分 | `\eqtagscore{1}{3}` → 公式(1)，3分 |
| `\eqtag{编号}` | 仅公式编号，不计分 | `\eqtag{1}` → 公式(1) |
| `\addtext{描述}{分值}` | 文字描述计分 | `\addtext{正确画图}{2}` |

### `\scoring{总分}` 命令

自动生成评分标准段落，汇总所有已声明的分值。

```latex
\scoring{25}
```

**输出示例：**
> **评分标准：**本题满分25分。  
> Part A 10分：（1）（2）式各3分，（3）式2分，正确画图2分；  
> Part B 15分：第（1）问8分：...

### 分值检查

开启分值检查后，声明分值与实际计分不匹配时会显示红色警告。

```latex
\enablescorecheck   % 开启分值检查
\disablescorecheck  % 关闭分值检查
```

### 手动评分标准

若不使用自动系统，可直接手写：

```latex
\scoringmanual{（1）式3分，（2）式4分...}
```

---

## 页眉页脚设置

### 标题设置命令

```latex
\cphostitle{第XX届CPHOS物理竞赛联考}
\cphossubtitle{理论试题参考答案及评分标准}
```

**页眉布局：**
- 左侧：主标题
- 右侧：副标题

**页脚布局：**
- 左侧：页码 / 总页数
- 右侧：版权信息

### 首页样式

首页自动使用无页眉样式（通过 `\maketitlepage` 设置）。

### 水印设置

```latex
\cphoswatermark{assets/watermark.jpg}  % 设置水印图片路径
```

默认水印路径为 `assets/watermark.jpg`，透明度 15%。

---

## 图片编号系统

模板实现了题干与解答的独立图片编号：

| 位置 | 编号格式 | 示例 |
|------|----------|------|
| `problemstatement` 内 | 图 X.Y | 图 1.1、图 1.2 |
| `solution` 内 | 答图 X.Y | 答图 1.1、答图 1.2 |

**使用方式：**
```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.5\textwidth]{fig1.png}
    \caption{电路示意图}
\end{figure}
```

编号自动根据所在环境生成，无需手动设置。

---

## 参考文献环境

用于在解答后展示参考资料，使用楷体、五号字。

### `problemreferences` 环境

```latex
\begin{problemreferences}
    \refitem{作者. 《书名》. 出版社, 年份.}
    \refitem{作者. 论文标题. 期刊名, 卷(期): 页码, 年份.}
\end{problemreferences}
```

### 引用命令

```latex
文中引用\supercite{[1]}可以...
```

输出为上标形式：文中引用^[1]^可以...

---

## 附录环境

使用带边框的方框展示附加内容，支持跨页。

```latex
\begin{problemappendix}[附录标题]
    附录内容...
\end{problemappendix}
```

**参数说明：**
- 可选参数：自定义标题（默认为"附录"）

---

## 联系方式与版权

### `copyrightinfo` 环境

确保内容不跨页：

```latex
\begin{copyrightinfo}
    版权声明内容...
\end{copyrightinfo}
```

### `\contactinfo` 命令

生成联系方式表格：

```latex
\contactinfo{二维码1路径}{二维码2路径}{二维码3路径}{email@cphos.cn}{小程序名}
```

**输出布局：**

| 微信公众号 | 官方网站 | CPHOS论坛 | 邮箱/小程序 |
|------------|----------|-----------|-------------|
| [二维码1]  | [二维码2] | [二维码3] | 联系信息 |

---

## 答题卡模板

答题卡使用独立模板 `example-answersheet.tex`，不依赖 cphos.cls。

### 基本结构

```latex
\documentclass[11pt, a4paper]{article}
\usepackage[UTF8]{ctex}
% ... 其他宏包

\answersheettitle{第XX届CPHOS物理竞赛联考}
\answersheetsubtitle{理论答题卡}

\begin{document}
% 第一页（手动设置）
\begin{tikzpicture}[remember picture, overlay]
    \draw[line width=0.5pt] ...;
\end{tikzpicture}
\hspace{0.3cm}{\bfseries\large 一、}

\vfill

% 后续页面
\answerpagebox{二、}
\vfill

\answerpagebox{三、}
\vfill
\end{document}
```

### 答题卡命令

| 命令 | 用途 |
|------|------|
| `\answersheettitle{标题}` | 设置页眉左侧标题 |
| `\answersheetsubtitle{副标题}` | 设置页眉右侧副标题 |
| `\answerpagebox{题号}` | 新建一页并绘制边框，显示题号 |

---

## 其他命令

### 编译时间

```latex
\compiletime
```

输出当前编译时间，格式：`2026年3月8日14:30`

### 楷体切换

```latex
{\kaiti 这段文字使用楷体}
```

### 题号转中文

```latex
\chnnum{3}  % 输出：三
```

支持 1-10 的转换。

---

## 文件结构

```
CPHOS-Latex/
├── cphos.cls           # 文档类
├── cphos-scoring.sty   # 评分系统宏包
├── example-problem.tex # 命题模板
├── example-paper.tex   # 试题模板
├── example-answersheet.tex # 答题卡模板
├── assets/
│   └── watermark.jpg   # 水印图片
└── README.md           # 本文档
```

---

## 注意事项

1. **必须使用 XeLaTeX 编译**，不支持 pdfLaTeX
2. 确保系统安装以下字体：
   - Times New Roman
   - SimSun（宋体）
   - SimHei（黑体）
   - KaiTi（楷体）
3. 评分系统在每个 `problem` 环境开始时自动清空，无需手动调用 `\clearScores`
4. 图片建议使用 `[H]` 选项固定位置
