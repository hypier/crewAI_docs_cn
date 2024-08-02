---
title: crewAI 工具
description: 理解和利用 crewAI 框架内的工具以实现代理协作和任务执行。
---

## 介绍
CrewAI 工具为代理提供了从网络搜索和数据分析到协作和在同事之间分配任务的各种能力。本文件概述了如何在 CrewAI 框架内创建、集成和利用这些工具，包括对协作工具的新关注。

## 什么是工具？
!!! note "定义"
    在CrewAI中，工具是代理可以利用的技能或功能，以执行各种操作。这包括来自[crewAI Toolkit](https://github.com/joaomdmoura/crewai-tools)和[LangChain Tools](https://python.langchain.com/docs/integrations/tools)的工具，能够实现从简单搜索到复杂交互以及代理之间的有效协作。
``` 
```

## 工具的关键特性

- **实用性**：旨在执行诸如网络搜索、数据分析、内容生成和代理协作等任务。
- **集成性**：通过将工具无缝集成到工作流程中，增强代理的能力。
- **可定制性**：提供灵活性，以开发自定义工具或利用现有工具，满足代理的特定需求。
- **错误处理**：包含强大的错误处理机制，以确保顺利操作。
- **缓存机制**：具有智能缓存功能，以优化性能并减少冗余操作。

```
# This is a code block
def example_function():
    print("Hello, World!")
```

## 使用 crewAI 工具

要通过 crewAI 工具增强您的代理能力，请首先安装我们的额外工具包：

```bash
pip install 'crewai[tools]'
```

以下是演示其使用的示例：

```python
import os
from crewai import Agent, Task, Crew
# Importing crewAI tools
from crewai_tools import (
    DirectoryReadTool,
    FileReadTool,
    SerperDevTool,
    WebsiteSearchTool
)

# Set up API keys
os.environ["SERPER_API_KEY"] = "Your Key" # serper.dev API key
os.environ["OPENAI_API_KEY"] = "Your Key"

# Instantiate tools
docs_tool = DirectoryReadTool(directory='./blog-posts')
file_tool = FileReadTool()
search_tool = SerperDevTool()
web_rag_tool = WebsiteSearchTool()

# Create agents
researcher = Agent(
    role='市场研究分析师',
    goal='提供最新的人工智能行业市场分析',
    backstory='一位对市场趋势有敏锐洞察力的专家分析师。',
    tools=[search_tool, web_rag_tool],
    verbose=True
)

writer = Agent(
    role='内容撰写者',
    goal='撰写关于人工智能行业的引人入胜的博客文章',
    backstory='一位对技术充满热情的熟练写手。',
    tools=[docs_tool, file_tool],
    verbose=True
)

# Define tasks
research = Task(
    description='研究人工智能行业的最新趋势并提供摘要。',
    expected_output='关于人工智能行业前三大趋势发展的摘要，并独特地阐述其重要性。',
    agent=researcher
)

write = Task(
    description='根据研究分析师的摘要撰写一篇引人入胜的关于人工智能行业的博客文章。从目录中的最新博客文章中汲取灵感。',
    expected_output='一篇包含4段的博客文章，格式为markdown，内容引人入胜、信息丰富且易于理解，避免复杂的行话。',
    agent=writer,
    output_file='blog-posts/new_post.md'  # The final blog post will be saved here
)

# Assemble a crew
crew = Crew(
    agents=[researcher, writer],
    tasks=[research, write],
    verbose=2
)

# Execute tasks
crew.kickoff()
```

## 可用的 crewAI 工具

- **错误处理**：所有工具都具备错误处理能力，使代理能够优雅地管理异常并继续其任务。
- **缓存机制**：所有工具支持缓存，允许代理高效地重用先前获得的结果，从而减少对外部资源的负担并加快执行时间。您还可以使用工具上的 `cache_function` 属性定义更细致的缓存机制控制。

以下是可用工具及其描述的列表：

| 工具                          | 描述                                                                                       |
| :---------------------------- | :------------------------------------------------------------------------------------------ |
| **BrowserbaseLoadTool**       | 与网页浏览器交互并提取数据的工具。                                                          |
| **CodeDocsSearchTool**        | 针对代码文档和相关技术文档搜索优化的 RAG 工具。                                             |
| **CodeInterpreterTool**       | 用于解释 Python 代码的工具。                                                               |
| **ComposioTool**              | 使用户能够使用 Composio 工具。                                                             |
| **CSVSearchTool**             | 设计用于在 CSV 文件中搜索的 RAG 工具，专门处理结构化数据。                                 |
| **DirectorySearchTool**       | 用于在目录中搜索的 RAG 工具，适合在文件系统中导航。                                       |
| **DOCXSearchTool**            | 针对在 DOCX 文档中搜索的 RAG 工具，适合处理 Word 文件。                                    |
| **DirectoryReadTool**         | 促进目录结构及其内容的读取和处理。                                                         |
| **EXASearchTool**             | 设计用于在各种数据源中执行全面搜索的工具。                                               |
| **FileReadTool**              | 使用户能够从文件中读取和提取数据，支持多种文件格式。                                       |
| **FirecrawlSearchTool**       | 用于使用 Firecrawl 搜索网页并返回结果的工具。                                             |
| **FirecrawlCrawlWebsiteTool** | 用于使用 Firecrawl 爬取网页的工具。                                                       |
| **FirecrawlScrapeWebsiteTool** | 用于使用 Firecrawl 爬取网页 URL 并返回其内容的工具。                                     |
| **GithubSearchTool**          | 用于在 GitHub 仓库中搜索的 RAG 工具，适合代码和文档搜索。                                 |
| **SerperDevTool**             | 专为开发目的而设计的工具，具有特定功能正在开发中。                                          |
| **TXTSearchTool**             | 专注于在文本（.txt）文件中搜索的 RAG 工具，适合处理非结构化数据。                          |
| **JSONSearchTool**            | 设计用于在 JSON 文件中搜索的 RAG 工具，适合结构化数据处理。                              |
| **LlamaIndexTool**            | 使用户能够使用 LlamaIndex 工具。                                                           |
| **MDXSearchTool**             | 针对在 Markdown（MDX）文件中搜索的 RAG 工具，适合文档处理。                               |
| **PDFSearchTool**             | 旨在在 PDF 文档中搜索的 RAG 工具，适合处理扫描文档。                                      |
| **PGSearchTool**              | 针对在 PostgreSQL 数据库中搜索优化的 RAG 工具，适合数据库查询。                          |
| **RagTool**                   | 一种通用的 RAG 工具，能够处理各种数据源和类型。                                            |
| **ScrapeElementFromWebsiteTool** | 使用户能够从网站抓取特定元素，适合有针对性的数据提取。                                    |
| **ScrapeWebsiteTool**         | 促进整个网站的抓取，适合全面的数据收集。                                                  |
| **WebsiteSearchTool**         | 用于搜索网站内容的 RAG 工具，优化了网络数据提取。                                        |
| **XMLSearchTool**             | 设计用于在 XML 文件中搜索的 RAG 工具，适合结构化数据格式。                                |
| **YoutubeChannelSearchTool**  | 用于在 YouTube 频道中搜索的 RAG 工具，适合视频内容分析。                                  |
| **YoutubeVideoSearchTool**    | 旨在在 YouTube 视频中搜索的 RAG 工具，适合视频数据提取。                                  |

## 创建您自己的工具

!!! example "自定义工具创建"
    开发人员可以根据其代理的需求制作自定义工具或使用预构建的选项：

要创建您自己的 crewAI 工具，您需要安装我们的额外工具包：

```bash
pip install 'crewai[tools]'
```

完成后，有两种主要方法可以创建 crewAI 工具：

### 子类化 `BaseTool`

```python
from crewai_tools import BaseTool

class MyCustomTool(BaseTool):
    name: str = "Name of my tool"
    description: str = "Clear description for what this tool is useful for, your agent will need this information to use it."

    def _run(self, argument: str) -> str:
        # Implementation goes here
        return "Result from custom tool"
```

### 使用 `tool` 装饰器

```python
from crewai_tools import tool
@tool("Name of my tool")
def my_tool(question: str) -> str:
    """Clear description for what this tool is useful for, your agent will need this information to use it."""
    # Function logic here
    return "Result from your custom tool"
```

### 自定义缓存机制
!!! note "缓存"
    工具可以选择性地实现一个 `cache_function` 来微调缓存行为。此函数根据特定条件确定何时缓存结果，从而提供对缓存逻辑的细粒度控制。

```python
from crewai_tools import tool

@tool
def multiplication_tool(first_number: int, second_number: int) -> str:
    """Useful for when you need to multiply two numbers together."""
    return first_number * second_number

def cache_func(args, result):
    # In this case, we only cache the result if it's a multiple of 2
    cache = result % 2 == 0
    return cache

multiplication_tool.cache_function = cache_func

writer1 = Agent(
        role="Writer",
        goal="You write lessons of math for kids.",
        backstory="You're an expert in writing and you love to teach kids but you know nothing of math.",
        tools=[multiplication_tool],
        allow_delegation=False,
    )
    #...
```

## Conclusion
Tools are essential for extending the capabilities of CrewAI agents, enabling them to undertake a wide range of tasks and collaborate effectively. When building solutions with CrewAI, leverage custom tools and existing tools to enhance your agents and elevate the AI ecosystem. Consider utilizing error handling, caching mechanisms, and the flexibility of tool parameters to optimize the performance and capabilities of your agents.