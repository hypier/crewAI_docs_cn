# SerperDevTool 文档

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会出现意外行为或变化。

## 描述
该工具旨在对互联网上文本内容进行指定查询的语义搜索。它利用 [serper.dev](https://serper.dev) API 来获取并显示基于用户提供的查询的最相关搜索结果。

```
# Code block example
def example_function():
    print("This is an example function.")
```

## 安装
要将此工具集成到您的项目中，请按照以下安装说明进行操作：
```shell
pip install 'crewai[tools]'
```

## 示例
以下示例演示了如何初始化工具并使用给定查询执行搜索：

```python
from crewai_tools import SerperDevTool

# Initialize the tool for internet searching capabilities
tool = SerperDevTool()
```

## 开始的步骤
要有效使用 `SerperDevTool`，请按照以下步骤操作：

1. **软件包安装**：确认在您的 Python 环境中已安装 `crewai[tools]` 软件包。
2. **获取 API 密钥**：通过在 `serper.dev` 注册免费账户来获取 `serper.dev` API 密钥。
3. **环境配置**：将获得的 API 密钥存储在名为 `SERPER_API_KEY` 的环境变量中，以便工具使用。

## 参数

`SerperDevTool` 有几个参数将传递给 API：

- **search_url**: 搜索 API 的 URL 端点。 (默认是 `https://google.serper.dev/search`)

- **country**: 可选。指定搜索结果的国家。
- **location**: 可选。指定搜索结果的位置。
- **locale**: 可选。指定搜索结果的语言环境。
- **n_results**: 返回的搜索结果数量。默认是 `10`。

`country`、`location`、`locale` 和 `search_url` 的值可以在 [Serper Playground](https://serper.dev/playground) 上找到。

## 示例与参数

这是一个演示如何使用工具与附加参数的示例：

```python
from crewai_tools import SerperDevTool

tool = SerperDevTool(
    search_url="https://google.serper.dev/scholar",
    n_results=2,
)

print(tool.run(search_query="ChatGPT"))

# 使用工具：搜索互联网

# 搜索结果：标题：ChatGPT在公共卫生中的作用
# 链接：https://link.springer.com/article/10.1007/s10439-023-03172-7
# 摘要：… ChatGPT在公共卫生中的应用。在本概述中，我们将探讨ChatGPT在
# ---
# 标题：ChatGPT在全球变暖中的潜在应用
# 链接：https://link.springer.com/article/10.1007/s10439-023-03171-8
# 摘要：… 作为ChatGPT，具有在推动我们对气候理解方面发挥关键作用的潜力
# ---

```

```python
from crewai_tools import SerperDevTool

tool = SerperDevTool(
    country="fr",
    locale="fr",
    location="Paris, Paris, Ile-de-France, France",
    n_results=2,
)

print(tool.run(search_query="Jeux Olympiques"))

# 使用工具：搜索互联网

# 搜索结果：标题：巴黎2024年奥运会 - 新闻、日程、结果
# 链接：https://olympics.com/fr/paris-2024
# 摘要：巴黎2024年奥运会有哪些运动项目？· 田径 · 划船 · 羽毛球 · 篮球 · 3x3篮球 · 拳击 · 街舞 · 皮划艇 ...
# ---
# 标题：巴黎2024年奥运会官方票务 - 奥运会和残奥会
# 链接：https://tickets.paris2024.org/
# 摘要：请在巴黎2024年官方票务网站上独家购买您的票，以参与全球最大体育盛事。
# ---

```

## 结论
通过将 `SerperDevTool` 集成到 Python 项目中，用户可以直接从他们的应用程序中执行实时、相关的互联网搜索。更新的参数允许更定制化和本地化的搜索结果。遵循提供的设置和使用指南，使得将此工具纳入项目变得简单明了。