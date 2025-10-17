# 安装

NautilusTrader正式支持以下64位平台上的Python 3.11-3.13：

| 操作系统       | 支持版本    | CPU架构  |
|------------------------|-----------------------|-------------------|
| Linux (Ubuntu)         | 22.04及更高版本       | x86_64            |
| Linux (Ubuntu)         | 22.04及更高版本       | ARM64             |
| macOS                  | 14.7及更高版本        | ARM64             |
| Windows Server         | 2022及更高版本        | x86_64            |

:::note
NautilusTrader可能在其他平台上工作，但只有上面列出的平台被开发者定期使用并在CI中测试。
:::

我们建议使用最新支持的Python版本，并在虚拟环境中安装[nautilus_trader](https://pypi.org/project/nautilus_trader/)以隔离依赖项。

**有两种支持的安装方式**：

1. 从PyPI或Nautech Systems包索引安装预构建的二进制轮子。
2. 从源码构建。

:::tip
我们强烈建议使用[uv](https://docs.astral.sh/uv)包管理器与"vanilla"CPython进行安装。

Conda和其他Python发行版*可能*工作，但不被官方支持。
:::

## 从PyPI安装

使用Python的pip包管理器从PyPI安装最新的[nautilus_trader](https://pypi.org/project/nautilus_trader/)二进制轮子（或sdist包）：

```bash
pip install -U nautilus_trader
```

## 额外依赖

为特定集成安装可选依赖项作为'extras'：

- `betfair`：Betfair适配器（集成）依赖项。
- `docker`：使用IB网关时（与Interactive Brokers适配器一起）需要Docker。
- `dydx`：dYdX适配器（集成）依赖项。
- `ib`：Interactive Brokers适配器（集成）依赖项。
- `polymarket`：Polymarket适配器（集成）依赖项。

使用pip安装特定extras：

```bash
pip install -U "nautilus_trader[docker,ib]"
```

## 从Nautech Systems包索引安装

Nautech Systems包索引（`packages.nautechsystems.io`）符合[PEP-503](https://peps.python.org/pep-0503/)标准，并为`nautilus_trader`托管稳定版和开发版二进制轮子。
这使用户可以安装最新的稳定版本或预发布版本进行测试。

### 稳定版轮子

稳定版轮子对应于PyPI上`nautilus_trader`的官方发布，并使用标准版本控制。

安装最新的稳定版本：

```bash
pip install -U nautilus_trader --index-url=https://packages.nautechsystems.io/simple
```

### 开发版轮子

开发版轮子从`nightly`和`develop`分支发布，
允许用户在稳定版本之前测试功能和修复。

**注意**：`develop`分支的轮子仅为Linux x86_64平台构建以节省时间和计算资源，而`nightly`轮子支持如下所示的额外平台。

| 平台           | Nightly | Develop |
| :----------------- | :------ | :------ |
| `Linux (x86_64)`   | ✓       | ✓       |
| `Linux (ARM64)`    | ✓       | -       |
| `macOS (ARM64)`    | ✓       | -       |
| `Windows (x86_64)` | ✓       | -       |

此过程还有助于保护计算资源并确保轻松访问在CI管道中测试的确切二进制文件，同时遵循[PEP-440](https://peps.python.org/pep-0440/)版本控制标准：

- `develop`轮子使用版本格式`dev{date}+{build_number}`（例如，`1.208.0.dev20241212+7001`）。
- `nightly`轮子使用版本格式`a{date}`（alpha）（例如，`1.208.0a20241212`）。

:::warning
我们不建议在生产环境中使用开发版轮子，例如控制真实资本的实盘交易。
:::

### 安装命令

默认情况下，pip将安装最新的稳定版本。添加`--pre`标志确保考虑预发布版本，包括开发版轮子。

安装最新的可用预发布版本（包括开发版轮子）：

```bash
pip install -U nautilus_trader --pre --index-url=https://packages.nautechsystems.io/simple
```

安装特定的开发版轮子（例如，2024年12月12日的`1.208.0a20241212`）：

```bash
pip install nautilus_trader==1.208.0a20241212 --index-url=https://packages.nautechsystems.io/simple
```

### 可用版本

您可以在[包索引](https://packages.nautechsystems.io/simple/nautilus-trader/index.html)上查看`nautilus_trader`的所有可用版本。

以编程方式获取和列出可用版本：

```bash
curl -s https://packages.nautechsystems.io/simple/nautilus-trader/index.html | grep -oP '(?<=<a href=")[^"]+(?=")' | awk -F'#' '{print $1}' | sort
```

### 分支更新

- `develop`分支轮子（`.dev`）：每次合并提交时连续构建和发布。
- `nightly`分支轮子（`a`）：当我们每天在**14:00 UTC**自动合并`develop`分支时构建和发布（如果有更改）。

### 保留策略

- `develop`分支轮子（`.dev`）：我们只保留最新的轮子构建。
- `nightly`分支轮子（`a`）：我们只保留30个最新的轮子构建。

## 从源码构建

如果您首先按照`pyproject.toml`中指定的方式安装构建依赖项，则可以从源码使用pip安装。

1. 安装[rustup](https://rustup.rs/)（Rust工具链安装程序）：
   - Linux和macOS：

       ```bash
       curl https://sh.rustup.rs -sSf | sh
       ```

   - Windows：
       - 下载并安装[`rustup-init.exe`](https://win.rustup.rs/x86_64)
       - 使用[Visual Studio 2019构建工具](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)安装"使用C++的桌面开发"
   - 验证（任何系统）：
       从终端会话运行：`rustc --version`

2. 在当前shell中启用`cargo`：
   - Linux和macOS：

       ```bash
       source $HOME/.cargo/env
       ```

   - Windows：
     - 启动新的PowerShell

3. 安装[clang](https://clang.llvm.org/)（LLVM的C语言前端）：
   - Linux：

       ```bash
       sudo apt-get install clang
       ```

   - Windows：
       1. 将Clang添加到您的[Visual Studio 2019构建工具](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)：
          - 开始 | Visual Studio安装程序 | 修改 | C++ Clang tools for Windows (12.0.0 - x64…) = 已选中 | 修改
       2. 在当前shell中启用`clang`：

          ```powershell
          [System.Environment]::SetEnvironmentVariable('path', "C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools\VC\Tools\Llvm\x64\bin\;" + $env:Path,"User")
          ```

   - 验证（任何系统）：
       从终端会话运行：`clang --version`

4. 安装uv（有关更多详细信息，请参阅[uv安装指南](https://docs.astral.sh/uv/getting-started/installation)）：

    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

5. 使用`git`克隆源码，并从项目根目录安装：

    ```bash
    git clone --branch develop --depth 1 https://github.com/nautechsystems/nautilus_trader
    cd nautilus_trader
    uv sync --all-extras
    ```

:::note
`--depth 1`标志仅获取最新提交以实现更快、轻量级的克隆。
:::

6. 设置PyO3编译的环境变量（仅限Linux和macOS）：

    ```bash
    # 设置Python解释器的库路径（在这种情况下是Python 3.13.4）
    export LD_LIBRARY_PATH="$HOME/.local/share/uv/python/cpython-3.13.4-linux-x86_64-gnu/lib:$LD_LIBRARY_PATH"

    # 为PyO3设置Python可执行文件路径
    export PYO3_PYTHON=$(pwd)/.venv/bin/python
    ```

:::note
调整`LD_LIBRARY_PATH`中的Python版本和架构以匹配您的系统。
使用`uv python list`查找您的Python安装的确切路径。
:::

有关其他选项和更多详细信息，请参阅[安装指南](https://nautilustrader.io/docs/latest/getting_started/installation)。

## Redis

使用[Redis](https://redis.io)与NautilusTrader是**可选的**，只有在配置为[缓存](https://nautilustrader.io/docs/latest/concepts/cache)数据库或[消息总线](https://nautilustrader.io/docs/latest/concepts/message_bus)的后端时才需要。
有关更多详细信息，请参阅[安装指南](https://nautilustrader.io/docs/latest/getting_started/installation#redis)的**Redis**部分。

## Makefile

提供了`Makefile`来自动化开发的大部分安装和构建任务。一些目标包括：

- `make install`：在`release`构建模式下安装所有依赖组和extras。
- `make install-debug`：与`make install`相同，但使用`debug`构建模式。
- `make install-just-deps`：仅安装`main`、`dev`和`test`依赖项（不安装包）。
- `make build`：在`release`构建模式下运行构建脚本（默认）。
- `make build-debug`：在`debug`构建模式下运行构建脚本。
- `make build-wheel`：在`release`模式下使用轮子格式运行uv build。
- `make build-wheel-debug`：在`debug`模式下使用轮子格式运行uv build。
- `make cargo-test`：使用`cargo-nextest`运行所有Rust crate测试。
- `make clean`：删除所有构建结果，如`.so`或`.dll`文件。
- `make distclean`：**注意**从仓库中删除git索引中未包含的所有工件。这包括尚未`git add`的源文件。
- `make docs`：使用Sphinx构建文档HTML。
- `make pre-commit`：对所有文件运行预提交检查。
- `make ruff`：使用`pyproject.toml`配置对所有文件运行ruff（带自动修复）。
- `make pytest`：使用`pytest`运行所有测试。
- `make test-performance`：使用[codspeed](https://codspeed.io)运行性能测试。

:::tip
运行`make help`以获取所有可用make目标的文档。
:::

:::tip
有关运行基础设施集成测试，请参阅[crates/infrastructure/TESTS.md](https://github.com/nautechsystems/nautilus_trader/blob/develop/crates/infrastructure/TESTS.md)文件。
:::

## 示例

指标和策略可以在Python和Cython中开发。对于性能和延迟敏感的应用，我们建议使用Cython。以下是一些示例：

- [指标](/nautilus_trader/examples/indicators/ema_python.py)示例用Python编写。
- [指标](/nautilus_trader/indicators/)示例用Cython编写。
- [策略](/nautilus_trader/examples/strategies/)示例用Python编写。
- [回测](/examples/backtest/)示例直接使用`BacktestEngine`。

## Docker

Docker容器使用基础镜像`python:3.12-slim`构建，具有以下变体标签：

- `nautilus_trader:latest`已安装最新发布版本。
- `nautilus_trader:nightly`已安装`nightly`分支的头部。
- `jupyterlab:latest`已安装最新发布版本以及`jupyterlab`和带有配套数据的示例回测notebook。
- `jupyterlab:nightly`已安装`nightly`分支的头部以及`jupyterlab`和带有配套数据的示例回测notebook。

您可以按如下方式拉取容器镜像：

```bash
docker pull ghcr.io/nautechsystems/<image_variant_tag> --platform linux/amd64
```

您可以通过运行以下命令启动回测示例容器：

```bash
docker pull ghcr.io/nautechsystems/jupyterlab:nightly --platform linux/amd64
docker run -p 8888:8888 ghcr.io/nautechsystems/jupyterlab:nightly
```

然后在以下地址打开浏览器：

```bash
http://127.0.0.1:8888/lab
```

:::warning
NautilusTrader目前超过了Jupyter notebook日志记录（stdout输出）的速率限制。
因此，我们在示例中将`log_level`设置为`ERROR`。降低此级别以查看更多日志记录将导致notebook在单元格执行期间挂起。我们正在调查修复方案，可能涉及提高Jupyter的配置速率限制或限制Nautilus的日志刷新。

- <https://github.com/jupyterlab/jupyterlab/issues/12845>
- <https://github.com/deshaw/jupyterlab-limit-output>

:::

## 开发

我们旨在为这个Python、Cython和Rust的混合代码库提供最愉快的开发者体验。
有关有用信息，请参阅[开发者指南](https://nautilustrader.io/docs/latest/developer_guide/index.html)。

:::tip
在对Rust或Cython代码进行更改后运行`make build-debug`以进行最有效的开发工作流程编译。
:::

### 使用Rust进行测试

[cargo-nextest](https://nexte.st)是NautilusTrader的标准Rust测试运行器。
其主要优势是在自己的进程中隔离每个测试，通过避免干扰确保测试可靠性。

您可以通过运行以下命令安装cargo-nextest：

```bash
cargo install cargo-nextest
```

:::tip
使用`make cargo-test`运行Rust测试，它使用**cargo-nextest**和高效配置文件。
:::

## 贡献

感谢您考虑为NautilusTrader做出贡献！我们欢迎任何帮助改进项目的帮助。
如果您有增强或错误修复的想法，第一步是在GitHub上打开一个[issue](https://github.com/nautechsystems/nautilus_trader/issues)与团队讨论。
这有助于确保您的贡献与项目的目标保持一致，并避免重复工作。

在开始之前，请务必查看项目路线图中概述的[开源范围](/ROADMAP.md#open-source-scope)，以了解哪些内容在范围内，哪些内容在范围外。

一旦您准备好开始处理您的贡献，请确保遵循[CONTRIBUTING.md](https://github.com/nautechsystems/nautilus_trader/blob/develop/CONTRIBUTING.md)文件中概述的指南。
这包括签署贡献者许可协议（CLA）以确保您的贡献可以包含在项目中。

:::note
拉取请求应针对`develop`分支（默认分支）。这是新功能和改进在发布前集成的地方。
:::

再次感谢您对NautilusTrader的兴趣！我们期待审查您的贡献并与您合作改进项目。

## 社区

加入我们在[Discord](https://discord.gg/NautilusTrader)上的用户和贡献者社区，聊天并了解NautilusTrader的最新公告和功能。
无论您是希望贡献的开发者，还是只是想了解更多关于平台的信息，我们欢迎所有人加入我们的Discord服务器。

:::warning
NautilusTrader不发行、推广或认可任何加密货币代币。任何声称或沟通暗示其他情况都是未经授权和虚假的。

NautilusTrader的所有官方更新和沟通将专门通过<https://nautilustrader.io>、我们的[Discord服务器](https://discord.gg/NautilusTrader)或我们的X（Twitter）账户[@NautilusTrader](https://x.com/NautilusTrader)分享。

如果您遇到任何可疑活动，请向相应平台报告并联系我们<info@nautechsystems.io>。
:::

## 许可证

NautilusTrader的源代码在GitHub上根据[GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.en.html)提供。
对项目的贡献是受欢迎的，需要完成标准的[贡献者许可协议（CLA）](https://github.com/nautechsystems/nautilus_trader/blob/develop/CLA.md)。

---

NautilusTrader™由Nautech Systems开发和维护，这是一家专门从事高性能交易系统开发的技术公司。
有关更多信息，请访问<https://nautilustrader.io>。

© 2015-2025 Nautech Systems Pty Ltd. 保留所有权利。

![nautechsystems](https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/ns-logo.png "nautechsystems")
<img src="https://github.com/nautechsystems/nautilus_trader/raw/develop/assets/ferris.png" width="128">
