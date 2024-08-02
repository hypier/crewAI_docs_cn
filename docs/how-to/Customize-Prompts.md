---
title: 初步支持在CrewAI中使用自定义提示
description: 通过允许用户在CrewAI中使用自定义提示来增强定制化和国际化。

---

# 初始支持自定义提示在CrewAI中

CrewAI现在支持自定义提示的功能，使得广泛的定制和国际化成为可能。此功能允许用户根据特定需求调整代理的内部工作，包括对多种语言的支持。

```
# Example code block
def hello_world():
    print("Hello, world!")
```

## Internationalization and Customization Support

### 自定义提示与 `prompt_file`

`prompt_file` 属性便于完全自定义代理提示，增强了 CrewAI 的全球可用性。用户可以指定他们的提示模板，确保代理以符合特定项目要求或语言偏好的方式进行沟通。

#### 自定义提示文件示例

自定义提示可以在 JSON 文件中定义，类似于提供的 [这里](https://github.com/joaomdmoura/crewAI/blob/main/src/crewai/translations/en.json) 的示例。

### Supported Languages

CrewAI's custom prompts support internationalization, allowing prompts to be written in different languages. This is especially useful for global teams or projects that require multilingual support.

## 如何使用 `prompt_file` 属性

要使用 `prompt_file` 属性，请将其包含在您的团队定义中。下面是一个示例，演示如何使用自定义提示设置代理和任务。

```
{
  "agents": [
    {
      "name": "example_agent",
      "prompt_file": "path/to/prompt/file"
    }
  ],
  "tasks": [
    {
      "name": "example_task",
      "description": "This is an example task."
    }
  ]
}
```

### 示例

```python
import os
from crewai import Agent, Task, Crew

# 定义你的代理
researcher = Agent(
    role="Researcher",
    goal="对关于人工智能和人工智能代理的内容进行最佳研究和分析",
    backstory="你是一位专家研究员，专注于技术、软件工程、人工智能和初创企业。你作为自由职业者工作，现在正在为一个新客户进行研究和分析。",
    allow_delegation=False,
)

writer = Agent(
    role="Senior Writer",
    goal="撰写关于人工智能和人工智能代理的最佳内容。",
    backstory="你是一位资深作家，专注于技术、软件工程、人工智能和初创企业。你作为自由职业者工作，现在正在为一个新客户撰写内容。",
    allow_delegation=False,
)

# 定义你的任务
tasks = [
    Task(
        description="Say Hi",
        expected_output="The word: Hi",
        agent=researcher,
    )
]

# 使用自定义提示实例化你的团队
crew = Crew(
    agents=[researcher],
    tasks=tasks,
    prompt_file="prompt.json",  # 自定义提示文件的路径
)

# 让你的团队开始工作！
crew.kickoff()
```

## Advanced Customization Features

### `language` 属性

除了 `prompt_file`，`language` 属性可以用来指定代理的提示语言。这确保了提示以所需语言生成，进一步增强了 CrewAI 的国际化能力。

```
# Example usage
agent = Agent(language='zh')
```

### 创建自定义提示文件

自定义提示文件应采用 JSON 格式结构，并包含所有必要的提示模板。以下是一个简化的提示 JSON 文件示例：

```json
{
    "system": "You are a system template.",
    "prompt": "Here is your prompt template.",
    "response": "Here is your response template."
}
```

### 自定义提示的好处

- **增强灵活性**：根据特定项目需求定制代理通信。
- **改善可用性**：支持多种语言，使其适合全球项目。
- **一致性**：确保不同代理和任务之间的提示结构统一。

通过引入这些更新，CrewAI 为用户提供了完全自定义和国际化代理提示的能力，使平台更加多功能和用户友好。

```
# Example Code Block
def example_function():
    print("This is an example.")
```