# 课程 V1 章节 Spec 骨架

> 版本：v1.0（Phase 3 产物，待确认后进入录制/实现）
> 来源：`01-tech-outline.md`（17 章）+ PRD v1.1（M1–M7 模块）+ `openapi.yaml` + `05-demo-spec.md`
> 范围：课程 V1（self-serve，M1–M7 全含；陪跑 M12 defer v2）

---

## 1. 模块 ↔ 章节 归属

课程产品层 7 个模块（PM RICE 排序）与 17 章技术大纲的咬合关系：

| 模块 | 主题 | 覆盖章节 | 性质 |
|------|------|----------|------|
| M5 | 环境与工具链 | 预备章（环境搭建 / 工具链升级策略）| 零基础友好，对应差评高频词「过时」 |
| M2 | 六阶段方法论主线串讲 | 方法论总览（开篇 1 节）| 贯穿全程的骨架引入 |
| M1 | Spec 驱动开发 | 第 1–5 章（需求 / PRD / 调研 / 选型 / Spec）| 课程差异化核心 |
| M3 | AI 代码安全与验收 | 第 2 章(验收标准) + 第 11 章(审查) + 第 13 章(CI 门禁) | 对应 82.8% 漏洞率 |
| M6 | 真实项目从零重跑 | 第 6–10 章 + 第 12 章 + 第 14–17 章 | 演示产物全程（实战 spine） |
| M4 | 上线后增长诊断 | 第 15–17 章增长视角 + GSC/GA4 真实复盘专题 | 选择性披露（Q2 已定） |
| M7 | 图文速查手册 | 全部章节图文版 + 附录 | 交付物，非独立章 |

> M6 与 M4 共享第 14–17 章素材：**M6 是「重跑实操」视角，M4 是「上线后怎么办」增长视角**，不重复录制，同一批素材换讲解角度。

---

## 2. 章节明细（V1 全 17 章 + 预备章）

字段：编号 / 标题 / Phase / 类型(视频·图文·实战) / 预估时长 / 核心演示操作 / `lesson-XX` 命名 / chapters 锚点源

### 预备章 · M5
- **00 环境与工具链**：Phase 0 / 图文+视频 / 18min。演示：装 Node(LTS) + Vercel CLI + Calibre + IDE(GitHub Dark Default) 配置；讲「工具升级了怎么办」（稳定层/易变层分离，对应 PRD 抗过时设计）。`lesson-00` ← 锚点：环境清单、升级策略。

### M2 方法论总览
- **0X 六阶段方法论串讲**：Phase 0 / 视频 / 10min。演示：6 Phase 横向时间轴（需求→调研→Spec→设计→开发→测试→部署→增长）。`lesson-0x` ← 锚点：6 节点。

### M1 · Phase 1–2（第 1–5 章）
- **01 从想法到可验收需求**：Phase 1 / 视频 / 12min。演示：开 GSC 后台用真实 query 反推需求；展示 sitemap 收录差距。`lesson-01` ← 锚点：需求澄清提问清单。
- **02 写一份 AI 能执行的 PRD**：Phase 1 / 视频 / 12min。演示：现场写演示产物 PRD，对比「模糊 PRD」vs「带 AC 的 PRD」生成差异。`lesson-02` ← 锚点：PRD 模板、AC 写法。
- **03 技术调研正确姿势**：Phase 2 / 视频 / 11min。演示：反例搜「Next vs Remix」vs 正例查 Vercel 函数时长限制 / Lucide 1.0 brand icons 移除。`lesson-03` ← 锚点：调研记录模板。
- **04 MVP 技术选型**：Phase 2 / 视频 / 12min。演示：开 bookconv `package.json` 逐条讲依赖；锁定 lucide-react 进 Spec；讲 emoji 为何不能当图标。`lesson-04` ← 锚点：选型决策矩阵。
- **05 Spec 即契约**：Phase 2 / 视频 / 12min。演示：现场写 `openapi.yaml`；展示 Redoc；讲「存量无 /api/v1/ 前缀、新增遵循」。`lesson-05` ← 锚点：openapi 骨架、ADR 模板。

### M3 · 第 11/13 章（穿插）
- **11 AI 写的代码哪里最容易出事**：Phase 4 / 视频 / 12min。演示：现场审 `auth/storage.ts`，找 `providedKey` 死代码 + 非常量时间比较，当场修。`lesson-11` ← 锚点：AI 代码审查清单。
- **13 把规则变成 CI 门禁**：Phase 5 / 视频 / 10min。演示：把 emoji 扫描 + 单文件行数接进 workflow。`lesson-13` ← 锚点：CI 配置。

