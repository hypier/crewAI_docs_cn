# YoutubeVideoSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此未来可能会出现意外行为或变化。

## 描述

该工具是`crewai_tools`包的一部分，旨在利用检索增强生成（RAG）技术在Youtube视频内容中执行语义搜索。它是该包中多个利用RAG进行不同来源搜索的“搜索”工具之一。YoutubeVideoSearchTool允许灵活的搜索；用户可以在任何Youtube视频内容中进行搜索，而无需指定视频URL，或者通过提供特定视频的URL来针对性地进行搜索。

## 安装

要使用YoutubeVideoSearchTool，您必须首先安装`crewai_tools`包。该包包含YoutubeVideoSearchTool以及其他旨在增强您的数据分析和处理任务的工具。通过在终端中执行以下命令来安装该包：

```
pip install 'crewai[tools]'
```

## 示例

要将YoutubeVideoSearchTool集成到您的Python项目中，请按照下面的示例进行操作。这演示了如何使用该工具进行一般的Youtube内容搜索以及在特定视频内容内进行针对性搜索。

```python
from crewai_tools import YoutubeVideoSearchTool

# General search across Youtube content without specifying a video URL, so the agent can search within any Youtube video content it learns about irs url during its operation
tool = YoutubeVideoSearchTool()

# Targeted search within a specific Youtube video's content
tool = YoutubeVideoSearchTool(youtube_video_url='https://youtube.com/watch?v=example')
```

## 参数

YoutubeVideoSearchTool 接受以下初始化参数：

- `youtube_video_url`：在初始化时为可选参数，但如果目标是特定的 Youtube 视频，则为必需参数。它指定您想要搜索的 Youtube 视频 URL 路径。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = YoutubeVideoSearchTool(
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