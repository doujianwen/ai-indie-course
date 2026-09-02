# ai-indie-course 项目长期记忆

## 工作规矩（2026-09-02 用户明确指令）
- **素材落仓即提交**：`materials/` 下任何课程/培训素材（分析、案例、方法论、数据线、图表）一旦写入仓库落盘，必须**立即 git 提交**，不允许跨会话累积未提交文件。
  - 多份素材同批到达时，合并为**一次提交**（不按日期拆批）。
  - 提交信息遵循 conventional commits：`docs(materials): <简短描述>`。
  - 判据：素材文件 `git status` 出现即视为待提交，不在本地留过夜。
- 触发场景：每日 GSC 数据线、Bing AI 分析、审计/复盘产出、可复用教学素材——凡落盘即提交。

## 仓库信息
- Git 根 `E:/一人公司/独立站/ai-indie-course`
- remote `ssh://git@github.com/doujianwen/ai-indie-course.git`（SSH，无 insteadOf 改写）
- 模块映射与素材索引见 `materials/README.md`（素材落仓后同步更新该索引）
- 提交后推送 `git push origin master`，并用 `git ls-remote` / `git rev-list HEAD..origin/master` 硬验证（绝不信任 behind=0）
