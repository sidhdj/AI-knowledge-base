---
title: "RAG 检索增强生成入门指南"
summary: "介绍 RAG 的基本概念、工作流程、核心组件和简单实现方式"
document_created_at: "2026-08-09"
document_updated_at: "2026-08-09"
tags: ["rag", "llm", "example"]
related:
  - "../programming/python-best-practices.md"
source: "综合整理自 LangChain 文档和多篇博客"
author: "知识库维护者"
confidence: "high"
---

# RAG 检索增强生成入门指南

> 💡 RAG (Retrieval-Augmented Generation) 是让大模型接入私有/外部知识的主流方案。

---

## 📌 核心概念

**RAG = 检索 (Retrieval) + 生成 (Generation)**

在大模型回答问题之前，先从知识库中检索相关文档，把这些文档内容作为上下文一起喂给 LLM，让它基于提供的资料生成答案。这样可以：
- ✅ 解决 LLM 知识过时问题（幻觉）
- ✅ 接入私有数据，无需微调
- ✅ 答案有据可查（可附带来源）

---

## 📖 详细内容

### 1. 标准工作流程

```
用户提问
   ↓
① 查询改写 (Query Transformation) — 可选
   ↓   把用户问题改写成更适合检索的形式
② 向量检索 (Retrieval)
   ↓   用 Embedding 模型把问题转成向量，去向量库找 Top-K 相似文档
③ 重排序 (Rerank) — 可选
   ↓   用更精细的 Cross-Encoder 模型重新排序候选文档
④ 上下文拼接 (Context Building)
   ↓   把选出来的文档块拼成 Prompt 的上下文部分
⑤ LLM 生成 (Generation)
   ↓   System Prompt + Context + User Question → 答案
⑥ 带引用输出 (Answer with Citations)
   最终答案标注引用了哪些文档片段
```

### 2. 核心组件

| 组件 | 作用 | 常见选型 |
|---|---|---|
| **文档切分 (Chunking)** | 把长文档切成适合 Embedding 的块 | RecursiveCharacterSplitter, 按 Markdown 标题切 |
| **Embedding 模型** | 把文本转成向量 | OpenAI text-embedding-3-small, BGE-M3, Cohere Embed |
| **向量数据库** | 存储向量并做相似度搜索 | Chroma, FAISS, Milvus, pgvector, Qdrant |
| **LLM** | 最终生成答案 | GPT-4o, Claude 3.5 Sonnet, 国产模型等 |
| **Reranker** (可选) | 提升检索精度 | Cohere Rerank, BGE-Reranker |

### 3. 文档分块的经验

- **块大小**：通常 500-1000 tokens（中文约 300-800 字）
- **重叠 (Overlap)**：10-20%，避免跨块上下文断裂
- **优先按结构切**：Markdown 按 `##` 标题切，比单纯按字符切效果好很多
- **保留元数据**：每个块带上来源文件、章节标题、页码等信息

---

## ⚠️ 常见坑

- **切块太小**：语义不完整，检索到了也没法回答
- **切块太大**：噪声多，占用上下文窗口，LLM 抓不住重点
- **只看 Top-3**：关键信息可能排在第 4、5 位，适当放宽后再用 Reranker
- **忽略元数据过滤**：可以先按标签/时间过滤再做向量检索，更高效
- **Prompt 不强调引用**：LLM 容易把检索到的内容和自己的知识混在一起，要在 System Prompt 里明确要求"只根据提供的上下文回答，不知道就说不知道"

---

## ✅ 最佳实践

1. **先做基线，再优化**：先跑通 `简单切分 + 向量检索 + LLM` 的最小流程，再加 Query 改写、Rerank 等高级功能
2. **要有评测集**：准备 20-50 个标准问答对，用命中率、答案正确率等指标量化每次改动的效果
3. **混合检索 (Hybrid Search)**：向量检索 + 关键词检索（BM25）结合，对专有名词、数字、代码效果更好
4. **缓存常用问题**：对高频问题直接缓存答案，省 token 又快
5. **给用户展示引用来源**：提升信任感，也方便排查错误

---

## 🔗 相关资源

- LangChain 官方 RAG 教程: https://python.langchain.com/docs/tutorials/rag/
- [Python 最佳实践速查](../programming/python-best-practices.md) — 实现 RAG 系统时的代码规范参考
