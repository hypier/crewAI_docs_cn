---
title: crewAI 任务
description: 关于在 crewAI 框架内管理和创建任务的详细指南，反映最新的代码库更新。
---

## 任务概述

!!! note "什么是任务？"
在 crewAI 框架中，任务是由代理完成的具体分配。它们提供执行所需的所有详细信息，例如描述、负责的代理、所需工具等，从而促进各种行动复杂性的实现。

crewAI 中的任务可以是协作性的，要求多个代理共同工作。这通过任务属性进行管理，并由 Crew 的流程进行协调，从而增强团队合作和效率。

## 任务属性

| 属性                             | 参数               | 描述                                                                                                                |
| :------------------------------- | :---------------- | :----------------------------------------------------------------------------------------------------------------- |
| **描述**                         | `description`     | 清晰、简洁地说明任务的内容。                                                                                      |
| **代理**                         | `agent`           | 负责该任务的代理，可以直接指定或由团队的流程分配。                                                                |
| **预期输出**                     | `expected_output` | 详细描述任务完成的样子。                                                                                          |
| **工具** _(可选)_               | `tools`           | 代理可以利用来执行任务的功能或能力。                                                                              |
| **异步执行** _(可选)_           | `async_execution` | 如果设置，任务将异步执行，允许在不等待完成的情况下进行进展。                                                    |
| **上下文** _(可选)_             | `context`         | 指定输出作为该任务上下文的任务。                                                                                  |
| **配置** _(可选)_               | `config`          | 执行任务的代理的附加配置细节，允许进一步定制。                                                                    |
| **输出 JSON** _(可选)_          | `output_json`     | 输出一个 JSON 对象，需要一个 OpenAI 客户端。只能设置一种输出格式。                                               |
| **输出 Pydantic** _(可选)_      | `output_pydantic` | 输出一个 Pydantic 模型对象，需要一个 OpenAI 客户端。只能设置一种输出格式。                                       |
| **输出文件** _(可选)_           | `output_file`     | 将任务输出保存到文件。如果与 `输出 JSON` 或 `输出 Pydantic` 一起使用，指定输出的保存方式。                      |
| **输出** _(可选)_               | `output`          | 任务的输出，包括原始、JSON 和 Pydantic 输出以及其他详细信息。                                                     |
| **回调** _(可选)_               | `callback`        | 任务完成时执行的 Python 可调用对象，带有任务的输出。                                                              |
| **人工输入** _(可选)_           | `human_input`     | 指示任务在结束时是否需要人工反馈，适用于需要人工监督的任务。                                                      |

## 创建任务

创建任务涉及定义其范围、负责的代理人以及任何附加属性以增加灵活性：

```python
from crewai import Task

task = Task(
    description='Find and summarize the latest and most relevant news on AI',
    agent=sales_agent,
    expected_output='A bullet list summary of the top 5 most important AI news',
)
```

!!! note "任务分配"
直接指定一个 `agent` 进行分配，或者让 `hierarchical` CrewAI 的流程根据角色、可用性等进行决定。

## 任务输出

!!! note "理解任务输出"
在 crewAI 框架中，任务的输出被封装在 `TaskOutput` 类中。该类提供了一种结构化的方式来访问任务的结果，包括原始字符串、JSON 和 Pydantic 模型等各种格式。  
默认情况下，`TaskOutput` 只会包含 `raw` 输出。只有当原始 `Task` 对象配置了 `output_pydantic` 或 `output_json` 时，`TaskOutput` 才会包含 `pydantic` 或 `json_dict` 输出。

### 任务输出属性

