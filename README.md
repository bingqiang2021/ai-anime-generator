# ai-anime-generator提示词
ai-anime-generator

帮我构建一个ai动漫生成的web应用：

1、一句话通过deepseek生成故事，产品前端参考：/Users/brucejan/Downloads/stitch ； 

2、使用deepseek针对故事做内容切片，切4个片段；

 3、针对故事片段采用seedream-v4（https://wavespeed.ai/models/bytedance/seedre am-v4）生成分镜； 

4、使用hailuo-2.3（https://wavespeed.ai/models/minimax/hailuo-2.3/i2v-pro） 针对分镜图像生成视频； 

5、将视频拼凑起来 

补充： 

1、seedream-v4和hailuo-2.3使用wavespeed的API，deepseek使用官方的apikey，两个apikey可以在产品里配置

2、curl --location --request POST  'https://api.wavespeed.ai/api/v3/bytedance/seedream-v4' --header "Content-Type: application/json" --header "Authorization: Bearer ${WAVESPEED_API_KEY}" --data-raw '{ "enable_base64_output": false, "enable_sync_mode": false, "prompt": "American retro style: a girl wearing a polka-dot dress with  sunglasses adorning her head.", "size": "2048*2048" }' Query Result curl --location --request GET  "https://api.wavespeed.ai/api/v3/predictions/${requestId}/result" --header "Authorization: Bearer ${WAVESPEED_API_KEY}" 

3、curl --location --request POST  'https://api.wavespeed.ai/api/v3/minimax/hailuo-2.3/i2v-pro' --header "Content-Type: application/json" --header "Authorization: Bearer ${WAVESPEED_API_KEY}" --data-raw '{ "enable_prompt_expansion": true, "image": "https://d1q70pf5vjeyhc.cloudfront.net/media/92ecf66930134a49a5a425 b9def0c266/images/1761673269273441777_pFPLIEBx.jpeg", "prompt": "A passionate male and female dance duo performing a fiery,  high-energy Latin dance routine (like Salsa or Cha-Cha). They execute fast,  intricate footwork and sensual, rhythmic hip movements with visible  chemistry. The man leads the woman through a series of rapid spins and sharp  turns, culminating in a dramatic dip where her hair fans out, all under a  warm, intense spotlight on a wooden dance floor. Cinematic, passionate,  rhythmic, energetic, dynamic lighting, professional dancers, fiery  chemistry." }' Query Result curl --location --request GET  "https://api.wavespeed.ai/api/v3/predictions/${requestId}/result" --header "Authorization: Bearer ${WAVESPEED_API_KEY}"


#快速启动

  ##1、cd ai-anime-generator

  ## 2、安装依赖
  npm install
  cd client && npm install && cd ..
  cd server && npm install && cd ..

  ## 3、安装FFmpeg（如果未安装）
  brew install ffmpeg  # macOS

  ## 4、启动应用
  npm run dev

  ## 5、访问：http://localhost:5173

  使用步骤

  1. 配置API密钥
    - 访问设置页面配置Deepseek和Wavespeed API密钥
    - Deepseek: https://platform.deepseek.com
    - Wavespeed: https://wavespeed.ai
  2. 生成动漫
    - 输入故事想法（例如："一个少年发现魔法书"）
    - 点击"开始生成动漫"
    - 等待15-30分钟完成
  3. 下载分享
    - 查看完整故事和分镜
    - 下载最终视频

  项目文件说明

  | 文件                 | 说明        |
  |--------------------|-----------|
  | README.md          | 项目基本说明    |
  | SETUP.md           | 详细安装和使用指南 |
  | QUICKSTART.md      | 3分钟快速启动   |
  | PROJECT_SUMMARY.md | 完整项目总结    |

  注意事项

  ⚠️ 重要提示：
  - 生成时间：15-30分钟（取决于API速度）
  - API成本：每次生成需调用10次API（2次Deepseek + 8次Wavespeed）
  - 需要FFmpeg：用于视频拼接
  - 网络稳定：确保生成过程不中断

  项目特色

  ✨ 参考stitch设计：
  - 深色主题（#191022背景色）
  - 紫色品牌色（#7f13ec）
  - Material Icons图标
  - 流畅的响应式布局

  🚀 完整功能：
  - API密钥配置界面
  - 实时生成进度显示
  - 完整的错误处理
  - 视频下载功能

  所有代码已准备就绪，可以立即使用！查看 PROJECT_SUMMARY.md
  了解完整的项目架构和功能说明。

