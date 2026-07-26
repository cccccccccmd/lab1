# AI/CS 前沿方向调查报告（2025–2026）

> 调查日期：2026-07。方法：联网检索 ICLR/NeurIPS 官方统计与回顾、arXiv 趋势、OpenReview、顶会 workshop 列表与领域综述，每个方向至少附一个来源链接。标注「推断」的判断是综合分析而非直接证据。
> 视角：为「RTX 4050 6GB + DeepSeek/Qwen/GPT API」的资源条件量身评估可行性。

## 宏观背景（有检索证据支持）

- **ICLR 2026 投稿量同比暴涨 68% 至 19,525 篇，接收率从 32% 降至 27%**；"Agent" 是最高频关键词，多模态/全模态与物理世界接地是另两大主导趋势（[Encord ICLR 2026 趋势](https://encord.com/iclr-2026/)、[ICLR 官方回顾](https://blog.iclr.cc/2026/03/31/a-retrospective-on-the-iclr-2026-review-process/)）。
- NeurIPS 2025 接收 5300+ 篇；LLM 评测、强化学习、多模态融合与 AI 安全上升；**84% 的接收论文包含新数据集/基准类贡献**（[IntuitionLabs NeurIPS 2025 总结](https://intuitionlabs.ai/articles/neurips-2025-conference-summary-trends)）。
- 对算力有限研究者的关键启示（推断）：**评测/基准/分析类论文已成为顶会完全合法化的贡献类型**，而这类工作恰恰不吃算力，吃的是严谨性与洞察。

---

## 路线一：热门方向（认可度高、基线丰富、学习资源多）

| # | 方向 | 为什么热 | 关键开放问题 | 竞争 | 6GB+API 可行性 |
|---|---|---|---|---|---|
| H1 | LLM Agent（总体） | ICLR 2026 第一关键词 | 长程规划、agent 幻觉、评测可靠性 | 极高 | 高（纯 API），但"加个模块"类框架论文正被大批拒稿（推断） |
| H2 | **多智能体系统及其失效分析** | MAST 失效分类学（14 种失效模式、1600+ 标注轨迹）中 NeurIPS 2025（[OpenReview](https://openreview.net/forum?id=fAjbYBmonr)） | 多 agent 常常不显著优于单 agent；错误传播、通信成本瓶颈（[arXiv 2511.09149](https://arxiv.org/pdf/2511.09149)） | 高，但**分析/诊断子方向远不如造框架拥挤**（推断） | 极佳：API + 人工标注密集，正合你的资源画像 |
| H3 | RLVR / 推理后训练 | DeepSeek-R1 开创的 2025 主范式 | 扩展到不可验证域、RLVR 到底教会了什么（[arXiv 2506.14245](https://arxiv.org/abs/2506.14245)） | 极高，大厂主导 | 勉强：本地 GRPO 训练超出 6GB；可走"对公开检查点做科学分析"或"奖励设计研究"侧翼 |
| H4 | 测试时计算 / 推理扩展 | s1、inference scaling laws 等（[arXiv 2501.19393](https://arxiv.org/pdf/2501.19393)） | **相同条件下各策略的系统对比缺失**（[HF 论文页](https://huggingface.co/papers/2512.02008)）——明确的空档 | 高 | 好：小模型+API 可做受控实证研究，正中上述空档 |
| H5 | **Agent 记忆** | 基准爆发期：LoCoMo、LongMemEval、MemBench 等；专门综述频出（[Mem0 报告](https://mem0.ai/blog/state-of-ai-agent-memory-2026)） | 记忆库增长下的容量退化、检索效率、多 agent 场景记忆 | 高且快速上升（2025 年内从冷变热，推断） | 极佳：全 API 可跑，基线全开源 |
| H6 | 统一多模态理解+生成 | ICLR 2026 三大趋势之一 | 模态对齐、理解-生成互相干扰 | 极高，训练侧算力天堑 | 低（建模）/中（对已发布模型做探测分析） |
| H7 | **LLM 评测科学：污染、饱和、LLM 裁判** | 公认的"评测危机"：前沿模型多基准 >94%；judge 偏置研究中 ICLR 2026（[LiveBench](https://openreview.net/forum?id=sKYHBTAxVa)、[llm-as-a-judge 资源站](https://llm-as-a-judge.github.io/)） | judge 不确定性、动态评测、agent 评测可靠性 | 高但异质，严谨实证仍能脱颖而出（推断） | 极佳：API + CPU 统计 |
| H8 | GUI / computer-use agent | OSWorld 2.0 等环境爆发，一年内成绩 12%→83.5%（[综述](https://zylos.ai/research/2026-02-08-computer-use-gui-agents/)） | 长程多应用任务、grounding | 高 | 中：评测环境 CPU/磁盘重但可行，训练 grounding 模型不可行 |
| H9 | 扩散语言模型（dLLM） | LLaDA 中 NeurIPS 2025，已扩展到 100B | 变长生成、对齐、扩散解码特有的安全失效 | 中高、快速上升 | 中：对 ≤1B 开源检查点做分析/LoRA 可行 |
| H10 | Agent 安全 / 提示注入 | OWASP 2025 头号 LLM 风险；ASB 中 ICLR 2025 | 自适应攻击攻破多数防御（成功率 50–85%，[arXiv 2606.26479](https://arxiv.org/pdf/2606.26479)） | 高 | 极佳：API 红队实验，与多智能体方向天然重叠 |

## 路线二：较冷门方向（竞争小、更可能出新发现/SOTA）

| # | 方向 | 为什么冷 | 上升证据 | 入场门槛 | 6GB+API 可行性 |
|---|---|---|---|---|---|
| C1 | **LLM 因果推理评测与增强** | 需要因果形式化+NLP 双重素养——技能护城河 | CausalBench、CausalVLBench（EMNLP 2025）、InterveneBench 等新基准接连出现且参赛者少（[综述 arXiv 2403.09606](https://arxiv.org/pdf/2403.09606)） | 概念门槛（因果形式化），非算力门槛 | 近乎完美：API 评测 + CPU 因果基线 + 小模型微调 |
| C2 | LLM 社会模拟与机制设计 | 跨学科（经济学+ML）；模拟可复现性是公认方法论难题（[arXiv 2605.30258](https://arxiv.org/pdf/2605.30258)） | LLM Economist、EconSimulacra 等 2025–26 涌现 | 低-中（博弈论基础） | 极佳：百 agent 模拟纯 API |
| C3 | **信息不对称下的多 agent 协作/通信科学** | 现有工作聚焦信息透明或单向通信，"信息不对称协作仍是核心挑战"（原文陈述，[arXiv 2510.25595](https://arxiv.org/html/2510.25595)） | 通信被点名为主导瓶颈但系统研究少 | 低，靠实验设计功力 | 极佳：热领域的冷角落，审稿人懂行而基线稀少（推断） |
| C4 | 交互式/非信念类心智理论（ToM） | 现有基准"过度集中于信念推理"，情境化交互 ToM"基本未被测试"（[综述](https://www.emergentmind.com/topics/theory-of-mind-in-large-language-models)） | ToMBench、SocialNLI、DialToM 相继出现 | 低 | 极佳：造数据 + API 评测 + ≤3B 微调 |
| C5 | **多智能体系统的记忆** | 单 agent 记忆已拥挤，但多 agent 共享记忆刚有第一篇综述（[TechRxiv](https://www.techrxiv.org/users/1007269/articles/1367390)），**尚无权威基准 = 造基准的机会**（推断） | AMA 等早期工作 2026 年初出现 | 低 | 极佳：纯 API + 现成记忆框架当基线 |
| C6 | LLM 机器遗忘（评测侧） | 被明确描述为"仍欠发达"；现有基准被批"弱度量"；重学习攻击能复活已遗忘知识（[arXiv 2406.13356](https://arxiv.org/pdf/2406.13356)） | SemEval-2025 官方任务、OpenUnlearning 统一框架 | 中 | 好：1B 模型 LoRA 遗忘实验入 6GB；攻击/评测论文更便宜 |
| C7 | LLM 判断预测（forecasting） | 小社区；结果要等现实事件揭晓，需要耐心 | ForecastBench 中 ICLR 2025、动态千题榜单资助到 2027（[OpenReview](https://openreview.net/forum?id=lfPkGWXLLf)） | 低 | 极佳：纯 API，天然免污染，与因果推理相通（推断） |
| C8 | LLM/agent 的不确定性量化与校准 | 方法碎片化；"人类对齐的不确定性"研究稀少（[综述 arXiv 2510.12040](https://arxiv.org/pdf/2510.12040)）；**agent/多步任务的 UQ 几乎空白**（推断） | ACL 2025 教程、LM-Polygraph 统一工具箱 | 中 | 极佳：本地小模型白盒信号 + API 黑盒采样 |
| C9 | 评测科学 / 基准统计学 | "元研究"不时髦；但学界明确呼吁统计严谨性（[ICLR 2025 博文](https://iclr-blogposts.github.io/2025/blog/towards-more-rigorous-llm-evals/)） | Benchmark²、测量误差研究等出现 | 低 | 近乎完美：CPU + 现有评测日志 + 少量 API |
| C10 | 表格基础模型（TabPFN 系）× 因果推断 | 表格数据在 LLM 时代不时髦；但 TabPFN 已被外溢用于时序与因果推断（[TabPFN-2.5](https://www.researchgate.net/publication/397555905)） | TabPFN 生态活跃扩张 | 中 | 极佳：TabPFN 类模型小 GPU/CPU 即跑（注意先查已有 prior-fitted-network 因果工作的新颖性） |

**候补冷方向**（证据较薄但值得跟踪）：Agent 互操作协议（MCP/A2A）作为研究对象；LLM 持续学习与虚假遗忘；低资源/多语言场景的 LLM 裁判；模型合并（略过峰值）；KG+LLM/GraphRAG（介于冷热之间：工业驱动、开放问题明确——高效子图检索、动态 schema、真值子图评测，[arXiv 2511.04473](https://arxiv.org/pdf/2511.04473)——适合作为 agent 记忆论文的组件而非独立赛道，推断）。

---

## 针对你的推荐（结合七个意愿方向 + 硬件）

**热门 3 选**：
1. **H5 Agent 记忆**——直接命中你的意愿；基准与开源基线齐全，API 即可公平对比；容量退化与效率指标是开放且可测量的问题。→ 配 `domains/agent-memory/`
2. **H2 多智能体失效分析**——命中多 agent 意愿；MAST 给了现成分类学与数据可供扩展；诊断型论文奖励细致标注而非算力。→ 配 `domains/multi-agent-collaboration/`
3. **H7 评测危机**——6GB 预算下"可行性/认可度"比值最高；且其方法（judge 偏置、统计检验）会反哺你之后的每一个项目。

**冷门 3 选**：
1. **C1 LLM 因果推理**——你的因果意愿构成真实护城河；新基准参赛者少；纯 API+CPU 工作流；还可做 CausalVLBench 式多模态延伸兼顾 MLLM 意愿。→ 配 `domains/causal-reasoning/`
2. **C5 多智能体记忆**——你两个意愿方向的交集，先行工作停留在综述阶段、无权威基准；"造基准 + KG 支撑的共享记忆基线"（顺带用上 KG 意愿）是现实的首篇顶会攻击路线。
3. **C3 信息不对称通信**——最热领域的冷角落：论文原文明确承认空白、基线稀少、全 API 可做，且与上面两个选择的发现可以互相复用。

**组合策略建议**（推断）：冷热搭配——主攻一个冷方向（C1 或 C5，出"人无我有"的贡献），同时用其实验副产品在相邻热方向（H5/H2）发分析型论文，风险对冲且工作量复用。
