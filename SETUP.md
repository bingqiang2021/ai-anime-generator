# AI动漫生成器 - 安装与使用指南

## 项目概述

这是一个完整的AI动漫生成Web应用，可以通过一句话生成完整的动漫短片。

**核心功能流程**：
1. 用户输入故事想法
2. Deepseek AI生成完整故事
3. Deepseek将故事切分为4个场景
4. Seedream-v4为每个场景生成分镜图像
5. Hailuo-2.3将图像转换为视频
6. FFmpeg自动合并所有视频

## 系统要求

- Node.js >= 18
- npm 或 yarn
- FFmpeg（用于视频合并）

## 安装步骤

### 1. 安装依赖

```bash
# 进入项目目录
cd ai-anime-generator

# 安装根目录依赖
npm install

# 安装客户端依赖
cd client
npm install
cd ..

# 安装服务端依赖
cd server
npm install
cd ..
```

### 2. 安装 FFmpeg

**macOS**:
```bash
brew install ffmpeg
```

**Ubuntu/Debian**:
```bash
sudo apt update
sudo apt install ffmpeg
```

**Windows**:
下载并安装：https://ffmpeg.org/download.html

验证安装：
```bash
ffmpeg -version
```

### 3. 配置环境变量（可选）

在 `server` 目录下创建 `.env` 文件：

```bash
cd server
cp .env.example .env
```

编辑 `.env` 文件（可选，也可通过前端界面配置）：

```env
PORT=3001
NODE_ENV=development

# API密钥（可选，可通过前端配置界面设置）
DEEPSEEK_API_KEY=your_deepseek_api_key
WAVESPEED_API_KEY=your_wavespeed_api_key
```

## 启动应用

### 开发模式（推荐）

在项目根目录运行：

```bash
npm run dev
```

这将同时启动：
- 前端开发服务器：http://localhost:5173
- 后端API服务器：http://localhost:3001

### 分别启动

如果需要分别启动前后端：

```bash
# 终端1：启动后端
npm run dev:server

# 终端2：启动前端
npm run dev:client
```

### 生产模式

```bash
# 构建项目
npm run build

# 启动生产服务器
npm start
```

## 使用指南

### 1. 配置API密钥

首次使用时，点击右上角的"设置"按钮，配置以下API密钥：

- **DeepSeek API Key**: 从 https://platform.deepseek.com 获取
- **Wavespeed API Key**: 从 https://wavespeed.ai 获取

密钥将安全存储在浏览器本地存储中。

### 2. 生成动漫

1. 在主页面输入故事想法，例如：
   ```
   一个少年发现了一本神秘的魔法书，开启了奇幻冒险
   ```

2. 点击"开始生成动漫"按钮

3. 等待AI自动完成以下步骤：
   - ✓ 生成故事（~30秒）
   - ✓ 切分场景（~20秒）
   - ✓ 生成图像（~2-3分钟/张，共4张）
   - ✓ 生成视频（~5-10分钟/个，共4个）
   - ✓ 合并视频（~30秒）

4. 生成完成后，可以：
   - 查看完整故事
   - 预览各个场景的分镜图像
   - 下载最终视频

### 3. 查看生成的内容

所有生成的文件存储在 `server/uploads/` 目录：

```
server/uploads/
├── images/     # 分镜图像
├── videos/     # 场景视频
└── final/      # 最终合并的视频
```

## API文档

### 后端API接口

所有接口基础URL：`http://localhost:3001/api`

#### 1. 生成故事
```http
POST /story/generate
Content-Type: application/json

{
  "prompt": "故事想法",
  "apiKeys": {
    "deepseek": "your_key",
    "wavespeed": "your_key"
  }
}
```

#### 2. 切分故事
```http
POST /story/slice
Content-Type: application/json

{
  "story": "完整故事文本",
  "apiKeys": {
    "deepseek": "your_key"
  }
}
```

#### 3. 生成图像
```http
POST /image/generate
Content-Type: application/json

{
  "prompt": "图像描述",
  "apiKeys": {
    "wavespeed": "your_key"
  }
}
```

#### 4. 生成视频
```http
POST /video/generate
Content-Type: application/json

{
  "imageUrl": "https://...",
  "prompt": "动画描述",
  "apiKeys": {
    "wavespeed": "your_key"
  }
}
```

#### 5. 合并视频
```http
POST /video/merge
Content-Type: application/json

{
  "videoUrls": [
    "https://...",
    "https://..."
  ]
}
```

## 故障排除

### 1. FFmpeg未找到

**错误**: `Error: ffmpeg not found`

**解决**:
```bash
# 检查FFmpeg是否安装
ffmpeg -version

# 如果未安装，参考上面的安装步骤
```

### 2. API密钥错误

**错误**: `API key is required` 或 `Unauthorized`

**解决**:
- 检查API密钥是否正确配置
- 确认密钥有足够的额度
- 在设置页面重新配置密钥

### 3. 视频生成超时

**错误**: `Generation timeout`

**解决**:
- 视频生成可能需要5-10分钟，请耐心等待
- 检查网络连接是否稳定
- 查看后端日志确认API调用状态

### 4. 端口被占用

**错误**: `Port 3001 is already in use`

**解决**:
```bash
# 查找占用端口的进程
lsof -i :3001

# 结束进程
kill -9 <PID>

# 或修改 server/.env 中的端口号
```

## 性能优化建议

1. **生成时间**: 完整流程约需15-30分钟，建议使用时预留充足时间

2. **成本控制**:
   - 每次生成需调用多个AI API
   - Deepseek: ~2次调用
   - Wavespeed: ~8次调用（4张图 + 4个视频）
   - 建议先小规模测试

3. **并行优化**: 可修改代码实现图像/视频的并行生成以加快速度

## 开发说明

### 项目结构

```
ai-anime-generator/
├── client/              # React前端
│   ├── src/
│   │   ├── components/  # 可复用组件
│   │   ├── pages/       # 页面组件
│   │   ├── services/    # API服务
│   │   └── types/       # TypeScript类型
│   └── package.json
├── server/              # Node.js后端
│   ├── src/
│   │   ├── routes/      # API路由
│   │   ├── services/    # 业务逻辑
│   │   └── types/       # TypeScript类型
│   └── package.json
└── README.md
```

### 技术栈

- **前端**: React 18 + TypeScript + Tailwind CSS
- **后端**: Node.js + Express + TypeScript
- **AI服务**: Deepseek API + Wavespeed API
- **视频处理**: FFmpeg

## 支持

如有问题，请检查：
1. 所有依赖是否正确安装
2. FFmpeg是否可用
3. API密钥是否有效
4. 后端服务器日志

## 许可证

MIT
