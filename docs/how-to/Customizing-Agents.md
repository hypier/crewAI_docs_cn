---
title: 在CrewAI中定制代理
description: 一份全面的指南，专门针对在CrewAI框架内为特定角色、任务和高级定制进行代理定制。
---

## 可定制属性
打造高效的 CrewAI 团队取决于能够动态调整您的 AI 代理，以满足任何项目的独特需求。本节涵盖您可以自定义的基础属性。  
```  
# Example code block  
print("Hello, World!")  
```

### 自定义的关键属性
- **角色**: 指定代理在团队中的工作，例如 '分析师' 或 '客户服务代表'。
- **目标**: 定义代理旨在实现的目标，与其角色和团队的总体目标一致。
- **背景故事**: 为代理的人物增添深度，丰富其动机和在团队中的互动。
- **工具** *(可选)*: 表示代理用于执行任务的能力或方法，从简单功能到复杂集成。
- **缓存** *(可选)*: 决定代理是否应使用工具使用的缓存。
- **最大请求每分钟**: 设置每分钟的最大请求数 (`max_rpm`)。此属性是可选的，可以设置为 `None` 以表示没有限制，允许在需要时对外部服务进行无限查询。
- **详细信息** *(可选)*: 启用代理操作的详细日志记录，便于调试和优化。具体而言，它提供代理执行过程的洞察，帮助优化性能。
- **允许委托** *(可选)*: `allow_delegation` 控制代理是否被允许将任务委托给其他代理。
- **最大迭代次数** *(可选)*: `max_iter` 属性允许用户定义代理在单个任务中可以执行的最大迭代次数，以防止无限循环或过长的执行时间。默认值设置为 25，提供了全面性与效率之间的平衡。一旦代理接近此数字，它将尽力给出一个好的答案。
- **最大执行时间** *(可选)*: `max_execution_time` 设置代理完成任务的最大执行时间。
- **系统模板** *(可选)*: `system_template` 定义代理的系统格式。
- **提示模板** *(可选)*: `prompt_template` 定义代理的提示格式。
- **响应模板** *(可选)*: `response_template` 定义代理的响应格式。

## 高级自定义选项
除了基本属性，CrewAI 还允许更深入的自定义，以显著增强代理的行为和能力。

### 语言模型定制
代理可以使用特定的语言模型 (`llm`) 和函数调用语言模型 (`function_calling_llm`) 进行定制，从而提供对其处理和决策能力的高级控制。需要注意的是，设置 `function_calling_llm` 允许覆盖默认的 crew 函数调用语言模型，从而提供更大的定制化程度。

## 性能和调试设置
调整代理的性能并监控其操作对于高效的任务执行至关重要。

### 详细模式和RPM限制
- **详细模式**：启用代理操作的详细日志记录，有助于调试和优化。具体来说，它提供关于代理执行过程的深入见解，有助于性能优化。
- **RPM限制**：设置每分钟最大请求数（`max_rpm`）。此属性是可选的，可以设置为`None`以表示无限制，从而在需要时允许对外部服务进行无限查询。

```
# 代码示例
def example_function():
    pass
```

### 任务执行的最大迭代次数
`max_iter` 属性允许用户定义代理在单个任务中可以执行的最大迭代次数，以防止无限循环或过长的执行时间。默认值设置为 25，提供了全面性和效率之间的平衡。一旦代理接近这个数字，它将努力提供一个好的答案。

## 自定义代理和工具
代理通过在初始化时定义其属性和工具来进行定制。工具对于代理的功能至关重要，使其能够执行专业任务。`tools` 属性应该是代理可以使用的工具数组，默认初始化为空列表。可以在代理初始化后添加或修改工具，以适应新需求。

```shell
pip install 'crewai[tools]'
```

### 示例：为代理分配工具
```python
import os
from crewai import Agent
from crewai_tools import SerperDevTool

# 设置工具初始化的 API 密钥
os.environ["OPENAI_API_KEY"] = "Your Key"
os.environ["SERPER_API_KEY"] = "Your Key"

# 初始化搜索工具
search_tool = SerperDevTool()

# 使用高级选项初始化代理
agent = Agent(
  role='Research Analyst',
  goal='提供最新的市场分析',
  backstory='一位对市场趋势有敏锐洞察力的专家分析师。',
  tools=[search_tool],
  memory=True, # 启用记忆
  verbose=True,
  max_rpm=None, # 每分钟请求没有限制
  max_iter=25, # 最大迭代次数的默认值
  allow_delegation=False
)
```

## 委托与自主性
控制代理人委托任务或提问的能力对于在CrewAI框架内调整其自主性和协作动态至关重要。默认情况下，`allow_delegation`属性设置为`True`，允许代理人根据需要寻求帮助或委托任务。这种默认行为促进了CrewAI生态系统内的协作问题解决和效率。如果需要，可以禁用委托以适应特定的操作要求。

```
# 代码块示例
def example_function():
    print("这段代码不会被翻译。")
```

### 示例：为代理禁用委托
```python
agent = Agent(
  role='Content Writer',
  goal='Write engaging content on market trends',
  backstory='A seasoned writer with expertise in market analysis.',
  allow_delegation=False # Disabling delegation
)
```

## 结论
通过设定角色、目标、背景故事和工具，定制CrewAI中的代理，并利用语言模型定制、记忆、性能设置和委派偏好等高级选项，建立了一个灵活高效的AI团队，准备应对复杂挑战。