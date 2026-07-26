# Claude Code 自主科研提示词系统（AI-Researcher Prompts）

一套让 Claude Code 端到端自主完成科研工作的提示词体系，目标产出为 **CCF-A / 顶会级别**（ICLR、NeurIPS、ACL、CVPR 等）的研究：从创新点探索 → 可行性验证 → 实验 → 指标迭代 → 论文撰写 → 开源整理。

## 一、设计哲学

大多数"让 AI 做科研"的尝试失败在两个地方：

1. **想出来的创新点是假的**——要么早已有人做过，要么是无法验证的空想，要么只是 A+B 的缝合。
2. **实验做成了流水账**——跑一次就写论文，没有假设驱动的迭代，指标上不去也不知道为什么。

本系统针对这两点做了专门设计：

- **创新点不是"想"出来的，是"筛"出来的**：先批量生成候选（每轮 ≥10 个），经过「新颖性检索闸门 → 可行性闸门 → 廉价否证实验（kill experiment）」三道过滤，只有活下来的想法才允许进入完整实验。
- **实验是假设驱动的闭环**：每一次改动必须先写下假设和预期，跑完后做误差分析（error analysis），由失败样本产生下一个假设。所有结果（包括负结果）永久记录，禁止删除。
- **状态外置**：Claude Code 的上下文会耗尽/被压缩，所以所有研究状态写在 `RESEARCH_LOG.md`、`IDEAS.md`、`EXPERIMENTS/` 等文件里。任何一次新会话都能从文件恢复全部进度，这是长周期自主科研的关键。
- **诚实性红线**：提示词中硬性规定禁止编造/美化实验数字、禁止在测试集上调参。AI 科研最大的风险是它"帮你"把数字变好看——这在学术上是灾难。

## 二、文件结构

```
CLAUDE.md                      # 研究智能体"宪法"：放入研究项目根目录，长期生效
prompts/
  00-master-pipeline.md        # 主控提示词：一条指令驱动全流程 + 状态机
  01-literature-survey.md      # 阶段1：文献调研与 gap 挖掘
  02-ideation.md               # 阶段2：创新点探索与筛选（核心）
  03-feasibility-proposal.md   # 阶段3：可行性论证 + 研究提案
  04-experiment-design.md      # 阶段4：实验设计
  05-implementation.md         # 阶段5：代码实现规范
  06-improvement-loop.md       # 阶段6：指标改进循环（核心）
  07-ablation-analysis.md      # 阶段7：消融实验与深入分析
  08-paper-writing.md          # 阶段8：论文撰写
  09-reviewer-simulation.md    # 阶段9：审稿人模拟与论文迭代
  10-open-source-release.md    # 阶段10：开源代码整理
templates/
  RESEARCH_LOG.template.md     # 研究日志（全局状态文件）
  idea-card.template.md        # 创新点评估卡
  experiment-report.template.md# 单次实验报告
```

## 三、如何使用

### 快速开始

1. 新建一个空的研究项目目录（不是本仓库），把 `CLAUDE.md` 拷贝到该目录根部。
2. 把 `prompts/` 和 `templates/` 整个拷贝到该目录下。
3. 在项目目录里启动 Claude Code，把 `prompts/00-master-pipeline.md` 的内容作为第一条消息发出（或直接说：*"读取 prompts/00-master-pipeline.md 并按其执行，研究方向是 ______"*）。
4. 之后每次开新会话，只需要说：*"读取 RESEARCH_LOG.md 恢复状态，继续执行主流程"*。

### 配置你的资源

在 `CLAUDE.md` 的「资源与硬件约束」一节填入你的实际情况（已按 Legion Y7000P / RTX 4050 6GB 预填），并在环境变量或 `.env` 中配置模型 API（DeepSeek / Qwen / GPT 等），用于：

- 大规模数据构造 / 打标 / 蒸馏的教师模型；
- LLM-as-a-judge 评测；
- 作为研究对象本身（agent / reasoning / prompting 类研究）。

### 硬件现实（务必先读）

RTX 4050 Laptop（6GB 显存）**做不了**：预训练、ImageNet/COCO 级全量训练、>3B 模型的全参微调。**做得好**的方向（也是本系统推荐聚焦的、在 6GB + API 条件下有真实顶会先例的方向）：

| 方向 | 说明 | 典型目标会议 |
|---|---|---|
| LLM 推理/Agent/提示方法 | 纯 API 驱动，测 GSM8K、MATH、HumanEval、AgentBench 等 | ICLR / ACL / NeurIPS |
| 数据为中心的方法 | 数据选择、合成、课程学习；用 API 造数据，本地 QLoRA 验证 | ICLR / ACL |
| 参数高效微调 / 小模型 | ≤3B 模型 QLoRA、蒸馏、量化、剪枝的新方法 | ICLR / ACL / EMNLP |
| 测试时计算（test-time） | 解码策略、自一致性、搜索、验证器 | ICLR / NeurIPS |
| 评测与 Benchmark | 新基准、新评测协议、对现有方法的系统性实证研究 | NeurIPS D&B / ACL |
| 小规模可控视觉实验 | CIFAR/小分辨率上验证新机制，主打洞察而非刷 SOTA | ICLR / CVPR（分析类） |

如果你坚持做需要大规模训练的 CVPR 主流方向，需要额外租用云 GPU（AutoDL、Vast.ai 等），系统的提示词兼容这种情况，但默认按本地 6GB 设计。

## 四、诚实的期望管理

这套提示词能显著提升 Claude Code 做科研的**系统性、严谨性和迭代效率**，但它不能保证产出 CCF-A 论文——顶会论文最终取决于想法本身的价值和实验证据的强度，而这两者有真实的失败率（顶会录取率 20–30%，且投稿者多为全职研究者）。合理的用法是：**把它当作一个不知疲倦、方法论严格的博士生合作者**，你作为人类研究者负责最终的品味判断（哪个 idea 值得押注）和学术把关（结果是否可信）。每个阶段的产出都设计为人类可审查的文档，建议你在阶段边界处介入审阅。

## 五、许可

MIT。欢迎按自己的研究领域修改各阶段提示词。
