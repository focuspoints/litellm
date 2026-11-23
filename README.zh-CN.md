# LiteLLM 简体中文使用指南

[English README](README.md) · [完整文档](https://docs.litellm.ai/docs/)

LiteLLM 有两种常见用法。Python SDK 将统一的模型调用接口集成到应用中。AI Gateway（Proxy Server）作为独立服务运行，为团队提供统一 API、密钥管理和用量管理。

## Python SDK

安装 SDK，并设置所用模型提供方的 API 密钥。下面以 OpenAI 为例。

```bash
uv add litellm
export OPENAI_API_KEY="your-openai-key"
```

```python
from litellm import completion

response = completion(
    model="openai/gpt-6.1-sol",
    messages=[{"role": "user", "content": "你好"}],
)
print(response.choices[0].message.content)
```

## AI Gateway（Proxy Server）

安装 Proxy 并启动本地服务。此示例使用 OpenAI API 密钥。

```bash
uv tool install 'litellm[proxy]'
export OPENAI_API_KEY="your-openai-key"
litellm --model gpt-6.1-sol
```

服务启动后，可通过 OpenAI 兼容接口发送请求。

```bash
curl http://localhost:4000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-6.1-sol","messages":[{"role":"user","content":"你好"}]}'
```

在配置了身份验证的部署中，请在请求中使用分配的虚拟密钥。生产部署、Docker 启动和首次创建密钥的步骤见 [Proxy 快速入门](https://docs.litellm.ai/docs/proxy/docker_quick_start)。

## 从源码构建本次中文控制台

本仓库的中文控制台来自 `litellm/ui/litellm-dashboard`。生产 Dockerfile 使用仓库根目录作为构建上下文，在 `litellm/deploy/docker/Dockerfile` 中执行 `npm ci` 和 `npm run build`，再将生成的 `litellm/ui/litellm-dashboard/out` 内容复制到运行时 UI 目录。可从仓库根目录构建镜像。

```bash
docker build -f litellm/deploy/docker/Dockerfile .
```

直接安装 PyPI 包或使用预构建的上游镜像时，使用的是对应发行物内置的上游控制台，不会自动包含本次源码中的新增译文。要使用本仓库的简体中文入口，请从本仓库源码构建控制台所在的镜像。

## 模块文档

### 模型与 API

- [模型提供方与模型配置](https://docs.litellm.ai/docs/providers) 查看提供方、模型名称和认证方式。
- [支持的 API 端点](https://docs.litellm.ai/docs/supported_endpoints) 查看聊天、Responses、嵌入、音频、图像等接口。

### 路由与可靠性

- [Router](https://docs.litellm.ai/docs/routing) 了解模型组、负载均衡、重试和回退。
- [自动路由](https://docs.litellm.ai/docs/proxy/auto_routing) 按请求选择模型。
- [缓存](https://docs.litellm.ai/docs/proxy/caching) 配置请求结果缓存。

### 密钥、权限与预算

- [虚拟密钥](https://docs.litellm.ai/docs/proxy/virtual_keys) 创建和管理调用密钥，并为密钥配置模型访问范围和限制。
- [访问管理](https://docs.litellm.ai/docs/proxy/access_management/overview) 了解用户、团队和角色的权限。
- [用户与预算](https://docs.litellm.ai/docs/proxy/users) 查看用户、团队预算和速率限制。

### 用量与可观测性

- [成本与用量跟踪](https://docs.litellm.ai/docs/proxy/cost_tracking) 查看密钥、用户、团队和模型的用量与支出。
- [日志与回调](https://docs.litellm.ai/docs/proxy/logging) 接入日志和可观测性平台。

### 安全与集成

- [Guardrails](https://docs.litellm.ai/docs/proxy/guardrails/quick_start) 为请求和响应配置安全检查。
- [MCP](https://docs.litellm.ai/docs/mcp) 将 MCP 工具接入模型调用。
- [A2A](https://docs.litellm.ai/docs/a2a) 将 A2A Agent 接入 LiteLLM。

## 控制台语言

控制台默认使用英文。在登录页或登录后的账户菜单中打开语言选择器，选择“中文（中国）”即可切换到简体中文。语言偏好会保存在当前浏览器中，缺少译文的界面文案会显示为英文。
