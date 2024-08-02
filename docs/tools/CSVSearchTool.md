# CSVSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会出现意外行为或变化。

## 描述

此工具用于在CSV文件内容中执行RAG（检索增强生成）搜索。它允许用户在指定CSV文件的内容中进行语义搜索。该功能对于从大型CSV数据集中提取信息特别有用，因为传统搜索方法可能效率低下。所有名称中包含“Search”的工具，包括CSVSearchTool，都是旨在搜索不同数据源的RAG工具。 

```
# Code block example
print("Hello, world!")
```

## 安装

安装 crewai_tools 包

```shell
pip install 'crewai[tools]'
```

## 示例

```python
from crewai_tools import CSVSearchTool

# 使用特定的 CSV 文件初始化工具。此设置允许代理仅搜索给定的 CSV 文件。
tool = CSVSearchTool(csv='path/to/your/csvfile.csv')

# 或者

# 在没有特定 CSV 文件的情况下初始化工具。代理需要在运行时提供 CSV 路径。
tool = CSVSearchTool()
```

## 参数

- `csv` : 要搜索的 CSV 文件的路径。如果工具在没有指定 CSV 文件的情况下初始化，则这是一个必需的参数；否则，它是可选的。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用以下配置字典：

```python
tool = CSVSearchTool(
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