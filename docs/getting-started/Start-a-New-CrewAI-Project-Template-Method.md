---
title: Start a New CrewAI Project - Using a Template
description: A comprehensive guide on how to start a new CrewAI project, including the latest updates and how to set up your project.
---

# 开始您的 CrewAI 项目

欢迎来到启动新 CrewAI 项目的终极指南。本文件将引导您完成创建、定制和运行 CrewAI 项目的步骤，确保您拥有开始所需的一切。

在我们开始之前，有几点需要注意：

1. CrewAI 是一个 Python 包，运行需要 Python >=3.10 和 <=3.13。
2. 设置 CrewAI 的首选方式是使用 `crewai create` 命令。这将创建一个新的项目文件夹，并为您安装一个骨架模板以供您使用。

```
# 代码块示例
def hello_world():
    print("Hello, World!")
```

## 前提条件

在开始使用 CrewAI 之前，请确保您已经通过 pip 安装了它：

```shell
$ pip install crewai crewai-tools
```

### 虚拟环境
强烈建议您使用虚拟环境，以确保您的 CrewAI 项目与其他项目和依赖项隔离。虚拟环境为每个项目提供了一个干净、独立的工作区，防止不同版本的包和库之间发生冲突。这种隔离对于维护开发过程中的一致性和可重复性至关重要。根据您的操作系统和 Python 版本，您有多种设置虚拟环境的选项：

