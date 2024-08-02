# MDXSearchTool

!!! note "实验性"
    MDXSearchTool 正在持续开发中。功能可能会添加或删除，功能也可能会在我们完善工具时发生不可预测的变化。

## 描述
MDX搜索工具是`crewai_tools`包的一个组件，旨在促进高级Markdown语言的提取。它使用户能够有效地使用基于查询的搜索从MD文件中搜索和提取相关信息。该工具对于数据分析、信息管理和研究任务非常重要，简化了在大型文档集合中查找特定信息的过程。

## 安装
在使用 MDX 搜索工具之前，请确保已安装 `crewai_tools` 包。如果尚未安装，可以使用以下命令进行安装：

```shell
pip install 'crewai[tools]'
```

## 使用示例
要使用 MDX 搜索工具，您必须首先设置必要的环境变量。然后，将该工具集成到您的 crewAI 项目中，以开始市场研究。以下是如何做到这一点的基本示例：

```python
from crewai_tools import MDXSearchTool

# Initialize the tool to search any MDX content it learns about during execution
tool = MDXSearchTool()

# OR

# Initialize the tool with a specific MDX file path for an exclusive search within that document
tool = MDXSearchTool(mdx='path/to/your/document.mdx')
```

## 参数
- mdx: **可选**。指定搜索的 MDX 文件路径。可以在初始化时提供。

## 模型和嵌入的定制

该工具默认使用 OpenAI 进行嵌入和摘要。要进行定制，请使用如下配置字典：

```python
tool = MDXSearchTool(
    config=dict(
        llm=dict(
            provider="ollama", # 可选项包括 google, openai, anthropic, llama2 等。
            config=dict(
                model="llama2",
                # 可在此处包含可选参数。
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
                # 可在此处添加嵌入的可选标题。
                # title="Embeddings",
            ),
        ),
    )
)
```