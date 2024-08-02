# ScrapeWebsiteTool

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会出现意外行为或变化。

## 描述
一个旨在提取和读取指定网站内容的工具。它能够通过发起HTTP请求和解析接收到的HTML内容来处理各种类型的网页。该工具在网页抓取任务、数据收集或从网站提取特定信息时特别有用。

```
# Example code
import requests
from bs4 import BeautifulSoup

url = "http://example.com"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
print(soup.title.string)
```

## 安装
安装 crewai_tools 包
```shell
pip install 'crewai[tools]'
```

## 示例
```python
from crewai_tools import ScrapeWebsiteTool

# 要启用在执行过程中找到的任何网站的抓取
tool = ScrapeWebsiteTool()

# 使用网站 URL 初始化工具，以便代理只能抓取指定网站的内容
tool = ScrapeWebsiteTool(website_url='https://www.example.com')

# 从网站提取文本
text = tool.run()
print(text)
```

## 参数
- `website_url` : 必填的网站 URL，用于读取文件。这是该工具的主要输入，指定应该抓取和读取哪个网站的内容。