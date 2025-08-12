# DeepSeek 模型集成指南

## 概述

本项目现在支持 DeepSeek 的 AI 模型，包括专门为代码生成优化的模型。

## 支持的模型

- **DeepSeek Coder** (`deepseek/deepseek-coder`) - 专门为代码生成和编程任务优化
- **DeepSeek Chat** (`deepseek/deepseek-chat`) - 通用对话模型
- **DeepSeek 7B Chat** (`deepseek/deepseek-llm-7b-chat`) - 轻量级对话模型

## 配置步骤

### 1. 获取 API 密钥

1. 访问 [DeepSeek Platform](https://platform.deepseek.com)
2. 注册账户并登录
3. 在控制台中创建新的 API 密钥
4. 复制 API 密钥

### 2. 设置环境变量

在项目根目录创建 `.env.local` 文件（如果不存在），添加：

```env
DEEPSEEK_API_KEY=your_deepseek_api_key_here
```

### 3. 重启开发服务器

```bash
npm run dev
```

## 使用方法

### 在 UI 中选择模型

1. 在主页面的模型选择器下拉菜单中选择 DeepSeek 模型
2. 模型选择器位于页面顶部右侧
3. 选择后，所有 AI 请求将使用选定的 DeepSeek 模型

### 通过 URL 参数指定模型

可以在 URL 中直接指定模型：

```
http://localhost:3000/?model=deepseek/deepseek-coder
```

## 模型特性

### DeepSeek Coder
- **最佳用途**: 代码生成、代码审查、编程问题解决
- **优势**: 专门为编程任务训练，代码质量高
- **适用场景**: 生成 React 组件、修复代码错误、重构代码

### DeepSeek Chat
- **最佳用途**: 通用对话、内容生成、问题解答
- **优势**: 平衡的对话能力，适合多种任务
- **适用场景**: 网站内容生成、用户交互设计

### DeepSeek 7B Chat
- **最佳用途**: 轻量级任务、快速响应
- **优势**: 响应速度快，资源消耗低
- **适用场景**: 简单代码生成、快速原型开发

## 技术实现

### API 路由支持

项目中的以下 API 路由已支持 DeepSeek：

- `/api/analyze-edit-intent` - 编辑意图分析
- `/api/generate-ai-code-stream` - 代码生成流式响应

### 模型选择逻辑

```typescript
// 模型提供商选择
const isDeepSeek = model.startsWith('deepseek/');
const modelProvider = isAnthropic ? anthropic : 
                     (isOpenAI ? openai : 
                     (isDeepSeek ? deepseek : groq));

// 模型名称处理
const actualModel = isDeepSeek ? model.replace('deepseek/', '') : model;
```

## 故障排除

### 常见问题

1. **API 密钥错误**
   - 确保 `.env.local` 文件中的 `DEEPSEEK_API_KEY` 正确设置
   - 检查 API 密钥是否有效且未过期

2. **模型不可用**
   - 确保选择了正确的模型名称
   - 检查 DeepSeek 服务状态

3. **响应质量**
   - 对于代码生成任务，推荐使用 `deepseek/deepseek-coder`
   - 对于通用对话，推荐使用 `deepseek/deepseek-chat`

### 调试信息

在浏览器控制台中可以看到模型选择的日志信息：

```
[generate-ai-code-stream] Using AI model: deepseek/deepseek-coder
[generate-ai-code-stream] Model provider: deepseek
```

## 性能优化

### 模型选择建议

- **开发阶段**: 使用 `deepseek/deepseek-llm-7b-chat` 快速迭代
- **生产环境**: 使用 `deepseek/deepseek-coder` 获得最佳代码质量
- **平衡需求**: 使用 `deepseek/deepseek-chat` 兼顾速度和质量

### 成本控制

- DeepSeek 的定价相对合理，适合开发和生产使用
- 7B 模型成本最低，适合频繁的开发和测试
- Coder 模型虽然成本较高，但代码质量显著提升

## 更新日志

- **v1.0.0**: 初始 DeepSeek 集成
- 支持三种主要模型
- 完整的 API 路由集成
- UI 模型选择器支持
