# AI-knowledge-base

> 一个专为 AI Agent 优化的个人知识库。结构化、带索引、方便读取和检索。

---

## 特点

- **双索引系统**：人类可读的 `INDEX.md` + 机器可读的 `.meta/index.json`
- **结构化元数据**：每个条目都有 YAML Front Matter（标题、摘要、标签、关联、可信度）
- **分类目录**：按主题分区（AI/ML、编程、工具、笔记、政治）
- **标准模板**：创建新条目直接套用 `templates/` 下的模板
- **AI 读取指南**：告诉 AI Agent 如何高效遍历和检索本知识库

---

## 快速开始

### 浏览知识库

直接打开 [INDEX.md](INDEX.md) 查看目录和所有条目。

### 给 AI 用

如果您是 AI Agent / 大模型助手，请按以下顺序读取：

1. **首选**：读取 `.meta/index.json` — 结构化索引，最适合机器解析
2. **然后**：读取 `.meta/config.yaml` — 了解知识库配置和条目 Schema
3. **最后**：按索引定位到具体条目文件，解析 Front Matter + 正文

### 添加新条目

1. 复制 `templates/entry-template.md` 或 `templates/cheatsheet-template.md`
2. 放到对应 `topics/` 子目录下
3. 填写 Front Matter 元数据（必填：title, created, updated, tags）
4. 在 `INDEX.md` 中添加条目
5. 更新 `.meta/index.json` 的 `entries`、`tags_index`、`topics` 三个字段

---

## 目录结构

```
AI-knowledge-base/
├── INDEX.md                     ← 人类目录索引
├── README.md                    ← 本文件
├── topics/                      ← 知识内容
│   ├── ai-ml/                   ← AI & 机器学习
│   ├── programming/             ← 编程语言 & 框架
│   ├── tools/                   ← 开发工具
│   ├── writing/                 ← 写作相关
│   │   ├── novels/              ← 小说原文（AI 学习素材）
│   │   └── techniques/          ← 写作技巧笔记
│   ├── images/                  ← 图片素材（AI 视觉学习素材）
│   │   ├── black-white/         ← 黑白手绘作品（原创）
│   │   ├── color/               ← 彩色作品（原创）
│   │   └── reference/           ← 外部参考图（非原创，风格学习用）
│   │       └── zzzero/          ←   绝区零 官方立绘
│   ├── political/               ← 政治 & 党建
│   ├── tech/                    ← 科技 & 硬件参数
│   └── notes/                   ← 杂记
├── inbox/                       ← 临时缓存区（文件中转站）
├── references/                  ← 参考资料
├── templates/                   ← 新建条目模板
│   ├── entry-template.md
│   └── cheatsheet-template.md
└── .meta/                       ← AI 配置区
    ├── config.yaml              ← 知识库 Schema & 配置
    └── index.json               ← 机器可读索引
```

---

## 设计说明

为什么这样设计？

1. **Front Matter**：让 AI 无需读取全文即可判断内容相关性
2. **标签系统**：`tags` 字段支持 AI 做语义聚类和关联检索
3. **related 字段**：构建知识图谱的链接关系
4. **confidence 字段**：区分权威信息 vs 个人笔记，帮助 AI 判断可靠性
5. **双索引**：人类和 AI 各取所需，互不干扰
