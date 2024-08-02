# EXASearchTool 文档

## 描述

EXASearchTool旨在对互联网文本内容进行指定查询的语义搜索。它利用[exa.ai](https://exa.ai/) API根据用户提供的查询获取并显示最相关的搜索结果。

```
# 示例代码
def search(query):
    results = exa_api.search(query)
    return results
```

## 安装

要将此工具集成到您的项目中，请按照以下安装说明操作：

```shell
pip install 'crewai[tools]'
```

## 示例

以下示例演示如何初始化工具并使用给定查询执行搜索：

```python
from crewai_tools import EXASearchTool

# Initialize the tool for internet searching capabilities
tool = EXASearchTool()
```

## 开始的步骤

要有效使用 EXASearchTool，请遵循以下步骤：

1. **软件包安装**：确认在您的 Python 环境中已安装 `crewai[tools]` 软件包。
2. **API 密钥获取**：通过在 [exa.ai](https://exa.ai/) 注册一个免费账户来获取 [exa.ai](https://exa.ai/) API 密钥。
3. **环境配置**：将获取的 API 密钥存储在名为 `EXA_API_KEY` 的环境变量中，以便工具使用。

```
# 示例代码
import os

os.environ['EXA_API_KEY'] = 'your_api_key_here'
```

## Conclusion

By integrating EXASearchTool into a Python project, users can perform real-time, relevant internet searches directly from their applications. Following the provided setup and usage guidelines makes incorporating the tool into the project simple and straightforward.