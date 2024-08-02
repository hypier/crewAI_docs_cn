---
title: 在 crewAI 中使用顺序过程
description: 一份全面的指南，旨在利用顺序过程在 crewAI 项目中执行任务。
---

## 介绍
CrewAI 提供了一个灵活的框架，用于以结构化的方式执行任务，支持顺序和层次化的流程。本指南概述了如何有效地实施这些流程，以确保高效的任务执行和项目完成。

## 顺序过程概述
顺序过程确保任务一个接一个地执行，遵循线性进程。这种方法非常适合需要按特定顺序完成任务的项目。

### 关键特性
- **线性任务流程**：通过按照预定顺序处理任务，确保有序的进展。
- **简单性**：最适合具有清晰、逐步任务的项目。
- **易于监控**：便于轻松跟踪任务完成情况和项目进展。

## 实施顺序过程
要使用顺序过程，组建您的团队并定义任务的执行顺序。

```python
from crewai import Crew, Process, Agent, Task

# Define your agents
researcher = Agent(
  role='Researcher',
  goal='Conduct foundational research',
  backstory='An experienced researcher with a passion for uncovering insights'
)
analyst = Agent(
  role='Data Analyst',
  goal='Analyze research findings',
  backstory='A meticulous analyst with a knack for uncovering patterns'
)
writer = Agent(
  role='Writer',
  goal='Draft the final report',
  backstory='A skilled writer with a talent for crafting compelling narratives'
)

research_task = Task(description='Gather relevant data...', agent=researcher, expected_output='Raw Data')
analysis_task = Task(description='Analyze the data...', agent=analyst, expected_output='Data Insights')
writing_task = Task(description='Compose the report...', agent=writer, expected_output='Final Report')

# Form the crew with a sequential process
report_crew = Crew(
  agents=[researcher, analyst, writer],
  tasks=[research_task, analysis_task, writing_task],
  process=Process.sequential
)

# Execute the crew
result = report_crew.kickoff()
```

### 工作流程示例
1. **初始任务**：在顺序工作流程中，第一个代理完成他们的任务并发出完成信号。
2. **后续任务**：代理根据工作流程类型接管他们的任务，依据之前任务的结果或管理层的指令进行指导。
3. **完成**：一旦最后一个任务执行完毕，工作流程结束，项目完成。

## 高级功能

### 任务委派
在顺序流程中，如果代理的 `allow_delegation` 设置为 `True`，则他们可以将任务委派给团队中的其他代理。当团队中有多个代理时，此功能会自动设置。

### 异步执行
任务可以异步执行，在适当的情况下允许并行处理。要创建异步任务，请在定义任务时设置 `async_execution=True`。

### 内存与缓存
CrewAI 支持内存和缓存功能：
- **内存**：在创建 Crew 时通过设置 `memory=True` 来启用。这允许代理在任务之间保留信息。
- **缓存**：默认情况下，缓存是启用的。设置 `cache=False` 可禁用它。

```
# 示例代码
def example_function():
    return "Hello, World!"
```

### 回调
您可以在任务和步骤级别设置回调：
- `task_callback`: 在每个任务完成后执行。
- `step_callback`: 在代理执行的每个步骤后执行。

```
# 代码示例
def example():
    pass
```

### 使用指标
CrewAI 跟踪所有任务和代理的令牌使用情况。您可以在执行后访问这些指标。

```
# 示例代码
def example_function():
    pass
```

## 顺序流程的最佳实践
1. **顺序重要**：以逻辑顺序排列任务，使每个任务都建立在前一个任务的基础上。
2. **清晰的任务描述**：为每个任务提供详细描述，以有效指导代理。
3. **适当的代理选择**：将代理的技能和角色与每个任务的要求相匹配。
4. **利用上下文**：使用前一个任务的上下文来指导后续任务。