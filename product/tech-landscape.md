# Hybrid Workspace 技术方案全景（开源/免费）

> 调研时间：2026-04-11 | 仅收录开源/免费方案，付费限制已标注

---

## 1. Agent 编排与多 Agent 框架

### LangGraph
- **做什么：** LangChain 生态的有状态 agent 编排框架，支持单 agent、多 agent、层级式、顺序式控制流
- **链接：** https://github.com/langchain-ai/langgraph
- **协议：** MIT
- **匹配点：** Build（多角色 agent 编排）、Flow（内置 human-in-the-loop、状态管理、中断/恢复）
- **优点：** 生态成熟，HITL 原生支持，长期记忆集成，生产级模板丰富
- **缺点：** 学习曲线陡，深度绑定 LangChain 生态
- **活跃度：** ⭐ 20k+，持续活跃

### AutoGen (v0.4)
- **做什么：** 微软研究院出品，多 agent 对话式协作框架，支持异步消息、代码执行、HITL
- **链接：** https://github.com/microsoft/autogen
- **协议：** MIT
- **匹配点：** Build（角色定义 + 对话编排）、Flow（human-in-the-loop、决策门控）
- **优点：** 跨语言（Python/.NET）、AutoGen Studio 低代码 UI、生产就绪、可分布式部署
- **缺点：** v0.4 API 变动大，社区文档跟不上版本迭代
- **活跃度：** ⭐ 40k+，2025.10 GA

### CrewAI
- **做什么：** 角色扮演式多 agent 协作框架，按"团队"组织 agent，各有角色/目标/工具
- **链接：** https://github.com/crewAIInc/crewAI
- **协议：** MIT
- **匹配点：** Build（角色模板、团队组建）、Flow（Crews + Flows 工作流架构）
- **优点：** 上手快，角色定义直觉化，适合快速原型
- **缺点：** HITL 支持较基础（仅回调）；企业版 CrewAI+ 部分功能收费
- **付费限制：** 核心框架免费，CrewAI+ 企业功能（监控、部署管理）需付费
- **活跃度：** ⭐ 25k+，2025.10 v1.0

### Hermes Agent
- **做什么：** Nous Research 出品，自学习型持久 AI agent，能自主创建/改进 skill，跨 session 记忆
- **链接：** https://github.com/nousresearch/hermes-agent
- **协议：** MIT
- **匹配点：** Build（持久运行的 agent 基座）、Train（自学习 loop、skill 自进化）、Flow（sub-agent 编排）
- **优点：** 内置学习循环和 skill 沉淀，多平台接入（Telegram/Discord/Slack），模型无关（200+ endpoints）
- **缺点：** 2026 年初刚发布，生态尚不成熟；文档偏少
- **活跃度：** ⭐ 15k+（增长快），2026 初发布

### OpenAI Agents SDK
- **做什么：** 轻量 Python 框架，多 agent 工作流 + 内置 tracing + guardrails
- **链接：** https://github.com/openai/openai-agents-python
- **协议：** MIT
- **匹配点：** Build（agent 定义）、Flow（guardrails、tracing）
- **优点：** 极简 API，兼容 100+ LLM（非绑定 OpenAI），内置 guardrails
- **缺点：** 功能相对基础，不适合复杂编排
- **活跃度：** ⭐ 15k+，2025.03 发布

### Google Agent Development Kit (ADK)
- **做什么：** 模块化 agent 框架，支持层级式 agent 组合
- **链接：** https://github.com/google/adk-python
- **协议：** Apache 2.0
- **匹配点：** Build（层级 agent 组合、自定义工具）
- **优点：** 与 Gemini/Vertex AI 深度集成，模块化设计
- **缺点：** 生态较新，社区小；Vertex AI 部分功能需付费
- **付费限制：** 框架免费，部分 Vertex AI 服务需付费
- **活跃度：** 2025.04 发布

### Agno (AgentOS)
- **做什么：** 多 agent 框架，内置生产运行时（AgentOS）和控制平面，模型无关 + 多模态
- **链接：** https://github.com/agno-agi/agno
- **协议：** Apache 2.0
- **匹配点：** Build（agent 运行时）、Flow（状态管理、可观测性）
- **优点：** 内置执行引擎、状态和可观测性管理
- **缺点：** 较新项目，生态尚在建设中
- **活跃度：** ⭐ 增长中

---

## 2. Memory 与知识库

### Mem0
- **做什么：** 通用 AI 记忆层，多级记忆架构（用户偏好/会话上下文/agent 知识），自动过滤+衰减
- **链接：** https://github.com/mem0ai/mem0
- **协议：** Apache 2.0
- **匹配点：** Build（共享知识库）、Train（跨 session 学习积累）
- **优点：** 记忆自动管理（过滤/衰减），支持向量库 + 知识图谱，Python/JS 双语言，benchmark 表现优秀
- **缺点：** 托管版部分功能收费
- **付费限制：** 开源版全功能可用，Mem0 Platform（托管版）按用量收费
- **活跃度：** ⭐ 25k+，非常活跃

