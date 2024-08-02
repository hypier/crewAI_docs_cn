---
title: crewAI Training
description: Learn how to train your crewAI agents by providing early feedback for consistent results.
---

## 介绍
CrewAI中的训练功能允许您使用命令行界面（CLI）训练您的AI代理。通过运行命令`crewai train -n <n_iterations>`，您可以指定训练过程的迭代次数。

在训练过程中，CrewAI利用技术来优化代理的性能以及人类反馈。这有助于代理提高其理解、决策和解决问题的能力。

### 使用CLI培训您的团队
要使用培训功能，请按照以下步骤操作：

1. 打开终端或命令提示符。
2. 导航到您的CrewAI项目所在的目录。
3. 运行以下命令：

```shell
crewai train -n <n_iterations>
```

### 以编程方式训练您的团队
要以编程方式训练您的团队，请按照以下步骤操作：

1. 定义训练的迭代次数。
2. 指定训练过程的输入参数。
3. 在 try-except 块中执行训练命令，以处理潜在错误。

```python
    n_iterations = 2
    inputs = {"topic": "CrewAI Training"}

    try:
        YourCrewName_Crew().crew().train(n_iterations= n_iterations, inputs=inputs)

    except Exception as e:
        raise Exception(f"An error occurred while training the crew: {e}")
```

!!! note "将 `<n_iterations>` 替换为所需的训练迭代次数。这决定了代理将进行多少次训练过程。"

### 需要注意的要点：
- **正整数要求：** 确保迭代次数（`n_iterations`）是一个正整数。如果不满足此条件，代码将引发 `ValueError`。
- **错误处理：** 代码处理子进程错误和意外异常，为用户提供错误信息。

需要注意的是，训练过程可能需要一些时间，这取决于您代理的复杂性，并且还需要您在每次迭代中提供反馈。

一旦训练完成，您的代理将具备增强的能力和知识，能够处理复杂任务并提供更一致和有价值的见解。

请记得定期更新和重新训练您的代理，以确保它们与该领域的最新信息和进展保持同步。

祝您在 CrewAI 上训练愉快！