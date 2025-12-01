# 极快的 Python 包安装器和解析器

<a href="https://astral.sh">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/astral-sh/uv/raw/main/assets/borderless-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/astral-sh/uv/raw/main/assets/borderless-light.svg">
    <img alt="uv" src="https://github.com/astral-sh/uv/raw/main/assets/borderless-light.svg" width="120px" height="40px">
  </picture>
</a>

<p align="center">
  <a href="https://github.com/astral-sh/uv/actions/workflows/ci.yml?query=branch%3Amain">
    <img src="https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg" alt="CI">
  </a>
  <a href="https://pepy.tech/project/uv">
    <img src="https://static.pepy.tech/badge/uv" alt="下载量">
  </a>
  <a href="https://pypi.org/project/uv/">
    <img src="https://img.shields.io/pypi/v/uv" alt="PyPI 版本">
  </a>
  <img src="https://img.shields.io/pypi/l/uv" alt="许可证">
</p>

<p align="center">
  <a href="https://github.com/astral-sh/uv#installation">安装</a> •
  <a href="https://github.com/astral-sh/uv#getting-started">入门</a> •
  <a href="https://github.com/astral-sh/uv#the-project">项目</a> •
  <a href="https://github.com/astral-sh/uv#the-tool">工具</a> •
  <a href="https://github.com/astral-sh/uv#notes">说明</a>
</p>

**uv** 是一个极速的 Python 包安装器和解析器，用 Rust 编写，旨在作为 `pip` 和 `pip-tools` 工作流的替代品。

<p align="center">
  <a href="https://github.com/astral-sh/uv">
    <img src="https://github.com/astral-sh/uv/raw/main/assets/uv.png" alt="uv 演示" width="600">
  </a>
</p>

功能特点包括：

- ⚡ 比 `pip` 快 10-100 倍
- 🐍 提供可脚本化的 Python 安装和管理
- 📦 可用作 `pip-tools` 的直接替代品
- 🔄 包含 [pip-tools](https://github.com/jazzband/pip-tools) 兼容接口 (`uv pip compile`/`uv pip sync`)
- 🌐 支持 [依赖解析器](https://github.com/davidhalter/forest) 的所有高级功能（例如依赖冲突洞察）
- 🔩 无需 Python 即可安装，自包含
- 🚀 无需 Python 或 Rust 即可运行，单一二进制文件

## 安装

通过独立安装器获取 uv（二进制文件为单一可执行文件，无需依赖）：

```bash
# 在 macOS 和 Linux 上。
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
# 在 Windows 上。
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

或者通过 [PyPI](https://pypi.tuna.tsinghua.edu.cn/project/uv/)：

```bash
# 使用 pip。
pip install uv
```

```bash
# 或者 pipx。
pipx install uv
```

如果通过独立安装器安装，uv 可以自行更新到最新版本：

```bash
uv self update
```

详情请参见 [安装文档](https://docs.astral.sh/uv/getting-started/installation/) 了解详情和其他安装方法。

## 文档

uv 的文档可在 [docs.astral.sh/uv](https://docs.astral.sh/uv) 获取。

此外，命令行参考文档可通过 `uv help` 查看。

## 功能特点

### 项目

uv 管理项目依赖和环境，支持锁文件、工作区等，类似于 `rye` 或 `poetry`：

```console
$ uv init example
在 `/home/user/example` 初始化项目 `example`

$ cd example

$ uv add ruff
在 .venv 创建虚拟环境
在 170ms 内解析 2 个包
   从 file:///home/user/example 构建 example
在 627ms 内准备 2 个包
在 1ms 内安装 2 个包
 + example==0.1.0 (来自 file:///home/user/example)
 + ruff==0.5.0

$ uv run ruff check
所有检查通过！

$ uv lock
在 0.33ms 内解析 2 个包

$ uv sync
在 0.70ms 内解析 2 个包
在 0.02ms 审计 1 个包
```

参见 [项目文档](https://docs.astral.sh/uv/guides/projects/) 以开始使用。

uv 还支持构建和发布项目，即使它们不是用 uv 管理的。参见 [发布指南](https://docs.astral.sh/uv/guides/publish/) 了解更多信息。

### 脚本

uv 管理单文件脚本的依赖和环境。

创建带内联元数据声明其依赖项的新脚本：

```console
$ echo 'import requests; print(requests.get("https://astral.sh"))' > example.py

$ uv add --script example.py requests
更新 `example.py`
```

然后在隔离的虚拟环境中运行该脚本：

```console
$ uv run example.py
从：example.py 读取内联脚本元数据
在 12ms 内安装 5 个包
<Response [200]>
```

参见 [脚本文档](https://docs.astral.sh/uv/guides/scripts/) 以开始使用。