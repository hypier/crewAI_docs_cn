# XMLSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## 描述
XMLSearchTool 是一款尖端的 RAG 工具，旨在对 XML 文件进行语义搜索。非常适合需要高效解析和提取 XML 内容信息的用户，该工具支持输入搜索查询和可选的 XML 文件路径。通过指定 XML 路径，用户可以更精确地定位搜索内容，从而获得更相关的搜索结果。

```python
def search_xml(query, xml_path=None):
    # implementation details
    pass
```

## 安装
要开始使用 XMLSearchTool，您必须首先安装 crewai_tools 包。您可以使用以下命令轻松完成此操作：

```shell
pip install 'crewai[tools]'
```

## 示例
以下是两个示例，演示如何使用 XMLSearchTool。第一个示例显示在特定 XML 文件中进行搜索，而第二个示例说明在不预定义 XML 路径的情况下启动搜索，提供了搜索范围的灵活性。

```python
from crewai_tools import XMLSearchTool

# Allow agents to search within any XML file's content as it learns about their paths during execution
tool = XMLSearchTool()

# OR

# Initialize the tool with a specific XML file path for exclusive search within that document
tool = XMLSearchTool(xml='path/to/your/xmlfile.xml')
```

## 参数
- `xml`: 这是您希望搜索的 XML 文件的路径。它是在工具初始化时的可选参数，但必须在初始化时提供，或者作为 `run` 方法参数的一部分提供，以执行搜索。  
```  
```

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = XMLSearchTool(
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