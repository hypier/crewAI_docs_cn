# 网站搜索工具

!!! note "实验状态"
    网站搜索工具目前处于实验阶段。我们正在积极将此工具纳入我们的产品套件，并将相应更新文档。

## 描述
WebsiteSearchTool 被设计为一个概念，用于在网站内容中进行语义搜索。它旨在利用先进的机器学习模型，如检索增强生成（RAG），有效地导航和提取指定 URL 中的信息。该工具旨在提供灵活性，允许用户在任何网站上进行搜索或专注于特定感兴趣的网站。请注意，WebsiteSearchTool 的当前实现细节仍在开发中，其描述的功能可能尚不可用。

## 安装
为了准备您的环境，以便在WebsiteSearchTool可用时使用，您可以通过以下命令安装基础包：

```shell
pip install 'crewai[tools]'
```

此命令安装必要的依赖项，以确保一旦工具完全集成，用户可以立即开始使用它。

## 示例用法
以下是如何在不同场景中使用 WebsiteSearchTool 的示例。请注意，这些示例是说明性的，代表计划的功能：

```python
from crewai_tools import WebsiteSearchTool

# Example of initiating tool that agents can use to search across any discovered websites
tool = WebsiteSearchTool()

# Example of limiting the search to the content of a specific website, so now agents can only search within that website
tool = WebsiteSearchTool(website='https://example.com')
```

## 参数
- `website`: 一个可选参数，用于指定用于定向搜索的网站 URL。此参数旨在通过在必要时允许定向搜索来增强工具的灵活性。

## 自定义选项
默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = WebsiteSearchTool(
    config=dict(
        llm=dict(
            provider="ollama", # 或 google, openai, anthropic, llama2, ...
            config=dict(
                model="llama2",
                # temperature=0.5,
                # top_p=1,
                # stream=true,
            ),
        ),
        embedder=dict(
            provider="google", # 或 openai, ollama, ...
            config=dict(
                model="models/embedding-001",
                task_type="retrieval_document",
                # title="Embeddings",
            ),
        ),
    )
)
```