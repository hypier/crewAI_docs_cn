---
title: crewAI 记忆系统
description: 在 crewAI 框架中利用记忆系统提升智能体能力。
---

## crewAI中的内存系统介绍
!!! note "增强代理智能"
    crewAI框架引入了一种复杂的内存系统，旨在显著增强AI代理的能力。该系统包括短期记忆、长期记忆、实体记忆和上下文记忆，每种记忆都在帮助代理记住、推理和从过去的互动中学习方面发挥独特作用。

## 内存系统组件

| 组件                  | 描述                                                       |
| :------------------- | :--------------------------------------------------------- |
| **短期记忆**        | 临时存储最近的交互和结果，使代理能够在当前执行过程中回忆和利用与其当前上下文相关的信息。 |
| **长期记忆**        | 保留过去执行中的宝贵见解和学习，使代理能够随着时间的推移建立和完善其知识。因此，代理可以记住在多次执行中做对和做错的事情。 |
| **实体记忆**        | 捕捉和组织在任务中遇到的实体（人、地点、概念）信息，促进更深入的理解和关系映射。 |
| **上下文记忆**      | 通过结合 `ShortTermMemory`、`LongTermMemory` 和 `EntityMemory` 来维护交互的上下文，有助于在一系列任务或对话中保持代理响应的一致性和相关性。 |

## 如何记忆系统赋能智能体

1. **上下文意识**：通过短期和上下文记忆，智能体能够在对话或任务序列中保持上下文，从而产生更连贯和相关的响应。

2. **经验积累**：长期记忆使智能体能够积累经验，从过去的行动中学习，以改善未来的决策和问题解决能力。

3. **实体理解**：通过维护实体记忆，智能体可以识别和记住关键实体，增强其处理和互动复杂信息的能力。

## 在您的团队中实现内存

在配置团队时，您可以启用并自定义每个内存组件，以适应团队的目标和将要执行的任务性质。  
默认情况下，内存系统是禁用的，您可以通过在团队配置中设置 `memory=True` 来确保其处于活动状态。内存默认使用 OpenAI Embeddings，但您可以通过将 `embedder` 设置为其他模型来更改它。

'embedder' 仅适用于 **短期记忆**，该记忆使用 Chroma 进行 RAG，使用 EmbedChain 包。  
**长期记忆** 使用 SQLLite3 存储任务结果。目前，没有办法覆盖这些存储实现。  
数据存储文件保存在使用 appdirs 包找到的特定平台位置中，项目名称可以通过 **CREWAI_STORAGE_DIR** 环境变量进行覆盖。

### 示例：为团队配置内存

```python
from crewai import Crew, Agent, Task, Process

# Assemble your crew with memory capabilities
my_crew = Crew(
    agents=[...],
    tasks=[...],
    process=Process.sequential,
    memory=True,
    verbose=True
)
```

## Other Embedding Providers

### 使用 OpenAI 嵌入（已为默认设置）
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
				"provider": "openai",
				"config":{
						"model": 'text-embedding-3-small'
				}
		}
)
```

### 使用 Google AI 嵌入
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
			"provider": "google",
			"config":{
				"model": 'models/embedding-001',
				"task_type": "retrieval_document",
				"title": "Embeddings for Embedchain"
			}
		}
)
```

### 使用 Azure OpenAI 嵌入
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
			"provider": "azure_openai",
			"config":{
				"model": 'text-embedding-ada-002',
				"deployment_name": "you_embedding_model_deployment_name"
			}
		}
)
```

### 使用 GPT4ALL 嵌入
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
			"provider": "gpt4all"
		}
)
```

### 使用 Vertex AI 嵌入
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
			"provider": "vertexai",
			"config":{
				"model": 'textembedding-gecko'
			}
		}
)
```

### 使用 Cohere 嵌入
```python
from crewai import Crew, Agent, Task, Process

my_crew = Crew(
		agents=[...],
		tasks=[...],
		process=Process.sequential,
		memory=True,
		verbose=True,
		embedder={
			"provider": "cohere",
			"config":{
				"model": "embed-english-v3.0"
    		"vector_dimension": 1024
			}
		}
)
```

### 重置记忆
```sh
crewai reset_memories [OPTIONS]
```

#### 重置记忆选项
- **`-l, --long`**
  - **描述:** 重置长期记忆。
  - **类型:** 标志（布尔值）
  - **默认:** False

- **`-s, --short`**
  - **描述:** 重置短期记忆。
  - **类型:** 标志（布尔值）
  - **默认:** False

- **`-e, --entities`**
  - **描述:** 重置实体记忆。
  - **类型:** 标志（布尔值）
  - **默认:** False

- **`-k, --kickoff-outputs`**
  - **描述:** 重置最新启动任务输出。
  - **类型:** 标志（布尔值）
  - **默认:** False

- **`-a, --all`**
  - **描述:** 重置所有记忆。
  - **类型:** 标志（布尔值）
  - **默认:** False

## 使用 crewAI 记忆系统的好处
- **自适应学习：** 团队随着时间的推移变得更加高效，能够适应新信息并完善任务处理方式。
- **增强个性化：** 记忆使代理能够记住用户偏好和历史互动，从而提供个性化体验。
- **改善问题解决：** 访问丰富的记忆库帮助代理做出更明智的决策，借鉴过去的学习和上下文洞察。

## 开始使用
将 crewAI 的记忆系统集成到您的项目中非常简单。通过利用提供的记忆组件和配置，您可以快速赋予您的代理记住、推理和从交互中学习的能力，从而解锁新的智能和能力水平。

```
# 示例代码
def example_function():
    print("Hello, World!")
```