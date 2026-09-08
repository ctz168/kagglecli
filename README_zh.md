# 🚀 KaggleCLI

[![Open In Kaggle](https://img.shields.io/badge/Open%20in%20Kaggle-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code/new)
[![GitHub](https://img.shields.io/badge/GitHub-ctz168%2Fkagglecli-blue?logo=github)](https://github.com/ctz168/kagglecli)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](https://github.com/ctz168/kagglecli)

🌐 [English](README.md) | **中文**

一个强大的命令行工具，用于运行 Jupyter Notebook（`.ipynb`），支持**逐 cell 流式输出**，以及针对长时间运行任务的**实时 SSE 流式**功能。

---

## 🎯 快速开始

### 1. 在 Kaggle 上部署服务器

点击上方 **Open in Kaggle** 徽章（或直接打开 [kaggle.com/code/new](https://www.kaggle.com/code/new)）—— Kaggle 会一键为你创建一个全新 Notebook，然后把服务器 Notebook 导入即可：

1. 点击徽章 → 你的账号下会新建一个 Kaggle Notebook
2. **File → Import Notebook → GitHub** 标签页 → 选择仓库 `ctz168/kagglecli` → 文件 `kaggle_server.ipynb`
   *（或者：下载 [`kaggle_server.ipynb`](kaggle_server.ipynb) 后用 **File → Import Notebook → Upload file** 上传）*
3. 在 Notebook 右侧 **Settings** 中：开启 **Internet → ON**（需要已验证手机号的账号）；可选选择 **GPU 加速器**（T4 x2 / P100）
4. 点击 **Run All（运行全部）** — 隧道 cell 会输出公网 URL，例如 `https://xxxx.aitun.cc/xxxx`
5. 保持 Notebook 页面打开（保活循环会自动维持会话）

### 2. 本地安装 CLI

```bash
pip install git+https://github.com/ctz168/kagglecli.git
```

### 3. 远程运行 Notebook

```bash
# 检查服务器健康状态
kagglemcp health --url https://aitun.cc/your-code

# 远程运行 notebook（批量模式）
kagglemcp remote notebook.ipynb --url https://aitun.cc/your-code

# 实时流式运行 notebook（新功能！）
kagglemcp stream notebook.ipynb --url https://aitun.cc/your-code
```

---

## ✨ 功能特性

### 核心功能
- 📓 **本地运行 Jupyter Notebook** - 直接从 CLI 执行 `.ipynb` 文件
- 🌐 **远程执行** - 在 Kaggle 上运行 notebook，免费 GPU（T4 x2 / P100）
- 📊 **流式输出** - 实时查看每个 cell 的执行输出
- 🔧 **IPython magic 支持** - 完整支持 `%cd`、`%env`、`!cmd`、`%%bash` 等
- 🔄 **变量持久化** - 变量在 cell 之间保持
- ⏱️ **执行计时** - 跟踪每个 cell 的执行时间
- 🛑 **错误处理** - 出错时停止或继续执行

### v1.0.0 新功能 🆕
- 🌊 **实时 SSE 流式** - 长时间任务（Bot、训练等）的实时输出
- 👀 **Watch 模式** - 实时监控服务器状态
- ⏹️ **中断执行** - 停止长时间运行的代码，无需重启服务器
- 📊 **状态跟踪** - 了解当前目录、运行状态、命令历史
- 📜 **命令历史** - 查看过去的执行及其结果
- 🚫 **重复检测** - 自动跳过冗余的 `cd` 命令

### 其他功能
- 📝 **Notebook 转换** - 将 `.ipynb` 转换为 `.py` 脚本
- 🔍 **Notebook 检查** - 查看 notebook 结构和元数据

---

## 📦 安装

> 💡 CLI 安装在**你自己的电脑**上 —— Kaggle Notebook 只需要运行服务端（它会自动安装自己的依赖）。
>
> ⚠️ **如果要在 Kaggle Notebook 里安装？** 必须先在右侧 Settings 面板开启 **Internet → ON**，否则 pip 会报错 `Could not resolve host: github.com`。参见[常见问题排查](#️-常见问题排查)。

### 从 GitHub 安装

```bash
pip install git+https://github.com/ctz168/kagglecli.git
```

### 从源码安装

```bash
git clone https://github.com/ctz168/kagglecli.git
cd kagglecli
pip install -e .
```

### 依赖

```bash
pip install requests rich click ipython
```

---

## 🌐 多语言支持

默认语言为英文，可通过 `--lang` 选项或环境变量切换为中文：

```bash
# 单次命令切换
kagglemcp --lang zh --version

# 通过环境变量永久切换
export KAGGLEMCP_LANG=zh
```

---

## 🚀 使用方法

### 本地运行 Notebook

```bash
# 基本用法
kagglemcp run notebook.ipynb

# 带选项
kagglemcp run notebook.ipynb --start 5 --end 10 --verbose

# 保存输出到 JSON
kagglemcp run notebook.ipynb -o results.json
```

### 在远程服务器上运行（Kaggle）

```bash
# 检查服务器状态
kagglemcp health --url https://aitun.cc/your-code

# 远程执行 notebook（批量模式 - 等待完成）
kagglemcp remote notebook.ipynb --url https://aitun.cc/your-code

# 带超时（适合长时间运行的任务）
kagglemcp remote train_model.ipynb -u https://aitun.cc/your-code -t 3600
```

### 🆕 实时流式执行（v1.0.0）

非常适合训练、Bot、或持续运行等长时间任务：

```bash
# 实时流式输出 notebook
kagglemcp stream notebook.ipynb -u https://aitun.cc/your-code

# 只流式执行特定 cell
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code --start 3 --end 4

# 实时监控服务器状态
kagglemcp watch -u https://aitun.cc/your-code -d 300
```

**何时使用 `stream` 而非 `remote`：**
- 短任务使用 `remote`（数据处理、快速脚本）
- 长时间运行的任务使用 `stream`（训练、Bot、服务器）

### 其他命令

```bash
# 查看 notebook 信息
kagglemcp info notebook.ipynb

# 列出并预览 cell
kagglemcp cells notebook.ipynb

# 转换为 Python 脚本
kagglemcp convert notebook.ipynb -o script.py

# 交互式 REPL
kagglemcp repl
```

---

## 📖 命令参考

### 命令概览

| 命令 | 描述 | 用途 |
|------|------|------|
| `kagglemcp run` | 本地执行 notebook | 快速测试、本地开发 |
| `kagglemcp remote` | 远程批量执行 | 短时间任务、数据处理 |
| `kagglemcp stream` | 🆕 远程流式执行 | 长时间任务、Bot、训练 |
| `kagglemcp watch` | 🆕 监控服务器状态 | 查看远程执行进度 |
| `kagglemcp health` | 检查服务器健康状态 | 验证连接 |
| `kagglemcp status` | 获取执行状态 | 查看当前目录和状态 |
| `kagglemcp interrupt` | 中断当前执行 | 停止运行中的代码 |
| `kagglemcp history` | 查看命令历史 | 调试、回溯 |
| `kagglemcp info` | 查看 notebook 信息 | 了解 notebook 结构 |
| `kagglemcp cells` | 列出 cell 内容 | 预览代码 |
| `kagglemcp convert` | 转换为 Python 脚本 | 导出代码 |
| `kagglemcp repl` | 交互式 Python REPL | 本地测试 |

---

### `kagglemcp run` - 本地执行

在本地执行 Jupyter Notebook，支持实时输出。

```bash
kagglemcp run NOTEBOOK [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--start` | `-s` | 0 | 起始 cell 索引（从哪个 cell 开始执行） |
| `--end` | `-e` | 最后一个 | 结束 cell 索引（不包含该索引，类似 Python slice） |
| `--show-code` | - | True | 执行前显示代码 |
| `--show-markdown` | - | False | 显示 markdown cell |
| `--stop-on-error` | - | True | 遇到错误时停止 |
| `--continue-on-error` | - | False | 遇到错误时继续 |
| `--verbose` | `-V` | False | 详细输出 |
| `--output` | `-o` | - | 保存结果到 JSON 文件 |

**示例：**

```bash
# 执行整个 notebook
kagglemcp run notebook.ipynb

# 只执行 cell 5 到 cell 9（不包含 cell 10）
kagglemcp run notebook.ipynb --start 5 --end 10

# 执行从 cell 3 开始到结尾
kagglemcp run notebook.ipynb -s 3

# 只执行 cell 0（第一个 cell）
kagglemcp run notebook.ipynb --end 1

# 错误时继续执行
kagglemcp run notebook.ipynb --continue-on-error

# 保存执行结果
kagglemcp run notebook.ipynb -o results.json
```

---

### `kagglemcp remote` - 远程批量执行

在远程服务器上执行 notebook，等待所有 cell 完成后返回结果。

```bash
kagglemcp remote NOTEBOOK --url URL [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 必需 | 说明 |
|------|------|--------|------|------|
| `--url` | `-u` | - | ✅ | KaggleMCP 服务器 URL |
| `--start` | `-s` | 0 | - | 起始 cell 索引 |
| `--end` | `-e` | 最后一个 | - | 结束 cell 索引（不包含） |
| `--stop-on-error` | - | True | - | 遇到错误时停止 |
| `--continue-on-error` | - | False | - | 遇到错误时继续 |
| `--timeout` | `-t` | 300 | - | 超时时间（秒） |
| `--verbose` | `-V` | False | - | 详细输出 |

**示例：**

```bash
# 基本远程执行
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code

# 执行特定 cell（cell 3 到 cell 7）
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code --start 3 --end 8

# 只执行第 4 个 cell（索引为 3）
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code -s 3 -e 4

# 长时间任务（设置 1 小时超时）
kagglemcp remote train.ipynb -u https://aitun.cc/your-code -t 3600

# 详细模式（显示每个 cell 的代码）
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code -V
```

---

### `kagglemcp stream` 🆕 - 远程流式执行

使用 SSE (Server-Sent Events) 实时推送输出，**适合长时间运行的任务**。

```bash
kagglemcp stream NOTEBOOK --url URL [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 必需 | 说明 |
|------|------|--------|------|------|
| `--url` | `-u` | - | ✅ | KaggleMCP 服务器 URL |
| `--start` | `-s` | 0 | - | 起始 cell 索引 |
| `--end` | `-e` | 最后一个 | - | 结束 cell 索引（不包含） |
| `--timeout` | `-t` | 600 | - | 流式超时时间（秒） |
| `--verbose` | `-V` | False | - | 显示代码 |

**使用场景：**
- 🤖 运行 Telegram/Discord Bot
- 🧠 模型训练（实时查看进度）
- 📊 数据处理（查看中间输出）
- 🔄 持续运行的监控脚本

**示例：**

```bash
# 流式执行整个 notebook
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code

# 只执行 Bot 启动的 cell（假设是 cell 4）
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -s 4 -e 5

# 执行 cell 3 到 cell 5（跳过前面的安装和环境设置）
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code --start 3 --end 6

# 详细模式（显示代码和输出）
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -V

# 长时间运行（设置 2 小时超时）
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -t 7200
```

**中断执行：** 按 `Ctrl+C` 可中断执行，服务器保持运行。

---

### `kagglemcp watch` 🆕 - 服务器监控

实时监控远程服务器的状态。

```bash
kagglemcp watch --url URL [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--url` | `-u` | - (必需) | KaggleMCP 服务器 URL |
| `--duration` | `-d` | 300 | 监控时长（秒），设为 0 无限监控 |

**示例：**

```bash
# 监控 5 分钟
kagglemcp watch -u https://aitun.cc/your-code

# 无限监控（按 Ctrl+C 退出）
kagglemcp watch -u https://aitun.cc/your-code -d 0

# 监控 1 小时
kagglemcp watch -u https://aitun.cc/your-code -d 3600
```

---

### `kagglemcp interrupt` - 中断执行

中断远程服务器上正在执行的代码，**不会停止服务器本身**。

```bash
kagglemcp interrupt --url URL
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--url` | `-u` | - (必需) | KaggleMCP 服务器 URL |

**使用场景：**
- Bot 运行时想要停止，但不想重新启动服务器
- 训练过程中发现参数错误，需要中断
- 死循环代码需要强制停止

**示例：**

```bash
# 中断当前执行
kagglemcp interrupt -u https://aitun.cc/your-code

# 简写形式
kagglemcp interrupt --url https://aitun.cc/your-code
```

**注意：** 中断后可以继续发送新的执行命令，服务器保持运行。

---

### `kagglemcp status` - 获取执行状态

查看远程服务器的当前状态。

```bash
kagglemcp status --url URL
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--url` | `-u` | - (必需) | KaggleMCP 服务器 URL |

**返回信息：**
- `is_executing` - 是否正在执行代码
- `current_directory` - 当前工作目录
- `last_command` - 最后执行的命令
- `last_execution_time` - 最后执行耗时
- `recent_history` - 最近 5 条命令

**示例：**

```bash
kagglemcp status -u https://aitun.cc/your-code
```

---

### `kagglemcp history` - 查看命令历史

查看远程服务器上的命令执行历史。

```bash
kagglemcp history --url URL [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--url` | `-u` | - (必需) | KaggleMCP 服务器 URL |
| `--limit` | `-l` | 20 | 显示的历史条数 |

**示例：**

```bash
# 查看最近 20 条历史
kagglemcp history -u https://aitun.cc/your-code

# 查看最近 50 条历史
kagglemcp history -u https://aitun.cc/your-code -l 50
```

---

### `kagglemcp health` - 健康检查

检查远程服务器的健康状态。

```bash
kagglemcp health --url URL
```

**返回信息：**
- 服务器状态和运行时间
- 内存使用情况
- GPU 可用性
- 当前工作目录

**示例：**

```bash
kagglemcp health -u https://aitun.cc/your-code
```

---

### `kagglemcp cells` - 列出 Cell

列出 notebook 中的 cell 内容，用于预览代码。

```bash
kagglemcp cells NOTEBOOK [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--start` | `-s` | 0 | 起始 cell 索引 |
| `--end` | `-e` | 最后一个 | 结束 cell 索引（不包含） |
| `--verbose` | `-V` | False | 显示元数据 |

**示例：**

```bash
# 查看所有 cell
kagglemcp cells notebook.ipynb

# 查看 cell 3 到 cell 7
kagglemcp cells notebook.ipynb -s 3 -e 8

# 查看第一个 cell
kagglemcp cells notebook.ipynb -e 1
```

---

### `kagglemcp info` - Notebook 信息

显示 notebook 的基本信息。

```bash
kagglemcp info NOTEBOOK
```

**返回信息：**
- 文件路径和格式版本
- 总 cell 数量（代码 cell 和 markdown cell）
- 内核信息
- Cell 概览表格

---

### `kagglemcp convert` - 转换为 Python

将 notebook 转换为 Python 脚本。

```bash
kagglemcp convert NOTEBOOK [OPTIONS]
```

**参数说明：**

| 参数 | 简写 | 默认值 | 说明 |
|------|------|--------|------|
| `--output` | `-o` | notebook同名.py | 输出文件路径 |

**示例：**

```bash
# 转换（输出到同名 .py 文件）
kagglemcp convert notebook.ipynb

# 指定输出文件
kagglemcp convert notebook.ipynb -o script.py
```

---

## 🔧 Cell 索引详解

### 索引规则

Cell 索引从 **0** 开始，`--end` 参数是**不包含**的（类似 Python 的 slice）。

```
Notebook 结构:
Cell 0: # 导入库        ← Markdown
Cell 1: import numpy    ← 代码
Cell 2: # 数据加载      ← Markdown  
Cell 3: load_data()     ← 代码
Cell 4: # 模型训练      ← Markdown
Cell 5: train_model()   ← 代码
```

### 执行范围示例

| 命令 | 执行的 Cell | 说明 |
|------|-------------|------|
| `-s 0 -e 6` 或省略 | 1, 3, 5 | 全部代码 cell（跳过 markdown） |
| `-s 3 -e 6` | 3, 5 | 从 cell 3 开始 |
| `-s 5 -e 6` | 5 | 只执行 cell 5 |
| `-s 1 -e 4` | 1, 3 | 执行 cell 1 和 3 |
| `-s 3` | 3, 5 | 从 cell 3 到结尾 |
| `-e 3` | 1 | 只执行第一个代码 cell |

### 常用场景

```bash
# 场景 1: 跳过安装和环境设置，直接运行核心逻辑
kagglemcp stream bot.ipynb -u $URL --start 4

# 场景 2: 只运行某个特定 cell（假设是 cell 3）
kagglemcp remote notebook.ipynb -u $URL -s 3 -e 4

# 场景 3: 调试某个范围的问题
kagglemcp run notebook.ipynb --start 5 --end 8 -V

# 场景 4: 先预览再执行
kagglemcp cells notebook.ipynb -s 4 -e 6
kagglemcp stream notebook.ipynb -u $URL -s 4 -e 6
```

---

## 🔧 支持的 IPython Magic 命令

| Magic 命令 | 示例 | 说明 |
|-----------|------|------|
| `%cd` | `%cd /content/project` | 切换目录 |
| `%pwd` | `%pwd` | 打印当前工作目录 |
| `%env` | `%env`、`%env VAR` | 显示/获取环境变量 |
| `%set_env` | `%set_env VAR value` | 设置环境变量 |
| `%pip` | `%pip install package` | 安装 Python 包 |
| `!cmd` | `!git clone URL` | 执行 shell 命令 |
| `%%writefile` | `%%writefile file.py` | 将 cell 内容写入文件 |
| `%%bash` | `%%bash` | 以 bash 脚本运行 cell |
| `%time` | `%time func()` | 计时执行 |
| `%who` | `%who` | 列出变量 |

---

## 📝 输出示例

### 控制台输出

```
━━━ Cell [0] ━━━
╭──────────────────────────────────────────────────────────────────────────────╮
│ print("Hello, World!")                                                       │
│ for i in range(3):                                                           │
│     print(f"Count: {i}")                                                     │
╰──────────────────────────────────────────────────────────────────────────────╯
⏳ 运行中...
Hello, World!
Count: 0
Count: 1
Count: 2
✅ 完成 (45ms)

━━━ Cell [1] ━━━
...
```

### 流式输出（v1.0.0）

```
━━━ Cell [3] ━━━
⏳ 流式执行中...
📌 执行: python main.py --mode telegram
[Bot] Token: 7983263905:AAFs...
[Bot] 启动中...
按 Ctrl+C 停止 Bot
============================================================
[心跳] 14:32:15 - 服务运行中 | 目录: /content/stdpbrain
[Bot] 收到消息: 你好
[Bot] 回复: 你好！我是类人脑AI...
[心跳] 14:32:45 - 服务运行中 | 目录: /content/stdpbrain
...
⏹️ 执行被用户中断

📊 流式执行摘要:
总耗时      5m 32.1s
输出行数    127
```

### JSON 输出（使用 `-o` 选项）

```json
{
  "notebook": "analysis.ipynb",
  "total_time": 2.345,
  "results": [
    {
      "cell_index": 0,
      "status": "success",
      "stdout": "Hello, World!\nCount: 0\nCount: 1\nCount: 2\n",
      "execution_time": 0.045,
      "variables": ["data"]
    }
  ]
}
```

---

## 🎯 使用场景

### 数据分析流水线

```bash
# 运行数据预处理
kagglemcp run preprocess.ipynb

# 运行分析（跳过前 3 个 cell）
kagglemcp run analysis.ipynb --start 3

# 生成报告
kagglemcp run report.ipynb -o report_output.json
```

### 远程 GPU 计算

```bash
# 在 Kaggle 上部署服务器（Settings → Accelerator → GPU T4 x2）
# 然后在本地运行：
kagglemcp remote train_model.ipynb -u https://aitun.cc/your-code -t 3600
```

### 🆕 长时间运行的 Bot/服务器

```bash
# 实时流式输出 Bot 运行结果
kagglemcp stream telegram_bot.ipynb -u https://aitun.cc/your-code --start 3

# 在 Bot 运行时监控服务器
kagglemcp watch -u https://aitun.cc/your-code -d 0
```

### CI/CD 集成

```bash
# 在 CI 流水线中
kagglemcp run tests.ipynb --continue-on-error -o test_results.json

# 检查退出码
if [ $? -eq 0 ]; then
    echo "All tests passed!"
fi
```

---

## 🔧 架构

```
┌─────────────────────────────────────────────────────────────┐
│                      KaggleCLI v1.0.0                        │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────────┐   │
│  │   CLI       │   │  Notebook   │   │   Execution     │   │
│  │   (click)   │──▶│   Parser    │──▶│   Engine        │   │
│  └─────────────┘   └─────────────┘   └─────────────────┘   │
│                                              │              │
│                          ┌──────────────────┼─────────────┐ │
│                          ▼                  ▼             │ │
│                   ┌───────────┐      ┌───────────┐       │ │
│                   │  Local    │      │  Remote   │       │ │
│                   │  Engine   │      │  Engine   │       │ │
│                   └───────────┘      └───────────┘       │ │
│                          │                  │             │ │
│                          ▼                  ▼             │ │
│                   ┌─────────────────────────────────────┐ │ │
│                   │      Streaming Output Display       │ │ │
│                   │    (rich console + SSE support)     │ │ │
│                   └─────────────────────────────────────┘ │ │
└─────────────────────────────────────────────────────────────┘
```

## 🤝 与 Kaggle 集成

此 CLI 可与 Kaggle Notebooks 无缝配合：

1. 在 Kaggle 中**导入 `kaggle_server.ipynb`**（File → Import Notebook），并在 Settings 中开启 **Internet**
2. **运行所有 cell** - aitun 隧道自动启动（免注册、无需 token）
3. **复制公网 URL**，并与 `kagglemcp remote` 或 `kagglemcp stream` 一起使用

> ⚠️ **Kaggle 会话限制**：CPU 会话最长 **12 小时**，GPU 会话最长 **9 小时**（每周 GPU 配额约 30 小时）。会话存活期间隧道 URL 有效。保存版本（Commit）不会保留隧道运行 - 请保持交互式会话打开。

---

## 🛠️ 常见问题排查

### pip 安装时报 `Could not resolve host: github.com`

说明 Kaggle Notebook **没有开启联网** —— Kaggle 默认关闭 Internet：

1. 打开 Notebook 编辑器右侧的 **Settings** 面板
2. 将 **Internet → ON**（需要已验证手机号的 Kaggle 账号）
3. 重新运行该 cell：`!pip install git+https://github.com/ctz168/kagglecli.git`

> 💡 注意：通常你**不需要**在 Kaggle 里安装 kagglecli。CLI 装在**本地电脑**；Kaggle 里只需运行 `kaggle_server.ipynb`，它的第一个 cell 会自动安装服务端自身依赖（`flask aitun requests psutil`）。

### CLI 端报 `Connection refused` / 超时

- 检查服务器 URL：由隧道 cell 输出，形如 `https://xxxx.aitun.cc/xxxx`（末尾不要多斜杠）
- 先执行 `kagglemcp health --url <url>` —— 如果失败，Kaggle 会话可能已结束（CPU 会话最长 **12 小时**，GPU **9 小时**），重新运行服务端 Notebook 即可
- 确保 Notebook 页面保持打开 —— 保存版本（Commit）**不会**让隧道保持存活

### `--start/--end` 范围似乎不生效

已在 v1.0.0 修复 —— 请确保安装了最新版：`pip install -U git+https://github.com/ctz168/kagglecli.git`。

---

## 📋 更新日志

### v1.0.0（最新）
- 🎉 KaggleCLI 首个版本，基于 [colabcli](https://github.com/ctz168/colabcli) 改造
- 🆕 远程执行服务器适配 Kaggle Notebooks（工作目录 `/kaggle/working`）
- 🆕 `/health` 端点返回 Kaggle 会话信息（GPU 型号、磁盘用量）
- 🆕 实时 SSE 流式输出（`kagglemcp stream`）
- 🆕 服务器监控（`kagglemcp watch`）
- ✨ aitun.cc 免费隧道 - 免注册、无需 token

## 🔒 安全说明

- **本地执行**：代码以您的用户权限运行
- **远程执行**：代码在远程服务器上以完全权限运行
- **无认证**：KaggleMCP 服务器没有内置认证 - 请将 URL 保密

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。

## 🙏 致谢

- 基于 [colabcli](https://github.com/ctz168/colabcli) 改造 - 同样的思路，适配 Kaggle
- 基于 [Click](https://click.palletsprojects.com/)、[Rich](https://github.com/Textualize/rich) 和 [IPython](https://ipython.org/) 构建