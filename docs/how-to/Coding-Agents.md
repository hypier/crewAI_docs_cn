---
title: 编程代理
description: 了解如何使您的 crewAI 代理能够编写和执行代码，并探索增强功能的高级特性。
---

## 介绍

crewAI Agents 现在具备编写和执行代码的强大能力，显著提升了它们解决问题的能力。此功能对于需要计算或编程解决方案的任务特别有用。

## Enable Code Execution

要为代理启用代码执行，在创建代理时将 `allow_code_execution` 参数设置为 `True`。以下是一个示例：

```python
from crewai import Agent

coding_agent = Agent(
    role="Senior Python Developer",
    goal="Craft well-designed and thought-out code",
    backstory="You are a senior Python developer with extensive experience in software architecture and best practices.",
    allow_code_execution=True
)
```

## 重要考虑事项

1. **模型选择**：强烈建议在启用代码执行时使用更强大的模型，如 Claude 3.5 Sonnet 和 GPT-4。这些模型对编程概念有更好的理解，更有可能生成正确和高效的代码。

2. **错误处理**：代码执行功能包括错误处理。如果执行的代码引发异常，代理将收到错误消息，并可以尝试修正代码或提供替代解决方案。

3. **依赖项**：要使用代码执行功能，您需要安装 `crewai_tools` 包。如果未安装，代理将记录一条信息消息：“编码工具不可用。请安装 crewai_tools。”

## 代码执行过程

当一个启用代码执行的代理遇到需要编程的任务时：

1. 代理分析任务并确定需要代码执行。
2. 它制定解决问题所需的Python代码。
3. 代码被发送到内部代码执行工具（`CodeInterpreterTool`）。
4. 工具在受控环境中执行代码并返回结果。
5. 代理解释结果并将其纳入响应中或用于进一步的问题解决。

## 示例用法

这是一个创建具有代码执行能力的代理并在任务中使用的详细示例：

```python
from crewai import Agent, Task, Crew

# Create an agent with code execution enabled
coding_agent = Agent(
    role="Python Data Analyst",
    goal="Analyze data and provide insights using Python",
    backstory="You are an experienced data analyst with strong Python skills.",
    allow_code_execution=True
)

# Create a task that requires code execution
data_analysis_task = Task(
    description="Analyze the given dataset and calculate the average age of participants.",
    agent=coding_agent
)

# Create a crew and add the task
analysis_crew = Crew(
    agents=[coding_agent],
    tasks=[data_analysis_task]
)

# Execute the crew
result = analysis_crew.kickoff()

print(result)
```

在这个示例中，`coding_agent` 可以编写和执行 Python 代码以执行数据分析任务。