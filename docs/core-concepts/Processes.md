---
title: Managing Workflows in CrewAI
description: A detailed guide on managing workflows through processes in CrewAI, including updated implementation details.
---

## Understanding Processes
!!! note "Core Concept"
    In CrewAI, the process coordination agent's task execution is similar to project management in human teams. These processes ensure that tasks are allocated and executed efficiently according to predefined strategies.

## 过程实现

- **顺序**：按顺序执行任务，确保任务以有序的进展完成。
- **层级**：以管理层级组织任务，任务根据结构化的指挥链进行委派和执行。必须在团队中指定一个管理语言模型（`manager_llm`）或自定义管理代理（`manager_agent`），以启用层级过程，从而促进任务的创建和管理。
- **协商过程（计划中）**：旨在实现代理之间的协作决策执行任务，这种过程类型在CrewAI中引入了一种民主的任务管理方法。该过程计划在未来开发中实现，目前尚未在代码库中实施。

## The Role of Processes in Team Collaboration
Processes enable individual agents to operate as a unified entity, streamlining their efforts to achieve common goals efficiently and coherently.

## 将进程分配给团队
要将进程分配给团队，在创建团队时指定进程类型以设置执行策略。对于层次进程，请确保为管理代理定义 `manager_llm` 或 `manager_agent`。

```python
from crewai import Crew
from crewai.process import Process
from langchain_openai import ChatOpenAI

# Example: Creating a crew with a sequential process
crew = Crew(
    agents=my_agents,
    tasks=my_tasks,
    process=Process.sequential
)

# Example: Creating a crew with a hierarchical process
# Ensure to provide a manager_llm or manager_agent
crew = Crew(
    agents=my_agents,
    tasks=my_tasks,
    process=Process.hierarchical,
    manager_llm=ChatOpenAI(model="gpt-4")
    # or
    # manager_agent=my_manager_agent
)
```
**注意：** 确保在创建 `Crew` 对象之前定义 `my_agents` 和 `my_tasks`，并且对于层次进程，也需要提供 `manager_llm` 或 `manager_agent`。

## 顺序过程
该方法反映了动态团队工作流程，以深思熟虑和系统化的方式推进任务。任务执行遵循任务列表中预定义的顺序，一个任务的输出作为下一个任务的上下文。

要自定义任务上下文，请在 `Task` 类中使用 `context` 参数指定应作为后续任务上下文的输出。

## 层级流程
模拟企业层级结构，CrewAI 允许指定自定义经理代理，或自动创建一个，要求指定经理语言模型 (`manager_llm`)。该代理负责任务执行，包括规划、委派和验证。任务并非预先分配；经理根据代理的能力分配任务，审核输出并评估任务完成情况。

```
# Sample code block
def example_function():
    print("This is a code block.")
```

## 过程类：详细概述
`Process` 类被实现为枚举 (`Enum`)，确保类型安全并将过程值限制为定义的类型 (`sequential`, `hierarchical`)。共识过程计划在未来纳入，强调我们对持续发展和创新的承诺。

## Additional Task Features
- **Asynchronous Execution**: Tasks can now be executed asynchronously, allowing for parallel processing and efficiency improvements. This feature is designed to enable tasks to occur simultaneously, thereby enhancing the overall productivity of the team.
- **Manual Input Review**: An optional feature that allows for manual review of task outputs to ensure quality and accuracy before finalization. This additional step introduces a layer of oversight, providing opportunities for human intervention and validation.
- **Output Customization**: Tasks support multiple output formats, including JSON (`output_json`), Pydantic models (`output_pydantic`), and file outputs (`output_file`), providing flexibility in capturing and utilizing task results. This makes the possibilities for output vast, catering to diverse needs and requirements.

## Conclusion  
The structured collaboration facilitated by the workflows in CrewAI is essential for achieving systematic teamwork among agents. This document has been updated to reflect the latest features, enhancements, and planned consensus process integrations, ensuring that users have access to the most current and comprehensive information.