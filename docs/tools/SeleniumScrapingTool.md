# SeleniumScrapingTool

!!! note "实验性"
    此工具目前正在开发中。在我们完善其功能的过程中，用户可能会遇到意外行为。您的反馈对我们改进非常重要。

## 描述
SeleniumScrapingTool 是为高效网页抓取任务而设计的。它通过使用 CSS 选择器来精确提取网页内容，针对特定元素。其设计满足广泛的抓取需求，提供灵活性以处理任何提供的网站 URL。

## 安装
要开始使用 SeleniumScrapingTool，请使用 pip 安装 crewai_tools 包：

```
pip install 'crewai[tools]'
```

## 使用示例
以下是一些可以使用 SeleniumScrapingTool 的场景：

```python
from crewai_tools import SeleniumScrapingTool

# 示例 1：初始化工具，无需任何参数以抓取它导航到的当前页面
tool = SeleniumScrapingTool()

# 示例 2：抓取给定 URL 的整个网页
tool = SeleniumScrapingTool(website_url='https://example.com')

# 示例 3：从网页中定位并抓取特定的 CSS 元素
tool = SeleniumScrapingTool(website_url='https://example.com', css_element='.main-content')

# 示例 4：使用附加参数执行抓取以获得自定义体验
tool = SeleniumScrapingTool(website_url='https://example.com', css_element='.main-content', cookie={'name': 'user', 'value': 'John Doe'}, wait_time=10)
```

## 参数
以下参数可用于自定义 SeleniumScrapingTool 的抓取过程：

- `website_url`: **必填**。指定要抓取内容的网站的 URL。
- `css_element`: **必填**。用于定位网站上特定元素的 CSS 选择器。这使得能够集中抓取网页的特定部分。
- `cookie`: **可选**。包含 cookie 信息的字典。用于模拟登录会话，从而访问可能对未登录用户限制的内容。
- `wait_time`: **可选**。指定抓取内容之前的延迟（以秒为单位）。此延迟允许网站及任何动态内容完全加载，确保成功抓取。

!!! 注意
    因为 SeleniumScrapingTool 正在积极开发中，参数和功能可能会随时间演变。鼓励用户保持工具更新，并报告任何问题或改进建议。