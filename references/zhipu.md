# 智谱AI (ZhipuAI)

## 1. 提供商说明

智谱AI 是国内领先的大模型服务商，提供 GLM-Image 系列图像生成模型。支持从文本提示生成高质量图像，对用户文字描述理解精准，图像表达个性化。

## 2. 请求地址

```
POST https://open.bigmodel.cn/api/paas/v4/images/generations
```

## 3. 可用模型

| 模型 | 说明 |
|------|------|
| `glm-image` | 最新图像生成模型，仅支持 hd 质量，推荐尺寸 1280x1280 |
| `cogview-4-250304` | CogView-4 模型版本 |
| `cogview-4` | CogView-4 模型 |
| `cogview-3-flash` | 快速生成模型，适合对速度有要求的场景 |

## 4. 请求体参数说明

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | 是 | 模型编码，可选值见上表 |
| `prompt` | string | 是 | 所需图像的文本描述 |
| `quality` | string | 否 | 图像质量：`hd`（精细，约20秒）或 `standard`（快速，约5-10秒）。glm-image 仅支持 hd |
| `size` | string | 否 | 图片尺寸，见下方推荐值 |
| `watermark_enabled` | boolean | 否 | 是否添加水印，默认 true。设为 false 需签署免责声明 |
| `user_id` | string | 否 | 终端用户唯一ID，用于安全干预，6-128字符 |

## 5. 尺寸参数

### glm-image 推荐尺寸

| 尺寸 | 宽高比 |
|------|--------|
| 1280x1280 (默认) | 1:1 |
| 1568×1056 | 3:2 |
| 1056×1568 | 2:3 |
| 1472×1088 | 4:3 |
| 1088×1472 | 3:4 |
| 1728×960 | 16:9 |
| 960×1728 | 9:16 |

自定义要求：长宽在 1024px-2048px 范围内，最大像素数不超过 2^22，长宽需为 32 的整数倍。

### 其他模型推荐尺寸

| 尺寸 | 宽高比 |
|------|--------|
| 1024x1024 (默认) | 1:1 |
| 768x1344 | 9:16 |
| 864x1152 | 3:4 |
| 1344x768 | 16:9 |
| 1152x864 | 4:3 |
| 1440x720 | 2:1 |
| 720x1440 | 1:2 |

自定义要求：长宽在 512px-2048px 之间，需被 16 整除，最大像素数不超过 2^21。

## 6. CURL 请求示例

### 基础文生图

```bash
curl -X POST https://open.bigmodel.cn/api/paas/v4/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "glm-image",
    "prompt": "一只可爱的小猫咪，坐在阳光明媚的窗台上，背景是蓝天白云",
    "size": "1280x1280"
  }'
```

### 快速生成（cogview-3-flash）

```bash
curl -X POST https://open.bigmodel.cn/api/paas/v4/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "cogview-3-flash",
    "prompt": "一幅山水画，远山近水，云雾缭绕",
    "size": "1024x1024",
    "quality": "standard"
  }'
```

## 7. 响应示例

```json
{
  "created": 1677700000,
  "data": [
    {
      "url": "https://example.com/generated-image.png"
    }
  ]
}
```

**注意**：图片临时链接有效期为 30 天，请及时转存。

## 8. 错误处理

响应中包含 `content_filter` 字段，返回内容安全相关信息：
- `level 0`：最严重
- `level 3`：轻微
