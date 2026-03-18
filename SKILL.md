---
name: image-generation
description: 图像生成技能。当用户需要生成图片、创建图像、AI绘画时使用此技能。触发场景包括：文生图、图生图、生成插画、生成海报、生成头像、生成背景图、AI作画等。即使用户没有明确说"生成图片"，但上下文暗示需要创建视觉内容时也应触发。
---

# Image Generation Skill

图像生成技能。当 Agent 需要生成图片时调用此技能。

## 使用流程

1. **理解需求**：分析用户的图片生成需求（文生图/图生图/组图、尺寸、风格等）
2. **选择提供商**：根据需求选择合适的图像生成提供商
3. **查阅文档**：阅读 `references/` 目录下对应提供商的请求说明书，了解 API 参数
4. **获取密钥**：从 `.env` 文件读取对应提供商的 API Key
5. **构造请求**：根据文档说明构造 HTTP 请求
6. **生成图片**：调用 API 生成图片，返回结果给用户

## 可用提供商

| 提供商 | 说明 | 参考文档 |
|--------|------|----------|
| volcengine | 火山引擎 - 字节跳动旗下云服务，提供 Seedream 5.0 图像生成模型 | `references/volcengine.md` |
| zhipu | 智谱AI - 国内领先大模型服务商，提供 GLM-Image 系列图像生成模型 | `references/zhipu.md` |
| aliyun | 阿里云百炼 - 提供千问图像生成模型（Qwen-Image），擅长复杂文本渲染 | `references/aliyun.md` |

## API Keys

所有提供商的 API Key 存储在 `.env` 文件中：

| 提供商 | 环境变量名 |
|--------|-----------|
| volcengine | `volcengine_api_key` |
| zhipu | `zhipu_api_key` |
| aliyun | `aliyun_api_key` |

## 输出处理

生成图片后，根据 `response_format` 参数处理结果：
- **url**：返回临时访问链接，可直接展示给用户
- **base64**：将 base64 数据保存为图片文件，返回文件路径

## 错误处理

如果 API 调用失败，检查以下常见问题：
1. API Key 是否正确
2. 请求参数是否符合要求（如图片尺寸、提示词长度）
3. 参考图片格式和大小是否合规
