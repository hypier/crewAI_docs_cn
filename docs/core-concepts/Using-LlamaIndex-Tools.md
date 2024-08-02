---
title: 使用 LlamaIndex 工具
description: 了解如何将 LlamaIndex 工具与 CrewAI 代理集成，以增强基于搜索的查询等功能。
---

## 使用 LlamaIndex 工具

!!! info "LlamaIndex 集成"
    CrewAI 无缝集成了 LlamaIndex 的全面工具包，用于 RAG（检索增强生成）和代理管道，支持高级基于搜索的查询等功能。以下是 LlamaIndex 提供的内置工具。

```python
from crewai import Agent
from crewai_tools import LlamaIndexTool

# Example 1: Initialize from FunctionTool
from llama_index.core.tools import FunctionTool

your_python_function = lambda ...: ...
og_tool = FunctionTool.from_defaults(your_python_function, name="<name>", description='<description>')
tool = LlamaIndexTool.from_tool(og_tool)

# Example 2: Initialize from LlamaHub Tools
from llama_index.tools.wolfram_alpha import WolframAlphaToolSpec
wolfram_spec = WolframAlphaToolSpec(app_id="<app_id>")
wolfram_tools = wolfram_spec.to_tool_list()
tools = [LlamaIndexTool.from_tool(t) for t in wolfram_tools]

# Example 3: Initialize Tool from a LlamaIndex Query Engine
query_engine = index.as_query_engine()
query_tool = LlamaIndexTool.from_query_engine(
    query_engine,
    name="Uber 2019 10K 查询工具",
    description="使用此工具查找 2019 年 Uber 10K 年度报告"
)

# Create and assign the tools to an agent
agent = Agent(
  role='研究分析师',
  goal='提供最新的市场分析',
  backstory='一位对市场趋势有敏锐洞察力的专家分析师。',
  tools=[tool, *tools, query_tool]
)

# rest of the code ...
```

## 开始的步骤

要有效使用 LlamaIndexTool，请按照以下步骤操作：

1. **软件包安装**：确认您的 Python 环境中已安装 `crewai[tools]` 软件包。

    ```shell
    pip install 'crewai[tools]'
    ```

2. **安装和使用 LlamaIndex**：按照 LlamaIndex 文档 [LlamaIndex Documentation](https://docs.llamaindex.ai/) 设置 RAG/代理管道。