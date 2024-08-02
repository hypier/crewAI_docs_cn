---
title: Asynchronous Startup
description: Asynchronously start a team
---

## 介绍
CrewAI 提供了异步启动队伍的能力，允许您以非阻塞的方式开始队伍执行。当您想要并发运行多个队伍或在队伍执行时需要执行其他任务时，此功能特别有用。

```
# 示例代码
def start_crew():
    crew = CrewAI()
    crew.start_async()
```

## 异步团队执行
要异步启动一个团队，使用 `kickoff_async()` 方法。该方法在一个单独的线程中启动团队执行，允许主线程继续执行其他任务。

以下是如何异步启动一个团队的示例：

```python
from crewai import Crew, Agent, Task

# Create an agent with code execution enabled
coding_agent = Agent(
    role="Python Data Analyst",
    goal="Analyze data and provide insights using Python",
    backstory="You are an experienced data analyst with strong Python skills.",
    allow_code_execution=True
)

# Create a task that requires code execution
data_analysis_task = Task(
    description="Analyze the given dataset and calculate the average age of participants. Ages: {ages}",
    agent=coding_agent
)

# Create a crew and add the task
analysis_crew = Crew(
    agents=[coding_agent],
    tasks=[data_analysis_task]
)

# Execute the crew
result = analysis_crew.kickoff_async(inputs={"ages": [25, 30, 35, 40, 45]})
```