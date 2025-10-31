# AI动漫生成器

一个基于AI的动漫视频自动生成Web应用。

## 功能特性

- 通过Deepseek AI一句话生成完整故事
- 自动将故事切分为4个场景片段
- 使用Seedream-v4生成分镜图像
- 使用Hailuo-2.3生成视频动画
- 自动拼接所有视频为完整作品

## 技术栈

### 前端
- React 18 + TypeScript
- Tailwind CSS
- React Router
- Axios

### 后端
- Node.js + Express + TypeScript
- FFmpeg（视频处理）
- Axios

### AI服务
- Deepseek API：故事生成和分片
- Wavespeed API：图像和视频生成

## 快速开始

### 环境要求

- Node.js >= 18
- FFmpeg
- npm 或 yarn

### 安装

```bash
# 克隆项目
git clone <repository-url>
cd ai-anime-generator

# 安装依赖
npm install
cd client && npm install
cd ../server && npm install
cd ..
```

### 配置

在 `server` 目录下创建 `.env` 文件：

```env
PORT=3001
DEEPSEEK_API_KEY=your_deepseek_api_key
WAVESPEED_API_KEY=your_wavespeed_api_key
```

### 运行

```bash
# 开发模式（同时启动前后端）
npm run dev

# 或分别启动
npm run dev:server  # 后端运行在 http://localhost:3001
npm run dev:client  # 前端运行在 http://localhost:5173
```

### 构建生产版本

```bash
npm run build
npm start
```

## API文档

### 故事生成
```
POST /api/story/generate
Body: { prompt: string, apiKeys: { deepseek: string } }
```

### 故事分片
```
POST /api/story/slice
Body: { story: string, apiKeys: { deepseek: string } }
```

### 生成分镜图像
```
POST /api/image/generate
Body: { prompt: string, apiKeys: { wavespeed: string } }
```

### 生成视频
```
POST /api/video/generate
Body: { imageUrl: string, prompt: string, apiKeys: { wavespeed: string } }
```

### 拼接视频
```
POST /api/video/merge
Body: { videoUrls: string[] }
```

## 许可证

MIT
