---
title: 安装 crewAI
description: 安装 crewAI 及其依赖项的综合指南，包括最新更新和安装方法。
---

# 安装 crewAI

欢迎使用 crewAI！本指南将引导您完成 crewAI 及其依赖项的安装过程。crewAI 是一个灵活且强大的 AI 框架，使您能够高效地创建和管理 AI 代理、工具和任务。让我们开始吧！

## 安装

要安装 crewAI，您需要在系统上安装 Python >=3.10 和 <=3.13：

```shell
# 安装主要的 crewAI 包
pip install crewai

# 安装主要的 crewAI 包和工具包
# 包含一系列对您的代理有帮助的工具
pip install 'crewai[tools]'

# 或者，您也可以使用：
pip install crewai crewai-tools
```