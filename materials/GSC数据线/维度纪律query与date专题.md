# GSC 取数维度纪律：query 维度 vs date 维度（bookconv.com 真实案例）

> 课程模块：**M9 SEO/GEO 运营增长** · 子主题「GSC 取数维度纪律」
> 数据窗口：2026-07-26 ~ 2026-08-14
> 真值来源：`GSC数据线/2026-08-16.md` §二（维度纪律）、§四（逐日展示）

---

## 一、一句话结论（课程开场白可用）

> 用 GSC API 做 SEO 分析，第一个坑不是"怎么分析"，而是"用哪个维度取数"。
> **词数用 `query` 维度，展示/点击/CTR/排名用 `date` 维度**——混用会把数据腰斩一半，而且你根本看不出来错了。

---

## 二、两条铁律

| 指标 | 必须用维度 | 原因 |
|---|---|---|
| 有排名**唯一关键词数** | `query` | 要逐词枚举，且**必须丢弃 `(other)` 行** |
| **展示 / 点击 / CTR / 均排名** | `date` | 全站汇总真值，跨所有查询 |

⚠️ 致命反例：用 `query` 维度去"求和展示"当全站展示 = **严重偏低**，且毫无报错提示。

---

## 三、实锤反例：8/9 当天，query 求和 vs date 真值

8/9 是 bookconv 内容爆发日（当天新增 46 词、`date` 维度展示 150）。同一天两种取数方式：

| 取数方式 | 8/9 展示 | 相对真值 |
|---|---:|---:|
| `query` 维度逐词求和 | 71 | 仅为真值 **47%** |
| `date` 维度（全站真值） | 150 | 基准 |

```svg
<svg viewBox="0 0 680 250" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="8/9 query求和展示 vs date真值展示对比">
  <rect x="0" y="0" width="680" height="250" fill="#ffffff"/>
  <text x="340" y="26" font-size="14" fill="#0f172a" text-anchor="middle" font-weight="bold">8/9 同一天：query 求和 vs date 真值（展示次数）</text>
  <!-- baseline -->
  <line x1="60" y1="205" x2="620" y2="205" stroke="#94a3b8" stroke-width="1.5"/>
  <!-- query 71 (scale: 150 -> 145px, factor 0.967) -->
  <rect x="130" y="136" width="120" height="69" fill="#f59e0b"/>
  <text x="190" y="126" font-size="22" fill="#b45309" text-anchor="middle" font-weight="bold">71</text>
  <text x="190" y="226" font-size="12" fill="#475569" text-anchor="middle">query 维度求和（仅 47%）</text>
  <!-- date 150 -->
  <rect x="370" y="60" width="120" height="145" fill="#1d4ed8"/>
  <text x="430" y="50" font-size="22" fill="#1e40af" text-anchor="middle" font-weight="bold">150</text>
  <text x="430" y="226" font-size="12" fill="#475569" text-anchor="middle">date 维度（全站真值）</text>
  <!-- delta annotation -->
  <text x="300" y="105" font-size="13" fill="#dc2626" text-anchor="middle" font-weight="bold">腰斩 ≈53%</text>
  <line x1="250" y1="100" x2="370" y2="100" stroke="#dc2626" stroke-width="1.5" stroke-dasharray="4,3"/>
</svg>
```

- 结论：`query` 维度当天**丢了 79 个展示（约 53%）**。这不是 bug，是 GSC 的机制——**高流量日 GSC 会不返回 `(other)` 行就直接丢弃亚阈值低频词**，这些词的展示不计入 `query` 维度的逐词求和。
- 教学点：如果你在报告里写"8/9 我们拿到 71 次展示"，那是个**假数字**——真实是 150。取词数时绝不能顺手用同一份 query 数据去求和展示。

---

## 四、正确取数模板（Python · GSC API v3）

```python
from googleapiclient.discovery import build

# 1) 词数：query 维度，必须丢弃 (other)
body_q = {"startDate": START, "endDate": END, "dimensions": ["query"], "rowLimit": 25000}
rows_q = service.searchanalytics().query(siteUrl=SITE, body=body_q).execute().get("rows", [])
keywords = [r["keys"][0] for r in rows_q if r["keys"][0] != "(other)"]
# 累计去重：每天拉一次，跨日做 set union（不是逐日求和！）

# 2) 展示/点击：date 维度（全站真值）
body_d = {"startDate": START, "endDate": END, "dimensions": ["date"]}
rows_d = service.searchanalytics().query(siteUrl=SITE, body=body_d).execute().get("rows", [])
total_impr = sum(r["impressions"] for r in rows_d)   # 正确全站展示
total_clicks = sum(r["clicks"] for r in rows_d)
```

---

## 五、课程可引用的 4 个教学点 ⭐

1. **维度选错 = 数据腰斩**：8/9 实锤 71 vs 150，学员一眼记住"query 求和展示是陷阱"。
2. **`(other)` 桶是隐形杀手**：GSC 隐私阈值会吞掉低频词，`query` 维度求和展示永远偏低；`date` 维度才是汇总真值。
3. **词数必须去重并集**：每天 `query` 维度拉词，跨日做 `set union` 才是"累计唯一词"，**不是逐日求和**（逐日求和会重复计数同一词）。
4. **口径写进报告**：任何 GSC 报告必须标注"词数=query 维度去重并集 / 展示=date 维度"，否则读者无法复现、易误判。

---

## 六、数据溯源与配套

- 8/9 `date` 真值 150：`GSC数据线/2026-08-16.md` §四「2026-08-09 | 150 | 0 | 0% | 39.53」。
- 8/9 `query` 求和 71：早期拉取 `query` 维度原始行求和（增长趋势专题教学点 5 引用）。
- 配套素材：`有排名关键词增长趋势.md`（教学点 5 引用本现象）、`零点击CTR瓶颈专题.md`。

---

*本文件为 M9 课程素材，配 `GSC数据线/` 每日报告使用。*
