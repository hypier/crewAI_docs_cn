---
title: 使用 AgentOps 进行代理监控
description: 理解并记录您的代理性能，使用 AgentOps。
---

# 介绍
可观察性是开发和部署对话式人工智能代理的关键方面。它使开发人员能够了解他们的代理表现如何、与用户的互动情况以及代理如何使用外部工具和API。AgentOps是一个独立于CrewAI的产品，提供全面的代理可观察性解决方案。 

```
# This is a code block
print("Hello, World!")
```

## AgentOps

[AgentOps](https://agentops.ai/?=crew) 提供会话重播、指标和代理监控。

从高层次来看，AgentOps 使您能够监控成本、令牌使用、延迟、代理失败、会话范围统计等更多信息。有关更多信息，请查看 [AgentOps Repo](https://github.com/AgentOps-AI/agentops).

```
# Code block example
def example_function():
    return "This is a code block"
```

### 概述
AgentOps 提供对开发和生产中的代理进行监控。它提供了一个仪表板，用于跟踪代理性能、会话回放和自定义报告。

此外，AgentOps 还提供会话深入分析，以实时查看 Crew 代理交互、LLM 调用和工具使用情况。此功能对于调试和理解代理如何与用户以及其他代理进行交互非常有用。

![选定代理会话运行的概述](..%2Fassets%2Fagentops-overview.png)
![检查代理运行的会话深入分析概述](..%2Fassets%2Fagentops-session.png)
![查看逐步代理回放执行图](..%2Fassets%2Fagentops-replay.png)

### 特性
- **LLM 成本管理与追踪**：跟踪与基础模型提供商的支出。
- **重放分析**：查看逐步的代理执行图。
- **递归思维检测**：识别代理何时陷入无限循环。
- **自定义报告**：创建代理性能的自定义分析。
- **分析仪表板**：监控开发和生产中代理的高层统计信息。
- **公共模型测试**：根据基准和排行榜测试您的代理。
- **自定义测试**：针对特定领域的测试运行您的代理。
- **时间旅行调试**：从检查点重新启动您的会话。
- **合规性与安全性**：创建审计日志并检测潜在威胁，如粗俗语言和个人信息泄露。
- **提示注入检测**：识别潜在的代码注入和秘密泄露。

### 使用 AgentOps

1. **创建 API 密钥：**
   在这里创建用户 API 密钥：[创建 API 密钥](app.agentops.ai/account)

2. **配置您的环境：**
   将您的 API 密钥添加到环境变量中

   ```bash
   AGENTOPS_API_KEY=<YOUR_AGENTOPS_API_KEY>
   ```

3. **安装 AgentOps：**
   使用以下命令安装 AgentOps：
   ```bash
   pip install crewai[agentops]
   ```
   或者
   ```bash
   pip install agentops
   ```

   在您的脚本中使用 `Crew` 之前，请包含以下行：

   ```python
   import agentops
   agentops.init()
   ```

   这将启动一个 AgentOps 会话，并自动跟踪 Crew 代理。有关如何配置更复杂的代理系统的更多信息，请查看 [AgentOps 文档](https://docs.agentops.ai) 或加入 [Discord](https://discord.gg/j4f3KbeH)。

### Crew + AgentOps 示例
- [职位发布](https://github.com/joaomdmoura/crewAI-examples/tree/main/job-posting)
- [Markdown 验证器](https://github.com/joaomdmoura/crewAI-examples/tree/main/markdown_validator)
- [Instagram 帖子](https://github.com/joaomdmoura/crewAI-examples/tree/main/instagram_post)

### 进一步信息

要开始，请创建一个 [AgentOps 账户](https://agentops.ai/?=crew)。

如需功能请求或错误报告，请通过 [AgentOps Repo](https://github.com/AgentOps-AI/agentops) 联系 AgentOps 团队。

#### 额外链接

<a href="https://twitter.com/agentopsai/">🐦 Twitter</a>
<span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
<a href="https://discord.gg/JHPt4C7r">📢 Discord</a>
<span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
<a href="https://app.agentops.ai/?=crew">🖇️ AgentOps 控制面板</a>
<span>&nbsp;&nbsp;•&nbsp;&nbsp;</span>
<a href="https://docs.agentops.ai/introduction">📙 文档</a>