---
title: crewAI 团队
description: 理解和利用 crewAI 框架中的团队，具有全面的属性和功能。
---

## 什么是团队？

在 crewAI 中，团队代表一个协作的代理组，他们共同努力以完成一系列任务。每个团队定义了任务执行、代理协作和整体工作流程的策略。

```
# 示例代码
def example_function():
    print("Hello, World!")
```

## 船员属性

| 属性                                   | 参数                    | 描述                                                                                                                                                                                                                                                   |
| :------------------------------------- | :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **任务**                               | `tasks`                 | 分配给船员的任务列表。                                                                                                                                                                                                                                 |
| **代理**                               | `agents`                | 属于船员的代理列表。                                                                                                                                                                                                                                   |
| **过程** _(可选)_                     | `process`               | 船员遵循的过程流程（例如，顺序、层级）。                                                                                                                                                                                                               |
| **详细程度** _(可选)_                 | `verbose`               | 执行期间记录的详细程度。                                                                                                                                                                                                                               |
| **管理者 LLM** _(可选)_               | `manager_llm`           | 管理代理在层级过程中使用的语言模型。**在使用层级过程时必需。**                                                                                                                                                                                       |
| **功能调用 LLM** _(可选)_             | `function_calling_llm`  | 如果传递，船员将使用此 LLM 为船员中的所有代理进行工具的功能调用。每个代理可以拥有自己的 LLM，这将覆盖船员的功能调用 LLM。                                                                                                                        |
| **配置** _(可选)_                     | `config`                | 船员的可选配置设置，格式为 `Json` 或 `Dict[str, Any]`。                                                                                                                                                                                              |
| **最大请求速率** _(可选)_             | `max_rpm`               | 船员在执行期间遵循的每分钟最大请求数。                                                                                                                                                                                                                 |
| **语言** _(可选)_                     | `language`              | 船员使用的语言，默认为英语。                                                                                                                                                                                                                           |
| **语言文件** _(可选)_                 | `language_file`         | 用于船员的语言文件路径。                                                                                                                                                                                                                               |
| **记忆** _(可选)_                     | `memory`                | 用于存储执行记忆（短期、长期、实体记忆）。                                                                                                                                                                                                               |
| **缓存** _(可选)_                     | `cache`                 | 指定是否使用缓存来存储工具执行的结果。                                                                                                                                                                                                               |
| **嵌入器** _(可选)_                   | `embedder`              | 船员使用的嵌入器的配置。当前主要用于记忆。                                                                                                                                                                                                               |
| **完整输出** _(可选)_                 | `full_output`           | 船员是否应返回包含所有任务输出的完整输出，或仅返回最终输出。                                                                                                                                                                                         |
| **步骤回调** _(可选)_                 | `step_callback`         | 每个代理每一步之后调用的函数。可用于记录代理的操作或执行其他操作；它不会覆盖代理特定的 `step_callback`。                                                                                                                                         |
| **任务回调** _(可选)_                 | `task_callback`         | 每个任务完成后调用的函数。用于监控或任务执行后的额外操作。                                                                                                                                                                                           |
| **共享船员** _(可选)_                 | `share_crew`            | 是否希望与 crewAI 团队共享完整的船员信息和执行，以改善库，并允许我们训练模型。                                                                                                                                                                       |
| **输出日志文件** _(可选)_             | `output_log_file`       | 是否希望拥有一个包含完整船员输出和执行的文件。您可以使用 True 设置它，它将默认为您当前所在的文件夹，并命名为 logs.txt，或者传递一个字符串，包含文件的完整路径和名称。                                                            |
| **管理者代理** _(可选)_               | `manager_agent`         | `manager` 设置将用作管理者的自定义代理。                                                                                                                                                                                                               |
| **管理者回调** _(可选)_               | `manager_callbacks`     | `manager_callbacks` 接受一个回调处理程序的列表，当使用层级过程时由管理代理执行。                                                                                                                                                                   |
| **提示文件** _(可选)_                 | `prompt_file`           | 用于船员的提示 JSON 文件路径。                                                                                                                                                                                                                         |
| **规划** *(可选)*                     | `planning`              | 为船员添加规划能力。在每次船员迭代之前激活时，所有船员数据将发送到 AgentPlanner，AgentPlanner 将规划任务，并将此计划添加到每个任务描述中。                                                                                                   |
| **规划 LLM** *(可选)*                 | `planning_llm`          | 在规划过程中，AgentPlanner 使用的语言模型。                                                                                                                                                                                                         |

