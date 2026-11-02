# 院士速成器 · Academician Speedrun

名字是玩笑，核验是认真的。一个**引用可核验**的科研工作台原型：输入研究主题，真实检索论文与代码仓库；每条引用都可当场向 Crossref 核验真伪——DOI 必须真实存在，且登记标题必须与所引标题对得上。速成什么都行，唯独引用不许速成编造。

A research workbench prototype built around one claim: **every citation can be checked, live.** Search real papers and repos, then verify each reference against Crossref — a DOI must exist *and* its registered title must match the cited title.

![status](https://img.shields.io/badge/status-prototype-orange) ![license](https://img.shields.io/badge/license-MIT-blue) ![build](https://img.shields.io/badge/build-none%20(single%20file)-green)

## 直接使用

下载或克隆本仓库，用浏览器打开 `index.html` 即可，无构建、无后端、无需注册。也可以在本仓库开启 GitHub Pages（Settings → Pages → Deploy from branch `main` / root）得到在线演示地址。

## 已实现（真实功能）

- **真实检索**：主题输入后并行查询 [OpenAlex](https://openalex.org)、[Semantic Scholar](https://www.semanticscholar.org) 与 GitHub 仓库搜索，结果按 DOI／标题去重、按引用数排序，渲染为论文卡与仓库卡（标题、来源、年份、作者、引用数、许可证、最近活跃）。
  - arXiv API 不开放浏览器跨域直连，其论文已由 OpenAlex 收录；网页检索需后端服务，单文件版未接入（界面上如实标注）。
- **引用核验台**（核心）：粘贴参考文献（一行一条），逐条向 [Crossref](https://www.crossref.org) 核验：
  - ✓ **已核验**：DOI 存在且登记标题与所引标题一致；
  - ✗ **查无此 DOI**：DOI 在 Crossref 不存在（编造或抄错）；
  - ✗ **标题不符**：DOI 真实存在、但登记的是另一篇论文——“真 DOI 配假标题”，是最常见的 LLM 编造形态，只查存在性会放过它；
  - ⚠ **存疑／无 DOI 反查**：无 DOI 的条目按标题反查候选记录，提示补全。
  - 核验全部在浏览器本地直连 Crossref 完成，不经过任何服务器。核验台内置一组示例（含 1 条假 DOI、1 条真 DOI 配假标题），可一键填入试抓。
- **卡片级核验**：论文卡与「核验本列 DOI」可对当前列表逐条核验，不通过的卡片自动标红并写明原因。
- **思维导图**：一键把当前课题展开成一张图——主题 → 三条路线（锚点＋问题/数据/方法/验证/产出/风险）→ 资源 Top 与引用核验状态；内容直接取自界面当前数据（演示集或真实检索结果），点路线节点直达推演流程图，可导出 Markdown 大纲。
- **证据先行界面**（原型形态）：资源双列（我的工作集 30% ／ 候选池 70%，收藏持久化于 localStorage）、结论双行制（作者主张／验证状态分开写，无证据只许写“未验证”）、三条研究路线（纵深／跨界／空白位）与流程图推演、被引 Top 图表。

## 仍为演示的部分（如实标注）

- 三条研究路线及其推演流程仍是随附演示课题（城市树木点云单木分割）的内容：**路线归纳需要接入大模型**，规划中的形态是本地优先的 MCP 工具层＋可替换模型，不在单文件版内伪造生成结果。
- 演示集里有一张故意标红的样例卡，演示“查无此条”的拦截形态。

## 设计要点

- 差异点只有一句：**敢担保引用是真的。** LLM 编造引用是已被多项实测记录的现象；本工作台让“事实”只来自学术 API（OpenAlex / Semantic Scholar / Crossref / GitHub），模型只负责检索、阅读与归纳，不负责报数。
- 核验判据来自一次小规模实测（20 条真引用＋5 条编造）：挑假 5/5、误杀 0/20；关键规则是**验 DOI 与标题的绑定**，而非只验 DOI 存在。

## 路线图

- v0.2：路线推演接真实检索结果、撤稿筛查（Retraction Watch）、引用图谱聚焦视图、BibTeX 导出。
- v1：本地优先完整版（MCP 工具层＋总览／聚焦双态壳）、页码级出处、实验记录挂接。

## License

MIT — 见 [LICENSE](LICENSE)。
