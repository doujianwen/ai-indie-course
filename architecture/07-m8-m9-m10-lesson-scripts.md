# M8 / M9 / M10 — Per-Lesson Recording Scripts & Real-Data Desensitization Spec

> Version: v1.0 (Phase 5 detailed scripting, post ADR-004 scope upgrade)
> **Language role (per ADR-005)**: This English doc is the **localization reference** for the eventual English course. The execution-first working source is **Chinese** — actual per-lesson recording scripts will be produced in Chinese first, validated, then localized to English (translate / voice-over), not rewritten.
> **Chinese execution source (recording-ready)**: `07-zh-m8-m9-m10-lesson-scripts.md` — the per-lesson scripts instructors actually record from (Chinese narration, expanded from this outline).
> Source of truth for teaching assets: `bookconv.com` real repo + real GSC/GA4 data (selective disclosure, see §0).
> Companion docs: `prd/PRD.md` (v1.2, AC-7), `01-tech-outline.md` (Phase 7, Ch 18–20), `06-curriculum-spec.md`, `decisions/ADR-005-language-sequencing.md`.

---

## 0. Real-Data Desensitization Master Spec (applies to M8, M9, M10)

This is the **single rule** for using bookconv's real data on camera. Violating it risks (a) exposing the creator's private analytics and (b) tripping Google's scaled-content policy if numbers look like "proof of success" we can't stand behind.

### 0.1 What is ALWAYS safe to show (no masking)
- **Methodology, structures, architecture**: content-cluster maps, single-source registration code, internal-link graph, sitemap/llms.txt/robots config, code snippets, the verifyConversion flow.
- **Relative / comparative facts**: "cluster A ranks at position ~21, scattered pages at ~69" — show the *gap*, mask the absolute impression counts behind it.
- **Trend shapes & diagnosis paths**: "impressions rose then flattened", "query coverage matrix has 3 gaps" — show the *shape*, mask axis numbers.
- **Timelines as phases, not dates**: "within the first ~2 weeks after launch" — never reveal the exact calendar date that lets viewers pin the site's age.

### 0.2 What MUST be masked (absolute values → placeholder)
| Data class | On-camera rule |
|---|---|
| GSC impressions / clicks / CTR | Blur or replace with `N impressions`, `0 clicks (early stage)`; keep the *curve* and *relative position*. |
| GA4 users / sessions / events | Mosaic the absolute counts; show funnel *shape* and *drop-off %* only. |
| Revenue / subscribers / $ amounts | Never show. Reference only as "early, pre-revenue / pre-scale". |
| Exact launch date / site age | Say "a few weeks old" / "new domain". Do not show the real date. |
| PII (emails, IPs, filenames with names) | Full mask. |
| Competitive specifics that identify the site negatively | Avoid naming failed experiments in a way that doxxes the project. |

### 0.3 How to mask (production technique)
- Screenshots of GSC/GA4: apply a **mosaic / blur layer** over numeric cells; leave labels, chart shapes, and axis *direction* visible.
- Live demo: navigate to the real dashboard but **zoom/blur** the numbers, or use a sanitized clone screenshot in the edit.
- Verbal script: teach the *diagnosis process* ("here's how I found coverage was the bottleneck"), not the *score* ("I had only 287 impressions").
- This is the already-approved **Decision #3** (screenshots hide absolute values) extended to all three modules.

### 0.4 The "honest ledger" framing (selective disclosure, Q2)
- Teach **"0 clicks → how to diagnose"** as a *universal new-site pattern*, not as bookconv's shame.
- Message: "A new domain with ~0 clicks in the first weeks is the norm, not a failure. The skill is *what you check next*." This builds trust with overseas students who are themselves at 0.

---

## M8 — Content Strategy System (Module 8)