| 属性              | 参数            | 类型                        | 描述                                                                                              |
| :---------------- | :-------------- | :------------------------- | :------------------------------------------------------------------------------------------------- |
| **描述**          | `description`   | `str`                      | 任务的简要描述。                                                                                   |
| **摘要**          | `summary`       | `Optional[str]`            | 任务的简短摘要，由描述自动生成。                                                                   |
| **原始**          | `raw`           | `str`                      | 任务的原始输出。这是输出的默认格式。                                                               |
| **Pydantic**      | `pydantic`      | `Optional[BaseModel]`      | 表示任务结构化输出的 Pydantic 模型对象。                                                           |
| **JSON 字典**     | `json_dict`     | `Optional[Dict[str, Any]]` | 表示任务 JSON 输出的字典。                                                                         |
| **代理**          | `agent`         | `str`                      | 执行任务的代理。                                                                                   |
| **输出格式**      | `output_format` | `OutputFormat`             | 任务输出的格式，包括 RAW、JSON 和 Pydantic 等选项。默认格式为 RAW。                                |

### 任务输出方法和属性

| 方法/属性       | 描述                                                                                             |
| :-------------- | :------------------------------------------------------------------------------------------------ |
| **json**        | 如果输出格式为 JSON，则返回任务输出的 JSON 字符串表示。                                           |
| **to_dict**     | 将 JSON 和 Pydantic 输出转换为字典。                                                             |
| \***\*str\*\*** | 返回任务输出的字符串表示，优先考虑 Pydantic，然后是 JSON，最后是原始输出。                    |

### 访问任务输出

一旦任务执行完成，可以通过 `Task` 对象的 `output` 属性访问其输出。`TaskOutput` 类提供了多种与此输出交互和展示的方式。

#### 示例

```python
# Example task
task = Task(
    description='Find and summarize the latest AI news',
    expected_output='A bullet list summary of the top 5 most important AI news',
    agent=research_agent,
    tools=[search_tool]
)

# Execute the crew
crew = Crew(
    agents=[research_agent],
    tasks=[task],
    verbose=2
)

result = crew.kickoff()

# Accessing the task output
task_output = task.output

print(f"Task Description: {task_output.description}")
print(f"Task Summary: {task_output.summary}")
print(f"Raw Output: {task_output.raw}")
if task_output.json_dict:
    print(f"JSON Output: {json.dumps(task_output.json_dict, indent=2)}")
if task_output.pydantic:
    print(f"Pydantic Output: {task_output.pydantic}")
```

## 将工具与任务集成

