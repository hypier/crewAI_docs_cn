---
title: 强制工具输出作为结果
description: 了解如何在 crewAI 中强制工具输出作为 Agent 任务的结果。
---

## 介绍
在 CrewAI 中，您可以强制将工具的输出作为代理任务的结果。此功能在您希望确保工具输出被捕获并作为任务结果返回时非常有用，并且可以避免代理在任务执行过程中修改输出。

```
# Sample code
def example_function():
    return "This is an example."
```

## 强制工具输出作为结果
要强制工具输出作为代理任务的结果，您可以在创建代理时将 `result_as_answer` 参数设置为 `True`。此参数确保工具输出被捕获并作为任务结果返回，而不会被代理修改。

以下是如何强制工具输出作为代理任务结果的示例：

```python
# ...
# Define a custom tool that returns the result as the answer
coding_agent =Agent(
        role="Data Scientist",
        goal="Product amazing resports on AI",
        backstory="You work with data and AI",
        tools=[MyCustomTool(result_as_answer=True)],
    )
# ...
```

### Workflow Example

1. **Task Execution**: The agent performs tasks using the provided tools.
2. **Tool Output**: The tool generates output, which is recorded as the task result.
3. **Agent Interaction**: The agent can reflect and draw insights from the tools, but the output is not modified.
4. **Result Return**: The tool output is returned as the task result, without any modifications.