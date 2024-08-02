# CodeDocsSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## Description

CodeDocsSearchTool is a powerful RAG (Retrieval-Augmented Generation) tool designed for semantic search of code documentation. It enables users to efficiently find specific information or topics within code documentation. By providing a `docs_url` during initialization, the tool narrows its search scope to a specific documentation site. Alternatively, if no specific `docs_url` is provided, it will search through a broad range of code documentation known or discovered during its execution, making it suitable for various documentation search needs.

## 安装

要开始使用 CodeDocsSearchTool，请首先通过 pip 安装 crewai_tools 包：

```
pip install 'crewai[tools]'
```

## 示例

使用 CodeDocsSearchTool 进行代码文档搜索，如下所示：

```python
from crewai_tools import CodeDocsSearchTool

# To search any code documentation content if the URL is known or discovered during its execution:
tool = CodeDocsSearchTool()

# OR

# To specifically focus your search on a given documentation site by providing its URL:
tool = CodeDocsSearchTool(docs_url='https://docs.example.com/reference')
```
注意：将 'https://docs.example.com/reference' 替换为您的目标文档 URL，将 'How to use search tool' 替换为与您的需求相关的搜索查询。

## 参数

- `docs_url`: 可选。指定要搜索的代码文档的 URL。在工具初始化时提供此参数可以将搜索重点放在指定的文档内容上。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = CodeDocsSearchTool(
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