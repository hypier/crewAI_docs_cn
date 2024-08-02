---
title: Set a Specific Agent as Manager in CrewAI
description: Learn how to set a custom agent as a manager in CrewAI for better task management and coordination control.

---

# 在CrewAI中设置特定代理作为经理

CrewAI允许用户将特定代理设置为团队的经理，从而提供更好的任务管理和协调控制。此功能使管理角色的定制更加符合您的项目需求。

```
# Example code block
def set_manager(agent):
    team.set_manager(agent)
```

## 使用 `manager_agent` 属性

### 自定义管理代理

`manager_agent` 属性允许您定义一个自定义代理来管理团队。该代理将监督整个过程，确保任务高效且达到最高标准。

```
# 代码示例
def manage_team(tasks):
    for task in tasks:
        print(f"Managing task: {task}")
```

### 示例

```python
import os
from crewai import Agent, Task, Crew, Process

# 定义你的代理
researcher = Agent(
    role="Researcher",
    goal="对人工智能及其代理进行深入研究和分析",
    backstory="你是一位专家研究员，专注于技术、软件工程、人工智能和初创企业。你作为自由职业者，目前正在为一个新客户进行研究。",
    allow_delegation=False,
)

writer = Agent(
    role="Senior Writer",
    goal="撰写关于人工智能及其代理的引人入胜的内容",
    backstory="你是一位高级作家，专注于技术、软件工程、人工智能和初创企业。你作为自由职业者，目前正在为一个新客户撰写内容。",
    allow_delegation=False,
)

# 定义你的任务
task = Task(
    description="生成5个有趣的文章创意列表，然后为每个创意撰写一个引人注目的段落，展示该主题完整文章的潜力。返回创意列表及其段落和你的笔记。",
    expected_output="5个要点，每个要点都有一个段落和相关的笔记。",
)

# 定义管理代理
manager = Agent(
    role="Project Manager",
    goal="高效管理团队，确保高质量的任务完成",
    backstory="你是一位经验丰富的项目经理，擅长监督复杂项目并指导团队走向成功。你的角色是协调团队成员的工作，确保每项任务按时并达到最高标准完成。",
    allow_delegation=True,
)

# 用自定义管理者实例化你的团队
crew = Crew(
    agents=[researcher, writer],
    tasks=[task],
    manager_agent=manager,
    process=Process.hierarchical,
)

# 开始团队的工作
result = crew.kickoff()
```

## 自定义管理代理的好处

- **增强控制**：根据项目的具体需求量身定制管理方法。
- **改善协调**：通过经验丰富的代理确保高效的任务协调和管理。
- **可定制管理**：定义符合项目目标的管理角色和职责。

```
# Example code block
def example_function():
    print("This is an example function.")
```

## 设置管理者 LLM

如果您正在使用层级流程，并且不想设置自定义管理代理，您可以为管理者指定语言模型：

```python
from langchain_openai import ChatOpenAI

manager_llm = ChatOpenAI(model_name="gpt-4")

crew = Crew(
    agents=[researcher, writer],
    tasks=[task],
    process=Process.hierarchical,
    manager_llm=manager_llm
)
```

注意：在使用层级流程时，必须设置 `manager_agent` 或 `manager_llm` 之一。