利用 [crewAI Toolkit](https://github.com/joaomdmoura/crewai-tools) 和 [LangChain Tools](https://python.langchain.com/docs/integrations/tools) 中的工具，以增强任务性能和代理交互。

```
# 示例代码
def example_function():
    print("Hello, World!")
```

## 使用工具创建任务

```python
import os
os.environ["OPENAI_API_KEY"] = "Your Key"
os.environ["SERPER_API_KEY"] = "Your Key" # serper.dev API key

from crewai import Agent, Task, Crew
from crewai_tools import SerperDevTool

research_agent = Agent(
  role='Researcher',
  goal='Find and summarize the latest AI news',
  backstory="""You're a researcher at a large company.
  You're responsible for analyzing data and providing insights
  to the business.""",
  verbose=True
)

search_tool = SerperDevTool()

task = Task(
  description='Find and summarize the latest AI news',
  expected_output='A bullet list summary of the top 5 most important AI news',
  agent=research_agent,
  tools=[search_tool]
)

crew = Crew(
    agents=[research_agent],
    tasks=[task],
    verbose=2
)

result = crew.kickoff()
print(result)
```

这演示了如何使用特定工具创建任务，以覆盖代理的默认设置，从而实现量身定制的任务执行。

## 引用其他任务

在 crewAI 中，一个任务的输出会自动传递到下一个任务，但您可以具体定义哪些任务的输出（包括多个）应作为另一个任务的上下文。

当您有一个任务依赖于另一个任务的输出，而该任务并不是立即执行时，这非常有用。这是通过任务的 `context` 属性来实现的：

```python
# ...

research_ai_task = Task(
    description='Find and summarize the latest AI news',
    expected_output='A bullet list summary of the top 5 most important AI news',
    async_execution=True,
    agent=research_agent,
    tools=[search_tool]
)

research_ops_task = Task(
    description='Find and summarize the latest AI Ops news',
    expected_output='A bullet list summary of the top 5 most important AI Ops news',
    async_execution=True,
    agent=research_agent,
    tools=[search_tool]
)

write_blog_task = Task(
    description="Write a full blog post about the importance of AI and its latest news",
    expected_output='Full blog post that is 4 paragraphs long',
    agent=writer_agent,
    context=[research_ai_task, research_ops_task]
)

#...
```

## 异步执行

您可以定义一个异步执行的任务。这意味着团队不会等待该任务完成就继续进行下一个任务。这对于需要较长时间才能完成的任务，或者对下一个任务的执行不至关重要的任务非常有用。

然后，您可以使用 `context` 属性在将来的任务中定义该任务应等待异步任务的输出完成。

```python
#...

list_ideas = Task(
    description="List of 5 interesting ideas to explore for an article about AI.",
    expected_output="Bullet point list of 5 ideas for an article.",
    agent=researcher,
    async_execution=True # Will be executed asynchronously
)

list_important_history = Task(
    description="Research the history of AI and give me the 5 most important events.",
    expected_output="Bullet point list of 5 important events.",
    agent=researcher,
    async_execution=True # Will be executed asynchronously
)

write_article = Task(
    description="Write an article about AI, its history, and interesting ideas.",
    expected_output="A 4 paragraph article about AI.",
    agent=writer,
    context=[list_ideas, list_important_history] # Will wait for the output of the two tasks to be completed
)

#...
```

## 回调机制

回调函数在任务完成后执行，允许根据任务的结果触发相应的操作或通知。

```python
# ...

def callback_function(output: TaskOutput):
    # Do something after the task is completed
    # Example: Send an email to the manager
    print(f"""
        Task completed!
        Task: {output.description}
        Output: {output.raw_output}
    """)

research_task = Task(
    description='Find and summarize the latest AI news',
    expected_output='A bullet list summary of the top 5 most important AI news',
    agent=research_agent,
    tools=[search_tool],
    callback=callback_function
)

#...
```

## 访问特定任务输出

一旦团队完成运行，您可以通过使用任务对象的 `output` 属性来访问特定任务的输出：

```python
# ...
task1 = Task(
    description='Find and summarize the latest AI news',
    expected_output='A bullet list summary of the top 5 most important AI news',
    agent=research_agent,
    tools=[search_tool]
)

#...

crew = Crew(
    agents=[research_agent],
    tasks=[task1, task2, task3],
    verbose=2
)

result = crew.kickoff()

# Returns a TaskOutput object with the description and results of the task
print(f"""
    Task completed!
    Task: {task1.output.description}
    Output: {task1.output.raw_output}
""")
```

## 工具覆盖机制

在任务中指定工具可以动态调整代理的能力，突显了 CrewAI 的灵活性。

## 错误处理和验证机制

在创建和执行任务时，有一些验证机制可以确保任务属性的稳健性和可靠性。这些机制包括但不限于：

- 确保每个任务仅设置一个输出类型，以保持明确的输出期望。
- 防止手动分配 `id` 属性，以维护唯一标识符系统的完整性。

这些验证有助于维护 crewAI 框架内任务执行的一致性和可靠性。

## 保存文件时创建目录

您现在可以指定任务在将输出保存到文件时是否应创建目录。这对于组织输出和确保文件路径正确结构化特别有用。

```python
# ...

save_output_task = Task(
    description='Save the summarized AI news to a file',
    expected_output='File saved successfully',
    agent=research_agent,
    tools=[file_save_tool],
    output_file='outputs/ai_news_summary.txt',
    create_directory=True
)

#...
```

## Conclusion

任务是 crewAI 中代理人行动的驱动力。通过恰当地定义任务及其结果，您为您的 AI 代理人独立工作或作为协作单位奠定了基础。为任务配备适当的工具，理解执行过程，以及遵循严格的验证实践，对于最大化 CrewAI 的潜力至关重要，确保代理人有效地准备好他们的任务，并且任务按预期执行。