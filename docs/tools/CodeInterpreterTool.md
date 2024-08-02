# Code Interpreter Tool

## 描述
此工具用于赋予代理运行自身生成的代码（Python3）的能力。代码在沙箱环境中执行，因此运行任何代码都是安全的。

它非常有用，因为它允许代理生成代码，在同一环境中运行代码，获取结果并利用结果做出决策。

## 需求

- Docker

## 安装
安装 crewai_tools 包
```shell
pip install 'crewai[tools]'
```

## 示例

请记住，在使用此工具时，代码必须由代理本身生成。代码必须是 Python3 代码。第一次运行时可能需要一些时间，因为它需要构建 Docker 镜像。

```python
from crewai import Agent
from crewai_tools import CodeInterpreterTool

Agent(
    ...
    tools=[CodeInterpreterTool()],
)
```

我们还提供了一种简单的方法，可以直接从代理使用它。

```python
from crewai import Agent

agent = Agent(
    ...
    allow_code_execution=True,
)
```