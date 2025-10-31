# 故障排除指南

## 问题：500错误 / API调用失败

### 症状
- 前端显示"Request failed with status code 500"
- 后端日志显示"prediction undefined not found"

### 可能的原因

#### 1. API密钥未配置或无效

**检查方法**：
```bash
# 运行API诊断工具
cd /Users/brucejan/Downloads/stitch/ai-anime-generator
node server/test-api.js
```

**解决方案**：
- 确保在设置页面或 `server/.env` 文件中配置了有效的API密钥
- 检查API密钥是否有足够余额
- 从官方网站重新获取密钥：
  - Deepseek: https://platform.deepseek.com
  - Wavespeed: https://wavespeed.ai

#### 2. Wavespeed API响应格式问题

**症状**: 日志显示 "No request ID found in response"

**原因**: Wavespeed API的响应格式可能与预期不同

**已修复**: 代码已更新支持多种ID字段格式
- `id`
- `request_id`
- `prediction_id`
- `task_id`

#### 3. 网络连接问题

**检查方法**：
```bash
# 测试API连接
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.wavespeed.ai/api/v3/health

curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.deepseek.com/v1/models
```

**解决方案**：
- 检查防火墙设置
- 确认没有代理阻止API访问
- 尝试使用VPN

#### 4. API配额耗尽

**症状**: 401 Unauthorized 或 429 Too Many Requests

**解决方案**：
- 登录API提供商网站检查余额
- 充值账户
- 等待配额重置

## 调试步骤

### 1. 启用详细日志

服务器已经配置了详细日志，你会看到：
- API请求详情
- 完整的响应数据
- 错误堆栈跟踪

### 2. 查看服务器日志

```bash
# 日志会自动显示在终端
# 查找类似以下的信息：
# - "Sending request to Seedream-v4..."
# - "Seedream-v4 response: {...}"
# - "Response status: ..."
```

### 3. 测试单个API

**测试Deepseek**：
```bash
curl -X POST http://localhost:3001/api/story/generate \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "一个测试故事",
    "apiKeys": {
      "deepseek": "YOUR_DEEPSEEK_KEY",
      "wavespeed": "YOUR_WAVESPEED_KEY"
    }
  }'
```

**测试图像生成**：
```bash
curl -X POST http://localhost:3001/api/image/generate \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A beautiful anime girl",
    "apiKeys": {
      "wavespeed": "YOUR_WAVESPEED_KEY"
    }
  }'
```

### 4. 检查前端配置

1. 打开浏览器开发者工具（F12）
2. 查看 Console 标签页的错误信息
3. 查看 Network 标签页的API请求
4. 检查本地存储中的API密钥：
   ```javascript
   localStorage.getItem('ai_anime_api_keys')
   ```

## 常见错误代码

| 错误代码 | 含义 | 解决方案 |
|---------|------|---------|
| 400 | 请求格式错误 | 检查API参数是否正确 |
| 401 | 未授权 | 检查API密钥是否有效 |
| 403 | 禁止访问 | 检查账户权限和余额 |
| 429 | 请求过多 | 等待或升级API套餐 |
| 500 | 服务器错误 | 查看详细日志，联系支持 |
| 503 | 服务不可用 | API服务可能维护中 |

## 配置检查清单

- [ ] Node.js 已安装（v18+）
- [ ] npm 依赖已安装
- [ ] FFmpeg 已安装且可用
- [ ] server/.env 文件已创建
- [ ] DEEPSEEK_API_KEY 已配置
- [ ] WAVESPEED_API_KEY 已配置
- [ ] API密钥有效且有余额
- [ ] 端口3001和5173未被占用
- [ ] 网络连接正常
- [ ] 防火墙未阻止API访问

## 获取帮助

### 查看日志位置

- 服务器日志：终端输出
- 前端日志：浏览器开发者工具 Console
- 生成的文件：`server/uploads/` 目录

### 重置应用

```bash
# 清除本地存储
localStorage.clear()

# 重启服务器
# 在终端按 Ctrl+C，然后重新运行
cd /Users/brucejan/Downloads/stitch/ai-anime-generator
npm run dev
```

### 完全重新安装

```bash
cd /Users/brucejan/Downloads/stitch/ai-anime-generator

# 删除 node_modules
rm -rf node_modules client/node_modules server/node_modules

# 删除 package-lock.json
rm -f package-lock.json client/package-lock.json server/package-lock.json

# 重新安装
npm install
cd client && npm install && cd ..
cd server && npm install && cd ..

# 重启服务器
npm run dev
```

## 联系支持

如果问题仍未解决：

1. 收集以下信息：
   - 错误信息截图
   - 服务器完整日志
   - 浏览器控制台日志
   - 使用的API密钥提供商

2. 检查文档：
   - README.md
   - SETUP.md
   - PROJECT_SUMMARY.md

3. 验证API提供商状态：
   - Deepseek状态页
   - Wavespeed状态页

## 已知限制

1. **生成时间**: 完整流程需15-30分钟，无法加速
2. **API限制**: 受API提供商的速率限制约束
3. **视频大小**: 最终视频大小取决于生成的片段
4. **网络依赖**: 需要稳定的网络连接

## 性能优化建议

1. 使用稳定的网络连接
2. 确保有足够的磁盘空间（至少5GB）
3. 不要在生成期间关闭浏览器
4. 首次使用测试简单故事
5. 批量生成时注意API配额
