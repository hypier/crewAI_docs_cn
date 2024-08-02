---
title: crewAI 计划
description: 了解如何为您的 crewAI 团队添加计划并提高他们的表现。
---

## 介绍
CrewAI 中的规划功能允许您为您的团队添加规划能力。当启用时，在每次 Crew 迭代之前，所有 Crew 信息将被发送到 AgentPlanner，AgentPlanner 将逐步规划任务，并将该计划添加到每个任务描述中。

### 使用规划功能
开始使用规划功能非常简单，唯一需要的步骤是将 `planning=True` 添加到您的 Crew 中：

```python
from crewai import Crew, Agent, Task, Process

# Assemble your crew with planning capabilities
my_crew = Crew(
    agents=self.agents,
    tasks=self.tasks,
    process=Process.sequential,
    planning=True,
)
```

从这一点开始，您的团队将启用规划，并且任务将在每次迭代之前进行规划。

#### 规划 LLM

现在您可以定义将用于规划任务的 LLM。您可以使用任何可用的 ChatOpenAI LLM 模型。

```python
from crewai import Crew, Agent, Task, Process
from langchain_openai import ChatOpenAI

# Assemble your crew with planning capabilities and custom LLM
my_crew = Crew(
    agents=self.agents,
    tasks=self.tasks,
    process=Process.sequential,
    planning=True,
    planning_llm=ChatOpenAI(model="gpt-4o")
)
```

### 示例

在运行基本案例示例时，您将看到如下输出，这代表负责创建逐步逻辑以添加到代理任务中的AgentPlanner的输出。

```bash

[2024-07-15 16:49:11][INFO]: Planning the crew execution
**Step-by-Step Plan for Task Execution**

**Task Number 1: Conduct a thorough research about AI LLMs**

**Agent:** AI LLMs Senior Data Researcher

**Agent Goal:** Uncover cutting-edge developments in AI LLMs

**Task Expected Output:** A list with 10 bullet points of the most relevant information about AI LLMs

**Task Tools:** None specified

**Agent Tools:** None specified

**Step-by-Step Plan:**

1. **Define Research Scope:**
   - 确定要关注的AI LLMs的具体领域，例如架构的进展、应用案例、伦理考虑和性能指标。

2. **Identify Reliable Sources:**
   - 列出可信的AI研究来源，包括学术期刊、行业报告、会议（例如NeurIPS、ACL）、AI研究实验室（例如OpenAI、Google AI）和在线数据库（例如IEEE Xplore、arXiv）。

3. **Collect Data:**
   - 搜索2023年和2024年初发布的最新论文、文章和报告。
   - 使用关键词如“Large Language Models 2024”、“AI LLM advancements”、“AI ethics 2024”等。

4. **Analyze Findings:**
   - 阅读并总结每个来源的关键点。
   - 突出过去一年中引入的新技术、模型和应用。

5. **Organize Information:**
   - 将信息分类为相关主题（例如新架构、伦理影响、实际应用）。
   - 确保每个要点简洁但信息丰富。

6. **Create the List:**
   - 将10个最相关的信息汇编成一个要点列表。
   - 审查列表以确保清晰和相关性。

**Expected Output:**
A list with 10 bullet points of the most relevant information about AI LLMs.

---

**Task Number 2: Review the context you got and expand each topic into a full section for a report**

**Agent:** AI LLMs Reporting Analyst

**Agent Goal:** Create detailed reports based on AI LLMs data analysis and research findings

**Task Expected Output:** A fully fledge report with the main topics, each with a full section of information. Formatted as markdown without '```'

**Task Tools:** None specified

**Agent Tools:** None specified

**Step-by-Step Plan:**

1. **Review the Bullet Points:**
   - 仔细阅读AI LLMs高级数据研究员提供的10个要点列表。

2. **Outline the Report:**
   - 创建一个大纲，以每个要点作为主要章节标题。
   - 计划每个主要标题下的子章节，以涵盖主题的不同方面。

3. **Research Further Details:**
   - 对于每个要点，如有必要进行进一步研究，以收集更详细的信息。
   - 寻找案例研究、示例和统计数据以支持每个章节。

4. **Write Detailed Sections:**
   - 将每个要点扩展为一个全面的章节。
   - 确保每个章节包含介绍、详细说明、示例和结论。
   - 使用markdown格式化标题、副标题、列表和强调。

5. **Review and Edit:**
   - 校对报告以确保清晰、一致和正确。
   - 确保报告逻辑上从一个章节流畅过渡到下一个章节。
   - 根据markdown标准格式化报告。

6. **Finalize the Report:**
   - 确保报告完整，所有章节均已扩展和详细化。
   - 再次检查格式并进行必要的调整。

**Expected Output:**
A fully-fledged report with the main topics, each with a full section of information. Formatted as markdown without '```'.

---
```