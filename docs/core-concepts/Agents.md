---
title: crewAI Agent
description: 什么是 crewAI Agent 以及如何使用它们。
---

## 什么是代理？
!!! note "什么是代理？"
    代理是一个**自主单元**，其编程目的是：
    <ul>
      <li class='leading-3'>执行任务</li>
      <li class='leading-3'>做出决策</li>
      <li class='leading-3'>与其他代理沟通</li>
    </ul>
      <br/>
    可以将代理视为团队中的一员，拥有特定的技能和特定的工作。代理可以担任不同的角色，如“研究员”、“撰稿人”或“客户支持”，每个角色都为团队的整体目标做出贡献。

## 代理属性

| 属性                       | 参数      | 描述                                                                                                                                                                                                                                          |
| :------------------------ | :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **角色**                   | `role`  | 定义代理在团队中的功能。它决定了代理最适合执行的任务类型。                                                                                                                                                                                    |
| **目标**                   | `goal`  | 代理旨在实现的个人目标。它指导代理的决策过程。                                                                                                                                                                                                  |
| **背景故事**               | `backstory`  | 提供代理角色和目标的背景，使互动和协作动态更加丰富。                                                                                                                                                                                        |
| **语言模型** *(可选)*      | `llm`  | 表示将运行代理的语言模型。它动态从 `OPENAI_MODEL_NAME` 环境变量中获取模型名称，如果未指定，则默认使用 "gpt-4"。                                                                                                                        |
| **工具** *(可选)*          | `tools`  | 代理可以用来执行任务的一组能力或功能。预计是与代理执行环境兼容的自定义类的实例。工具的默认值为一个空列表。                                                                                                                               |
| **函数调用语言模型** *(可选)* | `function_calling_llm`  | 指定将处理该代理工具调用的语言模型，覆盖传递的团队函数调用语言模型。默认值为 `None`。                                                                                                                                                      |
| **最大迭代次数** *(可选)*   | `max_iter` | 最大迭代次数是代理在被迫给出最佳答案之前可以执行的最大迭代次数。默认值为 `25`。                                                                                                                                                             |
| **最大请求频率** *(可选)*   | `max_rpm`  | 最大请求频率是代理为避免速率限制而每分钟可以执行的最大请求次数。它是可选的，可以不指定，默认值为 `None`。                                                                                                                                  |
| **最大执行时间** *(可选)*   | `max_execution_time`  | 最大执行时间是代理执行任务的最大执行时间。它是可选的，可以不指定，默认值为 `None`，意味着没有最大执行时间。                                                                                                                             |
| **详细信息** *(可选)*       | `verbose`  | 将其设置为 `True` 会配置内部记录器提供详细的执行日志，有助于调试和监控。默认值为 `False`。                                                                                                                                              |
| **允许委托** *(可选)*      | `allow_delegation`  | 代理可以将任务或问题委托给彼此，确保每个任务由最合适的代理处理。默认值为 `True`。                                                                                                                                                       |
| **步骤回调** *(可选)*      | `step_callback`  | 在代理的每一步之后调用的函数。这可以用于记录代理的操作或执行其他操作。它将覆盖团队的 `step_callback`。                                                                                                                               |
| **缓存** *(可选)*          | `cache`  | 指示代理是否应使用工具使用的缓存。默认值为 `True`。                                                                                                                                                                                      |
| **系统模板** *(可选)*      | `system_template`  | 指定代理的系统格式。默认值为 `None`。                                                                                                                                                                                                      |
| **提示模板** *(可选)*      | `prompt_template`  | 指定代理的提示格式。默认值为 `None`。                                                                                                                                                                                                      |
| **响应模板** *(可选)*      | `response_template`  | 指定代理的响应格式。默认值为 `None`。                                                                                                                                                                                                      |

## 创建代理

!!! note "代理交互"
    代理可以使用crewAI内置的委托和通信机制相互交互。这允许在团队内进行动态任务管理和问题解决。

要创建一个代理，通常需要初始化一个`Agent`类的实例，并设置所需的属性。以下是一个包含所有属性的概念示例：

```python
# Example: Creating an agent with all attributes
from crewai import Agent

agent = Agent(
  role='Data Analyst',
  goal='Extract actionable insights',
  backstory="""You're a data analyst at a large company.
  You're responsible for analyzing data and providing insights
  to the business.
  You're currently working on a project to analyze the
  performance of our marketing campaigns.""",
  tools=[my_tool1, my_tool2],  # Optional, defaults to an empty list
  llm=my_llm,  # Optional
  function_calling_llm=my_llm,  # Optional
  max_iter=15,  # Optional
  max_rpm=None, # Optional
  max_execution_time=None, # Optional
  verbose=True,  # Optional
  allow_delegation=True,  # Optional
  step_callback=my_intermediate_step_callback,  # Optional
  cache=True,  # Optional
  system_template=my_system_template,  # Optional
  prompt_template=my_prompt_template,  # Optional
  response_template=my_response_template,  # Optional
  config=my_config,  # Optional
  crew=my_crew,  # Optional
  tools_handler=my_tools_handler,  # Optional
  cache_handler=my_cache_handler,  # Optional
  callbacks=[callback1, callback2],  # Optional
  agent_executor=my_agent_executor  # Optional
)
```

## 设置提示模板

提示模板用于格式化代理的提示。您可以使用它来更新代理的系统、常规和响应模板。以下是设置提示模板的示例：

```python
agent = Agent(
        role="{topic} specialist",
        goal="Figure {goal} out",
        backstory="I am the master of {role}",
        system_template="""<|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|>""",
        prompt_template="""<|start_header_id|>user<|end_header_id|>

{{ .Prompt }}<|eot_id|>""",
        response_template="""<|start_header_id|>assistant<|end_header_id|>

{{ .Response }}<|eot_id|>""",
    )
```

## 带上你的第三方代理
!!! note "扩展你的第三方代理，如 LlamaIndex、Langchain、Autogen 或使用 crewai 的 BaseAgent 类构建完全自定义的代理。"

    BaseAgent 包含与您的团队集成所需的属性和方法，以便在您自己的团队中运行和委派任务给其他代理。

    CrewAI 是一个通用的多代理框架，允许所有代理协同工作，以自动化任务和解决问题。


```py
from crewai import Agent, Task, Crew
from custom_agent import CustomAgent # You need to build and extend your own agent logic with the CrewAI BaseAgent class then import it here.

from langchain.agents import load_tools

langchain_tools = load_tools(["google-serper"], llm=llm)

agent1 = CustomAgent(
    role="agent role",
    goal="who is {input}?",
    backstory="agent backstory",
    verbose=True,
)

task1 = Task(
    expected_output="a short biography of {input}",
    description="a short biography of {input}",
    agent=agent1,
)

agent2 = Agent(
    role="agent role",
    goal="summarize the short bio for {input} and if needed do more research",
    backstory="agent backstory",
    verbose=True,
)

task2 = Task(
    description="a tldr summary of the short biography",
    expected_output="5 bullet point summary of the biography",
    agent=agent2,
    context=[task1],
)

my_crew = Crew(agents=[agent1, agent2], tasks=[task1, task2])
crew = my_crew.kickoff(inputs={"input": "Mark Twain"})
```

## 结论
代理是CrewAI框架的构建块。通过理解如何定义和与代理交互，您可以创建利用协作智能力量的复杂AI系统。

```
# Example code
def example_function():
    print("This is an example.")
```