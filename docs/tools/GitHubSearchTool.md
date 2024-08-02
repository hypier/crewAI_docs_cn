# GithubSearchTool

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会有意外行为或变化。

## 描述
GithubSearchTool 是一个检索增强生成 (RAG) 工具，专门用于在 GitHub 仓库中进行语义搜索。利用先进的语义搜索功能，它可以筛选代码、拉取请求、问题和仓库，使其成为开发人员、研究人员或任何需要从 GitHub 获取精确信息的人的重要工具。

## 安装
要使用GithubSearchTool，首先确保在您的Python环境中安装了crewai_tools包：

```shell
pip install 'crewai[tools]'
```

此命令安装运行GithubSearchTool所需的包，以及crewai_tools包中包含的其他工具。

## 示例
以下是如何使用 GithubSearchTool 在 GitHub 仓库中执行语义搜索：
```python
from crewai_tools import GithubSearchTool

# 初始化工具以在特定 GitHub 仓库中进行语义搜索
tool = GithubSearchTool(
	github_repo='https://github.com/example/repo',
	content_types=['code', 'issue'] # 选项：code, repo, pr, issue
)

# 或者

# 初始化工具以在特定 GitHub 仓库中进行语义搜索，以便代理可以在执行过程中搜索任何仓库
tool = GithubSearchTool(
	content_types=['code', 'issue'] # 选项：code, repo, pr, issue
)
```

## 参数
- `github_repo` : 要进行搜索的 GitHub 仓库的 URL。此字段为必填项，指定了搜索的目标仓库。
- `content_types` : 指定要包含在搜索中的内容类型。您必须提供以下选项中的内容类型列表：`code` 用于在代码中搜索，`repo` 用于在仓库的一般信息中搜索，`pr` 用于在拉取请求中搜索，以及 `issue` 用于在问题中搜索。此字段为必填项，允许根据 GitHub 仓库中的特定内容类型定制搜索。

## 自定义模型和嵌入

默认情况下，该工具使用 OpenAI 进行嵌入和摘要。要自定义模型，可以使用如下的配置字典：

```python
tool = GithubSearchTool(
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