# 火山引擎 Volcengine

## 1. 提供商说明

火山引擎是字节跳动旗下的云服务平台，提供多种AI大模型服务，其中包括Seedream系列图片生成模型。Seedream 5.0 lite是一款强大的图像生成模型，支持文生图、图生图、组图生成等多种功能。

## 2. 请求地址

```
POST https://ark.cn-beijing.volces.com/api/v3/images/generations
```

## 3. 模型ID

```
doubao-seedream-5-0-260128
```

## 4. 请求体参数说明

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `model` | string | 是 | 模型ID，固定为 `doubao-seedream-5-0-260128` |
| `prompt` | string | 是 | 文本提示词，建议不超过300个汉字或600个英文单词 |
| `image` | string/array | 否 | 参考图片信息，支持URL或Base64编码 |
| `size` | string | 否 | 生成图片的尺寸，支持两种方式：<br>方式1：指定分辨率（2K、4K），需在prompt中描述宽高比<br>方式2：指定宽高像素值（如2048x2048） |
| `sequential_image_generation` | string | 否 | 组图生成开关：<br>`auto` - 启用组图生成<br>`disabled` - 禁用组图生成，生成单图 |
| `output_format` | string | 否 | 生成图片的输出格式，可选 `png`、`jpeg`，默认 `png` |
| `response_format` | string | 否 | API响应格式，可选 `url`（返回临时URL）、`base64`（返回Base64），默认 `url` |
| `watermark` | boolean | 否 | 是否添加水印，`false`（默认不添加）、`true` |

### 参数详细说明

**image 参数：**
- 支持URL或Base64编码
- Base64格式：`data:image/<图片格式>;base64,<Base64编码>`
- 图片格式支持：jpeg、png、webp、bmp、tiff、gif
- 宽高比范围：[1/16, 16]
- 宽高长度：> 14px
- 大小：不超过10MB
- 总像素：不超过 6000×6000 = 36,000,000 px
- 最多支持传入14张参考图

**size 参数：**
- 默认值：2048x2048
- 总像素取值范围：[2560×1440=3,686,400, 4096×4096=16,777,216]
- 宽高比取值范围：[1/16, 16]
- 必须同时满足总像素和宽高比范围

**sequential_image_generation 参数：**
- `auto`：根据输入生成一组关联的图片（组图）
- `disabled`：生成单张图片

**output_format 参数：**
- 指定生成图片的输出格式
- 可选值：`png`、`jpeg`
- 默认值：`png`

**response_format 参数：**
- 指定API响应的返回格式
- 可选值：
  - `url`：返回图片的临时访问URL
  - `base64`：返回Base64编码的图片数据
- 默认值：`url`

**watermark 参数：**
- 指定是否在生成的图片中添加水印
- 可选值：
  - `false`（默认）：不添加水印
  - `true`：添加水印

### 推荐的宽高像素值

**2K分辨率：**

| 宽高比 | 宽高像素值 |
|--------|-----------|
| 1:1 | 2048×2048 |
| 4:3 | 2304×1728 |
| 3:4 | 1728×2304 |
| 16:9 | 2848×1600 |
| 9:16 | 1600×2848 |
| 3:2 | 2496×1664 |
| 2:3 | 1664×2496 |
| 21:9 | 3136×1344 |

**4K分辨率：**

| 宽高比 | 宽高像素值 |
|--------|-----------|
| 1:1 | 4096×4096 |
| 3:4 | 3520×4704 |
| 4:3 | 4704×3520 |
| 16:9 | 5504×3040 |
| 9:16 | 3040×5504 |
| 2:3 | 3328×4992 |
| 3:2 | 4992×3328 |
| 21:9 | 6240×2656 |

## 5. CURL 请求示例

### 文生图（生成单图）

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "一只可爱的橘猫坐在窗台上，阳光透过窗帘洒在它身上",
    "size": "2048x2048",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 文生组图

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "一组美食图片，包括披萨、寿司、汉堡、意面等",
    "size": "2048x2048",
    "sequential_image_generation": "auto",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 单图生图

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "将图片转换为油画风格，色彩更加浓郁",
    "image": "https://example.com/image.jpg",
    "size": "2048x2048",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 单图生组图

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "基于这张风景图，生成一组不同时间和季节的风景图",
    "image": "https://example.com/landscape.jpg",
    "size": "2048x2048",
    "sequential_image_generation": "auto",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 多图生图

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "将这两张图片的风格融合在一起",
    "image": [
      "https://example.com/image1.jpg",
      "https://example.com/image2.jpg"
    ],
    "size": "2048x2048",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 多图生组图

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "基于这些参考图片，生成一组风格统一的设计图",
    "image": [
      "https://example.com/ref1.jpg",
      "https://example.com/ref2.jpg",
      "https://example.com/ref3.jpg"
    ],
    "size": "2048x2048",
    "sequential_image_generation": "auto",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 使用Base64编码图片

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "将图片转换为素描风格",
    "image": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+M9QDwADhgGAWjR9awAAAABJRU5ErkJggg==",
    "size": "2048x2048",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 使用2K分辨率

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "一张16:9宽高比的高清风景照片，远处的山脉和近处的湖泊",
    "size": "2K",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```

### 使用4K分辨率

```bash
curl -X POST https://ark.cn-beijing.volces.com/api/v3/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "doubao-seedream-5-0-260128",
    "prompt": "一张4K分辨率的详细城市夜景图",
    "size": "4K",
    "sequential_image_generation": "disabled",
    "output_format": "png",
    "response_format": "url",
    "watermark": false
  }'
```
