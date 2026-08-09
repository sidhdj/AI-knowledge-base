# 知识库索引 INDEX

> 本知识库专为 AI 读取优化。AI Agent 请先阅读 [AI 读取指南](#ai-读取指南) 章节。

---

## 📚 目录结构总览

```
AI-knowledge-base/
├── INDEX.md                     ← 你正在看的文件（目录索引）
├── README.md                    ← 仓库简介
├── topics/                      ← 知识内容主体
│   ├── ai-ml/                   ← 人工智能 & 机器学习
│   ├── programming/             ← 编程语言 & 框架
│   ├── tools/                   ← 开发工具 & 软件
│   ├── writing/                 ← 写作相关
│   │   ├── novels/              ← 小说原文（AI 学习素材）
│   │   └── techniques/          ← 写作技巧笔记
│   ├── images/                  ← 图片素材（AI 视觉学习素材）
│   └── notes/                   ← 杂记 & 想法
├── references/                  ← 参考资料 & 外链
├── templates/                   ← 新建文档的模板
└── .meta/                       ← 元数据 & 机器配置（给 AI 看的）
    ├── config.yaml              ← 知识库配置说明
    └── index.json               ← 机器可读的结构化索引
```

---

## 📖 知识条目速览

### 🤖 topics/ai-ml - 人工智能 & 机器学习

| 标题 | 文件 | 标签 | 更新时间 |
|---|---|---|---|
| RAG 检索增强生成入门指南 | [rag-intro.md](topics/ai-ml/rag-intro.md) | `rag`, `llm`, `example` | 2026-08-09 |

### 💻 topics/programming - 编程语言 & 框架

| 标题 | 文件 | 标签 | 更新时间 |
|---|---|---|---|
| Python 最佳实践速查 | [python-best-practices.md](topics/programming/python-best-practices.md) | `python`, `programming`, `example` | 2026-08-09 |

### 🛠️ topics/tools - 开发工具 & 软件

| 标题 | 文件 | 标签 | 更新时间 |
|---|---|---|---|
| Git 常用命令速查表 | [git-cheatsheet.md](topics/tools/git-cheatsheet.md) | `git`, `tool` | 2026-08-09 |

### ✍️ topics/writing - 写作相关

**novels/** - 小说原文（[查看文件夹](topics/writing/novels/)）

暂无条目

**techniques/** - 写作技巧笔记（[查看文件夹](topics/writing/techniques/)）

| 标题 | 文件 | 标签 | 更新时间 |
|---|---|---|---|
| 芮芮系列手绘漫画 - 画风分析笔记 | [drawing-style-analysis.md](topics/writing/techniques/drawing-style-analysis.md) | `drawing`, `style-analysis`, `manga` | 2026-08-09 |

### 🖼️ topics/images - 图片素材（[查看文件夹](topics/images/)）

| 文件名 | 类型 | 关联笔记 |
|---|---|---|
| 芮芮戴眼镜的大头照.jpg | 单人画像 | [画风分析](topics/writing/techniques/drawing-style-analysis.md) |
| 芮芮与弟弟跑累了坐在长椅上休息.jpg | 双人场景 | [画风分析](topics/writing/techniques/drawing-style-analysis.md) |
| ai-generated-girl-reading-book.jpg | AI 生成示例 | [画风分析](topics/writing/techniques/drawing-style-analysis.md) |

### 📝 topics/notes - 杂记 & 想法

暂无条目

---

## 🏷️ 标签索引

按标签快速查找相关知识：

| 标签 | 关联条目 |
|---|---|
| `rag` | [RAG 检索增强生成入门指南](topics/ai-ml/rag-intro.md) |
| `llm` | [RAG 检索增强生成入门指南](topics/ai-ml/rag-intro.md) |
| `python` | [Python 最佳实践速查](topics/programming/python-best-practices.md) |
| `programming` | [Python 最佳实践速查](topics/programming/python-best-practices.md) |
| `git` | [Git 常用命令速查表](topics/tools/git-cheatsheet.md) |
| `tool` | [Git 常用命令速查表](topics/tools/git-cheatsheet.md) |
| `example` | [RAG 检索增强生成入门指南](topics/ai-ml/rag-intro.md) · [Python 最佳实践速查](topics/programming/python-best-practices.md) |
| `drawing` | [芮芮系列手绘漫画 - 画风分析笔记](topics/writing/techniques/drawing-style-analysis.md) |
| `style-analysis` | [芮芮系列手绘漫画 - 画风分析笔记](topics/writing/techniques/drawing-style-analysis.md) |
| `manga` | [芮芮系列手绘漫画 - 画风分析笔记](topics/writing/techniques/drawing-style-analysis.md) |
| `hand-drawn` | [芮芮系列手绘漫画 - 画风分析笔记](topics/writing/techniques/drawing-style-analysis.md) |
| `art-technique` | [芮芮系列手绘漫画 - 画风分析笔记](topics/writing/techniques/drawing-style-analysis.md) |

---

## 🤖 AI 读取指南

### 推荐阅读顺序

1. **第一步**：阅读 [.meta/config.yaml](.meta/config.yaml) 了解知识库配置和结构
2. **第二步**：读取 [.meta/index.json](.meta/index.json) 获取结构化的条目索引（比本文件更适合机器解析）
3. **第三步**：根据查询需求，定位到具体的知识条目文件

### 知识条目格式

每个知识条目文件开头都有 **YAML Front Matter** 元数据块，格式如下：

```markdown
---
title: "文档标题"
summary: "一句话摘要，便于快速理解内容"
created: "2026-08-09"
updated: "2026-08-09"
tags: ["tag1", "tag2"]
related: ["../relative/path/to/related.md"]
source: "可选，信息来源链接"
author: "可选，作者"
confidence: "optional: high/medium/low，信息可信度"
---

# 正文标题

正文内容从这里开始...
```

### 检索策略建议

- **主题检索**：先看 `INDEX.md` 的分类目录，找到对应 topic 文件夹
- **关键词检索**：使用 `index.json` 中的 `tags_index` 快速匹配标签
- **关联扩展**：读完一个条目后，查看 `related` 字段跳转相关知识
- **快速预览**：读取 `index.json` 中的 `summary` 字段，无需打开全文

---

## 📝 添加新知识

请使用 [templates/entry-template.md](templates/entry-template.md) 模板创建新条目，并：
1. 选择合适的 topic 目录存放
2. 填写完整的 Front Matter 元数据
3. 更新本 INDEX.md 中的条目表格和标签索引
4. 更新 [.meta/index.json](.meta/index.json) 的机器索引
