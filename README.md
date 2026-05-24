# 儿童英语启蒙识词小报生成器

输入主题，自动生成适合 2-9 岁儿童的英语启蒙识词小报图片。

## 功能

- **智能词汇生成** — 基于 DeepSeek AI，根据主题自动生成适龄英语词汇，含音标、例句、中文释义
- **AI 图片生成** — 支持 GPT Image 2（文字准确）和 Nano Banana Pro 两种模型
- **42 个预设主题** — 覆盖动物、食物、日常、交通、身体、自然、学校节日等 7 大类
- **年龄分段** — 2-4 岁（看图认物）、4-6 岁（学单词）、6-9 岁（学句子），自动调整词汇和句子难度
- **5 种画风** — 卡通绘本、水彩风、简笔画、3D 卡通、中国风
- **点击发音** — 基于 Web Speech API，点击单词或句子即可朗读
- **打印预览** — A4 排版，图片 + 配文词卡一键打印
- **批量生成** — 多选主题，一键生成整套词卡系列
- **配文词卡** — 图片下方按句型分组展示词汇，方便亲子共读

## 快速开始

1. 双击打开 `index.html`
2. 输入 kie.ai API Key 和 DeepSeek API Key（首次使用，之后自动保存）
3. 选择主题 → 生成词汇 → 生成小报

## API Key 获取

| 服务 | 用途 | 获取地址 |
|------|------|----------|
| kie.ai | 图片生成 | https://kie.ai/api-key |
| DeepSeek | 词汇生成 | https://platform.deepseek.com |

## 技术栈

- 纯 HTML + CSS + JS 单文件，无需构建工具
- 无后端依赖，双击即可运行
- 浏览器原生 Web Speech API 发音

## 示例

选择"动物园"主题，生成效果：
- 词汇：lion, monkey, panda, elephant, bird, tree, cage, ticket, map, water, sun, zoo
- 句型分组：I see / This is / The ... is / I like ...
- 配文含音标：lion `/ˈlaɪ.ən/`、monkey `/ˈmʌŋ.ki/`
