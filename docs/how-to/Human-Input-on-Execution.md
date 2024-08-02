---
title: 人工干预执行
description: 在复杂决策过程中，将CrewAI与人工输入结合，充分利用代理的属性和工具的全部能力。
---

# 代理执行中的人工输入

人工输入在多个代理执行场景中至关重要，使代理能够在必要时请求额外的信息或澄清。这一功能在复杂的决策过程中尤其有用，或者当代理需要更多细节以有效完成任务时。

```
# 示例代码
def agent_execution(input_data):
    # 处理输入数据
    if input_data.requires_clarification:
        request_additional_info()
    # 继续执行
```

## 使用 Human Input 与 CrewAI

要将人类输入集成到代理执行中，请在任务定义中设置 `human_input` 标志。启用后，代理会在给出最终答案之前提示用户输入。这些输入可以提供额外的上下文、澄清模糊之处或验证代理的输出。

```
# 示例代码
def example_function():
    pass
```

### 示例：

```shell
pip install crewai
```

```python
import os
from crewai import Agent, Task, Crew
from crewai_tools import SerperDevTool

os.environ["SERPER_API_KEY"] = "Your Key"  # serper.dev API key
os.environ["OPENAI_API_KEY"] = "Your Key"

# 加载工具
search_tool = SerperDevTool()

# 定义您的代理，包含角色、目标、工具和附加属性
researcher = Agent(
    role='高级研究分析师',
    goal='揭示人工智能和数据科学的前沿发展',
    backstory=(
        "您是一家领先科技智库的高级研究分析师。"
        "您的专长在于识别人工智能和数据科学中的新兴趋势和技术。"
        "您擅长剖析复杂数据并提供可行的见解。"
    ),
    verbose=True,
    allow_delegation=False,
    tools=[search_tool]
)
writer = Agent(
    role='技术内容策略师',
    goal='撰写关于技术进步的引人入胜的内容',
    backstory=(
        "您是一位知名的技术内容策略师，以对技术和创新的深刻见解和引人入胜的文章而闻名。"
        "凭借对科技行业的深刻理解，您将复杂概念转化为引人入胜的叙述。"
    ),
    verbose=True,
    allow_delegation=True,
    tools=[search_tool],
    cache=False,  # 禁用此代理的缓存
)

# 为您的代理创建任务
task1 = Task(
    description=(
        "对2024年人工智能的最新进展进行全面分析。"
        "识别关键趋势、突破性技术和潜在行业影响。"
        "将您的发现汇编成详细报告。"
        "确保在最终答案之前与人类确认草稿是否良好。"
    ),
    expected_output='关于2024年最新人工智能进展的全面完整报告，毫无遗漏',
    agent=researcher,
    human_input=True
)

task2 = Task(
    description=(
        "根据研究者的报告中的见解，撰写一篇引人入胜的博客文章，突出最重要的人工智能进展。"
        "您的文章应该信息丰富且易于理解，面向技术精通的受众。"
        "力争呈现出这些突破的本质及其对未来的影响的叙述。"
    ),
    expected_output='一篇关于2024年最新人工智能进展的引人入胜的3段博客文章，格式为markdown',
    agent=writer
)

# 通过顺序流程实例化您的团队
crew = Crew(
    agents=[researcher, writer],
    tasks=[task1, task2],
    verbose=2,
    memory=True,
)

# 让您的团队开始工作！
result = crew.kickoff()

print("######################")
print(result)
```