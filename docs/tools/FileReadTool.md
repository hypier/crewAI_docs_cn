# FileReadTool

!!! note "实验性"
    我们仍在努力改进工具，因此将来可能会出现意外行为或变化。

## 描述
FileReadTool 概念上代表了 crewai_tools 包中的一套功能，旨在促进文件读取和内容检索。该套件包括处理批量文本文件、读取运行时配置文件和导入用于分析的数据的工具。它支持多种基于文本的文件格式，如 `.txt`、`.csv`、`.json` 等。根据文件类型，该套件提供了专门的功能，例如将 JSON 内容转换为 Python 字典以便于使用。

## 安装
要使用之前归因于 FileReadTool 的功能，请安装 crewai_tools 包：

```shell
pip install 'crewai[tools]'
```

## 使用示例
要开始使用 FileReadTool：

```python
from crewai_tools import FileReadTool

# Initialize the tool to read any files the agents knows or lean the path for
file_read_tool = FileReadTool()

# OR

# Initialize the tool with a specific file path, so the agent can only read the content of the specified file
file_read_tool = FileReadTool(file_path='path/to/your/file.txt')
```

## 参数
- `file_path`: 要读取的文件路径。它接受绝对路径和相对路径。确保文件存在，并且您拥有访问该文件的必要权限。