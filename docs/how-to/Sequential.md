---
title: Using Sequential Processes in crewAI
description: A comprehensive guide to leveraging sequential processes for task execution in the crewAI project.
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

### Workflow Example
1. **Initial Task**: In a sequential workflow, the first agent completes their task and emits a completion signal.
2. **Subsequent Tasks**: Agents take over their tasks based on the workflow type, guided by the results of previous tasks or directives from management.
3. **Completion**: Once the final task is executed, the workflow ends, and the project is complete.

## Advanced Features

### 任务委派
在顺序流程中，如果代理的 `allow_delegation` 设置为 `True`，则他们可以将任务委派给团队中的其他代理。当团队中有多个代理时，此功能会自动设置。

### Asynchronous Execution
Tasks can be executed asynchronously, allowing for parallel processing when appropriate. To create an asynchronous task, set `async_execution=True` when defining the task.

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

## Best Practices for Sequential Processes
1. **Order Matters**: Arrange tasks in a logical sequence so that each task builds on the previous one.
2. **Clear Task Descriptions**: Provide detailed descriptions for each task to effectively guide agents.
3. **Appropriate Agent Selection**: Match the skills and roles of agents to the requirements of each task.
4. **Leverage Context**: Use the context from the previous task to inform subsequent tasks.