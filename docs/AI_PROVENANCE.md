# AI 调用与内容溯源 AI Provenance

## 1. 双模式（务必在答辩中说清）

| 模式 | 条件 | 行为 | UI 提示 |
|------|------|------|---------|
| **Live 真实 API** | `AI_API_KEY` 非空 | `POST {AI_API_BASE}/chat/completions`，模型 `AI_MODEL` | 模型名展示 |
| **Demo 演示模式** | 未配置 Key | `lib/ai.ts` 内规则 + 模板（多语叙事、风险清单、本土化） | **「演示模式」** 徽章 |

配置项（见 `.env.example`）：

```env
AI_API_BASE="https://api.deepseek.com/v1"
AI_API_KEY=""
AI_MODEL="deepseek-chat"
```

兼容 DeepSeek / 通义 DashScope / OpenAI 等 OpenAI 风格接口。

## 2. 三条工作流

| workflow | 输入 | 输出 |
|----------|------|------|
| `optimize_narrative` | 中文草稿 + 目标语种 + 人类备注 | 多语文本 + 叙事建议 |
| `risk_check` | 待发布正文 + 目标国家 | 风险条目（高/中/低）+ 修改建议 |
| `localize` | 文化素材 + 目标国家 + 标题 | 本土化改编 + 要点 |

## 3. 溯源机制（人机边界）

每次调用写入 **AiRecord**：

| 字段 | 含义 |
|------|------|
| `workflow` | 工作流类型 |
| `input` / `output` | 原始入参出参 JSON |
| `model` | 模型或「模型 · 演示模式」 |
| `humanPart` | 人类创意贡献摘要 |
| `aiPart` | AI 辅助生成说明 |
| `watermark` | `AI-assisted · AI辅助生成` |
| `createdAt` | 时间 |

UI 层：

- 结果面板挂「AI 辅助生成」朱印角标  
- 项目页分栏：**人类创意** vs **AI 辅助**  
- 工作台可查阅完整溯源记录  

## 4. 治理原则（产品文案与实现一致）

1. **青年为主、AI 为辅** — 不把 AI 输出当最终成稿  
2. **可追溯** — 输入/输出/模式/时间入库  
3. **风险前置** — 跨文化风险工作流覆盖饮食、宗教、刻板印象等规则  
4. **透明** — Demo / Live 状态在界面可见  

## 5. 未实现（勿在面试夸大）

- 人工反馈对齐 / RLHF / SFT  
- 模型微调与评测集  
- 输出内容的自动查重与版权扫描（仅有工作流与规范文案）  
- 流式 SSE 输出（当前为一次性 JSON 响应）  
