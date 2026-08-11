# 页面规格：章节列表页

> 源：DESIGN.md §3/§4/§8 + pages-visual-direction.md「二」
> 寄存器：Product（设计服务内容）· 主题：Light · DESIGN_VARIANCE=4 / DENSITY=6
> 角色：学习端，学员每天来的唯一决策是「上次学到哪、下一步点哪」→ 扫描效率优先

## 布局

```
┌─ 顶栏 64px（右侧换成头像 + 进度） ──────────────┐
├──────────────┬──────────────────────────────────┤
│ 左 240px sticky│ 右 主区 max 840px，左对齐不居中  │
│ 课程名 2xl/590 │ 模块头 + 进度条                  │
│ 环形进度 68px  │ lesson 行（divide-y 行式）       │
│  mono 42%     │ 模块间 48px 负空间分组            │
│ [继续学习]     │                                  │
│  Primary 全宽  │ 模块 02 …                        │
│ ─ 资料下载     │                                  │
└──────────────┴──────────────────────────────────┘
```

## Lesson 行（核心组件，与播放页/ Landing 同组件三密度变体：56 / 40 / 56px）

- 高度 **56px**，`grid-template-columns: 20px 1fr auto auto`，gap 12px，padding 0 20px
- 分隔 `border-bottom: 1px --border-soft`；模块间用 **48px 负空间分组，不用卡片包裹**
- 序号 mono 13px `--meta` `tabular-nums`
- 标题 16px / `--weight-emphasize` / `--fg`；已完成标题降 `--muted`（视觉沉下去）
- 类型 pill：1px 边框、`--radius-pill`、12px、字距 0.02em、**只描边不填色**，颜色取 `--track-*`
- 时长 mono 13px `--meta` `tabular-nums`（纵向对齐成一列）
- hover：整行 `background: --surface-warm`（140ms），最右淡入 `继续 →` ghost 按钮
- current 行：`background: --accent-wash`，图标 `circle-dot` 着 `--accent`
- locked 行：整行 `opacity: 0.55`，点击不跳转而是底部升起购买条

## 模块头

一行：mono 模块号（`--meta`）+ 模块标题（20px / 590）+ 右侧「5/7 节 · 48 分钟」(14px `--meta`) + `chevron-down`。下方紧贴 2px 全宽进度条（`--progress-rail` 底 / `--progress-fill` 填）。**不显百分比数字**，悬停出 tooltip。

## 图标映射（design-tokens iconMap）

| 含义 | 图标 | 颜色 |
|------|------|------|
| 已完成 | `circle-check` | `--success` |
| 当前 | `circle-dot` | `--accent` |
| 未开始 | `circle` | `--meta` |
| 锁定 | `lock` | `--meta` |
| 视频节 | `play-circle` | `--track-video` |
| 图文节 | `file-text` | `--track-doc` |
| 实战节 | `square-terminal` | `--track-practice` |

所有状态图标统一 20px，视为状态不占 accent 配额，但必须同色同尺寸，禁彩色泛滥。

## 五态

| 态 | 表现 |
|----|------|
| Loading | 7 行 skeleton，复用真实 56px 行高（防 CLS），微光扫过 1.4s |
| Empty | `book-open` 48px `--meta` + 「你还没有开始任何课程」+ `去看课程大纲` Secondary |
| Error | `triangle-alert` 24px `--danger` + 「进度同步失败，本地记录已保留」+ `重试` Secondary |
| Populated | 如上 |
| Edge | 超长标题 `text-overflow: ellipsis` + hover title；模块 >15 节默认折叠只展当前 |

## 移动端 <768px

左栏收顶部 sticky 条（课程名 + 细进度条 + `继续学习` 文字按钮）；行高 64px；类型 pill 与时长换行第二行（14px），触摸目标 ≥44px。
