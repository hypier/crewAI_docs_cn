# DOCXSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会出现意外行为或变化。

## 描述
DOCXSearchTool 是一个 RAG 工具，旨在对 DOCX 文档进行语义搜索。它使用户能够通过基于查询的搜索有效地查找和提取 DOCX 文件中的相关信息。该工具对数据分析、信息管理和研究任务至关重要，简化了在大型文档集合中查找特定信息的过程。

## 安装
通过在终端中运行以下命令安装 crewai_tools 包：

```shell
pip install 'crewai[tools]'
```

## 示例
以下示例演示了如何初始化 DOCXSearchTool 以在任何 DOCX 文件的内容中进行搜索或使用特定的 DOCX 文件路径进行搜索。

```python
from crewai_tools import DOCXSearchTool

# Initialize the tool to search within any DOCX file's content
tool = DOCXSearchTool()

# OR

# Initialize the tool with a specific DOCX file, so the agent can only search the content of the specified DOCX file
tool = DOCXSearchTool(docx='path/to/your/document.docx')
```

## 参数
- `docx`：一个可选的文件路径，用于指定您希望搜索的特定 DOCX 文档。如果在初始化时未提供，该工具允许稍后指定任何 DOCX 文件的内容路径进行搜索。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = DOCXSearchTool(
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