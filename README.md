# 🚀 KaggleCLI

[![Kaggle](https://img.shields.io/badge/Run%20on-Kaggle-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code)
[![GitHub](https://img.shields.io/badge/GitHub-ctz168%2Fkagglecli-blue?logo=github)](https://github.com/ctz168/kagglecli)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.0.0-green.svg)](https://github.com/ctz168/kagglecli)

A powerful command-line tool to run Jupyter Notebooks with streaming output on **Kaggle**.

一个强大的命令行工具，用于在 **Kaggle** 上远程运行 Jupyter Notebook，支持流式输出。

## 📖 Documentation

- **[English](README_en.md)** - Full documentation in English
- **[中文](README_zh.md)** - 完整中文文档

## 🎯 Quick Start

### English
1. Deploy server on Kaggle (import [`kaggle_server.ipynb`](kaggle_server.ipynb), enable Internet, Run All)
2. Install CLI: `pip install git+https://github.com/ctz168/kagglecli.git`
3. Run: `kagglemcp stream notebook.ipynb -u https://aitun.cc/your-code`

### 中文
1. 在 Kaggle 上部署服务器（导入 [kaggle_server.ipynb](kaggle_server.ipynb)，开启 Internet，Run All）
2. 安装 CLI：`pip install git+https://github.com/ctz168/kagglecli.git`
3. 运行：`kagglemcp stream notebook.ipynb -u https://aitun.cc/your-code`

## 📄 License

MIT License - See [LICENSE](LICENSE) file.

## 🙏 Credits

- Based on [colabcli](https://github.com/ctz168/colabcli) - adapted for Kaggle
- Built with [Click](https://click.palletsprojects.com/), [Rich](https://github.com/Textualize/rich), and [IPython](https://ipython.org/)
