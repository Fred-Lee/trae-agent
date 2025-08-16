# 工具

Trae Agent 提供了五个用于软件工程任务的内置工具：

## str_replace_based_edit_tool

具有持久状态的文件和目录操作工具。

**操作：**
- `view` - 显示文件内容（带行号），或列出目录内容（最多2层深度）
- `create` - 创建新文件（如果文件已存在则失败）
- `str_replace` - 替换文件中的精确字符串匹配（必须是唯一的）
- `insert` - 在指定行号后插入文本

**主要特性：**
- 需要绝对路径（例如 `/repo/file.py`）
- 字符串替换必须完全匹配，包括空格
- 支持大文件的行范围查看

## bash

在持久会话中执行 shell 命令。

**特性：**
- 命令在共享的 bash 会话中运行，保持状态
- 每个命令120秒超时
- 会话重启能力
- 后台进程支持

**使用说明：**
- 使用 `restart: true` 重置会话
- 避免使用输出过多的命令
- 长时间运行的命令应使用 `&` 进行后台执行

## sequential_thinking

用于复杂分析的结构化问题解决工具。

**功能：**
- 将问题分解为顺序思考步骤
- 从之前的想法中修改和分支
- 动态调整所需的想法数量
- 跟踪思考历史和替代方法
- 生成和验证解决方案假设

**参数：**
- `thought` - 当前思考步骤
- `thought_number` / `total_thoughts` - 进度跟踪
- `next_thought_needed` - 继续思考标志
- `is_revision` / `revises_thought` - 修订跟踪
- `branch_from_thought` / `branch_id` - 替代探索

## task_done

用验证要求标记任务完成。

**目的：**
- 将任务标记为成功完成
- 仅在适当验证后调用
- 鼓励编写测试/重现脚本

**输出：**
- 简单的 "Task done." 消息
- 不需要参数

## json_edit_tool

使用 JSONPath 表达式进行精确的 JSON 文件编辑。

**操作：**
- `view` - 显示整个文件或特定 JSONPath 的内容
- `set` - 更新指定路径处的现有值
- `add` - 向对象添加新属性或追加到数组
- `remove` - 删除指定路径处的元素

**JSONPath 示例：**
- `$.users[0].name` - 第一个用户的名称
- `$.config.database.host` - 嵌套对象属性
- `$.items[*].price` - 所有项目价格
- `$..key` - 递归搜索键

**特性：**
- 验证 JSON 语法和结构
- 使用美观打印选项保留格式
- 无效操作的详细错误消息