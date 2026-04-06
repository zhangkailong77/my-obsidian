# LangChain Python v1 进阶系统学习路线（2026 Q2，P/C/H 主线）

> 更新时间：2026-04-06
> 定位：非基础版，面向可上线 Agent 系统
> 主线：**Prompt Engineering + Context Engineering + Harness Engineering**

## 三工程总纲（必须同时推进）

### 1) Prompt Engineering（P）
- 目标：让模型输出稳定、可控、可审计。
- 核心动作：
  - System prompt 合同化（角色、边界、拒答规则、输出格式）。
  - Prompt pattern 库（分类/抽取/规划/工具调用/拒答）。
  - Prompt 版本管理（A/B、回滚、变更记录）。
- 交付物：`prompts/`、`prompt_changelog.md`、失败样例。

### 2) Context Engineering（C）
- 目标：让模型“拿到对的上下文”，而不是“拿到更多上下文”。
- 核心动作：
  - 上下文分层：短期会话、长期记忆、检索结果、工具结果。
  - 上下文预算：token budget、优先级裁剪、摘要压缩。
  - Context routing：按任务类型选择 RAG/Memory/Tool/MCP 输入源。
- 交付物：`context_policy.md`、`context_router.py`、预算策略。

### 3) Harness Engineering（H）
- 目标：把“感觉变好”变成“可量化变好”。
- 核心动作：
  - 基线评测集（离线回归 + 在线抽样）。
  - 多指标：正确率、格式合规率、延迟、成本、拒答质量。
  - 变更门禁：未通过 eval 不允许合并。
- 交付物：`eval_dataset.jsonl`、`eval_runner.py`、`eval_report.md`。

## 2 周冲刺（每天都映射 P/C/H）

### Week 1：打地基（可控 + 可观测 + 可评测）

#### Day 1：Prompt 合同化（P）
- 任务：把现有 agent 的 system prompt 升级为“合同模板”（目标、约束、格式、错误策略）。
- 产出：`prompts/system_contract_v1.md`。
- 验收：同一问题 20 次调用，输出结构稳定。

#### Day 2：Context 分层设计（C）
- 任务：设计并实现“会话历史 / 检索片段 / 工具输出”的注入顺序和优先级。
- 产出：`context_policy.md` + 注入顺序流程图。
- 验收：上下文长度受控，关键信息不丢。

#### Day 3：Harness Baseline（H）
- 任务：建立首版评测集（30~50 条），定义指标与阈值。
- 产出：`eval_dataset.jsonl`、`eval_runner.py`。
- 验收：能一键跑出基线报告。

#### Day 4：Prompt 鲁棒性（P+H）
- 任务：对 prompt 做对抗测试（歧义输入、越权请求、格式污染）。
- 产出：失败案例库 + prompt v2。
- 验收：关键失败类型显著下降。

#### Day 5：Context Router（C）
- 任务：实现按任务类型选择输入源（仅历史 / RAG / Tool / MCP）。
- 产出：`context_router.py`。
- 验收：错误检索率下降，答案相关性提升。

#### Day 6：LangSmith 观测接入（H）
- 任务：全链路 trace，标记模型调用、工具调用、延迟热点。
- 产出：观测 dashboard + 3 个典型失败根因。
- 验收：可定位失败发生在 prompt/context/tool 哪一层。

#### Day 7：周验收（P/C/H 联调）
- 任务：跑全套回归，整理“本周版本说明”。
- 产出：`weekly-review-w1.md`。
- 验收：达到你定义的最小上线阈值。

### Week 2：进阶运行时（LangGraph + MCP + Deep Agents）

#### Day 8：Agentic RAG（C+P）
- 任务：实现 2-step / Agentic / Hybrid 三种策略，并在 prompt 中注入检索决策规则。
- 产出：`rag_variants/`。
- 验收：能解释“何时检索、何时拒答”。

#### Day 9：LangGraph Interrupt + Checkpoint（C+H）
- 任务：实现可中断、可恢复执行，thread_id 持续化。
- 产出：`graph_runtime_demo.py`。
- 验收：中断后恢复不丢上下文。

#### Day 10：MCP 接入与上下文协同（C）
- 任务：接入至少 2 个 MCP server，将外部资源纳入 context routing。
- 产出：`mcp_client.py` + 调用策略说明。
- 验收：能稳定调用，且上下文注入可解释。

#### Day 11：Harness v2（H）
- 任务：加入成本/延迟阈值，做版本对比评测（v1 vs v2）。
- 产出：`eval_compare_report.md`。
- 验收：质量提升且成本可控。

#### Day 12：Deep Agents 试点（P+C）
- 任务：尝试规划 + 子代理任务，比较与传统 agent 的收益。
- 产出：`deepagents_poc.md`。
- 验收：给出采用/不采用决策标准。

#### Day 13：发布门禁（H）
- 任务：把 harness 变成发布门禁（未达阈值不放行）。
- 产出：`release_gate.md`。
- 验收：发布前自动生成质量报告。

#### Day 14：RC 与复盘（P/C/H）
- 任务：产出 Release Candidate + 工程复盘。
- 产出：`rc_notes.md` + `postmortem.md`。
- 验收：可演示、可追踪、可回归。

## 每日时间分配（建议）
- 45%：实现（P/C）
- 35%：评测与观测（H）
- 20%：复盘与文档沉淀

## 你这条路线的“完成定义”（Definition of Done）
- P：Prompt 版本化，核心任务输出稳定。
- C：上下文策略可解释，有预算与路由机制。
- H：每次改动都有可比较的评测结果。

## 官方参考（2026 当前建议优先级）
- Agents：https://docs.langchain.com/oss/python/langchain/agents
- Middleware：https://docs.langchain.com/oss/python/langchain/middleware/overview
- Guardrails：https://docs.langchain.com/oss/python/langchain/guardrails
- Retrieval：https://docs.langchain.com/oss/python/langchain/retrieval
- Short-term memory：https://docs.langchain.com/oss/python/langchain/short-term-memory
- Long-term memory：https://docs.langchain.com/oss/python/langchain/long-term-memory
- LangGraph Overview：https://docs.langchain.com/oss/python/langgraph/overview
- LangGraph Interrupts：https://docs.langchain.com/oss/python/langgraph/interrupts
- MCP：https://docs.langchain.com/oss/python/langchain/mcp
- Deep Agents Overview：https://docs.langchain.com/oss/python/deepagents/overview
- LangSmith Observability：https://docs.langchain.com/langsmith/observability
- LangSmith Evaluation：https://docs.langchain.com/langsmith/evaluation
