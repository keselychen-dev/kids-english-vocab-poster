# CLAUDE.md — 项目上下文文档

## GitHub 仓库

https://github.com/keselychen-dev/kids-english-vocab-poster

## 项目简介

这是一个**儿童英语启蒙识词小报生成器**，面向 2-9 岁儿童的家长/老师。用户输入一个主题（如动物园、水果），工具会自动生成一张带英语词汇、音标、例句的卡通小报图片，附带配文词卡。

- 目标用户：2-9 岁儿童的家长、幼儿园/小学老师
- 核心价值：一键生成英语启蒙学习材料，可直接打印使用
- 技术栈：纯 HTML + CSS + JS 单文件（index.html），无框架，无需构建

## 架构

```
index.html（单文件，约 900 行）
├── HTML 结构
│   ├── API Key 配置区（可折叠）
│   ├── Step 1：主题选择 + 参数配置
│   ├── Step 2：词汇生成 + 批量队列
│   └── Step 3：图片生成 + 结果展示
├── CSS（内嵌 <style>）
└── JavaScript（内嵌 <script>）
    ├── 状态管理（selectedModel, selectedAge, selectedStyle, vocabList, batchQueue）
    ├── DeepSeek API 调用（词汇生成）
    ├── kie.ai API 调用（图片生成，异步轮询）
    ├── TTS 发音（Web Speech API）
    ├── 批量生成逻辑
    └── 打印预览
```

## API 集成

### 1. DeepSeek（词汇生成）
- 端点：`POST https://api.deepseek.com/chat/completions`
- 模型：`deepseek-chat`
- 功能：根据主题生成 6-20 个英语启蒙词汇（含音标、例句、中文释义）
- Prompt 要点：按年龄分段调难度、按句型分组、禁止不自然表述（如 "I see a daddy"）
- 返回 JSON 格式：`{"words":[{"en","cn","phonetic","sentence","sentence_cn","pattern"}]}`

### 2. kie.ai（图片生成）
- 创建任务：`POST https://api.kie.ai/api/v1/jobs/createTask`
- 查询状态：`GET https://api.kie.ai/api/v1/jobs/recordInfo?taskId=xxx`
- 支持两个模型（用户可选）：
  - `gpt-image-2-text-to-image` — 文字渲染更准确（推荐）
  - `nano-banana-pro` — 支持参考图输入
- 异步模式：createTask → 每 3 秒轮询 recordInfo → state 变为 success 后取 resultUrls
- 图片参数：aspect_ratio 3:4（A4 竖版）、resolution 4K、output_format png

## 功能清单

| 功能 | 实现方式 | 备注 |
|------|----------|------|
| 42 个预设主题 | HTML 按钮，分 7 大类 | 也支持手动输入自定义主题 |
| 年龄分段（2-4/4-6/6-9） | 影响 DeepSeek prompt 词汇难度和句子长度 | |
| 画风选择（5 种） | 影响 buildPrompt() 的 STYLE 部分 | cartoon/watercolor/line/3d/chinese |
| 单词数量滑块（6-20） | 控制 DeepSeek 生成数量 | |
| 点击发音 | Web Speech API（speak()函数） | 单词和句子都可点击 |
| 批量生成 | batchQueue 数组 + 串行循环调用 | 多选主题，依次生成 |
| 打印预览 | window.open() 新窗口 + A4 排版 | 图片 + 词汇表格 |
| 配文词卡 | 按句型分组展示词汇 | showResult/buildResultCard |
| API Key 管理 | localStorage 持久化 | 首次使用时输入，之后自动填充 |

## 关键函数

- `generateVocab(themeOverride, titleOverride)` — 调 DeepSeek 生成词汇，支持批量模式传参
- `generateImage()` — 调 kie.ai 生成图片（单个模式）
- `startBatchGen()` — 批量生成，串行处理队列
- `buildPrompt(theme, title, words)` — 拼装图片生成 prompt，含画风/年龄/分组信息
- `buildDeepseekPrompt(wordCount)` — 构建 DeepSeek 的 system prompt，含年龄适配
- `speak(text, lang)` — TTS 发音
- `printPoster(imgUrl, title, theme)` — 打印预览
- `showResult(url, theme, title)` — 单个模式展示结果
- `buildResultCard(theme, title, imgUrl, words)` — 构建结果卡片 HTML（含配文）

## 用户偏好

- API Key 已预填在代码中作为默认值（DeepSeek 和 kie.ai）
- 所有选项（年龄、画风、模型、单词数量、模式）都是独立可选，不捆绑
- 配文需要按句型分组展示，句子要自然像亲子对话
- 图片 prompt 用英文，词汇 prompt 用中文
