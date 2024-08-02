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

### 工作流程示例

1. **任务执行**: 代理使用提供的工具执行任务。
2. **工具输出**: 工具生成输出，记录为任务结果。
3. **代理交互**: 代理可以从工具中反思并获取见解，但输出不会被修改。
4. **结果返回**: 工具输出作为任务结果返回，不进行任何修改。