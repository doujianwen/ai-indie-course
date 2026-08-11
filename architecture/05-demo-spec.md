# 演示产物 Spec：转换兼容性实测报告页生成器

> 版本：v1.0（Phase 3 Spec 细化产物，待用户确认后进入 Phase 5 实现）
> 适用范围：课程 M6 主线实战产物 + 主站真实 SEO 增量
> 依赖前置：`openapi.yaml`、`02-demo-candidates.md`、`03-constraints.md`，本文件与之一致生效

---

## 0. 定位与边界

这是专家团 6 阶段现场重跑并录制的对象，同时给 bookconv 主站交付真实 SEO 价值。它从候选 ①②③ 中胜出，理由已在 `02-demo-candidates.md` 写明：唯一 6/6 覆盖全部 Phase、唯一命中 bookconv 四项核心资产（App Router / 单源数据注册 / SEO 派生 / Vercel）、录制可控、效果可测量。

**反 Google 规模化内容政策的破解方式（课程第 15 章红线素材）**：每页承载 `verifyConversion()` 真跑出来的**独有数据**，不是 LLM 写的相似文章。这是别人抄不走的原始素材，也是本课程相对同类课的稀缺差异点。

边界：本产物**不重建转换管线**，只复用现有 Calibre 管线和 `verifyConversion()`。

---

## 1. 三条设计原则

1. **纯构建时生成，产物是静态数据文件**。线上 `/compat/[slug]` 只读静态数据，**零运行时转换、零新增依赖、零外部计费**。生成脚本在本地一次性跑完，结果固化为 `src/data/compat/*.ts` 提交仓库。这保证录制可控（无长任务、无 worker 掉线、无烧钱 API）。
2. **每页数据必须来自机器实测**。报告页只写 `verifyConversion()` 产出的结果，不写任何推测值（对齐 P0 规则 3：页面每个数字都有来源）。
3. **样本书闸门（AC-6）**。只用 Project Gutenberg 公共领域书目；生成后人工审核再提交；首批 ≤ 30 页；GSC 观察两周再扩量。

---

## 2. 数据模型（对齐 `openapi.yaml`，TS interface 禁止 `any`）

```ts
// src/lib/compat/schema.ts
export type Verdict = 'pass' | 'warn' | 'critical';

export interface CompatSample {
  title: string;            // 实测书目，如 "Alice's Adventures in Wonderland"
  source: string;           // 如 "Project Gutenberg #11"
  sizeBytes: number;
  bootstrapped: boolean;    // false=Gutenberg 原始格式样本；true=经 Calibre 引导生成
  bootstrapPath?: string;   // bootstrapped=true 时记录引导路径，便于追溯
}

export interface CompatCheck {
  id: string;               // 对应 verifyConversion 的 Verdict.id
  label: string;            // 中文可读标签，如 "目录结构保留"
  severity: 'critical' | 'warn';
  passed: boolean;
  detail: string | null;    // 来自 Verdict.message，如 "检测到 12 个目录项，转换后保留 12 个"
}

export interface CompatReport {
  slug: string;             // 如 "epub-to-mobi-on-kindle-paperwhite"
  sourceFormat: string;
  targetFormat: string;
  verdict: Verdict;         // 取自 verifyConversion 的整体判定
  testedAt: string;         // ISO date-time
  engineVersion: string;    // 如 "calibre 7.21.0"，保证可复现
  sample: CompatSample;
  outputSizeBytes: number;
  checks: CompatCheck[];
}

// 注册表：明确类型，不许照抄 CONTENT_MAP 的 Record<string, any>
export const COMPAT_MAP: Record<string, CompatReport> = { /* ... */ };
```

`CONTENT_MAP: Record<string, any>` 的 `any` 是课程第 9 章当场纠正的反面样例；本产物作为正确示范，类型必须显式。

---

## 3. 生成器算法

入口：`scripts/generate-compat.ts`（CLI，本地一次性运行；**不进生产运行时**）。