1. 使用 venv（Python 的内置虚拟环境工具）：
   venv 随 Python 3.3 及更高版本一起提供，是许多开发人员的便捷选择。它轻量且易于使用，非常适合简单的项目设置。

   要使用 venv 设置虚拟环境，请参考官方 [Python 文档](https://docs.python.org/3/tutorial/venv.html)。

2. 使用 Conda（Python 虚拟环境管理器）：
   Conda 是一个开源包管理器和环境管理系统，广泛用于数据科学家、开发人员和研究人员以可重复的方式管理依赖项和环境。

   要使用 Conda 设置虚拟环境，请参考官方 [Conda 文档](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html)。

3. 使用 Poetry（Python 包管理器和依赖管理工具）：
   Poetry 是一个开源 Python 包管理器，简化了包及其依赖项的安装。Poetry 提供了一种方便的方式来管理虚拟环境和依赖项。
   Poetry 是 CrewAI 在 CrewAI 中进行包/依赖管理的首选工具。

### 代码IDE

大多数CrewAI用户使用代码编辑器/集成开发环境（IDE）来构建他们的团队。您可以使用任何您选择的代码IDE。以下是一些流行的代码编辑器/集成开发环境（IDE）选项：

- [Visual Studio Code](https://code.visualstudio.com/) - 最受欢迎
- [PyCharm](https://www.jetbrains.com/pycharm/)
- [Cursor AI](https://cursor.com)

选择一个适合您风格和需求的。

## 创建新项目
在此示例中，我们将使用 Venv 作为我们的虚拟环境管理器。

要设置虚拟环境，请运行以下 CLI 命令：

```shell
$ python3 -m venv <venv-name>
```

通过运行以下 CLI 命令激活您的虚拟环境：

```shell
$ source <venv-name>/bin/activate
```

现在，要创建一个新的 CrewAI 项目，请运行以下 CLI 命令：

```shell
$ crewai create <project_name>
```

此命令将创建一个具有以下结构的新项目文件夹：

```shell
my_project/
├── .gitignore
├── pyproject.toml
├── README.md
└── src/
    └── my_project/
        ├── __init__.py
        ├── main.py
        ├── crew.py
        ├── tools/
        │   ├── custom_tool.py
        │   └── __init__.py
        └── config/
            ├── agents.yaml
            └── tasks.yaml
```

您现在可以通过编辑 `src/my_project` 文件夹中的文件来开始开发您的项目。`main.py` 文件是您项目的入口点，而 `crew.py` 文件是您定义代理和任务的地方。

## 自定义您的项目

要自定义您的项目，您可以：
- 修改 `src/my_project/config/agents.yaml` 来定义您的代理。
- 修改 `src/my_project/config/tasks.yaml` 来定义您的任务。
- 修改 `src/my_project/crew.py` 来添加您自己的逻辑、工具和特定参数。
- 修改 `src/my_project/main.py` 来为您的代理和任务添加自定义输入。
- 将您的环境变量添加到 `.env` 文件中。

### 示例：定义代理和任务

#### agents.yaml

```yaml
researcher:
  role: >
    职位候选人研究员
  goal: >
    寻找该职位的潜在候选人
  backstory: >
    你擅长通过探索各种在线资源找到合适的候选人。你识别合适候选人的能力确保了职位的最佳匹配。
```

#### tasks.yaml

```yaml
research_candidates_task:
  description: >
    进行全面的研究，以寻找指定职位的潜在候选人。
    利用各种在线资源和数据库，收集潜在候选人的综合名单。
    确保候选人符合提供的职位要求。

    职位要求：
    {job_requirements}
  expected_output: >
    一份包含10名潜在候选人及其联系信息和简要个人资料的名单，突出他们的适合性。
  agent: researcher # 这需要与agents.yaml文件中的代理名称和Crew.py文件中定义的代理匹配
  context: # 这些需要与上面定义的任务名称以及tasks.yaml文件和Crew.py文件中定义的任务匹配
    - researcher
```

### 变量引用：
您定义的同名函数将被使用。例如，您可以从 task.yaml 文件中引用特定任务的代理。确保您的注释代理和函数名称相同，否则您的任务将无法正确识别引用。

#### 示例引用
agent.yaml
```yaml
email_summarizer:
    role: >
      Email Summarizer
    goal: >
      Summarize emails into a concise and clear summary
    backstory: >
      You will create a 5 bullet point summary of the report
    llm: mixtal_llm
```

task.yaml
```yaml
email_summarizer_task:
    description: >
      Summarize the email into a 5 bullet point summary
    expected_output: >
      A 5 bullet point summary of the email
    agent: email_summarizer
    context:
      - reporting_task
      - research_task
```

使用注释来正确引用 crew.py 文件中的代理和任务。

### 注释包括：
* @agent
* @task
* @crew
* @llm
* @tool
* @callback
* @output_json
* @output_pydantic
* @cache_handler


crew.py
```py
...
    @llm
    def mixtal_llm(self):
        return ChatGroq(temperature=0, model_name="mixtral-8x7b-32768")

    @agent
    def email_summarizer(self) -> Agent:
        return Agent(
            config=self.agents_config["email_summarizer"],
        )
    ## ...其他任务定义
    @task
    def email_summarizer_task(self) -> Task:
        return Task(
            config=self.tasks_config["email_summarizer_task"],
        )
...
```

## 安装依赖

要为您的项目安装依赖，您可以使用 Poetry。首先，导航到您的项目目录：

```shell
$ cd my_project
$ poetry lock
$ poetry install
```

这将安装在 `pyproject.toml` 文件中指定的依赖。

## 插值变量

在您的 `agents.yaml` 和 `tasks.yaml` 文件中，任何插值的变量如 `{variable}` 将被 `main.py` 文件中变量的值替换。

#### agents.yaml

```yaml
research_task:
  description: >
    在 {customer_domain} 的背景下，对客户和竞争对手进行全面研究。
    确保您找到任何有趣和相关的信息，因为当前年份是 2024。
  expected_output: >
    关于客户及其客户和竞争对手的完整报告，
    包括他们的人口统计、偏好、市场定位和受众参与情况。
```

#### main.py

```python
# main.py
def run():
    inputs = {
        "customer_domain": "crewai.com"
    }
    MyProjectCrew(inputs).crew().kickoff(inputs=inputs)
```

## 运行您的项目

要运行您的项目，请使用以下命令：

```shell
$ poetry run my_project
```

这将初始化您的 AI 代理团队，并开始根据您在 `main.py` 文件中的配置定义的任务执行。

## 部署您的项目

部署您的团队最简单的方法是通过 [CrewAI+](https://www.crewai.com/crewaiplus)，您可以通过几次点击来部署您的团队。
