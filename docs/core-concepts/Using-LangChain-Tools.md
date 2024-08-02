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

## Conclusion
Tools are essential for extending the capabilities of CrewAI agents, enabling them to undertake a wide range of tasks and collaborate effectively. When building solutions with CrewAI, leverage custom tools and existing tools to enhance your agents and elevate the AI ecosystem. Consider utilizing error handling, caching mechanisms, and the flexibility of tool parameters to optimize the performance and capabilities of your agents.