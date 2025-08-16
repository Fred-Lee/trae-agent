# 旧版 JSON 配置指南

> **⚠️ 已弃用：** 此 JSON 配置格式已弃用，仅用于向后兼容。对于新安装，请使用 [YAML 配置格式](../README.md#configuration)。

## JSON 配置设置

**配置设置：**

1. **复制示例配置文件：**

   ```bash
   cp trae_config.json.example trae_config.json
   ```

2. **编辑 `trae_config.json` 并将占位符值替换为您的实际凭据：**
   - 将 `"your_openai_api_key"` 替换为您的实际 OpenAI API 密钥
   - 将 `"your_anthropic_api_key"` 替换为您的实际 Anthropic API 密钥
   - 将 `"your_google_api_key"` 替换为您的实际 Google API 密钥
   - 将 `"your_azure_base_url"` 替换为您的实际 Azure 基础 URL
   - 根据需要替换其他占位符 URL 和 API 密钥

**注意：** `trae_config.json` 文件被 git 忽略，以防止意外提交您的 API 密钥。

## JSON 配置结构

Trae Agent 使用 JSON 配置文件进行设置。请参考根目录中的 `trae_config.json.example` 文件获取详细的配置结构。

**配置优先级：**

1. 命令行参数（最高优先级）
2. 配置文件值
3. 环境变量
4. 默认值（最低优先级）

## JSON 配置示例

JSON 配置文件包含各种 LLM 服务的特定提供商设置：

```json
{
  "default_provider": "anthropic",
  "max_steps": 20,
  "enable_lakeview": true,
  "model_providers": {
    "openai": {
      "api_key": "your_openai_api_key",
      "base_url": "https://api.openai.com/v1",
      "model": "gpt-4o",
      "max_tokens": 128000,
      "temperature": 0.5,
      "top_p": 1,
      "max_retries": 10
    },
    "anthropic": {
      "api_key": "your_anthropic_api_key",
      "base_url": "https://api.anthropic.com",
      "model": "claude-sonnet-4-20250514",
      "max_tokens": 4096,
      "temperature": 0.5,
      "top_p": 1,
      "top_k": 0,
      "max_retries": 10
    }
  }
}
```

## 迁移到 YAML

要从 JSON 迁移到 YAML 配置：

1. **创建新的 YAML 配置文件：**
   ```bash
   cp trae_config.yaml.example trae_config.yaml
   ```

2. **转移您的设置** 从 `trae_config.json` 到 `trae_config.yaml`，遵循新结构

3. **删除旧的 JSON 文件**（可选但推荐）：
   ```bash
   rm trae_config.json
   ```

有关详细的 YAML 配置说明，请参考主要的 [README.md](../README.md#configuration)。