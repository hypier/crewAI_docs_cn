# TXTSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## Description
This tool is used to perform RAG (Retrieval-Augmented Generation) searches within the contents of text files. It allows for semantic searches on the content of specified text files, making it a valuable resource for quickly extracting information or finding specific text passages based on provided queries.

## 安装
要使用 TXTSearchTool，您首先需要安装 crewai_tools 包。可以使用 pip（Python 的包管理器）来完成此操作。打开您的终端或命令提示符，并输入以下命令：

```shell
pip install 'crewai[tools]'
```

此命令将下载并安装 TXTSearchTool 及其所有必要的依赖项。

## 示例
以下示例演示如何使用 TXTSearchTool 在文本文件中进行搜索。该示例展示了如何用特定文本文件初始化工具以及随后在该文件内容中进行搜索。

```python
from crewai_tools import TXTSearchTool

# Initialize the tool to search within any text file's content the agent learns about during its execution
tool = TXTSearchTool()

# OR

# Initialize the tool with a specific text file, so the agent can search within the given text file's content
tool = TXTSearchTool(txt='path/to/text/file.txt')
```

## 参数
- `txt` (str): **可选**。您要搜索的文本文件的路径。仅当工具未使用特定文本文件初始化时，此参数才是必需的；否则，搜索将在最初提供的文本文件中进行。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = TXTSearchTool(
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