### M6 · 第 6–10 / 12 / 14–17 章（演示产物主线）
- **06 设计规范先行**：Phase 3 / 视频 / 12min。演示：抽 bookconv Tailwind 色板；LucideProvider 统一 size；**跑 emoji 扫描命中 12 文件当场整改**。`lesson-06` ← 锚点：Token 映射、P0 整改。
- **07 设计稿变组件清单**：Phase 3 / 视频 / 11min。演示：拆演示产物页面为组件树 + props 契约交 AI。`lesson-07` ← 锚点：组件清单模板。
- **08 项目结构与组织约束**：Phase 4 / 视频 / 11min。演示：`wc -l` 查 `queue.ts` 661 行超约束，讲拆分。`lesson-08` ← 锚点：目录分层图。
- **09 单源数据 + 注册表模式**：Phase 4 / 视频 / 12min。演示：`content/*.ts`→`index.ts`→页面→`sitemap` 全链路；讲 `epub-docx` slug bug 教案；顺修 `CONTENT_MAP` 的 `any`。`lesson-09` ← 锚点：注册表模式图。
- **10 后端与 API 实现**：Phase 4 / 视频 / 12min。演示：读 `payments/webhook/route.ts` 讲 `custom_data.email` 订阅键防御。`lesson-10` ← 锚点：zod 校验、响应规范。
- **12 测试写多少才够**：Phase 5 / 视频 / 11min。演示：跑 bookconv 四类测试（unit/boundary/e2e/performance）。`lesson-12` ← 锚点：测试策略清单。
- **14 部署到 Vercel 与环境边界**：Phase 6 / 视频 / 12min。演示：讲 `.env.example` 分层；`docker-compose` + worker 讲 serverless 时长倒逼架构。`lesson-14` ← 锚点：环境变量清单。
- **15 SEO 是架构问题**：Phase 6 / 视频 / 12min。演示：走查 `sitemap.ts` 双 locale 派生 / `llms.txt` / robots 放行 GPTBot；演示 IndexNow 提交。**第 15 章专讲 AI 批量造内容红线（R1 现身说法）。**`lesson-15` ← 锚点：sitemap 派生图。
- **16 付费链路与数据闭环**：Phase 6 / 视频 / 12min。演示：checkout→webhook→Redis 订阅态→Pro 门禁；开 GA4 真实事件；**真实待办：内存 Map 迁持久化，课程真做完**。`lesson-16` ← 锚点：付费链路图。
- **17 灰度发布与迭代**：Phase 6 / 视频 / 10min。演示：给演示产物加 Feature Flag 5%→50%→100%。`lesson-17` ← 锚点：放量策略。

### M4 · 增长诊断（第 15–17 章增长视角 + 复盘专题）
- **G1 上线后没人用怎么办（GSC/GA4 复盘）**：Phase 6 / 视频+图文 / 14min。**选择性披露（Q2 已定）**：讲「0 点击怎么诊断」的过程（收录/排名/query 覆盖/技术 SEO 排查），淡化绝对数字；新站上线 2 周多无点击是普遍规律。**截图脱敏（决策#3 已定）**：GSC/GA4 看板截图只呈现诊断路径、趋势形状、结构（收录曲线/query 覆盖矩阵），绝对展示数/点击数/用户数做马赛克或占位处理。`lesson-g1` ← 锚点：诊断清单、GSC 看板截图（脱敏版）。

---

## 3. 录制与锚点契约

- 每节录制完必须产出 `lesson-XX.chapters.json`（DESIGN.md 附录 A.8），是视频↔图文唯一连接点。
- `type` 取值：`step`(circle-dot) / `prompt`(message-square-code) / `pitfall`(triangle-alert)，驱动播放页时间轴图标。
- **失败镜头保留**：AI 写错 / 构建报错 / 测试挂掉不剪（课程差异化）。
- 分支 `course/ch-NN-<topic>`，录制前打 tag；env 用 `.env.example` 占位，真实 key 不进画面。

## 4. V1 范围边界

- **包含**：M1–M7 全部（上表 00 + 0X + 01–17 + G1），约 20 节，视频实战为主、图文为辅。
- **defer v2**：M8 付费闭环深度 / M9 i18n·SEO 系统 / M10 Sentry 可观测 / M11 BullMQ 队列工程 / M12 30 天陪跑。
- **演示产物**：`05-demo-spec.md` 定义的「转换兼容性实测报告页生成器」即 M6 主线实战对象，上线后进 GSC 观测即 G1 素材。

## 5. 已确认（Phase 3 收口项，2026-08-11）

1. **预备章 00（环境）与 0X（方法论总览）计入总时长统计口径（已定）**。总时长 = 00 + 0X + 01–17 + G1 全节累加；对外口径可单列「核心 17 章」与「含预备章全量」，但内部排期与完课率北极星按全量统计。
2. **G1 复盘的 GSC/GA4 截图需隐去绝对数值再展示（已定）**。截图只呈现诊断路径、趋势形状、结构（收录曲线/query 覆盖矩阵），绝对展示数/点击数/用户数做马赛克或占位处理；讲解聚焦「怎么诊断」过程。
3. **课程页 `es` 版 v1 不做（已定，与决策#1 一致）**：演示产物 /compat 仅 en，课程 V1 整体仅 en，es 延后至 v2 范围。
