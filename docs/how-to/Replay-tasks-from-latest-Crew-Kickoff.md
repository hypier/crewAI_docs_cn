---
title: 从最新的 Crew Kickoff 重放任务
description: 从最新的 crew.kickoff(...) 重放任务
---

## 介绍
CrewAI 提供从最新的团队启动中指定任务进行重放的功能。此功能在您完成启动后特别有用，当您可能想要重试某些任务或不需要重新获取数据时，您的代理已经保存了启动执行的上下文，因此您只需重放您想要的任务。

```
# Sample code
def replay_task(task_id):
    # Logic to replay the task
    pass
```

## 注意：
在您可以重放任务之前，必须运行 `crew.kickoff()`。目前，仅支持最新的 kickoff，因此如果您使用 `kickoff_for_each`，它将仅允许您从最近的 crew 运行中重放。

以下是如何从任务中重放的示例：

### 从特定任务回放使用 CLI
要使用回放功能，请按照以下步骤操作：

1. 打开您的终端或命令提示符。
2. 导航到您的 CrewAI 项目所在的目录。
3. 运行以下命令：

要查看最新的 kickoff task_ids，请使用：
```shell
crewai log-tasks-outputs
```

一旦您有了要回放的 task_id，请使用：
```shell
crewai replay -t <task_id>
```

### 从任务中以编程方式重放
要以编程方式从任务中重放，请按照以下步骤操作：

1. 指定 task_id 和重放过程的输入参数。
2. 在 try-except 块中执行重放命令，以处理潜在的错误。

```python
   def replay():
    """
    Replay the crew execution from a specific task.
    """
    task_id = '<task_id>'
    inputs = {"topic": "CrewAI Training"} # this is optional, you can pass in the inputs you want to replay otherwise uses the previous kickoffs inputs
    try:
        YourCrewName_Crew().crew().replay(task_id=task_id, inputs=inputs)

    except Exception as e:
        raise Exception(f"An error occurred while replaying the crew: {e}")