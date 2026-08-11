# 页面规格：单节播放页

> 源：DESIGN.md §3/§4/§6/§8 + pages-visual-direction.md「三」
> 寄存器：Product · 主题：**Dark 默认** · DESIGN_VARIANCE=4 / DENSITY=5
> 角色：学习端，同时装「视频」与「图文」，靠「关键节点时间轴」缝合两形态

## 布局 ≥1280 三栏

```
顶栏 56px：← 返回大纲 | 模块 03 · 第 2 节 | 主题切换
 ▔▔▔▔▔▔▔▔▔▔░░░░░░░░░░  ← 2px 阅读进度条（跟正文滚动）
├──────┬─────────────────────────────┬──────────────┤
│ 280px│ 主区 max 1120               │ 260px        │
│ 章节  │ 16:9 播放器（无装饰边框）   │ 本节大纲+资源│
│ 导航  │ ── 关键节点时间轴 ──        │              │
│ (40px │ ── 图文正文 68ch 17/1.75 ── │ git-branch   │
│ 行高) │ [代码块][提示词块][踩坑块]  │ download     │
│ 当前  │ ┌ 上一节 ┐  ┌ 下一节 ┐      │ list-checks  │
│ -wash │ [标记完成] Primary          │              │
└──────┴─────────────────────────────┴──────────────┘
```

## 关键节点时间轴（最重要自定义部件）

数据来自录制产出的 `lesson-XX.chapters.json`（DESIGN.md 附录 A.8）。

- 每条 48px `divide-y --border-soft`；左 mono 13px 时间码 `--accent`（可点 seek）；中 18px Lucide 图标按 `type`：`step→circle-dot`(`--muted`) / `prompt→message-square-code`(`--track-video`) / `pitfall→triangle-alert`(`--warn`)；右中文步骤名 16px `--fg-2`
- 当前播放行 `background: --accent-wash`，随播放实时移动（`--motion-slow`）
- **同时是图文锚点**：点时间码 seek 视频，点步骤名滚动到正文对应小节

## 三类嵌入块

1. **代码块**：`--surface-warm` 底 + 1px `--border`；顶栏 36px（文件名 mono 13px `--meta` + 语言 + `copy` 16px）；行号 `--meta` / `tabular-nums` / `user-select:none`；**高亮行 = 整行 `--accent-wash` 底 + 该行行号变 `--accent`，不用左侧彩色边框**（规避侧条纹反模式）。
2. **提示词块**：头部 `message-square-code` 20px `--accent` + 「给 AI 的指令」+ 右上 `copy`；内容 mono 14px `--fg-2` 可换行；变量占位 `[方括号]` 着 `--accent`。
3. **踩坑块**：底 `color-mix(in srgb, --warn 8%, --surface)` + border `color-mix(in srgb, --warn 25%, transparent)`；头部 `triangle-alert` 20px `--warn` + 「这里会卡住」；**正文用 `--fg-2` 不用 warn 色**，避免整块刺眼。

## 底部导航

上一节 / 下一节双栏各 50%（显示节标题，`arrow-left` / `arrow-right`）；`标记完成` Primary；完成后变 Ghost + `circle-check` + 「已完成」，**不弹撒花动画**（MOTION_INTENSITY 3）。

## 五态

| 态 | 表现 |
|----|------|
| Loading | 播放器区 `aspect-ratio:16/9` 骨架（防 CLS）；时间轴 4 行 skeleton；正文 6 行骨架 |
| Empty | 本节暂无图文版：`file-text` 32px `--meta` + 「这节是纯实操，图文版整理中」+ 预计时间，不显空白区 |
| Error | 视频加载失败：播放器区内联 `triangle-alert` + 「视频加载失败」+ `重试` + 「或先看图文版 ↓」（给替代路径，非死胡同） |
| Populated | 如上 |
| Edge | 无 chapters.json → 时间轴整块隐藏、正文上移；超长代码块高上限 480px + 内滚 + 底部渐隐可展开 |

## 移动端 <768px

播放器 sticky 顶部；正文滚动 >200px 收为右下角 **mini player**（40vw、16:9、radius 10、1px `--border-soft`、可拖、右上 `x` 关闭）；章节导航 → 底部抽屉（`menu` 触发，70vh，含搜索）；右栏资源 → 顶部一行横向 chip；时间轴保留（行高 56px）；底部固定条 48px：`上一节` / `标记完成` / `下一节`，触摸 ≥44px。

## 无障碍

视频必带 `.vtt` 字幕轨；**图文版本身即视频的无障碍替代文本**（本课设计天然优势）；时间轴每行 `<button>` `aria-label="跳转到 2 分 14 秒：初始化 Next.js 项目"`；播放器 J/K/L 快捷键；`prefers-reduced-motion` → mini player 收起不做位移动画、时间轴高亮不做过渡。
