---
title: 为每个启动
description: 为列表启动一个小组
---

## 介绍
CrewAI 提供了为列表中的每个项目启动一个小组的能力，使您能够对列表中的每个项目执行小组操作。此功能在您需要对多个项目执行相同任务集时特别有用。

## 为每个项目启动团队
要为列表中的每个项目启动团队，请使用 `kickoff_for_each()` 方法。此方法为列表中的每个项目执行团队，使您能够高效处理多个项目。

以下是如何为列表中的每个项目启动团队的示例：

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

datasets = [
  { "ages": [25, 30, 35, 40, 45] },
  { "ages": [20, 25, 30, 35, 40] },
  { "ages": [30, 35, 40, 45, 50] }
]

# Execute the crew
result = analysis_crew.kickoff_for_each(inputs=datasets)
```