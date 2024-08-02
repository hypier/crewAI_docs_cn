---
title: 使用 Langtrace 监控 CrewAI Agent
description: 如何使用 Langtrace 这一外部可观测性工具监控 CrewAI Agents 的成本、延迟和性能。
---

# Langtrace 概述

Langtrace 是一个开源的外部工具，帮助您为大型语言模型（LLMs）、LLM 框架和向量数据库设置可观察性和评估。虽然 Langtrace 并没有直接集成到 CrewAI 中，但可以与 CrewAI 一起使用，以深入了解您的 CrewAI 代理的成本、延迟和性能。此集成允许您记录超参数、监控性能回归，并建立一个持续改进代理的流程。

## 设置说明

1. 通过访问 [https://langtrace.ai/signup](https://langtrace.ai/signup) 注册 [Langtrace](https://langtrace.ai/)。
2. 创建一个项目并生成一个 API 密钥。
3. 使用以下命令在您的 CrewAI 项目中安装 Langtrace：

```bash
# Install the SDK
pip install langtrace-python-sdk
```

## 使用 Langtrace 与 CrewAI

要将 Langtrace 集成到您的 CrewAI 项目中，请按照以下步骤操作：

1. 在脚本的开头导入并初始化 Langtrace，在任何 CrewAI 导入之前：

```python
from langtrace_python_sdk import langtrace
langtrace.init(api_key='<LANGTRACE_API_KEY>')

# 现在导入 CrewAI 模块
from crewai import Agent, Task, Crew
```

2. 像往常一样创建您的 CrewAI 代理和任务。

3. 使用 Langtrace 的跟踪功能来监控您的 CrewAI 操作。例如：

```python
with langtrace.trace("CrewAI Task Execution"):
    result = crew.kickoff()
```

### 特性及其在 CrewAI 中的应用

1. **LLM 令牌和成本跟踪**
   - 监控每个 CrewAI 代理交互的令牌使用情况及相关成本。
   - 示例：
     ```python
     with langtrace.trace("Agent Interaction"):
         agent_response = agent.execute(task)
     ```

2. **执行步骤的追踪图**
   - 可视化 CrewAI 任务的执行流程，包括延迟和日志。
   - 有助于识别代理工作流中的瓶颈。

3. **手动标注的数据集策划**
   - 从 CrewAI 任务输出中创建数据集，以便于未来的训练或评估。
   - 示例：
     ```python
     langtrace.log_dataset_item(task_input, agent_output, {"task_type": "research"})
     ```

4. **提示版本控制和管理**
   - 跟踪在 CrewAI 代理中使用的不同版本的提示。
   - 有助于 A/B 测试和优化代理性能。

5. **提示游乐场与模型比较**
   - 在部署之前测试和比较不同的提示和模型，以用于 CrewAI 代理。

6. **测试和评估**
   - 为 CrewAI 代理和任务设置自动化测试。
   - 示例：
     ```python
     langtrace.evaluate(agent_output, expected_output, "accuracy")
     ```

## 监控新功能 CrewAI

CrewAI 引入了几个新功能，可以通过 Langtrace 进行监控：

1. **代码执行**：监控代理执行的代码的性能和输出。
   ```python
   with langtrace.trace("Agent Code Execution"):
       code_output = agent.execute_code(code_snippet)
   ```

2. **第三方代理集成**：跟踪与 LlamaIndex、LangChain 和 Autogen 代理的交互。