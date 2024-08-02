# 目录搜索工具

!!! note "实验性"
    目录搜索工具正在持续开发中。功能和特性可能会不断演变，并且在我们改进工具的过程中可能会出现意外行为。

## 描述
DirectorySearchTool 使用户能够在指定目录的内容中进行语义搜索，利用检索增强生成（RAG）方法高效浏览文件。该工具设计灵活，允许用户在运行时动态指定搜索目录或在初始设置时设置固定目录。

```
# 示例代码
def search_directory(directory):
    # 在指定目录中进行搜索
    pass
```

## 安装
要使用 DirectorySearchTool，首先安装 crewai_tools 包。在终端中执行以下命令：

```shell
pip install 'crewai[tools]'
```

## 初始化和使用
从 `crewai_tools` 包中导入 DirectorySearchTool 以开始。您可以在不指定目录的情况下初始化该工具，从而在运行时设置搜索目录。或者，可以使用预定义的目录初始化该工具。

```python
from crewai_tools import DirectorySearchTool

# For dynamic directory specification at runtime
tool = DirectorySearchTool()

# For fixed directory searches
tool = DirectorySearchTool(directory='/path/to/directory')
```

## 参数
- `directory`: 一个字符串参数，用于指定搜索目录。初始化时这是可选的，但如果最初未设置，则在搜索时是必需的。

## 自定义模型和嵌入
DirectorySearchTool 默认使用 OpenAI 进行嵌入和摘要。这些设置的自定义选项包括更改模型提供者和配置，以增强高级用户的灵活性。

```python
tool = DirectorySearchTool(
    config=dict(
        llm=dict(
            provider="ollama", # 选项包括 ollama、google、anthropic、llama2 等
            config=dict(
                model="llama2",
                # 这里是额外的配置
            ),
        ),
        embedder=dict(
            provider="google", # 或 openai、ollama 等...
            config=dict(
                model="models/embedding-001",
                task_type="retrieval_document",
                # title="Embeddings",
            ),
        ),
    )
)
```