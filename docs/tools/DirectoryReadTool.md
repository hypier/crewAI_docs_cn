```markdown
# DirectoryReadTool

!!! note "实验性"
    我们仍在致力于改进工具，因此未来可能会出现意外的行为或变化。

## 描述
DirectoryReadTool 是一个强大的工具，旨在提供目录内容的全面列表。它可以递归地遍历指定目录，为用户提供所有文件的详细枚举，包括子目录中的文件。该工具对于需要全面清点目录结构或验证目录中文件组织的任务至关重要。

## 安装
要在您的项目中使用 DirectoryReadTool，请安装 `crewai_tools` 包。如果该包尚未在您的环境中，您可以使用以下命令通过 pip 安装它：

```shell
pip install 'crewai[tools]'
```

此命令安装 `crewai_tools` 包的最新版本，提供对 DirectoryReadTool 及其他工具的访问。

## 示例
使用 DirectoryReadTool 非常简单。以下代码片段演示了如何设置它并使用该工具列出指定目录的内容：

```python
from crewai_tools import DirectoryReadTool
```

# 初始化工具，以便代理可以读取在执行过程中了解到的任何目录的内容
tool = DirectoryReadTool()

# 或者

# 使用特定目录初始化工具，以便代理只能读取指定目录的内容
tool = DirectoryReadTool(directory='/path/to/your/directory')
```

## 参数
DirectoryReadTool 的使用只需最少的配置。该工具的基本参数如下：

- `directory`: **可选**。一个指定您希望列出其内容的目录路径的参数。它接受绝对路径和相对路径，引导工具到所需的目录以列出内容。