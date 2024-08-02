# JSONSearchTool

!!! note "实验状态"
    JSONSearchTool 目前处于实验阶段。这意味着该工具正在积极开发中，用户可能会遇到意外的行为或更改。我们非常鼓励对任何问题或改进建议提供反馈。

## 描述
JSONSearchTool旨在促进对JSON文件内容的高效和精确搜索。它利用RAG（检索和生成）搜索机制，允许用户指定JSON路径，以便在特定的JSON文件中进行有针对性的搜索。这一功能显著提高了搜索结果的准确性和相关性。

```python
def search_json(json_data, json_path):
    # Perform search operation
    pass
```

## 安装
要安装 JSONSearchTool，请使用以下 pip 命令：

```shell
pip install 'crewai[tools]'
```

## 使用示例
以下是如何有效利用 JSONSearchTool 在 JSON 文件中进行搜索的更新示例。这些示例考虑了当前实现和代码库中识别的使用模式。

```python
from crewai.json_tools import JSONSearchTool  # Updated import path

# General JSON content search
# This approach is suitable when the JSON path is either known beforehand or can be dynamically identified.
tool = JSONSearchTool()

# Restricting search to a specific JSON file
# Use this initialization method when you want to limit the search scope to a specific JSON file.
tool = JSONSearchTool(json_path='./path/to/your/file.json')
```

## 参数
- `json_path` (str, 可选): 指定要搜索的 JSON 文件的路径。如果工具被初始化为一般搜索，则此参数不是必需的。提供时，它将搜索限制在指定的 JSON 文件内。

## 配置选项
JSONSearchTool 支持通过配置字典进行广泛的自定义。这使得用户可以根据自己的需求选择不同的模型进行嵌入和摘要。

```python
tool = JSONSearchTool(
    config={
        "llm": {
            "provider": "ollama",  # 其他选项包括 google, openai, anthropic, llama2 等。
            "config": {
                "model": "llama2",
                # 这里可以指定其他可选配置。
                # temperature=0.5,
                # top_p=1,
                # stream=true,
            },
        },
        "embedder": {
            "provider": "google", # 或 openai, ollama, ...
            "config": {
                "model": "models/embedding-001",
                "task_type": "retrieval_document",
                # 这里可以添加进一步的自定义选项。
            },
        },
    }
)
```