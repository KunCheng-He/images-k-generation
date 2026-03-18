# 阿里云百炼 (Alibaba Cloud Bailian)

## 1. 提供商说明

阿里云百炼提供千问图像生成模型（Qwen-Image），支持多种艺术风格，尤其擅长复杂文本渲染。支持多行布局、段落级文本生成以及细粒度细节刻画。

## 2. 请求地址

### 同步接口（推荐）

- **北京地域**：`POST https://dashscope.aliyuncs.com/api/v1/services/aigc/multimodal-generation/generation`
- **新加坡地域**：`POST https://dashscope-intl.aliyuncs.com/api/v1/services/aigc/multimodal-generation/generation`

### 异步接口

- **北京地域**：`POST https://dashscope.aliyuncs.com/api/v1/services/aigc/text2image/image-synthesis`
- **新加坡地域**：`POST https://dashscope-intl.aliyuncs.com/api/v1/services/aigc/text2image/image-synthesis`

**注意**：北京和新加坡地域拥有独立的 API Key，不可混用。

## 3. 可用模型

| 模型 | 说明 | 输出规格 |
|------|------|----------|
| `qwen-image-2.0-pro` | Pro系列，文字渲染、真实质感、语义遵循能力更强 | 512-2048像素，1-6张 |
| `qwen-image-2.0` | 加速版，兼顾效果与响应速度 | 512-2048像素，1-6张 |
| `qwen-image-max` | Max系列，真实感、自然度更强 | 固定尺寸，1张 |
| `qwen-image-plus` | Plus系列，擅长多样化艺术风格与文字渲染 | 固定尺寸，1张 |
| `qwen-image` | 基础模型 | 固定尺寸，1张 |

## 4. 请求参数说明

### 同步接口参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | 是 | 模型名称 |
| `input.messages` | array | 是 | 消息数组，仅支持单轮对话 |
| `input.messages[].role` | string | 是 | 固定为 `user` |
| `input.messages[].content[].text` | string | 是 | 正向提示词，不超过800字符 |
| `parameters.negative_prompt` | string | 否 | 反向提示词，不超过500字符 |
| `parameters.size` | string | 否 | 图像尺寸，格式为 `宽*高` |
| `parameters.n` | integer | 否 | 输出图像数量，默认1 |
| `parameters.prompt_extend` | boolean | 否 | 是否开启提示词智能改写，默认true |
| `parameters.watermark` | boolean | 否 | 是否添加水印，默认false |
| `parameters.seed` | integer | 否 | 随机数种子，[0, 2147483647] |

### 异步接口参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | 是 | 仅支持 `qwen-image-plus`、`qwen-image` |
| `input.prompt` | string | 是 | 正向提示词，不超过800字符 |
| `input.negative_prompt` | string | 否 | 反向提示词，不超过500字符 |
| `parameters.size` | string | 否 | 图像尺寸 |
| `parameters.n` | integer | 否 | 固定为1 |
| `parameters.prompt_extend` | boolean | 否 | 是否开启提示词智能改写 |
| `parameters.watermark` | boolean | 否 | 是否添加水印 |
| `parameters.seed` | integer | 否 | 随机数种子 |

**请求头**：异步接口需添加 `X-DashScope-Async: enable`

## 5. 尺寸参数

### qwen-image-2.0 系列推荐尺寸

| 尺寸 | 宽高比 |
|------|--------|
| 2048*2048（默认） | 1:1 |
| 2688*1536 | 16:9 |
| 1536*2688 | 9:16 |
| 2368*1728 | 4:3 |
| 1728*2368 | 3:4 |

自定义要求：总像素在 512*512 至 2048*2048 之间。

### qwen-image-max/plus 系列可选尺寸

| 尺寸 | 宽高比 |
|------|--------|
| 1664*928（默认） | 16:9 |
| 1472*1104 | 4:3 |
| 1328*1328 | 1:1 |
| 1104*1472 | 3:4 |
| 928*1664 | 9:16 |

## 6. CURL 请求示例

### 同步接口（推荐）

```bash
curl --location 'https://dashscope.aliyuncs.com/api/v1/services/aigc/multimodal-generation/generation' \
  --header 'Content-Type: application/json' \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --data '{
    "model": "qwen-image-2.0-pro",
    "input": {
      "messages": [
        {
          "role": "user",
          "content": [
            {"text": "一只可爱的橘猫坐在窗台上，阳光透过窗帘洒在它身上"}
          ]
        }
      ]
    },
    "parameters": {
      "negative_prompt": "低分辨率，低画质，肢体畸形",
      "prompt_extend": true,
      "watermark": false,
      "size": "2048*2048"
    }
  }'
```

### 异步接口

```bash
# 步骤1：创建任务
curl -X POST https://dashscope.aliyuncs.com/api/v1/services/aigc/text2image/image-synthesis \
  -H 'X-DashScope-Async: enable' \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "qwen-image-plus",
    "input": {
      "prompt": "一只可爱的橘猫坐在窗台上"
    },
    "parameters": {
      "size": "1664*928",
      "n": 1,
      "watermark": false
    }
  }'

# 步骤2：查询任务结果
curl -X GET https://dashscope.aliyuncs.com/api/v1/tasks/{task_id} \
  --header "Authorization: Bearer YOUR_API_KEY"
```

## 7. 响应示例

### 同步接口响应

```json
{
  "output": {
    "choices": [
      {
        "finish_reason": "stop",
        "message": {
          "content": [
            {"image": "https://dashscope-result-sh.oss-cn-shanghai.aliyuncs.com/xxx.png?Expires=xxx"}
          ],
          "role": "assistant"
        }
      }
    ]
  },
  "usage": {
    "height": 2048,
    "image_count": 1,
    "width": 2048
  },
  "request_id": "xxx"
}
```

### 异步接口响应

```json
{
  "output": {
    "task_id": "xxx",
    "task_status": "SUCCEEDED",
    "results": [
      {"url": "https://dashscope-result-sz.oss-cn-shenzhen.aliyuncs.com/xxx.png?Expires=xxx"}
    ]
  },
  "usage": {
    "image_count": 1
  }
}
```

## 8. 注意事项

1. **图像URL有效期**：同步接口24小时，异步接口24小时，请及时下载保存
2. **地域隔离**：北京和新加坡地域的 API Key 不可混用
3. **异步任务状态**：PENDING → RUNNING → SUCCEEDED/FAILED
4. **轮询建议**：异步任务建议每10秒查询一次状态
