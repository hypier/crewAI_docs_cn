# PGSearchTool

!!! note "正在开发中"
    PGSearchTool 目前正在开发中。本文档概述了预期的功能和界面。随着开发的进行，请注意某些功能可能不可用或可能会发生变化。

## 描述
PGSearchTool 被设想为一个强大的工具，用于在 PostgreSQL 数据库表中促进语义搜索。通过利用先进的检索与生成 (RAG) 技术，它旨在提供一种高效的方式来查询数据库表内容，特别针对 PostgreSQL 数据库。该工具的目标是简化通过语义搜索查询找到相关数据的过程，为需要在 PostgreSQL 环境中对大量数据集进行高级查询的用户提供一个有价值的资源。

## 安装
`crewai_tools` 包将在发布时包含 PGSearchTool，可以使用以下命令进行安装：

```shell
pip install 'crewai[tools]'
```

（注意：PGSearchTool 在当前版本的 `crewai_tools` 包中尚不可用。该安装命令将在工具发布后进行更新。）

## 示例用法
以下是一个示例，展示如何使用 PGSearchTool 在 PostgreSQL 数据库中的表上进行语义搜索：

```python
from crewai_tools import PGSearchTool

# Initialize the tool with the database URI and the target table name
tool = PGSearchTool(db_uri='postgresql://user:password@localhost:5432/mydatabase', table_name='employees')
```

## 参数
PGSearchTool 设计为需要以下参数才能操作：

- `db_uri`: 一个字符串，表示要查询的 PostgreSQL 数据库的 URI。此参数是必需的，必须包含必要的身份验证详细信息和数据库的位置。
- `table_name`: 一个字符串，指定将在其上执行语义搜索的数据库表的名称。此参数也是必需的。

## 自定义模型和嵌入

该工具默认使用OpenAI进行嵌入和摘要。用户可以通过以下配置字典自定义模型：

```python
tool = PGSearchTool(
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