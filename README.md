# ai-anime-generator
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
