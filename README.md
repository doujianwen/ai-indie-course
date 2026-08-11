# AI 独立开发实战课（课程资产仓）

本仓存放「AI 独立开发实战课」的**知识资产与教学内容产品**，与电子书转换站主站（`github.com/doujianwen/ebook-converter`，线上 bookconv.com）**分离演进**。

## 为什么要分仓

- 课程文档、演示产物、主站生产代码（含付费 webhook / Redis / 兄弟目录杂项）原本挤在同一工作区，`git add .` 极易误提交占位数据或生产配置。分仓后本仓纯净。
- 学员需要一份**可独立 clone 运行的干净代码**；从主站大仓库挖教学切片体验极差。
- 本仓建立时主站 git 历史尚未被课程代码污染，是最佳分仓时机。

## 本仓包含什么

| 目录 | 内容 |
|---|---|
| `prd/` | 产品需求文档（PRD v1.2，含 AC-6 样本书闸门、诚实账本选择性披露、v1.2 范围升级：内容策划 + SEO/GEO 运营 + 防跑偏） |
| `architecture/` | 技术章节大纲（17 章映射 6 Phase）、演示产物 Spec、openapi 契约、ADR-001~003 |
| `design/` | 设计契约（DESIGN.md 9 节）、design-tokens.json（24 条 iconMap）、双主题 tokens.css、三页视觉方向 |

## 什么**不**在本仓（留主站）

「转换兼容性实测报告页生成器」是真实上线的**主站 feature**（`src/lib/compat/*`、`src/data/compat/*`、`src/app/[locale]/compat/*`、`sitemap.ts` 派生），它要给主站交真实 SEO 价值、进 GSC 观察、作为课程 Phase 6 的 showcase。因此它留在主站仓库演进，本仓只引用其结果。

## 未来结构（规划，尚未全部落地）

```
ai-indie-course/
├── prd/            ✅ 已落地
├── architecture/   ✅ 已落地
├── design/         ✅ 已落地
├── course-site/    🔜 课程官网（Landing / 章节列表 / 播放页，规范见 design/page-*.md）
└── student-scaffold/ 🔜 学员练习脚手架（可独立运行的教学切片）
```

## 关键决策记录（已拍板）

1. 文档目录统一到 `prd/architecture/design/`（原 docs/course、docs/ai-course 已并入）。
2. Q2 诚实账本 = **选择性披露**：讲「0 点击怎么诊断」的过程，淡化绝对数字（新站上线 2 周多、无点击是普遍规律）。
3. Q3 陪跑（现 M14）= **全 defer v2**，课程 V1 只交付 self-serve（M1–M10）。
4. 演示产物 `/compat` 仅做 `en`，不做 `es`（与 V1 范围一致）。
5. 预备章 00/0X 计入总时长统计口径。
6. G1 增长复盘截图**隐去绝对数值**再展示。
7. **v1.2 范围升级**：内容策划（M8）+ SEO/GEO 运营增长（M9）+ 防跑偏 / 第三视角纠错机制（M10，借旧版 `ai-independent-dev/correction-agent` 概念）从 v2 提前至 v1 主线；bookconv 已有 SEO/GEO 真实实践与数据（87 页内容体系、一页吃整簇、llms.txt/robots GEO、GSC/GA4 复盘）作为课程核心素材（详见 `architecture/decisions/ADR-004-scope-upgrade.md`）。

## 关联项目

- 主站仓库：`github.com/doujianwen/ebook-converter`（bookconv.com）
- 课程方法论：MVP 开发专家团 6 阶段（需求→调研→Spec→设计→开发→测试→部署→增长）
