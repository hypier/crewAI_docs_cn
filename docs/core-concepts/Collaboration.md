---
title: Agents在CrewAI中的协作方式
description: 探讨在CrewAI框架内代理协作的动态，重点关注新集成的功能以增强功能性。
---

## 协作基础
!!! note "代理人互动的核心"
    在CrewAI中，协作是基础，使代理人能够结合他们的技能、共享信息，并在任务执行中相互协助，体现了一个真正合作的生态系统。

- **信息共享**：确保所有代理人都能够充分了解情况，并通过共享数据和发现有效地贡献。
- **任务协助**：允许代理人向具备特定任务所需专业知识的同事寻求帮助。
- **资源分配**：通过在代理人之间有效分配和共享资源，优化任务执行。

```
# Code block example
def example_function():
    print("This is an example.")
```

## 增强属性以改善协作
`Crew` 类已增加多个属性以支持高级功能：

- **语言模型管理 (`manager_llm`, `function_calling_llm`)**：管理用于执行任务和工具的语言模型，促进复杂的代理与工具交互。请注意，`manager_llm` 是确保层级过程正确执行流的必需项，而 `function_calling_llm` 是可选的，默认值旨在简化工具交互。
- **自定义管理代理 (`manager_agent`)**：允许指定自定义代理作为管理者，而不是使用 CrewAI 提供的默认管理者。
- **过程流程 (`process`)**：定义执行逻辑（例如，顺序、层级），以简化任务分配和执行。
- **详细日志记录 (`verbose`)**：提供详细的日志记录功能，以便于监控和调试。它支持整数和布尔类型以指示详细程度。例如，将 `verbose` 设置为 1 可能启用基本日志记录，而将其设置为 True 则启用更详细的日志。
- **速率限制 (`max_rpm`)**：通过限制每分钟请求数来确保资源的有效利用。设置 `max_rpm` 的指导方针应考虑任务的复杂性和资源的预期负载。
- **国际化/自定义支持 (`language`, `prompt_file`)**：促进内部提示的完全自定义，增强全球可用性。支持的语言和利用 `prompt_file` 属性进行自定义的过程应清楚记录。[文件示例](https://github.com/joaomdmoura/crewAI/blob/main/src/crewai/translations/en.json)
- **执行和输出处理 (`full_output`)**：区分完整输出和最终输出，以便对任务结果进行细致控制。展示输出差异的示例可以帮助理解该属性的实际影响。
- **回调和遥测 (`step_callback`, `task_callback`)**：集成逐步和任务级执行监控的回调，以及用于性能分析的遥测。应清楚解释 `task_callback` 与 `step_callback` 的目的和用法，以便进行细致监控。
- **Crew 共享 (`share_crew`)**：允许与 CrewAI 共享团队信息，以便进行持续改进和训练模型。应概述该功能的隐私影响和好处，包括它如何促进模型改进。
- **使用指标 (`usage_metrics`)**：存储在所有任务执行期间语言模型（LLM）使用的所有指标，提供有关运营效率和改进领域的见解。应提供有关如何访问和解释这些指标以进行性能分析的详细信息。
- **内存使用 (`memory`)**：指示团队是否应使用内存来存储其执行的记忆，以增强任务执行和代理学习。
- **嵌入配置 (`embedder`)**：指定团队用于理解和生成语言的嵌入器配置。该属性支持语言模型提供者的自定义。
- **缓存管理 (`cache`)**：确定团队是否应使用缓存来存储工具执行的结果，以优化性能。
- **输出日志记录 (`output_log_file`)**：指定记录团队执行输出的文件路径。

## Delegation: Divide and Conquer
Delegation enhances functionality and improves the overall capability of the team by allowing agents to intelligently distribute tasks or seek assistance.

## Implementation Collaboration and Delegation
建立一个团队涉及定义每个代理的角色和能力。CrewAI 无缝管理他们的互动，确保高效的协作和委派，并提供增强的定制和监控功能，以适应各种操作需求。

## 示例场景
考虑一个团队，其中有一个负责数据收集的研究员代理和一个负责编写报告的作家代理。先进的语言模型管理和流程属性的整合使得更复杂的互动成为可能，例如作家将复杂的研究任务委派给研究员或查询特定信息，从而促进无缝的工作流程。 

```
def example_function():
    print("This is an example function.")
```

## Conclusion
Integrating advanced attributes and functionalities into the CrewAI framework significantly enriches the agent collaboration ecosystem. These enhancements not only simplify interactions but also provide unprecedented flexibility and control, paving the way for advanced AI-driven solutions capable of handling complex tasks through intelligent collaboration and delegation.