!!! note "船员最大请求速率"
`max_rpm` 属性设置船员每分钟可以执行的最大请求数，以避免速率限制，如果您设置它，将覆盖单个代理的 `max_rpm` 设置。

## 创建团队

在组建团队时，您需要将具有互补角色和工具的代理组合在一起，分配任务，并选择一个决定它们执行顺序和互动的流程。

```
# 示例代码
def create_team(agents):
    # 逻辑代码
    pass
```

### 示例：组建团队

```python
from crewai import Crew, Agent, Task, Process
from langchain_community.tools import DuckDuckGoSearchRun
from crewai_tools import tool

@tool('DuckDuckGoSearch')
def search(search_query: str):
    """Search the web for information on a given topic"""
    return DuckDuckGoSearchRun().run(search_query)

# 定义具有特定角色和工具的代理
researcher = Agent(
    role='高级研究分析师',
    goal='发现创新的人工智能技术',
    backstory="""你是一家大公司的高级研究分析师。
        你负责分析数据并为业务提供洞察。
        你目前正在进行一个项目，分析人工智能领域的趋势和创新。""",
    tools=[search]
)

writer = Agent(
    role='内容撰写者',
    goal='撰写关于人工智能发现的引人入胜的文章',
    backstory="""你是一家大公司的高级撰写者。
        你负责为业务创建内容。
        你目前正在进行一个项目，为下次会议撰写关于人工智能领域趋势和创新的内容。""",
    verbose=True
)

# 为代理创建任务
research_task = Task(
    description='识别突破性的人工智能技术',
    agent=researcher,
    expected_output='前5个最重要的人工智能新闻的要点总结'
)
write_article_task = Task(
    description='撰写关于最新人工智能技术的文章',
    agent=writer,
    expected_output='关于最新人工智能技术的3段博客文章'
)

# 通过顺序流程组建团队
my_crew = Crew(
    agents=[researcher, writer],
    tasks=[research_task, write_article_task],
    process=Process.sequential,
    full_output=True,
    verbose=True,
)
```

## Crew Output

!!! note "理解 Crew 输出"
在 crewAI 框架中，团队的输出封装在 `CrewOutput` 类中。
该类提供了一种结构化的方式来访问团队执行的结果，包括原始字符串、JSON 和 Pydantic 模型等多种格式。
`CrewOutput` 包含最终任务输出、令牌使用情况和各个任务输出的结果。

```
# 代码块内容保持不变
```

### Crew Output Attributes

| 属性             | 参数           | 类型                       | 描述                                                                                               |
| :--------------- | :------------- | :------------------------- | :-------------------------------------------------------------------------------------------------- |
| **原始**         | `raw`          | `str`                      | 船员的原始输出。这是输出的默认格式。                                                               |
| **Pydantic**     | `pydantic`     | `Optional[BaseModel]`      | 一个 Pydantic 模型对象，表示船员的结构化输出。                                                      |
| **JSON 字典**    | `json_dict`    | `Optional[Dict[str, Any]]` | 一个表示船员 JSON 输出的字典。                                                                      |
| **任务输出**     | `tasks_output` | `List[TaskOutput]`         | 一个 `TaskOutput` 对象的列表，每个对象表示船员中一个任务的输出。                                   |
| **令牌使用**     | `token_usage`  | `Dict[str, Any]`           | 令牌使用的摘要，提供有关语言模型在执行过程中的性能的见解。                                         |

### 船员输出方法和属性

| 方法/属性        | 描述                                                                                              |
| :-------------- | :------------------------------------------------------------------------------------------------- |
| **json**        | 如果输出格式为 JSON，则返回船员输出的 JSON 字符串表示。                                            |
| **to_dict**     | 将 JSON 和 Pydantic 输出转换为字典。                                                              |
| \***\*str\*\*** | 返回船员输出的字符串表示，优先考虑 Pydantic，然后是 JSON，最后是原始格式。                         |

### 访问团队输出

一旦团队执行完毕，可以通过 `Crew` 对象的 `output` 属性访问其输出。`CrewOutput` 类提供了多种方式与该输出进行交互和展示。

