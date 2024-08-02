---
title: 在CrewAI中管理工作流程
description: 关于通过CrewAI中的流程管理工作流程的详细指南，包括更新的实施细节。
---

## 理解流程
!!! note "核心概念"
    在 CrewAI 中，流程协调代理的任务执行类似于人类团队中的项目管理。这些流程确保任务根据预定义的策略有效分配和执行。

## 过程实现

- **顺序**：按顺序执行任务，确保任务以有序的进展完成。
- **层级**：以管理层级组织任务，任务根据结构化的指挥链进行委派和执行。必须在团队中指定一个管理语言模型（`manager_llm`）或自定义管理代理（`manager_agent`），以启用层级过程，从而促进任务的创建和管理。
- **协商过程（计划中）**：旨在实现代理之间的协作决策执行任务，这种过程类型在CrewAI中引入了一种民主的任务管理方法。该过程计划在未来开发中实现，目前尚未在代码库中实施。

## 流程在团队协作中的作用
流程使个体代理能够作为一个统一的实体运作，从而有效且连贯地简化他们的努力，以实现共同目标。

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

## 附加任务功能
- **异步执行**：任务现在可以异步执行，允许并行处理和效率提升。此功能旨在使任务能够同时进行，从而提高团队的整体生产力。
- **手动输入审核**：一个可选功能，允许手动审核任务输出，以确保质量和准确性在最终确定之前。此额外步骤引入了一层监督，提供了人为干预和验证的机会。
- **输出自定义**：任务支持多种输出格式，包括 JSON (`output_json`)、Pydantic 模型 (`output_pydantic`) 和文件输出 (`output_file`)，为捕获和利用任务结果提供灵活性。这使得输出的可能性非常广泛，以满足多样化的需求和要求。

## 结论  
CrewAI 中的工作流程促进的结构化协作对于实现代理之间的系统化团队合作至关重要。本文档已更新，以反映最新的功能、增强和计划中的共识过程集成，确保用户可以访问最新和最全面的信息。