# 入门指南

要开始使用NautilusTrader，您需要：

- 安装了`nautilus_trader`包的Python 3.11–3.13环境。
- 运行Python脚本或Jupyter notebook进行回测和/或实盘交易的方法。

## [安装](installation.md)

**安装**指南将帮助确保NautilusTrader在您的机器上正确安装。

## [快速开始](quickstart.md)

**快速开始**提供了设置您的第一个回测的分步指导。

## 仓库中的示例

[在线文档](https://nautilustrader.io/docs/latest/)只显示了一部分示例。要查看完整集合，请参阅GitHub上的此仓库。

下表按推荐的学习进度顺序列出了示例位置：

| 目录                   | 包含内容                                                                                                                    |
|:----------------------------|:----------------------------------------------------------------------------------------------------------------------------|
| [examples/](https://github.com/nautechsystems/nautilus_trader/tree/develop/examples)                 | 完全可运行的、自包含的Python示例。                                                                                     |
| [docs/tutorials/](tutorials/)           | 演示常见工作流程的Jupyter notebook教程。                                                                              |
| [docs/concepts/](concepts/)            | 带有简洁代码片段说明关键功能的概念指南。 |
| [nautilus_trader/examples/](../nautilus_trader/examples/) | 基本策略、指标和执行算法的纯Python示例。                                     |
| [tests/unit_tests/](../../tests/unit_tests/)         | 涵盖核心功能和边缘情况的单元测试。                      |

## 回测API级别

NautilusTrader为回测提供了两个不同的API级别：

| API级别      | 描述                           | 特征                                                                                                                                                                                                                                                                                                                                                        |
|:---------------|:--------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 高级API | 使用`BacktestNode`和`TradingNode` | 推荐用于生产：更容易过渡到实盘交易；需要基于Parquet的数据目录。 |
| 低级API  | 使用`BacktestEngine`                 | 用于库开发：无实盘交易路径；直接组件访问；可能鼓励非实盘兼容模式。 |

回测涉及在历史数据上运行模拟交易系统。

要开始使用NautilusTrader进行回测，您需要首先了解提供的两个不同API级别，以及哪一个可能更适合您的预期用例。

:::info
有关选择哪个API级别的更多信息，请参阅[回测](../concepts/backtesting.md)指南。
:::

### [回测（低级API）](backtest_low_level.md)

本教程介绍如何使用数据加载器和包装器加载原始数据（Nautilus外部），然后使用`BacktestEngine`使用此数据运行单个回测。

### [回测（高级API）](backtest_high_level.md)

本教程介绍如何将原始数据（Nautilus外部）加载到数据目录中，然后使用`BacktestNode`使用此数据运行单个回测。

## 在Docker中运行

或者，您可以下载一个自包含的Docker化Jupyter notebook服务器，无需设置或安装。这是启动和运行以试用NautilusTrader的最快方法。请注意，删除容器也会删除任何数据。

- 要开始，请安装docker：
  - 转到[Docker安装指南](https://docs.docker.com/get-docker/)并按照说明操作。
- 从终端下载最新镜像：
  - `docker pull ghcr.io/nautechsystems/jupyterlab:nightly --platform linux/amd64`
- 运行docker容器，暴露jupyter端口：
  - `docker run -p 8888:8888 ghcr.io/nautechsystems/jupyterlab:nightly`
- 在浏览器中打开`localhost:{port}`：
  - <http://localhost:8888>

:::info
NautilusTrader目前超过了Jupyter notebook日志记录（stdout输出）的速率限制，
因此我们在示例中将`log_level`设置为`ERROR`。降低此级别以查看
更多日志记录将导致notebook在单元格执行期间挂起。我们目前正在
调查修复方案，涉及提高Jupyter的配置速率限制，
或限制Nautilus的日志刷新。

- <https://github.com/jupyterlab/jupyterlab/issues/12845>
- <https://github.com/deshaw/jupyterlab-limit-output>

:::
