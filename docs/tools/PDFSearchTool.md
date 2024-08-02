# PDFSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## 描述  
PDFSearchTool 是一个 RAG 工具，旨在对 PDF 内容进行语义搜索。它允许输入搜索查询和 PDF 文档，利用先进的搜索技术高效地查找相关内容。这一功能使其在快速从大型 PDF 文件中提取特定信息方面特别有用。  
```  
# 示例代码  
def search_pdf(query, pdf_document):  
    # 实现搜索逻辑  
    pass  
```

## 安装
要开始使用 PDFSearchTool，首先确保安装了 crewai_tools 包，可以使用以下命令：

```shell
pip install 'crewai[tools]'
```

## 示例
以下是如何使用 PDFSearchTool 在 PDF 文档中进行搜索的方法：

```python
from crewai_tools import PDFSearchTool

# Initialize the tool allowing for any PDF content search if the path is provided during execution
tool = PDFSearchTool()

# OR

# Initialize the tool with a specific PDF path for exclusive search within that document
tool = PDFSearchTool(pdf='path/to/your/document.pdf')
```

## 参数
- `pdf`: **可选** 搜索的 PDF 路径。可以在初始化时提供，也可以在 `run` 方法的参数中提供。如果在初始化时提供，则工具将其搜索限制在指定的文档中。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = PDFSearchTool(
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