### Letta (原 MemGPT)
- **做什么：** 有状态 agent 平台，基于 MemGPT 原理，层级记忆 + 可编辑记忆块 + agent 自控上下文工程
- **链接：** https://github.com/letta-ai/letta
- **协议：** Apache 2.0
- **匹配点：** Build（长期记忆基座）、Train（持续学习、自我改进）
- **优点：** Agent 自主管理记忆，支持记忆编辑/更新/进化，模型无关
- **缺点：** 架构较重，部署复杂度高
- **活跃度：** ⭐ 15k+，活跃

### LangMem
- **做什么：** LangChain 的长期记忆 SDK，支持语义/情景/过程记忆，存储后端无关
- **链接：** https://github.com/langchain-ai/langmem
- **协议：** MIT
- **匹配点：** Train（实时学习 + 背景知识更新）、Build（多类型记忆管理）
- **优点：** 轻量灵活，存储无关（向量库/文档库/图库均可），实时+后台双模式记忆管理
- **缺点：** 2025 年初发布，相对年轻；依赖 LangChain 生态
- **活跃度：** ⭐ 3k+，活跃

### Google Always On Memory Agent
- **做什么：** LLM 驱动的持久记忆方案，无需向量数据库
- **链接：** 开源（GitHub，Google PM 个人项目）
- **匹配点：** Build（轻量记忆方案）
- **优点：** 架构简单，不依赖向量库
- **缺点：** 个人项目，成熟度低
- **活跃度：** 早期

---

## 3. 自学习与 Skill 进化

### Memento-Skills
- **做什么：** 让 agent 自主进化 skill 的框架，无需重训 LLM，作为外部记忆持续改进能力
- **链接：** GitHub（2026 发布）
- **匹配点：** Train（Skill 自进化、环境反馈驱动改进）——**与我们"纠正→规则沉淀→Skill 进化"需求高度匹配**
- **优点：** 不重训模型即可进化，外部记忆架构
- **缺点：** 2026 年新发布，生产验证不足

### OpenSpace (HKUDS)
- **做什么：** 自进化 skill 引擎，捕获已完成任务模式并复用，支持 FIX/DERIVED/CAPTURED 三种进化模式
- **链接：** GitHub（HKUDS）
- **匹配点：** Train（任务模式自动沉淀、token 节省 46%）
- **优点：** 三种进化模式覆盖不同场景，token 效率显著提升
- **缺点：** 学术项目，工程化程度待验证

### Self-Improving Skills (Rbuid.ai)
- **做什么：** Skill 自改进框架（为 Claude Code 等设计），捕获反馈 → 版本管理 → 自动进化，含模拟 agent 压测
- **链接：** GitHub 开源
- **匹配点：** Train（反馈捕获→规则沉淀→版本迭代）——**模拟 agent 压测功能很有价值**
- **优点：** 反馈闭环完整，含压测验证
- **缺点：** 针对 Claude Code 场景设计，通用性需验证

### EvoAgentX
- **做什么：** agent 进化与优化技术的分类体系 + 自动化 agent 工作流进化框架
- **链接：** https://github.com/EvoAgentX/Awesome-Self-Evolving-Agents
- **匹配点：** Train（进化方法论参考）
- **优点：** 学术综述完整，方法分类清晰（单 agent/多 agent/领域特定）
- **缺点：** 偏学术，直接可用性有限

### Darwin Gödel Machine (DGM)
- **做什么：** 开放式自我改进系统，反复生成并评估自修改变体，构建改进档案
- **链接：** 学术论文 + 开源代码
- **匹配点：** Train（自我改进机制）
- **优点：** 理论扎实，开放式进化
- **缺点：** 偏研究，生产距离远

---

## 4. 行为合规与 Guardrails

### NVIDIA NeMo Guardrails
- **做什么：** 可编程 guardrails 框架，用 Colang 语言定义对话安全规则
- **链接：** https://github.com/NVIDIA/NeMo-Guardrails
- **协议：** Apache 2.0
- **匹配点：** Train（行为合规检查）、Flow（输入/输出安全过滤）
- **优点：** Colang 规则语言表达力强，支持自定义安全策略，可拦截 prompt injection
- **缺点：** Colang 学习成本，性能开销
- **活跃度：** ⭐ 5k+，活跃

