# Cognitive Guardrails

> AI 的认知护栏 — 防止 AI 顺着你说话

这是 MobiQuant Intelligent 系统中最核心的模块之一。它解决一个所有用 AI 做投资分析的人都会遇到的问题：**AI 会顺着你的暗示方向找论据，导致高买低卖。**

## 模块组成

| 组件 | 功能 |
|------|------|
| Intent Isolation | 识别用户暗示方向，主动忽略 |
| Thesis Anchor | 回溯原始建仓论点，检查失效条件 |
| Direction Check | 结论方向独立性自检 |
| Flip Guard | 短期翻转拦截，要求新证据 |
| Anti-Confirmation | 主编排器，串联以上组件 |

## 独立使用

这个模块已发布为独立的 Claude Code Skill，可以单独安装使用：

**GitHub**: [github.com/0x43e96f/cognitive-guardrails](https://github.com/0x43e96f/cognitive-guardrails)

```bash
claude install github.com/0x43e96f/cognitive-guardrails
```

不使用 Claude Code 的用户也可以直接复制 prompt 版本喂给任何 AI（Claude / ChatGPT / Gemini / Kimi），详见上面的 repo。

## 在 MQI 系统中的位置

```
Layer 2: Intelligence（智能层）
  └── cognitive_guardrails/    ← 你在这里
       ├── 意图隔离
       ├── 论点锚定
       ├── 方向独立性检查
       └── 翻转拦截
```

认知护栏横切整个智能层 — 所有分析类工作流（/analyze、/trade、/debate、/impact）在输出结论前都会经过这个模块的检查。
