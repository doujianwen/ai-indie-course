# 页面规格：课程 Landing

> 源：DESIGN.md §0/§1/§2/§5/§7 + pages-visual-direction.md「一」
> 寄存器：Brand（设计即产品）· 主题：Light · DESIGN_VARIANCE=7 / DENSITY=4
> 角色：营销端，设计即产品本身，允许非对称与真实截图主视觉

## 全局约束（来自 DESIGN.md）

- 强调色 `--accent #14489C`，**每屏可见 ≤2 处**（主 CTA 1 + 当前状态 1）
- 背景 `--bg #F7F8FA` + `.blueprint-grid`（40px 蓝晒网格，opacity 内置 0.04）；**无渐变 / 无光晕 / 无 3D**
- 字体：展示 `--font-display`（Archivo）/ 正文 `--font-body`（Inter）/ 中文 `--font-cn`（Noto Sans SC）/ 代码数字 `--font-mono`（JetBrains Mono）
- 图标：`lucide-react` 16/20/24，CI emoji 正则拦截，**禁 emoji 作功能图标**
- 容器 `max 1200px`；所有数字 `mono + tabular-nums`；禁裸 hex（ESLint `no-hardcoded-color`）

## 区块清单（自上而下）

1. **顶栏 64px sticky**：左文字标识（Archivo 590/20px，无图形 Logo）；中导航 `课程大纲` `谁适合` `讲师` `常见问题`（16px `--muted`，hover→`--fg`）；右 `免费试看`（Ghost）+ `立即购买`（Primary）。移动端右侧仅 `立即购买` + `menu` 抽屉。
2. **Hero ≈58vh，padding-top 88px，非对称，不居中**：
   - 左 7/12：mono meta 行「12 视频节 · 12 图文 · …」+ H1 `--text-display` 60/1.1/-0.02em「跟着做完，你会有两个真实上线的站」+ 副文案 17px/1.75/`--muted` 两行（说清「不教什么」）+ `[立即购买][先看 3 节]` + 一行 14px 交付物清单（可下载仓库 · 全部提示词）。
   - 右 5/12（margin-top 40px 错位）：三张真实截图错落叠放——① bookconv 真实首页（旋转 -3°）② Cursor 真实对话（0° 前移）③ Vercel Deployment 成功面板（+4° 右下）。每张 radius 10 / 1px `--border` / 无阴影 / 旋转 ≤6°。
   - accent 配额：仅 `立即购买` 按钮 1 处（Vercel 截图内绿勾属截图内容不计）。
3. **谁适合 / 谁不适合**：双栏对照，中间 1px `--border` 竖线。左 `circle-check` + `--success`，右 `x` + `--muted`（不用红色）。每栏 4 条人话（如「你有个想法，但每次开到第三天就卡在部署」）。不用卡片、不用图标网格。
4. **你会亲手做出什么**：两个真实项目各占横向大区块（非三列卡片）。项目一左 5/12 文（项目名 + 解决什么 + 真实上线地址 external-link 16px + mono 真实指标）/ 右 7/12 真实截图（简化 chrome 包装：32px 灰条 + 三圆点 radius 10）。项目二左右互换打破重复。
5. **课程大纲预览**：可展开模块表（与章节列表页同构）。每行 mono 模块号 + 模块名 + 「7 节 · 68 分钟」+ `chevron-down`；展开为 lesson 行（免费节标题右侧 `play-circle` + 「试看」pill）。展开动画 320ms `--ease-out-soft`，只动 opacity+transform。
6. **方法论骨架（MVP 6 Phase）**：横向时间轴，非六卡片。2px `--border` 横线 + 六节点（12px 圆点，当前/关键 `--accent`）+ 节点下方竖排文字。移动端转竖向。
7. **讲师与真实数据**：左真实照片（非插画/头像 emoji），右三行履历 + 真实数据表（`divide-y` 两列：指标名 / 数值 mono tabular-nums，标注出处如「GSC 2026-08」）。禁「10000+ 学员信赖」类无源数字。
8. **免费试看**：页面内嵌真实播放器（非弹窗/封面占位），下方三节可切换标题（当前选中着 `--accent`，全页第二处配额）。
9. **定价 ≤3 档**：表格式对比（标准 / 进阶），非彩色卡片。推荐档 1px `--accent` 边框 + 顶部「多数人选这档」小字，无缩放放大。
10. **FAQ**：手风琴 8–10 条真实顾虑（「零基础真能跟上」「AI 模型换了会过时吗」「不会英语行不行」）。`chevron-down` 旋转 180° / 140ms。
11. **尾部 CTA**：重复主 CTA，背景 `--surface` + 上方 1px `--border`，无全屏渐变收尾。

## 禁用清单（出现即重写）

`CURRICULUM`/`PRICING` 小型大写标签、`01 · 关于` 编号标记、抽象 3D 图形、粒子背景、数字滚动动画、「开启你的 AI 之旅」类空文案、任何 emoji、奶油/米色背景、卡片圆角 ≥24px、彩色左边框、1px 边框 + blur≥16px 阴影同现。
