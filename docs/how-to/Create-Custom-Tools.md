---
title: Creating and Using Tools in crewAI
description: A comprehensive guide on creating, using, and managing custom tools within the crewAI framework, including new features and error handling.
---

## 在 crewAI 中创建和利用工具
本指南提供了有关为 crewAI 框架创建自定义工具的详细说明，以及如何高效管理和利用这些工具，结合了最新功能，如工具委派、错误处理和动态工具调用。它还强调了协作工具的重要性，使代理能够执行广泛的操作。

### 前提条件
在创建自己的工具之前，请确保已安装 crewAI 额外工具包：

```bash
pip install 'crewai[tools]'
```

### 子类化 `BaseTool`

要创建一个个性化的工具，从 `BaseTool` 继承并定义必要的属性和 `_run` 方法。

```python
from crewai_tools import BaseTool

class MyCustomTool(BaseTool):
    name: str = "Name of my tool"
    description: str = "What this tool does. It's vital for effective utilization."

    def _run(self, argument: str) -> str:
        # Your tool's logic here
        return "Tool's result"
```

### 使用 `tool` 装饰器

或者，使用 `tool` 装饰器以直接方式创建工具。这需要在函数中指定属性和工具的逻辑。

```python
from crewai_tools import tool

@tool("Tool Name")
def my_simple_tool(question: str) -> str:
    """Tool description for clarity."""
    # Tool logic here
    return "Tool output"
```

### 为工具定义缓存功能

为了通过缓存优化工具性能，请使用 `cache_function` 属性定义自定义缓存策略。

```python
@tool("Tool with Caching")
def cached_tool(argument: str) -> str:
    """Tool functionality description."""
    return "Cacheable result"

def my_cache_strategy(arguments: dict, result: str) -> bool:
    # Define custom caching logic
    return True if some_condition else False

cached_tool.cache_function = my_cache_strategy
```

通过遵循这些指南并将新功能和协作工具纳入您的工具创建和管理流程，您可以充分利用 crewAI 框架的全部功能，从而提升开发体验和 AI 代理的效率。