```
STEP 1  载入样本清单       读取 scripts/compat-samples.json（AC-6 闸门来源）
STEP 2  跑转换            调 Calibre（复用 src/lib/conversion.ts 的转换入口）
                           输入=样本文件，输出=临时文件
STEP 3  纠察              verifyConversion(inputPath, outputPath, srcFmt, tgtFmt)
                           → VerificationResult { pass, findings: Verdict[] }
STEP 4  映射 checks       Verdict[] → CompatCheck[]
                           - id         : 原样保留
                           - label      : 查 LABEL_MAP（见下），缺失则回退 id
                           - severity   : 原样保留
                           - passed     : !(该 id 出现在 findings 中)
                           - detail     : 对应 Verdict.message
STEP 5  推导 verdict      pass===true → 'pass'
                           否则有 critical 级 finding → 'critical'
                           否则 → 'warn'
STEP 6  写文件            emit src/data/compat/<slug>.ts，导出 const report: CompatReport
                           并自动更新 src/data/compat/index.ts 的 import + COMPAT_MAP
```

`LABEL_MAP` 覆盖 `verifyConversion` 已知 id：`no-output`、`empty-output`、`format-unverified`、`format-mismatch`、`content-loss`、`mojibake`、`image-loss`（如缺可补，不猜测）。

**关键约束**：STEP 4 中若 `findings` 为空（pass），仍要产出全部 checks 且 `passed=true`——报告页要展示"测了哪些项、全部通过"，这才是"独有数据"的价值。

---

## 4. 目录结构（照搬 `03-constraints.md` 第四节，补职责）

```
src/data/compat/
├── epub-on-kindle-paperwhite.ts   单个实测报告静态数据（emit 产物）
├── ...                            首批 ≤ 30 个
└── index.ts                       COMPAT_MAP 注册表（照 content/index.ts 模式）

src/lib/compat/
├── schema.ts        zod + TS interface（禁止 any）
├── generator.ts     调 verifyConversion 产出实测数据（被脚本调用）
└── report.ts        VerificationResult → CompatReport 视图模型映射

src/app/[locale]/compat/[slug]/page.tsx   展示页，generateStaticParams 取自 COMPAT_MAP

scripts/
├── generate-compat.ts    生成器 CLI（一次性，本地）
└── compat-samples.json   样本清单（AC-6 闸门）

src/app/sitemap.ts         新增一段 Object.keys(COMPAT_MAP) 派生
```

单文件 ≤ 300 行；route/page 不写业务逻辑，只调 `src/lib/compat`。

---

## 5. 展示页

`src/app/[locale]/compat/[slug]/page.tsx`：
- `generateStaticParams` 取自 `COMPAT_MAP`（与 convert 路由同源模式）。
- 渲染报告卡片：样本信息、引擎版本、逐项 checks（lucide 图标 + label + 通过/失败状态）、verdict 徽章。
- 图标唯一来源 `lucide-react`（P0 规则 1）；`passed` 用 `Check` / 失败用 `AlertTriangle`，语义图标显式 `aria-label`。
- 无 emoji、无渐变主视觉（P0 规则 2）、无 AI 模板味文案（P0 规则 3）。
- 复用现有 `/convert/[slug]` 的卡片布局风格与 Tailwind token，不引入新视觉语言。

---

## 6. SEO 派生改动点

`src/app/sitemap.ts` 在现有 `for (const key of CONVERSION_PAGES)` 段之后，新增：

```ts
import { COMPAT_MAP } from '@/data/compat';
const COMPAT_PAGES = Object.keys(COMPAT_MAP);
// 在 locales 循环内、转换页段之后追加：
for (const key of COMPAT_PAGES) {
  allUrls.push({
    url: baseUrl + prefix + '/compat/' + key,
    lastModified: new Date(),
    changeFrequency: 'monthly' as const,
    priority: 0.7,
  });
}
```

不改动现有 `CONTENT_MAP` 派生逻辑（历史 slug bug 注释保留作教案）。生成页与转换页共用同一套"单一数据源派生"范式，正是课程第 9 章要讲的可迁移模式。