#### 示例

```python
# Example crew execution
crew = Crew(
    agents=[research_agent, writer_agent],
    tasks=[research_task, write_article_task],
    verbose=2
)

crew_output = crew.kickoff()

# Accessing the crew output
print(f"Raw Output: {crew_output.raw}")
if crew_output.json_dict:
    print(f"JSON Output: {json.dumps(crew_output.json_dict, indent=2)}")
if crew_output.pydantic:
    print(f"Pydantic Output: {crew_output.pydantic}")
print(f"Tasks Output: {crew_output.tasks_output}")
print(f"Token Usage: {crew_output.token_usage}")
```

## 内存利用率

工作人员可以利用内存（短期内存、长期内存和实体内存）来增强他们的执行和学习能力。此功能允许工作人员存储和回忆执行记忆，从而帮助决策和任务执行策略。

```
# Code block example
def example_function():
    pass
```

## Cache Utilization

Caching can be used to store the results of tool executions, making the process more efficient by reducing the need to re-execute the same tasks.

## 团队使用指标

在团队执行后，您可以访问 `usage_metrics` 属性，以查看团队执行的所有任务的语言模型（LLM）使用指标。这提供了对运营效率和改进领域的洞察。

```python
# Access the crew's usage metrics
crew = Crew(agents=[agent1, agent2], tasks=[task1, task2])
crew.kickoff()
print(crew.usage_metrics)
```

## Crew Execution Process

- **顺序流程**: 任务一个接一个地执行，允许工作线性流动。
- **层级流程**: 一名经理代理协调团队，分配任务并在继续之前验证结果。 **注意**: 此流程需要一个 `manager_llm` 或 `manager_agent`，并且对验证流程至关重要。

### 启动团队

一旦您的团队组建完成，使用 `kickoff()` 方法启动工作流程。这将根据定义的流程开始执行过程。

```python
# Start the crew's task execution
result = my_crew.kickoff()
print(result)
```

### 不同的团队启动方式

一旦您的团队组建完成，使用适当的启动方法来启动工作流程。CrewAI 提供了几种方法以更好地控制启动过程：`kickoff()`, `kickoff_for_each()`, `kickoff_async()` 和 `kickoff_for_each_async()`。

`kickoff()`：根据定义的流程启动执行过程。  
`kickoff_for_each()`：为每个代理单独执行任务。  
`kickoff_async()`：异步启动工作流程。  
`kickoff_for_each_async()`：以异步方式为每个代理单独执行任务。  

```python
# Start the crew's task execution
result = my_crew.kickoff()
print(result)

# Example of using kickoff_for_each
inputs_array = [{'topic': 'AI in healthcare'}, {'topic': 'AI in finance'}]
results = my_crew.kickoff_for_each(inputs=inputs_array)
for result in results:
    print(result)

# Example of using kickoff_async
inputs = {'topic': 'AI in healthcare'}
async_result = my_crew.kickoff_async(inputs=inputs)
print(async_result)

# Example of using kickoff_for_each_async
inputs_array = [{'topic': 'AI in healthcare'}, {'topic': 'AI in finance'}]
async_results = my_crew.kickoff_for_each_async(inputs=inputs_array)
for async_result in async_results:
    print(async_result)
```

这些方法提供了在团队内管理和执行任务的灵活性，允许根据您的需求进行同步和异步工作流程。

### 从特定任务重放：
您现在可以使用我们的命令行界面命令 replay 从特定任务重放。

CrewAI 中的重放功能允许您使用命令行界面 (CLI) 从特定任务重放。通过运行命令 `crewai replay -t <task_id>`，您可以指定重放过程中的 `task_id`。

Kickoffs 现在将最新的 kickoffs 返回的任务输出保存在本地，以便您能够进行重放。

### 从特定任务使用CLI进行重放
要使用重放功能，请按照以下步骤操作：

1. 打开终端或命令提示符。
2. 导航到您的CrewAI项目所在的目录。
3. 运行以下命令：

要查看最新的启动任务_id，请使用：

```shell
crewai log-tasks-outputs
```

```shell
crewai replay -t <task_id>
```

这些命令允许您从最新的启动任务进行重放，同时保留之前执行任务的上下文。