### Guardrails AI
- **做什么：** LLM 输出验证框架，用 validators 确保输出符合预期格式/内容/安全要求
- **链接：** https://github.com/guardrails-ai/guardrails
- **协议：** Apache 2.0
- **匹配点：** Train（输出合规校验）
- **优点：** Validator 生态丰富，支持结构化输出验证
- **缺点：** 主要关注输出验证，不覆盖行为级合规
- **付费限制：** 开源核心免费，Guardrails Hub 部分 validator 需注册
- **活跃度：** ⭐ 5k+

### Invariant Guardrails
- **做什么：** 基于规则的 guardrail 代理层，部署为 proxy 监控和拦截 agent 的工具调用
- **链接：** https://github.com/invariantlabs-ai/invariant
- **协议：** 开源
- **匹配点：** Flow（实时拦截不安全工具调用）、Train（规则定义行为边界）——**proxy 架构很适合我们的 Kill Switch 需求**
- **优点：** 外部执行（agent 无法绕过），规则式确定性策略
- **缺点：** 规则需要手写维护

### OpenGuardrails
- **做什么：** 统一检测不安全/被操纵/隐私违规内容，可配置策略适配不同场景
- **链接：** https://openguardrails.com / GitHub
- **协议：** 开源（2025.11 发布）
- **匹配点：** Train（prompt injection 防护、数据泄露检测）
- **优点：** 统一框架覆盖多种安全场景，运行时防护
- **缺点：** 较新，社区小

### Microsoft Agent Governance Toolkit
- **做什么：** agent 运行时安全治理，针对 OWASP agentic AI 风险的确定性策略执行
- **链接：** GitHub（2026.04 开源）
- **协议：** MIT
- **匹配点：** Flow（运行时安全治理、目标劫持防护）
- **优点：** 微软出品，针对 OWASP Top 10 for Agentic Apps
- **缺点：** 2026.04 刚发布，极新

### Superagent
- **做什么：** 内置安全的 agent 框架，通过 Safety Agent 组件定义角色权限和操作限制
- **链接：** GitHub 开源（2025.12）
- **协议：** 开源
- **匹配点：** Build（角色权限定义）、Flow（Safety Agent 限制工具调用/数据访问）
- **优点：** 安全作为一等公民内置
- **缺点：** 新项目

---

## 5. 工作流引擎与人类门控

### Temporal
- **做什么：** 持久化工作流引擎，支持长时间运行的 agent 任务编排、HITL 审批、自动重试
- **链接：** https://github.com/temporalio/temporal
- **协议：** MIT
- **匹配点：** Flow（决策门控、human approval、状态持久化、自动重试）——**我们工作流引擎的强候选**
- **优点：** 生产级，状态持久化，天然支持长任务 + 人类审批 + 错误恢复
- **缺点：** 架构重，需要单独部署 Temporal Server
- **活跃度：** ⭐ 12k+，企业级成熟

### n8n
- **做什么：** 可视化 AI 工作流自动化平台，原生 AI 节点（工具/记忆/结构化输出）
- **链接：** https://github.com/n8n-io/n8n
- **协议：** Sustainable Use License（source-available，非严格开源）
- **匹配点：** Flow（可视化工作流、多 agent 编排）
- **优点：** 可视化 builder，400+ 集成，self-host 免费
- **付费限制：** self-host 免费，n8n Cloud 按用量收费；License 限制商业竞品使用
- **活跃度：** ⭐ 60k+，极活跃

### Loopgate
- **做什么：** 轻量 HITL 控制平面，agent 暂停→请求人类审批（如通过 Telegram）→等待响应后继续
- **链接：** GitHub 开源
- **匹配点：** Flow（决策门控、Kill Switch）——**极轻量的 HITL 实现，值得参考**
- **优点：** 极简，即插即用
- **缺点：** 功能单一，仅做审批门控

### KaibanJS
- **做什么：** JavaScript 多 agent 编排框架，原生动态 HITL 支持，任务可标记为需人类验证
- **链接：** GitHub 开源（2025.06）
- **协议：** MIT
- **匹配点：** Flow（动态 HITL、任务状态管理）
- **优点：** JS 原生，HITL 设计灵活（状态机模式）
- **缺点：** JS 生态，与 Python agent 框架集成需桥接
- **活跃度：** 2025.06 发布

### Flowise
- **做什么：** 低代码可视化 agent 构建平台，基于 LangChain/LlamaIndex
- **链接：** https://github.com/FlowiseAI/Flowise
- **协议：** Apache 2.0
- **匹配点：** Build（低代码 agent 构建）、Flow（Agentflow 多 agent 系统）
- **优点：** 拖拽式构建，上手极快
- **缺点：** 复杂编排能力有限
- **活跃度：** ⭐ 35k+

---

## 6. 浏览器自动化与工具层