**Why it's in v1:** content is what actually gets a product found; bookconv has a real 87-page system to teach from. No fabrication needed.
**Real assets used:** 87 content pages (36 blog + 21 guide + 30 conversion), 185 editorial internal links, 0 generic anchors; `docs/content/一页吃整簇策略.md`; `src/lib/internal-links.ts`; `src/data/{blog,guides,content}`.

### M8-L1 — "One Page, One Cluster" (the core principle)
- **Objective:** student can decide *when to merge* keyword variants into one page vs split.
- **Materials (desensitized):** cluster map of `azw3 vs mobi` (one page, ~29 impressions, position ~21) vs scattered `mobi→epub` pages (position ~69); Spanish version best at ~15.
- **Script outline (English, instructor voice):**
  1. Hook: "I almost made 6 pages for one intent. Here's the mistake."
  2. Principle: same-intent variants → one authoritative page; merge, don't multiply.
  3. Live: open `docs/content/一页吃整簇策略.md`, show the cluster decision table.
  4. Proof (masked): one cluster page ≈ position 21; the scattered version ≈ position 69. Show the *gap*, blur counts.
  5. Rule of thumb: "Would the searcher who clicks want the same answer? Yes → merge. No (e.g. `convert X to Y` transactional vs `X vs Y` informational) → split."
- **Demo:** rename a hypothetical scattered set into one cluster page; show H1 + comparison `<table>` (featured-snippet target).
- **Length:** ~12 min. **Acceptance:** student can classify 5 given keywords as merge/split with correct reasoning.

### M8-L2 — Single-Source Content Registration
- **Objective:** student can add content without breaking sitemap/RSS/LLMs.
- **Materials:** `src/data/blog/*.ts` registration + `index.ts`; auto-derive flow (list → sitemap → rss).
- **Script:** show the registration file; add a new blog post entry; rebuild; verify it appears in sitemap.xml and RSS without manual edits. Emphasize "one edit, everything derives."
- **Length:** ~10 min. **Acceptance:** student reproduces the add-content flow on a stub.

### M8-L3 — Internal Link Architecture
- **Objective:** student can wire pages into a link graph that concentrates authority.
- **Materials:** `src/lib/internal-links.ts` helper; the 185-link graph; P0/P1/P2 fix log (fixed 2 dead links, connected guide↔blog↔conversion triangle).
- **Script:** show the helper + a before/after internal-link audit; demo fixing a dead link and adding an editorial anchor (no "click here" generic text).
- **Length:** ~11 min. **Acceptance:** student audits a sample site and lists 3 link fixes.

---

## M9 — SEO / GEO Ops & Growth (Module 9)

**Why it's in v1:** overseas students need *discovery*; bookconv's GEO/SEO setup is real and teachable.
**Real assets used:** `public/llms.txt`, `robots.txt` (allows GPTBot/ClaudeBot/CCBot), `next.config.ts` (sitemap derive, compress), `scripts/seo-critic.mjs` gate, `src/app/sitemap.ts`, hreflang as-needed middleware, real GSC/GA4 diagnosis.

### M9-L1 — Technical SEO Foundation (sitemap / llms.txt / robots / hreflang)
- **Objective:** student ships crawlable, AI-indexable foundations.
- **Script:** show auto-derived sitemap; open `llms.txt` and explain GEO (let LLM crawlers read structured summaries); show `robots.txt` allowing GPTBot/ClaudeBot/CCBot; explain `localePrefix as-needed` (English no prefix, `/es` for Spanish, `/en/*`→301 rewrite) and that canonical is page-level only.
- **Demo:** run `seo-critic.mjs` (exit-code gate) to prove the foundation is enforced in CI.
- **Length:** ~13 min. **Acceptance:** student lists the 4 technical pillars and why each matters.

