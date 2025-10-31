# 快速启动指南

## 3分钟快速启动

### 1. 安装依赖 (1分钟)

```bash
cd ai-anime-generator

# 安装所有依赖
npm install
cd client && npm install && cd ..
cd server && npm install && cd ..
```

### 2. 检查FFmpeg (30秒)

```bash
# 检查FFmpeg是否已安装
ffmpeg -version

# 如果未安装：
# macOS: brew install ffmpeg
# Ubuntu: sudo apt install ffmpeg
```

### 3. 启动应用 (30秒)

```bash
# 回到项目根目录
cd ai-anime-generator

# 启动开发服务器（前端+后端）
npm run dev
```

### 4. 配置并使用 (1分钟)

1. 打开浏览器访问：http://localhost:5173

2. 点击右上角"设置"按钮，配置API密钥：
   - DeepSeek API Key: 从 https://platform.deepseek.com 获取
   - Wavespeed API Key: 从 https://wavespeed.ai 获取

3. 返回主页，输入故事想法，点击"开始生成动漫"

4. 等待15-30分钟完成生成（取决于API速度）

5. 下载并分享你的AI动漫！

## 故障排除

**端口被占用**:
```bash
# 查看并结束占用端口的进程
lsof -i :3001
kill -9 <PID>
```

**FFmpeg未找到**:
```bash
# 安装FFmpeg
brew install ffmpeg  # macOS
sudo apt install ffmpeg  # Ubuntu
```

**API密钥错误**:
- 检查密钥是否正确复制
- 确认账户有足够余额
- 在设置页面重新配置

## 更多信息

详细文档：`SETUP.md`
项目说明：`README.md`
