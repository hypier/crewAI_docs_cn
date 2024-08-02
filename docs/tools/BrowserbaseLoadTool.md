# BrowserbaseLoadTool

## 描述

[Browserbase](https://browserbase.com) 是一个开发者平台，用于可靠地运行、管理和监控无头浏览器。

通过以下方式为您的 AI 数据检索提供动力：
 - [无服务器基础设施](https://docs.browserbase.com/under-the-hood) 提供可靠的浏览器，从复杂的用户界面中提取数据
 - [隐身模式](https://docs.browserbase.com/features/stealth-mode) 包含指纹识别策略和自动验证码解决
 - [会话调试器](https://docs.browserbase.com/features/sessions) 以网络时间线和日志检查您的浏览器会话
 - [实时调试](https://docs.browserbase.com/guides/session-debug-connection/browser-remote-control) 快速调试您的自动化

## 安装

- 从 [browserbase.com](https://browserbase.com) 获取 API 密钥和项目 ID，并将其设置为环境变量（`BROWSERBASE_API_KEY`，`BROWSERBASE_PROJECT_ID`）。
- 安装 [Browserbase SDK](http://github.com/browserbase/python-sdk) 以及 `crewai[tools]` 包：

```
pip install browserbase 'crewai[tools]'
```

## 示例

使用 BrowserbaseLoadTool 如下所示，以允许您的代理加载网站：

```python
from crewai_tools import BrowserbaseLoadTool

tool = BrowserbaseLoadTool()
```

## 参数

- `api_key` 可选。Browserbase API 密钥。默认值为 `BROWSERBASE_API_KEY` 环境变量。
- `project_id` 可选。Browserbase 项目 ID。默认值为 `BROWSERBASE_PROJECT_ID` 环境变量。
- `text_content` 仅检索文本内容。默认值为 `False`。
- `session_id` 可选。提供现有的会话 ID。
- `proxy` 可选。启用/禁用代理。