---
title: 将 CrewAI 连接到 LLMs
description: 关于将 CrewAI 与各种大型语言模型 (LLMs) 集成的全面指南，包括详细的类属性、方法和配置选项。
---

## 将 CrewAI 连接到 LLMs

!!! note "默认 LLM"
    默认情况下，CrewAI 使用 OpenAI 的 GPT-4 模型（具体来说，是由 OPENAI_MODEL_NAME 环境变量指定的模型，默认为 "gpt-4o"）进行语言处理。您可以根据本指南配置您的代理使用不同的模型或 API。

CrewAI 提供了连接各种 LLM 的灵活性，包括通过 [Ollama](https://ollama.ai) 的本地模型以及 Azure 等不同的 API。它与所有 [LangChain LLM](https://python.langchain.com/docs/integrations/llms/) 组件兼容，使多样化的集成成为可能，以实现量身定制的 AI 解决方案。

## CrewAI Agent 概述

`Agent` 类是实现 CrewAI 中 AI 解决方案的基石。以下是 Agent 类属性和方法的全面概述：

- **属性**：
    - `role`：定义代理在解决方案中的角色。
    - `goal`：指定代理的目标。
    - `backstory`：为代理提供背景故事。
    - `cache` *可选*：确定代理是否应使用缓存以便于工具使用。默认值为 `True`。
    - `max_rpm` *可选*：代理执行应遵循的每分钟最大请求数。可选。
    - `verbose` *可选*：启用代理执行的详细日志记录。默认值为 `False`。
    - `allow_delegation` *可选*：允许代理将任务委派给其他代理，默认值为 `True`。
    - `tools`：指定可供代理执行任务的工具。可选。
    - `max_iter` *可选*：代理执行任务的最大迭代次数，默认值为 25。
    - `max_execution_time` *可选*：代理执行任务的最大执行时间。可选。
    - `step_callback` *可选*：提供在每一步后执行的回调函数。可选。
    - `llm` *可选*：指示代理使用的大型语言模型。默认情况下，它使用环境变量 "OPENAI_MODEL_NAME" 中定义的 GPT-4 模型。
    - `function_calling_llm` *可选*：将 ReAct CrewAI 代理转换为函数调用代理。
    - `callbacks` *可选*：在代理执行过程中触发的 LangChain 库回调函数列表。
    - `system_template` *可选*：定义代理系统格式的可选字符串。
    - `prompt_template` *可选*：定义代理提示格式的可选字符串。
    - `response_template` *可选*：定义代理响应格式的可选字符串。

```python
# Required
os.environ["OPENAI_MODEL_NAME"]="gpt-4-0125-preview"

# Agent will automatically use the model defined in the environment variable
example_agent = Agent(
  role='Local Expert',
  goal='Provide insights about the city',
  backstory="A knowledgeable local guide.",
  verbose=True
)
```

## Ollama 集成
Ollama 是本地 LLM 集成的首选，提供定制化和隐私保护的优势。要将 Ollama 与 CrewAI 集成，请设置如下所示的适当环境变量。

```
export OLLAMA_MODEL=your_model_name
export OLLAMA_API_KEY=your_api_key
```

### 设置 Ollama
- **环境变量配置**：要集成 Ollama，请设置以下环境变量：
```sh
OPENAI_API_BASE='http://localhost:11434'
OPENAI_MODEL_NAME='llama2'  # 根据可用模型进行调整
OPENAI_API_KEY=''
```

## Ollama 集成（例如：在本地使用 Llama 2）
1. [下载 Ollama](https://ollama.com/download)。  
2. 设置好 Ollama 后，在终端输入以下命令以拉取 Llama2 ```ollama pull llama2```。  
3. 享受由 crewai 的优秀代理提供支持的免费 Llama2 模型。  
```
from crewai import Agent, Task, Crew
from langchain.llms import Ollama
import os
os.environ["OPENAI_API_KEY"] = "NA"

llm = Ollama(
    model = "llama2",
    base_url = "http://localhost:11434")

general_agent = Agent(role = "Math Professor",
                      goal = """Provide the solution to the students that are asking mathematical questions and give them the answer.""",
                      backstory = """You are an excellent math professor that likes to solve math questions in a way that everyone can understand your solution""",
                      allow_delegation = False,
                      verbose = True,
                      llm = llm)

task = Task(description="""what is 3 + 5""",
             agent = general_agent,
             expected_output="A numerical answer.")

crew = Crew(
            agents=[general_agent],
            tasks=[task],
            verbose=2
        )

result = crew.kickoff()

print(result)
```

## HuggingFace 集成
有几种不同的方法可以使用 HuggingFace 来托管您的 LLM。

### 您自己的 HuggingFace 端点
```python
from langchain_community.llms import HuggingFaceEndpoint

llm = HuggingFaceEndpoint(
    endpoint_url="<YOUR_ENDPOINT_URL_HERE>",
    huggingfacehub_api_token="<HF_TOKEN_HERE>",
    task="text-generation",
    max_new_tokens=512
)

agent = Agent(
    role="HuggingFace Agent",
    goal="使用 HuggingFace 生成文本",
    backstory="一位勤奋的 GitHub 文档探索者。",
    llm=llm
)
```

### 来自 HuggingFaceHub 端点
```python
from langchain_community.llms import HuggingFaceHub

llm = HuggingFaceHub(
    repo_id="HuggingFaceH4/zephyr-7b-beta",
    huggingfacehub_api_token="<HF_TOKEN_HERE>",
    task="text-generation",
)
```

## OpenAI Compatible API Endpoints
Seamlessly switch between APIs and models via environment variables, supporting platforms like FastChat, LM Studio, Groq, and Mistral AI.

### 配置示例
#### FastChat
```sh
OPENAI_API_BASE="http://localhost:8001/v1"
OPENAI_MODEL_NAME='oh-2.5m7b-q51'
OPENAI_API_KEY=NA
```

#### LM Studio
启动 [LM Studio](https://lmstudio.ai) 并转到服务器标签。然后从下拉菜单中选择一个模型并等待其加载。一旦加载完成，点击绿色的启动服务器按钮，并使用显示的 URL、端口和 API 密钥（您可以修改它们）。以下是 LM Studio 0.2.19 的默认设置示例：
```sh
OPENAI_API_BASE="http://localhost:1234/v1"
OPENAI_API_KEY="lm-studio"
```

#### Groq API
```sh
OPENAI_API_KEY=your-groq-api-key
OPENAI_MODEL_NAME='llama3-8b-8192'
OPENAI_API_BASE=https://api.groq.com/openai/v1
```

#### Mistral API
```sh
OPENAI_API_KEY=your-mistral-api-key
OPENAI_API_BASE=https://api.mistral.ai/v1
OPENAI_MODEL_NAME="mistral-small"
```

### 太阳能
```python
from langchain_community.chat_models.solar import SolarChat
# 初始化语言模型
os.environ["SOLAR_API_KEY"] = "your-solar-api-key"
llm = SolarChat(max_tokens=1024)

# 免费开发者 API 密钥可在此获取: https://console.upstage.ai/services/solar
# Langchain 示例: https://github.com/langchain-ai/langchain/pull/18556
```

### text-gen-web-ui
```sh
OPENAI_API_BASE=http://localhost:5000/v1
OPENAI_MODEL_NAME=NA
OPENAI_API_KEY=NA
```

### Cohere
```python
from langchain_cohere import ChatCohere
# 初始化语言模型
os.environ["COHERE_API_KEY"] = "your-cohere-api-key"
llm = ChatCohere()

# 免费开发者 API 密钥可在此处获取: https://cohere.com/
# Langchain 文档: https://python.langchain.com/docs/integrations/chat/cohere
```

### Azure Open AI 配置
要进行 Azure OpenAI API 集成，请设置以下环境变量：
```sh
AZURE_OPENAI_VERSION="2022-12-01"
AZURE_OPENAI_DEPLOYMENT=""
AZURE_OPENAI_ENDPOINT=""
AZURE_OPENAI_KEY=""
```

### 示例代理与 Azure LLM
```python
from dotenv import load_dotenv
from crewai import Agent
from langchain_openai import AzureChatOpenAI

load_dotenv()

azure_llm = AzureChatOpenAI(
    azure_endpoint=os.environ.get("AZURE_OPENAI_ENDPOINT"),
    api_key=os.environ.get("AZURE_OPENAI_KEY")
)

azure_agent = Agent(
  role='示例代理',
  goal='演示自定义 LLM 配置',
  backstory='一位勤奋的 GitHub 文档探索者。',
  llm=azure_llm
)
```

## Conclusion
Integrating CrewAI with different LLMs expands the versatility of the framework, enabling it to deliver customized and efficient AI solutions across various domains and platforms.