---

## 7. 样本书闸门（AC-6 落地）

`scripts/compat-samples.json` 结构：

```json
{
  "gate": { "maxPages": 30, "requireHumanReview": true, "gutenbergOnly": true },
  "samples": [
    { "slug": "epub-to-mobi-on-kindle-paperwhite", "sourceFormat": "epub",
      "targetFormat": "mobi", "sample": { "title": "Alice's Adventures in Wonderland",
      "source": "Project Gutenberg #11", "bootstrapped": false } }
  ]
}
```

- `gutenbergOnly=true`：样本源只能是 Project Gutenberg 公共领域书目，规避版权。
- `requireHumanReview=true`：脚本 emit 后**不自动提交**，需在 PR/审核中人工确认数据真实再合入。
- `maxPages=30`：硬上限，超量报警。
- 选书优先级：覆盖主站高频转换对（mobi↔epub 为 P0）+ 多语言样本（验证 mojibake 检查项）。

---

## 8. 验收标准

| # | 标准 | 检查方式 |
|---|------|----------|
| V1 | `COMPAT_MAP: Record<string, CompatReport>`（非 any） | tsc |
| V2 | 零新增第三方依赖（依赖新增数=0） | `package.json` diff |
| V3 | 每页 `checks` 非空且来自 `verifyConversion` 实测 | 数据文件审查 |
| V4 | `sitemap.ts` 含 `/compat/*` 派生 | 构建后 sitemap 断言 |
| V5 | 源码无 emoji / 无紫粉渐变 / 无模板味文案 | `check-no-emoji.sh` + Review |
| V6 | 单文件 ≤ 300 行 | CI 门禁 |
| V7 | 首批 ≤ 30 页且经人工审核闸门 | `compat-samples.json` 闸门 |
| V8 | 与 `openapi.yaml` schema 同源（字段一致） | 对照审查 |

---

## 9. 录制脚本（每步对应课程章）

| 录制步骤 | 对应章 | 内容 |
|----------|--------|------|
| 立项：从 GSC query 选格式对 | 第 1-2 章 | 需求澄清 |
| 写 `schema.ts` + `openapi.yaml` 对齐 | 第 3-5 章 | Spec 驱动 |
| 设计报告卡片组件 | 第 6-7 章 | UI/UX |
| 实现 `generator.ts` / `report.ts` / 页面 | 第 8-11 章 | 前后端开发 |
| `check-no-emoji.sh` + 类型检查 + 快照测试 | 第 12-13 章 | 测试门禁 |
| `sitemap.ts` 派生 + Vercel 部署 + GSC 观察 | 第 14-17 章 | 部署与增长 |
| **保留失败镜头**：AI 写错、Calibre 报错、测试挂 | 全程 | 差异化素材 |

---

## 10. 风险对应

- **R1（Google 规模化内容政策，高）**：每页独有实测数据 + ≤30 页 + 人工闸门 + GSC 观察两周。本 Spec 第 1/3/7 节已落地。
- **R5（样本不足退化为编造，高）**：样本清单进 `compat-samples.json` 闸门（AC-6），**样本不齐不运行生成器**——脚本启动即校验清单完整性，缺项直接 fail。
- R2/R3/R4 已在 `03-constraints.md` 处置，本产物不新增。

---

## 已确认（Phase 3 收口项，2026-08-11）

1. 样本清单 `compat-samples.json` 的具体书目由 PM 在 PRD 验收标准 AC-6 下最终定稿（首批 30 个 slug）。
2. **`/compat/[slug]` 仅做 `en` 版，不做 `es` 西语版（已定）**。理由：与课程 V1 范围一致（en-only）。Next.js localePrefix as-needed 下 en 无前缀；页面仍放 `src/app/[locale]/compat/[slug]/page.tsx`，但 generation 只产出 en 数据、sitemap 只派 en URL。es 延后至有需求时。
