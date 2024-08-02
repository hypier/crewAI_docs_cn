---
title: 在CrewAI中实施层次过程
description: 一份全面的指南，帮助您理解和应用层次过程于您的CrewAI项目，已更新以反映最新的编码实践和功能。
---

## 介绍
CrewAI中的分层过程引入了一种结构化的任务管理方法，模拟传统的组织层级，以实现高效的任务分配和执行。这种系统化的工作流程通过确保任务以最佳效率和准确性处理，从而提升项目成果。

!!! note "复杂性与效率"
    分层过程旨在利用像GPT-4这样的先进模型，优化令牌使用，同时以更高的效率处理复杂任务。

## 层次流程概述
默认情况下，CrewAI中的任务通过顺序流程进行管理。然而，采用层次化的方法可以在任务管理中实现明确的层次结构，其中“管理者”代理协调工作流程，分配任务并验证结果，以实现高效的执行。该管理者代理现在可以由CrewAI自动创建，也可以由用户明确设置。
```
# Example code block
def example_function():
    print("This is an example.")
```

### 关键特性
- **任务分配**：管理代理根据团队成员的角色和能力分配任务。
- **结果验证**：管理者评估结果，以确保它们符合所需标准。
- **高效工作流程**：模拟企业结构，提供有组织的任务管理方法。

```
def example_function():
    print("This is an example function.")
```

## 实施层级过程
要利用层级过程，必须明确将过程属性设置为 `Process.hierarchical`，因为默认行为为 `Process.sequential`。定义一个有指定经理的团队，并建立明确的指挥链。

!!! note "工具和代理分配"
    在代理级别分配工具，以便在经理的指导下促进任务委派和执行。工具也可以在任务级别指定，以便在任务执行期间精确控制工具的可用性。

!!! note "经理 LLM 要求"
    配置 `manager_llm` 参数对于层级过程至关重要。系统需要设置一个经理 LLM 以确保正常功能，从而保证量身定制的决策。

```python
from langchain_openai import ChatOpenAI
from crewai import Crew, Process, Agent

# Agents are defined with attributes for backstory, cache, and verbose mode
researcher = Agent(
    role='Researcher',
    goal='Conduct in-depth analysis',
    backstory='Experienced data analyst with a knack for uncovering hidden trends.',
    cache=True,
    verbose=False,
    # tools=[]  # This can be optionally specified; defaults to an empty list
)
writer = Agent(
    role='Writer',
    goal='Create engaging content',
    backstory='Creative writer passionate about storytelling in technical domains.',
    cache=True,
    verbose=False,
    # tools=[]  # Optionally specify tools; defaults to an empty list
)

# Establishing the crew with a hierarchical process and additional configurations
project_crew = Crew(
    tasks=[...],  # Tasks to be delegated and executed under the manager's supervision
    agents=[researcher, writer],
    manager_llm=ChatOpenAI(temperature=0, model="gpt-4"),  # Mandatory if manager_agent is not set
    process=Process.hierarchical,  # Specifies the hierarchical management approach
    memory=True,  # Enable memory usage for enhanced task execution
    manager_agent=None,  # Optional: explicitly set a specific agent as manager instead of the manager_llm
)
```

### Workflow Implementation
1. **Task Allocation**: The manager strategically assigns tasks based on each agent's capabilities and available tools.
2. **Execution and Review**: Agents complete tasks with options for asynchronous execution and callback functions to ensure a smooth workflow.
3. **Sequential Task Progression**: Although it is a hierarchical process, tasks follow a logical sequence for smooth advancement, thanks to the manager's oversight.

## Conclusion  
Adopting a layered process in CrewAI, along with correctly configuring and understanding the system's functionalities, helps achieve orderly and efficient project management. Utilize advanced features and customization options to tailor workflows to your specific needs, ensuring optimal task execution and project success.