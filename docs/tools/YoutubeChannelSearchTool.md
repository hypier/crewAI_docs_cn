# YoutubeChannelSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## 描述
该工具旨在对特定YouTube频道内容进行语义搜索。利用RAG（检索增强生成）方法，它提供相关的搜索结果，使其在提取信息或查找特定内容时变得不可或缺，无需手动浏览视频。它简化了YouTube频道内的搜索过程，满足研究人员、内容创作者和寻求特定信息或主题的观众的需求。

## 安装
要使用 YoutubeChannelSearchTool，必须安装 `crewai_tools` 包。在您的 shell 中执行以下命令进行安装：

```shell
pip install 'crewai[tools]'
```

## 示例
要开始使用YoutubeChannelSearchTool，请按照下面的示例进行操作。这演示了如何使用特定的Youtube频道句柄初始化工具并在该频道的内容中进行搜索。

```python
from crewai_tools import YoutubeChannelSearchTool

# Initialize the tool to search within any Youtube channel's content the agent learns about during its execution
tool = YoutubeChannelSearchTool()

# OR

# Initialize the tool with a specific Youtube channel handle to target your search
tool = YoutubeChannelSearchTool(youtube_channel_handle='@exampleChannel')
```

## 参数
- `youtube_channel_handle` : 一个必需的字符串，表示Youtube频道的句柄。该参数对于初始化工具以指定您想要搜索的频道至关重要。该工具仅设计用于在提供的频道句柄的内容中进行搜索。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = YoutubeChannelSearchTool(
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