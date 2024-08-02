---
title: 使用 LangChain 工具
description: 了解如何将 LangChain 工具与 CrewAI 代理集成，以增强基于搜索的查询等功能。
---

## 使用 LangChain 工具
!!! info "LangChain 集成"
    CrewAI 无缝集成了 LangChain 的全面工具包，用于基于搜索的查询等，这里是 LangChain 提供的可用内置工具 [LangChain 工具包](https://python.langchain.com/docs/integrations/tools/)

```python
from crewai import Agent
from langchain.agents import Tool
from langchain.utilities import GoogleSerperAPIWrapper

# Setup API keys
os.environ["SERPER_API_KEY"] = "Your Key"

search = GoogleSerperAPIWrapper()

# Create and assign the search tool to an agent
serper_tool = Tool(
  name="Intermediate Answer",
  func=search.run,
  description="Useful for search-based queries",
)

agent = Agent(
  role='Research Analyst',
  goal='Provide up-to-date market analysis',
  backstory='An expert analyst with a keen eye for market trends.',
  tools=[serper_tool]
)

# rest of the code ...
```

## 结论
工具对于扩展 CrewAI 代理的能力至关重要，使它们能够承担各种任务并有效协作。在使用 CrewAI 构建解决方案时，利用自定义工具和现有工具来增强您的代理并提升 AI 生态系统。考虑利用错误处理、缓存机制以及工具参数的灵活性来优化代理的性能和能力。