### Playwright MCP Server
- **做什么：** Playwright 的 MCP 服务器，让 AI agent 通过 MCP 协议控制浏览器（基于无障碍树，非截图）
- **链接：** https://github.com/microsoft/playwright-mcp
- **协议：** Apache 2.0
- **匹配点：** Build（agent 工具层 — 浏览器操作能力）
- **优点：** 无障碍树方式比截图快且省 token，MCP 标准协议，微软维护
- **缺点：** 仅提供浏览器控制，不含 agent 逻辑

### Stagehand
- **做什么：** 浏览器 AI 框架，AI 运行时解析指令（非硬编码选择器），抗页面改版
- **链接：** https://github.com/browserbase/stagehand
- **协议：** MIT
- **匹配点：** Build（自愈式浏览器操作）
- **优点：** 自愈定位器，不怕 UI 变更
- **缺点：** 依赖 Browserbase 云服务获得最佳体验
- **付费限制：** 框架开源，Browserbase 云浏览器需付费
- **活跃度：** ⭐ 10k+

### Agent-Browser (Vercel Labs)
- **做什么：** AI agent 专用浏览器 CLI，语义定位器 + 紧凑元素引用，节省 93% 上下文窗口
- **链接：** GitHub（Vercel Labs，2026.01）
- **协议：** 开源
- **匹配点：** Build（token 高效的浏览器操作）
- **优点：** 极致 token 优化
- **缺点：** CLI 形态，需集成到 agent 框架

### Browser-Use
- **做什么：** 让 AI agent 直接控制浏览器执行复杂任务的开源框架
- **链接：** https://github.com/browser-use/browser-use
- **协议：** MIT
- **匹配点：** Build（浏览器作为 agent 工具）
- **优点：** 社区活跃，API 简洁
- **活跃度：** ⭐ 50k+，极活跃

---

## 7. 其他相关

### MCP (Model Context Protocol)
- **做什么：** Anthropic 提出的开放协议标准，让 AI agent 以统一方式连接外部工具/数据/服务
- **链接：** https://github.com/modelcontextprotocol
- **协议：** 开放标准
- **匹配点：** Build（统一工具接入层）、Flow（agent 可移植性）
- **优点：** 行业标准化趋势，主流框架均在适配
- **缺点：** 协议层，需各工具实现 MCP server

### Dify
- **做什么：** 可视化低代码 AI 应用开发平台，支持 agent/工作流/RAG
- **链接：** https://github.com/langgenius/dify
- **协议：** Apache 2.0（附加条款）
- **匹配点：** Build（快速搭建 agent 应用）
- **付费限制：** self-host 免费（限制多人协作），Cloud 按用量收费
- **活跃度：** ⭐ 60k+，极活跃

### Haystack
- **做什么：** 开源 AI 编排框架，管道式架构，覆盖检索/推理/记忆/工具使用
- **链接：** https://github.com/deepset-ai/haystack
- **协议：** Apache 2.0
- **匹配点：** Build（模块化 agent 管道）
- **优点：** 透明度高，上下文工程导向，生产就绪
- **活跃度：** ⭐ 18k+

### Agent Zero
- **做什么：** 全透明 AI agent 框架，agent 可自生成工具、学习、自纠正、执行工作流
- **链接：** https://github.com/frdel/agent-zero
- **协议：** 开源
- **匹配点：** Train（自纠正 + 工具自生成）、Build（全透明执行）
- **活跃度：** ⭐ 5k+

---

## 关键发现与推荐组合

### 按 Hybrid Workspace 三大需求匹配度排序：

| 需求维度 | 最佳候选 | 备选 |
|---------|---------|------|
| **Build — 组建 AI 团队** | CrewAI（角色模板）+ Mem0/Letta（共享知识库） | AutoGen, Agno |
| **Train — 培训 AI 员工** | Memento-Skills / OpenSpace（Skill 进化）+ LangMem（记忆学习）+ NeMo Guardrails（合规检查） | Hermes Agent（内置学习循环）, Self-Improving Skills |
| **Flow — 碳硅协作** | Temporal（工作流引擎 + HITL）+ Invariant Guardrails（Kill Switch/拦截）+ Loopgate（轻量门控） | LangGraph（状态 + HITL）, n8n（可视化） |

### 值得重点深入的方案：
1. **Hermes Agent** — 唯一同时覆盖 Build+Train 的框架，自学习+持久记忆+skill 进化，与我们理念最接近
2. **Temporal** — Flow 层最成熟的工作流引擎，天然支持人类审批+长任务+状态持久化
3. **Invariant Guardrails** — proxy 架构的外部 guardrail，agent 无法绕过，适合 Kill Switch
4. **Memento-Skills** — Skill 自进化理念与我们"纠正→沉淀→进化"高度吻合
5. **Mem0** — 最成熟的记忆层，多级架构适合团队共享知识库
