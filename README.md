# 🚀 KaggleCLI

[![Open In Kaggle](https://img.shields.io/badge/Open%20in%20Kaggle-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code/new)
[![GitHub](https://img.shields.io/badge/GitHub-ctz168%2Fkagglecli-blue?logo=github)](https://github.com/ctz168/kagglecli)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](https://github.com/ctz168/kagglecli)

🌐 **English** | [中文](README_zh.md)

A powerful command-line tool to run Jupyter Notebooks (`.ipynb`) with **streaming output per cell** and **real-time SSE streaming** for long-running tasks.

---

## 🎯 Quick Start

### 1. Deploy Server on Kaggle

Click the **Open in Kaggle** badge above (or open [kaggle.com/code/new](https://www.kaggle.com/code/new)) — Kaggle will create a fresh notebook for you in one click. Then load the server notebook into it:

1. Click the badge → a new Kaggle notebook opens in your account
2. **File → Import Notebook → GitHub** tab → pick repo `ctz168/kagglecli` → file `kaggle_server.ipynb`
   *(or: download [`kaggle_server.ipynb`](kaggle_server.ipynb) and use **File → Import Notebook → Upload file**)*
3. In notebook **Settings** (right panel): turn **Internet → ON** (phone-verified account required); optionally pick a **GPU accelerator** (T4 x2 / P100)
4. **Run All** — the tunnel cell prints the public URL, e.g. `https://xxxx.aitun.cc/xxxx`
5. Keep the notebook tab open (the keep-alive loop maintains the session)

### 2. Install CLI Locally

```bash
pip install git+https://github.com/ctz168/kagglecli.git
```

### 3. Run Notebooks Remotely

```bash
# Check server health
kagglemcp health --url https://aitun.cc/your-code

# Run notebook remotely (batch mode)
kagglemcp remote notebook.ipynb --url https://aitun.cc/your-code

# Run notebook with REAL-TIME streaming output (NEW!)
kagglemcp stream notebook.ipynb --url https://aitun.cc/your-code
```

---

## ✨ Features

### Core Features
- 📓 **Run Jupyter Notebooks locally** - Execute `.ipynb` files directly from CLI
- 🌐 **Remote execution** - Run notebooks on Kaggle with free GPU (T4 x2 / P100)
- 📊 **Streaming output** - See each cell's output in real-time as it executes
- 🔧 **IPython magic support** - Full support for `%cd`, `%env`, `!cmd`, `%%bash`, etc.
- 🔄 **Variable persistence** - Variables persist between cells
- ⏱️ **Execution timing** - Track execution time for each cell
- 🛑 **Error handling** - Stop on error or continue execution

### v1.0.0 New Features 🆕
- 🌊 **Real-time SSE Streaming** - Live output for long-running tasks (bots, training, etc.)
- 👀 **Watch Mode** - Monitor server status in real-time
- ⏹️ **Interrupt execution** - Stop long-running code without killing the server
- 📊 **Status tracking** - Know current directory, running status, command history
- 📜 **Command history** - View past executions and their results
- 🚫 **Duplicate detection** - Skip redundant `cd` commands automatically

### Other Features
- 📝 **Notebook conversion** - Convert `.ipynb` to `.py` scripts
- 🔍 **Notebook inspection** - View notebook structure and metadata

---

## 📦 Installation

> 💡 The CLI is installed **on your local computer** — the Kaggle notebook only runs the server side (it installs its own dependencies automatically).
>
> ⚠️ **Installing inside a Kaggle notebook?** Turn **Internet ON** first (right-side Settings panel), otherwise pip fails with `Could not resolve host: github.com`. See [Troubleshooting](#️-troubleshooting).

### From GitHub

```bash
pip install git+https://github.com/ctz168/kagglecli.git
```

### From Source

```bash
git clone https://github.com/ctz168/kagglecli.git
cd kagglecli
pip install -e .
```

### Dependencies

```bash
pip install requests rich click ipython
```

---

## 🌐 Language / 语言

English is the default language. Switch to Chinese via the `--lang` option or environment variable:

```bash
# Per-command
kagglemcp --lang zh --version

# Permanent via environment variable
export KAGGLEMCP_LANG=zh
```

---

## 🚀 Usage

### Run a Notebook Locally

```bash
# Basic usage
kagglemcp run notebook.ipynb

# With options
kagglemcp run notebook.ipynb --start 5 --end 10 --verbose

# Save output to JSON
kagglemcp run notebook.ipynb -o results.json
```

### Run on Remote Server (Kaggle)

```bash
# Check server status
kagglemcp health --url https://aitun.cc/your-code

# Execute notebook remotely (batch mode - waits for completion)
kagglemcp remote notebook.ipynb --url https://aitun.cc/your-code

# With timeout (for long-running tasks)
kagglemcp remote train_model.ipynb -u https://aitun.cc/your-code -t 3600
```

### 🆕 Real-Time Streaming (v1.0.0)

Perfect for long-running tasks like training, bots, or continuous processes:

```bash
# Stream notebook output in REAL-TIME
kagglemcp stream notebook.ipynb -u https://aitun.cc/your-code

# Stream specific cells only
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code --start 3 --end 4

# Watch server status in real-time
kagglemcp watch -u https://aitun.cc/your-code -d 300
```

**When to use `stream` vs `remote`:**
- Use `remote` for short tasks (data processing, quick scripts)
- Use `stream` for long-running tasks (training, bots, servers)

### Other Commands

```bash
# View notebook info
kagglemcp info notebook.ipynb

# List and preview cells
kagglemcp cells notebook.ipynb

# Convert to Python script
kagglemcp convert notebook.ipynb -o script.py

# Interactive REPL
kagglemcp repl
```

---

## 📖 Commands Reference

### Command Overview

| Command | Description | Use Case |
|---------|-------------|----------|
| `kagglemcp run` | Execute notebook locally | Quick testing, local development |
| `kagglemcp remote` | Execute remotely in batch mode | Short tasks, data processing |
| `kagglemcp stream` | 🆕 Remotely stream execution | Long-running tasks, Bots, training |
| `kagglemcp watch` | 🆕 Monitor server status | View remote execution progress |
| `kagglemcp health` | Check server health | Verify connection |
| `kagglemcp status` | Get execution status | View current directory and status |
| `kagglemcp interrupt` | Interrupt current execution | Stop running code |
| `kagglemcp history` | View command history | Debugging, traceback |
| `kagglemcp info` | View notebook info | Understand notebook structure |
| `kagglemcp cells` | List cell contents | Preview code |
| `kagglemcp convert` | Convert to Python script | Export code |
| `kagglemcp repl` | Interactive Python REPL | Local testing |

---

### `kagglemcp run` - Local Execution

Execute Jupyter Notebooks locally with real-time output.

```bash
kagglemcp run NOTEBOOK [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--start` | `-s` | 0 | Starting cell index (which cell to start from) |
| `--end` | `-e` | last | Ending cell index (exclusive, like Python slice) |
| `--show-code` | - | True | Show code before execution |
| `--show-markdown` | - | False | Show markdown cells |
| `--stop-on-error` | - | True | Stop on error |
| `--continue-on-error` | - | False | Continue on error |
| `--verbose` | `-V` | False | Verbose output |
| `--output` | `-o` | - | Save results to a JSON file |

**Examples:**

```bash
# Execute the entire notebook
kagglemcp run notebook.ipynb

# Execute only cells 5 to 9 (excluding cell 10)
kagglemcp run notebook.ipynb --start 5 --end 10

# Execute from cell 3 to the end
kagglemcp run notebook.ipynb -s 3

# Execute only cell 0 (first cell)
kagglemcp run notebook.ipynb --end 1

# Continue on error
kagglemcp run notebook.ipynb --continue-on-error

# Save execution results
kagglemcp run notebook.ipynb -o results.json
```

---

### `kagglemcp remote` - Remote Batch Execution

Execute the notebook on a remote server, waiting for all cells to complete before returning results.

```bash
kagglemcp remote NOTEBOOK --url URL [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Required | Description |
|-----------|-------|---------|----------|-------------|
| `--url` | `-u` | - | ✅ | KaggleMCP server URL |
| `--start` | `-s` | 0 | - | Starting cell index |
| `--end` | `-e` | last | - | Ending cell index (exclusive) |
| `--stop-on-error` | - | True | - | Stop on error |
| `--continue-on-error` | - | False | - | Continue on error |
| `--timeout` | `-t` | 300 | - | Timeout in seconds |
| `--verbose` | `-V` | False | - | Verbose output |

**Examples:**

```bash
# Basic remote execution
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code

# Execute specific cells (cells 3 to 7)
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code --start 3 --end 8

# Execute only cell 4 (index 3)
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code -s 3 -e 4

# Long-running task (1 hour timeout)
kagglemcp remote train.ipynb -u https://aitun.cc/your-code -t 3600

# Verbose mode (show code for each cell)
kagglemcp remote notebook.ipynb -u https://aitun.cc/your-code -V
```

---

### `kagglemcp stream` 🆕 - Remote Streaming Execution

Uses SSE (Server-Sent Events) to push output in real-time, **ideal for long-running tasks**.

```bash
kagglemcp stream NOTEBOOK --url URL [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Required | Description |
|-----------|-------|---------|----------|-------------|
| `--url` | `-u` | - | ✅ | KaggleMCP server URL |
| `--start` | `-s` | 0 | - | Starting cell index |
| `--end` | `-e` | last | - | Ending cell index (exclusive) |
| `--timeout` | `-t` | 600 | - | Streaming timeout in seconds |
| `--verbose` | `-V` | False | - | Show code |

**Use Cases:**
- 🤖 Run a Telegram/Discord Bot
- 🧠 Model training (view progress in real-time)
- 📊 Data processing (view intermediate output)
- 🔄 Continuously running monitoring scripts

**Examples:**

```bash
# Stream the entire notebook
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code

# Only execute the Bot startup cell (e.g., cell 4)
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -s 4 -e 5

# Execute cells 3 to 5 (skip previous install/env setup)
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code --start 3 --end 6

# Verbose mode (show code and output)
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -V

# Long-running (2 hour timeout)
kagglemcp stream bot.ipynb -u https://aitun.cc/your-code -t 7200
```

**Interrupting execution:** Press `Ctrl+C` to interrupt execution; the server keeps running.

---

### `kagglemcp watch` 🆕 - Server Monitoring

Monitor the status of a remote server in real-time.

```bash
kagglemcp watch --url URL [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--url` | `-u` | - (required) | KaggleMCP server URL |
| `--duration` | `-d` | 300 | Monitoring duration (seconds), set 0 for unlimited |

**Examples:**

```bash
# Monitor for 5 minutes
kagglemcp watch -u https://aitun.cc/your-code

# Unlimited monitoring (exit with Ctrl+C)
kagglemcp watch -u https://aitun.cc/your-code -d 0

# Monitor for 1 hour
kagglemcp watch -u https://aitun.cc/your-code -d 3600
```

---

### `kagglemcp interrupt` - Interrupt Execution

Interrupt code currently executing on the remote server, **without stopping the server itself**.

```bash
kagglemcp interrupt --url URL
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--url` | `-u` | - (required) | KaggleMCP server URL |

**Use Cases:**
- Want to stop a running Bot without restarting the server
- Found a parameter error during training and need to interrupt
- A loop is stuck and needs to be force-stopped

**Examples:**

```bash
# Interrupt current execution
kagglemcp interrupt -u https://aitun.cc/your-code

# Long form
kagglemcp interrupt --url https://aitun.cc/your-code
```

**Note:** After interrupting, you can continue sending new execution commands; the server keeps running.

---

### `kagglemcp status` - Get Execution Status

View the current status of the remote server.

```bash
kagglemcp status --url URL
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--url` | `-u` | - (required) | KaggleMCP server URL |

**Returned Information:**
- `is_executing` - whether code is currently executing
- `current_directory` - the current working directory
- `last_command` - the last executed command
- `last_execution_time` - duration of the last execution
- `recent_history` - the last 5 commands

**Example:**

```bash
kagglemcp status -u https://aitun.cc/your-code
```

---

### `kagglemcp history` - View Command History

View the command execution history on the remote server.

```bash
kagglemcp history --url URL [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--url` | `-u` | - (required) | KaggleMCP server URL |
| `--limit` | `-l` | 20 | Number of history entries to show |

**Examples:**

```bash
# View the last 20 history entries
kagglemcp history -u https://aitun.cc/your-code

# View the last 50 history entries
kagglemcp history -u https://aitun.cc/your-code -l 50
```

---

### `kagglemcp health` - Health Check

Check the health status of the remote server.

```bash
kagglemcp health --url URL
```

**Returned Information:**
- Server status and uptime
- Memory usage
- GPU availability
- Current working directory

**Example:**

```bash
kagglemcp health -u https://aitun.cc/your-code
```

---

### `kagglemcp cells` - List Cells

List the cell contents of a notebook, for previewing code.

```bash
kagglemcp cells NOTEBOOK [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--start` | `-s` | 0 | Starting cell index |
| `--end` | `-e` | last | Ending cell index (exclusive) |
| `--verbose` | `-V` | False | Show metadata |

**Examples:**

```bash
# View all cells
kagglemcp cells notebook.ipynb

# View cells 3 to 7
kagglemcp cells notebook.ipynb -s 3 -e 8

# View the first cell
kagglemcp cells notebook.ipynb -e 1
```

---

### `kagglemcp info` - Notebook Info

Display basic information about a notebook.

```bash
kagglemcp info NOTEBOOK
```

**Returned Information:**
- File path and format version
- Total cell count (code cells and markdown cells)
- Kernel information
- Cell overview table

---

### `kagglemcp convert` - Convert to Python

Convert a notebook to a Python script.

```bash
kagglemcp convert NOTEBOOK [OPTIONS]
```

**Options:**

| Parameter | Short | Default | Description |
|-----------|-------|---------|-------------|
| `--output` | `-o` | notebook-name.py | Output file path |

**Examples:**

```bash
# Convert (output to same-name .py file)
kagglemcp convert notebook.ipynb

# Specify output file
kagglemcp convert notebook.ipynb -o script.py
```

---

## 🔧 Cell Index Details

### Index Rules

Cell indices start from **0**, and the `--end` parameter is **exclusive** (similar to Python slice).

```
Notebook structure:
Cell 0: # Import libraries  ← Markdown
Cell 1: import numpy        ← Code
Cell 2: # Data loading      ← Markdown
Cell 3: load_data()         ← Code
Cell 4: # Model training    ← Markdown
Cell 5: train_model()       ← Code
```

### Execution Range Examples

| Command | Executed Cells | Description |
|---------|----------------|-------------|
| `-s 0 -e 6` or omitted | 1, 3, 5 | All code cells (skip markdown) |
| `-s 3 -e 6` | 3, 5 | Start from cell 3 |
| `-s 5 -e 6` | 5 | Execute only cell 5 |
| `-s 1 -e 4` | 1, 3 | Execute cells 1 and 3 |
| `-s 3` | 3, 5 | From cell 3 to the end |
| `-e 3` | 1 | Execute only the first code cell |

### Common Scenarios

```bash
# Scenario 1: Skip install and environment setup, run core logic directly
kagglemcp stream bot.ipynb -u $URL --start 4

# Scenario 2: Run only a specific cell (e.g., cell 3)
kagglemcp remote notebook.ipynb -u $URL -s 3 -e 4

# Scenario 3: Debug a range of issues
kagglemcp run notebook.ipynb --start 5 --end 8 -V

# Scenario 4: Preview first, then execute
kagglemcp cells notebook.ipynb -s 4 -e 6
kagglemcp stream notebook.ipynb -u $URL -s 4 -e 6
```

---

## 🔧 Supported IPython Magic Commands

| Magic Command | Example | Description |
|--------------|---------|-------------|
| `%cd` | `%cd /content/project` | Change directory |
| `%pwd` | `%pwd` | Print working directory |
| `%env` | `%env`, `%env VAR` | Show/get environment variables |
| `%set_env` | `%set_env VAR value` | Set environment variable |
| `%pip` | `%pip install package` | Install Python package |
| `!cmd` | `!git clone URL` | Execute shell command |
| `%%writefile` | `%%writefile file.py` | Write cell content to file |
| `%%bash` | `%%bash` | Run cell as bash script |
| `%time` | `%time func()` | Time execution |
| `%who` | `%who` | List variables |

---

## 📝 Output Example

### Console Output

```
━━━ Cell [0] ━━━
╭──────────────────────────────────────────────────────────────────────────────╮
│ print("Hello, World!")                                                       │
│ for i in range(3):                                                           │
│     print(f"Count: {i}")                                                     │
╰──────────────────────────────────────────────────────────────────────────────╯
⏳ Running...
Hello, World!
Count: 0
Count: 1
Count: 2
✅ Done (45ms)

━━━ Cell [1] ━━━
...
```

### Streaming Output (v1.0.0)

```
━━━ Cell [3] ━━━
⏳ Streaming...
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
⏹️ Execution interrupted by user

📊 Streaming Summary:
Total Time      5m 32.1s
Output Lines    127
```

### JSON Output (with `-o` option)

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

## 🎯 Use Cases

### Data Analysis Pipeline

```bash
# Run data preprocessing
kagglemcp run preprocess.ipynb

# Run analysis (skip first 3 cells)
kagglemcp run analysis.ipynb --start 3

# Generate report
kagglemcp run report.ipynb -o report_output.json
```

### Remote GPU Computation

```bash
# Deploy server on Kaggle (Settings → Accelerator → GPU T4 x2)
# Then run locally:
kagglemcp remote train_model.ipynb -u https://aitun.cc/your-code -t 3600
```

### 🆕 Long-Running Bot/Server

```bash
# Stream bot output in real-time
kagglemcp stream telegram_bot.ipynb -u https://aitun.cc/your-code --start 3

# Watch server while bot runs
kagglemcp watch -u https://aitun.cc/your-code -d 0
```

### CI/CD Integration

```bash
# In your CI pipeline
kagglemcp run tests.ipynb --continue-on-error -o test_results.json

# Check exit code
if [ $? -eq 0 ]; then
    echo "All tests passed!"
fi
```

---

## 🔧 Architecture

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

## 🤝 Integration with Kaggle

This CLI works seamlessly with Kaggle Notebooks:

1. **Import `kaggle_server.ipynb`** into Kaggle (File → Import Notebook) and enable **Internet** in Settings
2. **Run all cells** - the aitun tunnel starts automatically (no registration, no token)
3. **Copy the public URL** and use it with `kagglemcp remote` or `kagglemcp stream`

> ⚠️ **Kaggle session limits**: CPU sessions up to **12h**, GPU sessions up to **9h** (~30h GPU quota per week). The tunnel URL stays valid while the session is alive. Saving a version (Commit) will NOT keep the tunnel running - keep the interactive session open instead.

---

## 🛠️ Troubleshooting

### `Could not resolve host: github.com` when pip-installing

The Kaggle notebook has **no Internet access** — Kaggle disables it by default:

1. Open the right-side **Settings** panel in the notebook editor
2. Set **Internet → ON** (requires a phone-verified Kaggle account)
3. Re-run the cell: `!pip install git+https://github.com/ctz168/kagglecli.git`

> 💡 Remember: you normally don't need to install kagglecli inside Kaggle. The CLI runs **locally**; inside Kaggle you only run `kaggle_server.ipynb`, whose first cell installs its own dependencies (`flask aitun requests psutil`).

### `Connection refused` / timeout from the CLI

- Check the server URL: it is printed by the tunnel cell and looks like `https://xxxx.aitun.cc/xxxx` (no trailing slash)
- Run `kagglemcp health --url <url>` — if it fails, the Kaggle session has probably ended (CPU sessions max **12h**, GPU **9h**). Re-run the server notebook.
- Make sure the notebook tab stays open — saving a version (Commit) does **not** keep the tunnel alive.

### Cell range `--start/--end` seems ignored

Fixed in v1.0.0 — make sure you installed the latest version: `pip install -U git+https://github.com/ctz168/kagglecli.git`.

---

## 📋 Changelog

### v1.0.0 (Latest)
- 🎉 Initial release of KaggleCLI, based on [colabcli](https://github.com/ctz168/colabcli)
- 🆕 Remote execution server adapted for Kaggle Notebooks (working dir `/kaggle/working`)
- 🆕 Kaggle session info in `/health` (GPU type, disk usage)
- 🆕 Real-time SSE streaming output (`kagglemcp stream`)
- 🆕 Server monitoring (`kagglemcp watch`)
- ✨ aitun.cc free tunnel - no registration, no token

## 🔒 Security Notes

- **Local execution**: Code runs with your user permissions
- **Remote execution**: Code runs on the remote server with full access
- **No authentication**: KaggleMCP servers have no built-in auth - keep URLs private

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file.

## 🙏 Credits

- Based on [colabcli](https://github.com/ctz168/colabcli) - the same idea, adapted for Kaggle
- Built with [Click](https://click.palletsprojects.com/), [Rich](https://github.com/Textualize/rich), and [IPython](https://ipython.org/)