### M9-L2 — GSC in Practice: Diagnosing 0 Clicks (selective disclosure)
- **Objective:** student knows the *checklist* when a new site gets no traffic.
- **Materials (desensitized):** GSC coverage, ranking, query-coverage, technical-SEO tabs — show *paths and shapes*, mask absolute impressions/clicks.
- **Script:** "New domain, ~0 clicks in week 2 — normal. Here's the 4-step diagnosis: (1) is it indexed? (2) what position? (3) query coverage gaps? (4) technical SEO blockers?" Walk each tab, mask numbers, keep the *decision tree*.
- **Length:** ~14 min. **Acceptance:** student can run the 4-step audit on their own GSC.

### M9-L3 — GA4 in Practice: Behavior & Conversion Funnel
- **Objective:** student measures *what users do*, not just traffic.
- **Materials (desensitized):** GA4 custom key events (file-upload / convert-complete) created 2026-08-10; landing→upload→complete funnel; mask user counts, keep drop-off %.
- **Script:** show the event config; build the funnel report; interpret drop-off as the product's biggest leverage point.
- **Length:** ~12 min. **Acceptance:** student defines 2 key events for their product and a funnel.

### M9-L4 — External Links (ROI-priority, don't abuse)
- **Objective:** student earns authority without spamming.
- **Script:** bookconv's home-page outbound links done first, then inner-page links by ROI priority; rule: "links must flow value, not manipulate." Show the internal-link audit as the *onsite* half, backlinks as the *offsite* half.
- **Length:** ~8 min. **Acceptance:** student ranks 5 backlink targets by ROI.

---

## M10 — Anti-Drift / Third-View Correction (Module 10)

**Why it's in v1:** borrowed from the old `ai-independent-dev` *correction-agent* **kernel** (scope freeze + stage acceptance + correction checklist) — NOT its multi-agent shell, NOT its fictional content. This is the module that stops students (and the instructor) from burning months on the wrong thing.
**Real anchor case:** a 6-year PM who spent ~3 months building a *self-made editor* for a plugin instead of shipping the plugin — a textbook drift.

### M10-L1 — Why Indie Devs Drift (the real failure mode)
- **Objective:** student recognizes drift *before* it costs months.
- **Script:** tell the real case (PM, 3 months on a side-tool, zero progress on the product). Map the drift pattern: vague scope → shiny sub-problem → forgotten original goal. Show the *emotional* pull, not just the logical one.
- **Length:** ~9 min. **Acceptance:** student names the 3 drift warning signs.

### M10-L2 — The Correction-Agent Kernel (borrowed, stripped)
- **Objective:** student installs a lightweight "third view" into their own workflow.
- **Script:** explain the 3-mechanism kernel from the old project, re-grounded in reality:
  1. **Scope freeze** — write the v1 scope; anything new goes to a "later" list, not into v1.
  2. **Stage acceptance** — each phase has an explicit "done when" gate (mirrors our AC-1..AC-7).
  3. **Correction checklist** — a recurring prompt: "Is this still the original goal? Am I building a tool to build the tool?"
- **Length:** ~10 min. **Acceptance:** student writes their own scope-freeze note + checklist.

### M10-L3 — Build Your Own "Third View"
- **Objective:** student operationalizes anti-drift as a weekly 10-min habit.
- **Script:** demo a weekly scope-audit template (copy-pasteable): list this week's work → tag each as "core goal" / "adjacent" / "drift" → cap drift at 0 for v1. Tie back to bookconv's own scope discipline (we cut features to ship).
- **Length:** ~8 min. **Acceptance:** student runs one audit on their current project.

---

## Production Notes (for the instructor)
- All three modules are **video-primary, English narration**, with the Chinese-source strategy docs as the instructor's prep reference (not student-facing).
- Every real-number screenshot follows §0.2 masking. A 5-min "desensitize before you record" checklist is appended to the recording SOP.
- M8/M9 lean on *bookconv's real files*; M10 leans on the *real drift case* + the borrowed correction kernel. None require invented case studies.
- Next step (recommended): translate the existing Chinese PRD/architecture/README/ADR to English so the repo is uniformly English-canonical, matching the English-main decision.
