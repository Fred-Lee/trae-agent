# Trae Agent

[![arXiv:2507.23370](https://img.shields.io/badge/TechReport-arXiv%3A2507.23370-b31a1b)](https://arxiv.org/abs/2507.23370)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Pre-commit](https://github.com/bytedance/trae-agent/actions/workflows/pre-commit.yml/badge.svg)](https://github.com/bytedance/trae-agent/actions/workflows/pre-commit.yml)
[![Unit Tests](https://github.com/bytedance/trae-agent/actions/workflows/unit-test.yml/badge.svg)](https://github.com/bytedance/trae-agent/actions/workflows/unit-test.yml)
[![Discord](https://img.shields.io/discord/1320998163615846420?label=Join%20Discord&color=7289DA)](https://discord.gg/VwaQ4ZBHvC)

**Trae Agent** 是一个基于大语言模型的智能体，专门用于通用软件工程任务。它提供了强大的命令行界面，能够理解自然语言指令并使用各种工具和大语言模型提供商执行复杂的软件工程工作流。

技术细节请参考[我们的技术报告](https://arxiv.org/abs/2507.23370)。

**项目状态：** 该项目仍在积极开发中。如果您愿意帮助我们改进 Trae Agent，请参考 [docs/roadmap.md](docs/roadmap.md) 和 [CONTRIBUTING](CONTRIBUTING.md)。

**与其他CLI智能体的区别：** Trae Agent 提供了透明、模块化的架构，研究人员和开发者可以轻松修改、扩展和分析，使其成为**研究AI智能体架构、进行消融研究和开发新颖智能体能力**的理想平台。这种**_研究友好的设计_**使学术界和开源社区能够为基础智能体框架做出贡献并在此基础上构建，促进AI智能体快速发展领域的创新。

## ✨ 特性

- 🌊 **Lakeview**: 为智能体步骤提供简短而简洁的摘要
- 🤖 **多LLM支持**: 支持 OpenAI、Anthropic、豆包、Azure、OpenRouter、Ollama 和 Google Gemini API
- 🛠️ **丰富的工具生态系统**: 文件编辑、bash执行、顺序思考等
- 🎯 **交互模式**: 用于迭代开发的对话界面
- 📊 **轨迹记录**: 详细记录所有智能体操作，用于调试和分析
- ⚙️ **灵活配置**: 基于YAML的配置，支持环境变量
- 🚀 **简易安装**: 基于pip的简单安装

## 🚀 安装

### 要求
- UV (https://docs.astral.sh/uv/)
- 您选择的提供商的API密钥（OpenAI、Anthropic、Google Gemini、OpenRouter等）

### 设置

```bash
git clone https://github.com/bytedance/trae-agent.git
cd trae-agent
uv sync --all-extras
source .venv/bin/activate
```

## ⚙️ 配置

### YAML配置（推荐）

1. 复制示例配置文件：
   ```bash
   cp trae_config.yaml.example trae_config.yaml
   ```

2. 使用您的API凭据和偏好设置编辑 `trae_config.yaml`：

```yaml
agents:
  trae_agent:
    enable_lakeview: true
    model: trae_agent_model  # Trae Agent的模型配置名称
    max_steps: 200  # 智能体步骤的最大数量
    tools:  # 与Trae Agent一起使用的工具
      - bash
      - str_replace_based_edit_tool
      - sequentialthinking
      - task_done

model_providers:  # 模型提供商配置
  anthropic:
    api_key: your_anthropic_api_key
    provider: anthropic
  openai:
    api_key: your_openai_api_key
    provider: openai

models:
  trae_agent_model:
    model_provider: anthropic
    model: claude-sonnet-4-20250514
    max_tokens: 4096
    temperature: 0.5
```

**注意：** `trae_config.yaml` 文件被git忽略以保护您的API密钥。

### 环境变量（替代方案）

您也可以使用环境变量配置API密钥并将它们存储在.env文件中：

```bash
export OPENAI_API_KEY="your-openai-api-key"
export ANTHROPIC_API_KEY="your-anthropic-api-key"
export GOOGLE_API_KEY="your-google-api-key"
export OPENROUTER_API_KEY="your-openrouter-api-key"
export DOUBAO_API_KEY="your-doubao-api-key"
export DOUBAO_BASE_URL="https://ark.cn-beijing.volces.com/api/v3/"
```

### MCP服务（可选）

要启用模型上下文协议（MCP）服务，请在配置中添加 `mcp_servers` 部分：

```yaml
mcp_servers:
  playwright:
    command: npx
    args:
      - "@playwright/mcp@0.0.27"
```

**配置优先级：** 命令行参数 > 配置文件 > 环境变量 > 默认值

**旧版JSON配置：** 如果使用较旧的JSON格式，请参见 [docs/legacy_config.md](docs/legacy_config.md)。我们建议迁移到YAML。

## 📖 使用方法

### 基本命令

```bash
# 简单任务执行
trae-cli run "创建一个hello world Python脚本"

# 检查配置
trae-cli show-config

# 交互模式
trae-cli interactive
```

### 特定提供商示例

```bash
# OpenAI
trae-cli run "修复main.py中的bug" --provider openai --model gpt-4o

# Anthropic
trae-cli run "添加单元测试" --provider anthropic --model claude-sonnet-4-20250514

# Google Gemini
trae-cli run "优化这个算法" --provider google --model gemini-2.5-flash

# OpenRouter（访问多个提供商）
trae-cli run "审查这段代码" --provider openrouter --model "anthropic/claude-3-5-sonnet"
trae-cli run "生成文档" --provider openrouter --model "openai/gpt-4o"

# 豆包
trae-cli run "重构数据库模块" --provider doubao --model doubao-seed-1.6

# Ollama（本地模型）
trae-cli run "为这段代码添加注释" --provider ollama --model qwen3
```

### 高级选项

```bash
# 自定义工作目录
trae-cli run "为utils模块添加测试" --working-dir /path/to/project

# 保存执行轨迹
trae-cli run "调试身份验证" --trajectory-file debug_session.json

# 强制生成补丁
trae-cli run "更新API端点" --must-patch

# 带自定义设置的交互模式
trae-cli interactive --provider openai --model gpt-4o --max-steps 30
```

### 交互模式命令

在交互模式中，您可以使用：
- 输入任何任务描述来执行它
- `status` - 显示智能体信息
- `help` - 显示可用命令
- `clear` - 清屏
- `exit` 或 `quit` - 结束会话

## 🛠️ 高级功能

### 可用工具

Trae Agent 为软件工程任务提供了全面的工具包，包括文件编辑、bash执行、结构化思考和任务完成。有关所有可用工具及其功能的详细信息，请参见 [docs/tools.md](docs/tools.md)。

### 轨迹记录

Trae Agent 自动记录详细的执行轨迹用于调试和分析：

```bash
# 自动生成的轨迹文件
trae-cli run "调试身份验证模块"
# 保存到：trajectories/trajectory_YYYYMMDD_HHMMSS.json

# 自定义轨迹文件
trae-cli run "优化数据库查询" --trajectory-file optimization_debug.json
```

轨迹文件包含LLM交互、智能体步骤、工具使用和执行元数据。更多详情请参见 [docs/TRAJECTORY_RECORDING.md](docs/TRAJECTORY_RECORDING.md)。

## 🔧 开发

### 贡献

贡献指南请参考 [CONTRIBUTING.md](CONTRIBUTING.md)。

### 故障排除

**导入错误：**
```bash
PYTHONPATH=. trae-cli run "你的任务"
```

**API密钥问题：**
```bash
# 验证API密钥
echo $OPENAI_API_KEY
trae-cli show-config
```

**命令未找到：**
```bash
uv run trae-cli run "你的任务"
```

**权限错误：**
```bash
chmod +x /path/to/your/project
```

## 📄 许可证

本项目采用MIT许可证 - 详情请参见 [LICENSE](LICENSE) 文件。

## ✍️ 引用

```bibtex
@article{traeresearchteam2025traeagent,
      title={Trae Agent: An LLM-based Agent for Software Engineering with Test-time Scaling},
      author={Trae Research Team and Pengfei Gao and Zhao Tian and Xiangxin Meng and Xinchen Wang and Ruida Hu and Yuanan Xiao and Yizhou Liu and Zhao Zhang and Junjie Chen and Cuiyun Gao and Yun Lin and Yingfei Xiong and Chao Peng and Xia Liu},
      year={2025},
      eprint={2507.23370},
      archivePrefix={arXiv},
      primaryClass={cs.SE},
      url={https://arxiv.org/abs/2507.23370},
}
```

## 🙏 致谢

我们感谢 Anthropic 构建了 [anthropic-quickstart](https://github.com/anthropics/anthropic-quickstarts) 项目，它为工具生态系统提供了宝贵的参考。