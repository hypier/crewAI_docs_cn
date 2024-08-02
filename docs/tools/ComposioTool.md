# ComposioTool 文档

## 描述

该工具是 composio 工具集的封装，为您的代理提供访问 composio SDK 中各种工具的能力。

## 安装

要将此工具集成到您的项目中，请按照以下安装说明进行操作：

```shell
pip install composio-core
pip install 'crewai[tools]'
```

安装完成后，您可以运行 `composio login` 或将您的 composio API 密钥导出为 `COMPOSIO_API_KEY`。

## 示例

以下示例演示如何初始化工具并执行 GitHub 操作：

1. 初始化工具集

```python
from composio import App
from crewai_tools import ComposioTool
from crewai import Agent, Task


tools = [ComposioTool.from_action(action=Action.GITHUB_ACTIVITY_STAR_REPO_FOR_AUTHENTICATED_USER)]
```

如果您不知道要使用哪个操作，可以使用 `from_app` 和 `tags` 过滤器来获取相关操作

```python
tools = ComposioTool.from_app(App.GITHUB, tags=["important"])
```

或者使用 `use_case` 来搜索相关操作

```python
tools = ComposioTool.from_app(App.GITHUB, use_case="Star a github repository")
```

2. 定义代理

```python
crewai_agent = Agent(
    role="Github Agent",
    goal="You take action on Github using Github APIs",
    backstory=(
        "You are AI agent that is responsible for taking actions on Github "
        "on users behalf. You need to take action on Github using Github APIs"
    ),
    verbose=True,
    tools=tools,
)
```

3. 执行任务

```python
task = Task(
    description="Star a repo ComposioHQ/composio on GitHub",
    agent=crewai_agent,
    expected_output="if the star happened",
)

task.execute()
```

* 更详细的工具列表可以在 [这里](https://app.composio.dev) 找到