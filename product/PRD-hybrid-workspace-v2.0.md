# PRD: Hybrid Workspace v2.0

| 字段 | 内容 |
|------|------|
| **产品名称** | Hybrid Workspace |
| **版本** | v2.0 |
| **日期** | 2026-04-13 |
| **作者** | pm-Octopus |
| **状态** | Draft |

---

## 版本变更记录

| 版本 | 日期 | 变更内容 | 变更人 |
|------|------|----------|--------|
| v1.0 | 2026-04-11 | Agent Builder PRD 初版（表单驱动） | pm-Octopus |
| v1.1 | 2026-04-11 | 补充技术说明、规则面板设计 | pm-Octopus |
| v2.0 | 2026-04-13 | 全面重写：表单驱动改为对话驱动，规则从对话自动抽取，Web 内嵌聊天取代 Discord，Flow 改为 Process，覆盖 Chat 版和管理版两个 Prototype | pm-Octopus |

---

## 1. 概述

### 1.1 背景

Hybrid Workspace 是碳硅混合团队的操作系统。核心承诺：**Teach Once, Remember Forever**。

v1.0 采用表单驱动的 Agent Builder 设计（选角色 - 勾技能 - 配规则 - 生成）。经过团队讨论，v2.0 做出以下根本性变更：

| 维度 | v1.0 | v2.0 |
|------|------|------|
| 交互模式 | 表单 Builder（点选操作） | 对话驱动（自然语言） |
| 规则创建 | 手动配置红线/建议规则 | 从对话中自动抽取，人类审批 |
| 聊天界面 | Discord 嵌入 | Web 原生内嵌聊天 |
| 流程管理 | Flow（可视化流程图） | Process（规则驱动，系统强制执行） |

### 1.2 产品定位

Cloud 托管 Demo，面向非技术用户和投资人演示。不是完整产品，不需要多租户、计费、数据隔离。

### 1.3 目标用户

- 投资人（零技术背景，需要 wow moment）
- 非技术创始人/管理者（想体验 AI 团队协作）

### 1.4 成功指标

| 指标 | 目标值 |
|------|--------|
| 从打开页面到 3 个 agent 上线 | 1 分钟内（含对话式创建） |
| 用户通过自然语言完成团队创建 | 100% |
| Agent 在聊天界面首条消息 | 创建后 10 秒内 |
| 纠正一次后规则永久生效 | 100%（跨 session 验证） |
| Demo 全程（四幕）完成 | 5 分钟内 |
| Demo 观众理解"这是一个 AI 团队在协作" | 定性验证 |

---

## 2. 产品架构

### 2.1 两个 Prototype

产品由两个互补的界面组成：

**Prototype 1: Chat 版** -- 日常工作入口
- Build：通过对话创建 AI 团队
- Work：在聊天界面下指令，agent 协作干活
- Train：纠正 agent 行为，系统自动抽取规则
- Process：流程执行和拦截在聊天中实时展现

**Prototype 2: 管理版** -- 查看和管理
- Knowledge：项目知识库（PRD、决策、经验）
- Rules：行为规则列表（来源、状态、触发场景）
- Team：agent 状态和配置
- Process：流程定义和执行记录

### 2.2 四层技术架构

| 层级 | 名称 | 职责 |
|------|------|------|
| L1 | Harness Layer | 角色模板自带技能、规则与行为边界 |
| L2 | Memory Layer | 决策、经验、上下文跨 Agent 持久传承 |
| L3 | Learning Layer | 从人类纠正中自主学习，沉淀为规则 |
| L4 | Orchestration Layer | 人类定义流程，系统强制执行 |

### 2.3 系统架构

```
Web Frontend (React/Next.js)
  Chat + Knowledge + Rules + Team + Process
         |
Hybrid Workspace API (Node/Python)
  Agent 生命周期 / 规则引擎 / 审批状态机 / Kill Switch
         |
OpenClaw (Agent Runtime)
  执行环境 / pre-tool-call hook / Git repo
```

---

## 3. 功能清单

### P0 -- 必须交付

| # | 功能 | 所属模块 | 说明 |
|---|------|----------|------|
| 1 | 对话式团队创建 | Build | 用户用自然语言描述需求，系统推荐角色组合并创建 |
| 2 | Web 内嵌聊天 | Work | 原生聊天界面，支持多 agent 频道，替代 Discord |
| 3 | 对话指令下发 | Work | 用户在聊天界面下指令，agent 接收并执行 |
| 4 | 纠正检测与规则抽取 | Train | 系统从对话中检测纠正行为，自动提炼规则提案 |
| 5 | 人类审批规则 | Train | 规则提案需人类确认才生效 |
| 6 | 规则跨 session 生效 | Train | 审批通过的规则永久生效，跨 session 不丢失 |
| 7 | 流程强制执行 | Process | Agent 跳步时系统自动拦截 |
| 8 | Kill Switch | Process | 人类随时强制中断 agent 执行 |

### P1 -- 重要但可延后

| # | 功能 | 所属模块 | 说明 |
|---|------|----------|------|
| 9 | Knowledge 页面 | 管理版 | 浏览项目知识库（PRD、决策记录、踩坑经验） |
| 10 | Rules 页面 | 管理版 | 查看所有规则、来源、状态、触发记录 |
| 11 | Team 页面 | 管理版 | 查看 agent 状态、角色、当前任务 |
| 12 | Process 页面 | 管理版 | 查看流程定义和执行记录 |
| 13 | 规则推送给同角色 agent | Train | PM 学到的规则推送给其他 PM agent |
| 14 | 场景触发式规则召回 | Train | 执行特定工具前自动注入相关规则 |

### P2 -- 远期

| # | 功能 | 所属模块 | 说明 |
|---|------|----------|------|
| 15 | 自定义角色模板 | Build | 用户创建自定义角色 |
| 16 | 团队模板导入 | Build | 预设团队组合一键创建 |
| 17 | 规则自动升级/降级 | Train | 违反 N 次自动升级（建议到红线），长期未违反降级 |
| 18 | Live Monitor | Process | 实时查看 agent 当前动作和流程进度 |

---

## 4. 功能模块详细设计

---

## 4.1 对话式团队创建（Build）

### 4.1.1 目标

- 核心目标：用户通过自然语言对话，在 1 分钟内创建一支 AI 团队
- 体验原则：像跟人事说"帮我组个产品团队"一样自然，不需要理解技术概念

### 4.1.2 用户流程

**正常路径：**

1. 用户打开 Hybrid Workspace，进入聊天界面
2. 系统发送欢迎消息，引导用户描述需求
3. 用户输入类似"I need a team to build a landing page"
4. 系统分析需求，推荐角色组合（如 PM + Dev + QA）
5. 展示角色卡片预览（内嵌在聊天消息中），包含角色名、职责描述和装载的 Skills 列表：
   - PM-Octopus — Skills: `prd-writing`, `product-design`, `ui-review`
   - Dev-Fox — Skills: `code-impl`, `pr-submit`, `code-review`
   - QA-Eagle — Skills: `e2e-testing`, `acceptance`, `bug-report`
6. 用户确认"looks good"或调整"不需要 QA"
7. 系统创建 agent，每个 agent 在聊天频道自我介绍
8. 团队就位，用户可以开始下指令

**调整路径：**

- 用户说"再加一个设计师" -- 系统创建额外 agent
- 用户说"把 QA 去掉" -- 系统移除该 agent，确认后执行
- 用户描述模糊 -- 系统追问"你的项目主要做什么？需要写代码还是只做设计？"

**错误路径：**

- 用户输入完全无关内容 -- 系统引导回团队创建话题
- 创建失败 -- 系统提示错误原因，提供重试

### 4.1.3 逻辑规则

- 角色模板库预设 3 个角色：PM、Dev、QA
- 每个角色自带：基础技能（锁定）+ 角色专属技能（预选，可调整）+ 行为规则（红线锁定，建议可开关）
- 最少创建 1 个 agent
- 创建完成后自动进入 Work 模式，agent 在频道发送首条消息
- 系统需在用户确认后才执行创建，不自动创建

### 4.1.4 文案

| 场景 | 文案 |
|------|------|
| 欢迎消息 | "Welcome to Hybrid Workspace. Tell me about your project and I'll help you assemble the right team." |
| 推荐角色 | "Based on your needs, I recommend a team of 3: [角色列表]. Here's what each one brings:" |
| 确认创建 | "Ready to bring your team online?" |
| 创建成功 | "Your team is live. They'll introduce themselves now." |
| Agent 自我介绍（PM） | "I'm your PM. I'll handle requirements, PRDs, and product decisions. What's our first task?" |
| Agent 自我介绍（Dev） | "Dev here. Point me to the specs and I'll start building." |
| Agent 自我介绍（QA） | "QA reporting in. I'll set up test plans once we have requirements." |

### 4.1.5 技术说明

- 前端：聊天消息中内嵌角色卡片组件（非独立页面）
- 后端：对话意图识别 -- 判断用户描述的项目类型，匹配角色模板
- Agent 创建：调用 OpenClaw API 生成 workspace 配置文件（SOUL.md / AGENTS.md / Skills）
- 角色模板存储在 Git repo 的 `templates/` 目录

### 4.1.6 不做的事（MVP）

- 不做自定义角色创建（只用预设模板）
- 不做角色能力的细粒度配置（技能面板在管理版查看，不在对话中逐项配置）
- 不做多项目管理（Demo 只演示单项目）

### 4.1.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | 用户输入"I need a team to build a landing page"，系统推荐 PM + Dev + QA |
| 2 | 用户确认后，3 个 agent 在 10 秒内上线并发送自我介绍 |
| 3 | 用户说"remove QA"，系统确认后移除 QA agent |
| 4 | 用户输入模糊描述，系统追问澄清 |
| 5 | 全程不需要用户填写任何表单 |

---

## 4.2 Web 内嵌聊天（Work）

### 4.2.1 目标

- 核心目标：用户在 Web 原生聊天界面与 AI 团队协作，不依赖外部工具
- 体验原则：像 Slack/Discord 一样的即时通讯体验，但专为人机协作设计

### 4.2.2 用户流程

**正常路径：**

1. 团队创建完成后，自动进入项目聊天频道（# general）
2. 左侧导航显示：项目频道列表 + Chat 下的子频道（# general / # design-review / # dev-standup / # bug-triage）+ agent 列表
3. 用户在频道输入指令，如"PM, write a PRD for the landing page"
4. 被 @ 的 agent 响应，其他 agent 可见但不主动介入
5. Agent 执行结果以消息形式展示（支持 markdown、代码块、文件链接）
6. 用户可以 @ 多个 agent 协作，如"Dev, implement what PM just wrote"
7. 用户可以创建新频道或 thread 讨论特定话题

**Agent 间协作：**

- PM 输出 PRD 后，Dev 可引用该消息开始实现
- QA 在 Dev 提交后自动收到通知，开始验收
- 协作通过消息引用和 @ 触发，不需要用户手动编排

### 4.2.3 逻辑规则

- 每个项目有一个主频道，所有 agent 和用户共享
- @ 特定 agent 时只有被 @ 的 agent 响应
- 不 @ 任何人时，系统根据消息内容判断最合适的 agent 响应
- Agent 响应时显示角色标签（PM / Dev / QA）
- 消息支持 markdown 渲染、代码高亮、文件附件
- Agent 执行长任务时显示进度状态（思考中 / 执行中 / 等待审批）

### 4.2.4 文案

| 场景 | 文案 |
|------|------|
| 频道标题 | "[项目名] - Team Chat" |
| Agent 执行中状态 | "[角色名] is working on this..." |
| Agent 等待审批 | "[角色名] needs your approval to proceed" |
| Agent 完成 | "[角色名] completed the task" |
| Agent 被拦截 | "This action was blocked. [规则名] requires [缺少的步骤] first." |

### 4.2.5 技术说明

- 前端：React 组件，WebSocket 实时消息推送
- 消息格式：支持 markdown、代码块、内嵌卡片（角色预览、规则提案、审批按钮）
- 后端：消息路由到对应 agent 的 OpenClaw session
- 不使用 Discord API，完全自建聊天

### 4.2.6 不做的事（MVP）

- 不做消息搜索
- 不做文件上传/下载
- 不做消息编辑/删除
- 不做多项目频道切换
- 不做离线消息同步

### 4.2.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | 用户 @ PM 发消息，只有 PM 响应 |
| 2 | 用户不 @ 任何人，系统选择最合适的 agent 响应 |
| 3 | Agent 响应消息正确渲染 markdown 和代码块 |
| 4 | Agent 执行长任务时显示实时状态 |
| 5 | 多个 agent 消息按时间顺序正确排列 |

---

## 4.3 纠正检测与规则抽取（Train）

### 4.3.1 目标

- 核心目标：Teach Once, Remember Forever -- 人类纠正一次，系统自动提炼规则，永久生效
- 体验原则：用户只需要像带新员工一样说话，不需要手动编写规则

### 4.3.2 用户流程

**正常路径：**

1. Agent 执行任务时犯错（如 QA 没写 test case 就验收）
2. 用户在聊天中纠正："QA, you must write test cases before starting acceptance"
3. 系统检测到纠正行为，分析对话上下文
4. 系统在聊天中展示规则提案卡片：
   - 规则内容："QA 验收前必须先写 test case"
   - 规则类型：红线（必须遵守）
   - 适用角色：QA
   - 触发场景：验收任务
5. 用户审批：点击"Approve"确认，或"Edit"修改后确认
6. 规则写入系统，立即生效
7. 下次 QA 执行验收时，系统在 pre-tool-call hook 中注入该规则

**拒绝路径：**

- 用户点击"Dismiss" -- 规则提案被丢弃，不影响后续
- 用户修改规则内容 -- 以修改后的版本生效

**误检测路径：**

- 系统错误地将普通对话识别为纠正 -- 用户忽略即可，不强制操作

### 4.3.3 逻辑规则

- 纠正检测触发条件：用户消息中包含否定/纠正意图（"不要"、"必须"、"以后"、"每次都"等模式）
- 规则分级：红线（必须遵守，违反即拦截）/ 建议（提醒但不拦截）
- 规则归属：按角色标签（PM / Dev / QA），可选推送给同角色其他 agent
- 规则存储：写入 Git repo 的 `rules/` 目录，结构化 YAML 格式
- 规则生效：写入后立即跨 session 生效，通过 pre-tool-call hook 注入
- 规则不自动删除，只能人类手动在管理版中停用

### 4.3.4 文案

| 场景 | 文案 |
|------|------|
| 检测到纠正 | "I noticed a correction. Want to make this a permanent rule?" |
| 规则提案卡片标题 | "New Rule Proposal" |
| 审批按钮 | "Approve" / "Edit" / "Dismiss" |
| 审批成功 | "Rule saved. [角色名] will follow this from now on." |
| 规则触发提醒 | "Reminder: [规则内容] (Rule #XX)" |

### 4.3.5 技术说明

- 纠正检测：LLM 分析对话上下文，判断是否为纠正行为（不用关键词匹配，用语义理解）
- 规则抽取：LLM 从纠正对话中提炼结构化规则（if-then 格式，含触发场景和行为要求）
- 规则存储：YAML 文件存储在 Git repo，格式如下：
  ```yaml
  - id: rule-001
    content: "QA must write test cases before starting acceptance"
    type: redline  # redline | suggestion
    roles: [qa]
    trigger_scene: acceptance
    source: "conversation 2026-04-13, user correction"
    status: active
    created_at: 2026-04-13T10:30:00Z
  ```
- 规则注入：OpenClaw pre-tool-call hook 在 agent 执行工具前按 trigger_scene 匹配并注入相关规则
- 审批流：前端展示规则提案卡片，用户操作通过 API 写入 repo

### 4.3.6 不做的事（MVP）

- 不做规则自动升级/降级（违反 N 次升级）-- 远期 P2
- 不做规则冲突检测（两条规则矛盾时的处理）
- 不做规则版本管理（Git 天然有版本，但不做 UI 展示）
- 不做批量规则导入

### 4.3.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | 用户纠正 agent 行为后，系统在 10 秒内展示规则提案 |
| 2 | 用户点击 Approve，规则立即生效 |
| 3 | 新 session 中 agent 遵守已审批规则 |
| 4 | 用户点击 Dismiss，规则不生效，agent 行为不变 |
| 5 | 用户编辑规则内容后审批，以编辑后版本生效 |
| 6 | 普通对话不触发规则提案（误检率低） |

---

## 4.4 流程强制执行（Process）

### 4.4.1 目标

- 核心目标：人类定义流程规则，agent 必须按规则走，违反系统强制拦截
- 体验原则：流程不是建议，是强制。Agent 跳步和人类说"停"都必须生效

### 4.4.2 用户流程

**正常路径（流程执行）：**

1. 系统预设标准流程（如需求变更流程：开 issue - 更新 PRD - 人类审批 - 开发）
2. Agent 按流程执行，每完成一步在聊天中报告
3. 到达审批节点时，系统在聊天中发送审批卡片
4. 用户点击 Approve / Reject
5. Approve 后 agent 继续执行下一步

**拦截路径：**

1. Agent 试图跳过步骤（如 Dev 没等 PRD 审批就开始写代码）
2. 系统拦截，阻止执行
3. 聊天中展示拦截消息，说明缺少哪个前置步骤
4. Agent 自动回退到正确步骤

**Kill Switch 路径：**

1. 用户在任何时刻输入"stop"或点击 Kill Switch 按钮
2. 系统立即中断所有 agent 执行
3. 聊天中展示中断确认
4. Agent 状态冻结，等待用户指令

### 4.4.3 逻辑规则

- 流程定义存储在 Git repo 的 `processes/` 目录，YAML 格式
- 流程由步骤序列组成，每步定义：执行者、前置条件、是否需要审批
- 系统在 agent 执行动作前检查流程状态，不满足前置条件则拦截
- Kill Switch 双层实现：
  - Gateway 层：中断 LLM 调用，冻结 session
  - Proxy 层：tool call 拦截，agent 无法绕过
- "stop"指令 5 秒内生效
- 流程示例：
  ```yaml
  process: requirement-change
  steps:
    - id: open-issue
      actor: pm
      action: create GitHub issue
    - id: update-prd
      actor: pm
      action: update PRD document
      requires: [open-issue]
    - id: human-review
      actor: human
      action: approve PRD changes
      requires: [update-prd]
      type: gate
    - id: implement
      actor: dev
      action: write code
      requires: [human-review]
  ```

### 4.4.4 文案

| 场景 | 文案 |
|------|------|
| 审批请求 | "[角色名] has completed [步骤名]. Approve to continue?" |
| 审批按钮 | "Approve" / "Reject" |
| 拦截消息 | "Blocked: [角色名] tried to [动作], but [前置步骤] hasn't been completed yet." |
| Kill Switch 触发 | "All agents stopped. Waiting for your instructions." |
| 流程完成 | "Process [流程名] completed successfully." |

### 4.4.5 技术说明

- 流程引擎：状态机实现，每个流程实例维护当前步骤和完成记录
- 拦截机制：agent 发起 tool call 前，系统检查流程状态机，不满足前置条件则阻断
- Kill Switch：Gateway 层中断 + Proxy 层拦截，双层保证
- 流程状态存储在 API 层，前端通过 WebSocket 实时获取状态更新
- 审批操作通过 API 更新状态机，触发下一步执行

### 4.4.6 不做的事（MVP）

- 不做可视化流程编辑器（流程用 YAML 手写，管理版只展示）
- 不做自定义流程创建（MVP 预设标准流程）
- 不做流程分支和并行步骤
- 不做流程模板市场

### 4.4.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | Dev 在 PRD 审批前尝试写代码，系统拦截并提示 |
| 2 | 用户 Approve 后，agent 自动继续执行下一步 |
| 3 | 用户 Reject 后，agent 回退到上一步 |
| 4 | 用户输入"stop"，5 秒内所有 agent 停止 |
| 5 | Kill Switch 按钮点击后，agent 立即停止 |
| 6 | 拦截消息准确说明缺少哪个前置步骤 |

---

## 4.5 Knowledge 页面（管理版）

### 4.5.1 目标

- 核心目标：提供项目知识的结构化浏览和管理入口
- 体验原则：像企业 Wiki 一样浏览，所有知识可追溯

### 4.5.2 用户流程

1. 用户在左侧导航点击 Knowledge
2. 主区域展示知识库目录树：按类型分组（PRD / 决策记录 / 技术文档 / 经验教训）
3. 点击文档标题，右侧展示内容（markdown 渲染）
4. 每个文档显示元信息：作者（人类/agent）、创建时间、最后修改时间

### 4.5.3 逻辑规则

- 数据源：Git repo 的 `knowledge/` 目录
- 文档格式：markdown，带 YAML frontmatter（title / type / author / date）
- 只读浏览，不支持在 Web 上编辑（编辑通过对话指令完成）
- 支持按类型过滤

### 4.5.4 文案

| 场景 | 文案 |
|------|------|
| 页面标题 | "Knowledge Base" |
| 空状态 | "No documents yet. Ask your team to create a PRD to get started." |
| 文档类型标签 | "PRD" / "Decision" / "Technical" / "Lesson Learned" |

### 4.5.5 技术说明

- 前端从 API 获取 Git repo 文件列表和内容
- Markdown 渲染使用 react-markdown 或类似库
- 文档变更通过 Git webhook 实时刷新

### 4.5.6 不做的事（MVP）

- 不做全文搜索
- 不做在线编辑
- 不做评论/批注
- 不做权限管理

### 4.5.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | Knowledge 页面正确展示 Git repo 中的文档列表 |
| 2 | 点击文档，内容正确渲染 markdown |
| 3 | 文档元信息（作者、时间）正确显示 |
| 4 | Agent 通过对话创建的文档，自动出现在 Knowledge 页面 |

---

## 4.6 Rules 页面（管理版）

### 4.6.1 目标

- 核心目标：展示所有行为规则的来源、状态和触发记录
- 体验原则：让用户清楚知道团队有哪些规则、从哪来、生效了几次

### 4.6.2 用户流程

1. 用户在左侧导航点击 Rules
2. 主区域展示规则列表，每条规则显示：内容、类型（红线/建议）、适用角色、来源、状态、触发次数
3. 用户可以切换规则状态（启用/停用）
4. 点击规则展开详情：来源对话片段、触发历史

### 4.6.3 逻辑规则

- 数据源：Git repo 的 `rules/` 目录（YAML 文件）
- 规则状态：active / disabled
- 切换状态即修改 YAML 并 commit
- 规则按角色分组展示

### 4.6.4 文案

| 场景 | 文案 |
|------|------|
| 页面标题 | "Team Rules" |
| 空状态 | "No rules yet. Correct your agents in chat and rules will appear here." |
| 红线标签 | "Redline" |
| 建议标签 | "Suggestion" |
| 触发次数 | "Triggered X times" |

### 4.6.5 技术说明

- 规则 YAML 文件读写通过 API 操作 Git repo
- 触发记录存储在 API 层数据库（轻量 SQLite 或 JSON）
- 状态切换通过 API 修改 YAML 并 git commit

### 4.6.6 不做的事（MVP）

- 不做规则手动创建（只通过对话抽取）
- 不做规则冲突检测
- 不做规则导入/导出

### 4.6.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | 在 Chat 中审批的规则，自动出现在 Rules 页面 |
| 2 | 停用规则后，agent 不再被该规则约束 |
| 3 | 启用规则后，agent 恢复遵守该规则 |
| 4 | 规则详情正确显示来源对话片段 |

---

## 4.7 Team 页面（管理版）

### 4.7.1 目标

- 核心目标：一目了然地查看 AI 团队状态
- 体验原则：像团队看板一样，知道谁在干什么、谁空闲

### 4.7.2 用户流程

1. 用户在左侧导航点击 Team
2. 主区域以卡片形式展示每个 agent：头像、角色、当前状态、正在执行的任务
3. 点击 agent 卡片展开详情：技能列表、适用规则、最近活动

### 4.7.3 逻辑规则

- Agent 状态：idle / working / waiting_approval / stopped
- 状态实时更新（WebSocket）
- 卡片按状态排序：working 在前，idle 在后

### 4.7.4 文案

| 场景 | 文案 |
|------|------|
| 页面标题 | "Team" |
| 状态标签 | "Idle" / "Working" / "Waiting for approval" / "Stopped" |
| 空状态 | "No agents yet. Create your team in Chat." |

### 4.7.5 技术说明

- Agent 状态从 OpenClaw runtime API 获取
- 前端 WebSocket 订阅状态变更

### 4.7.6 不做的事（MVP）

- 不做 agent 配置修改（在管理版只查看，修改通过对话）
- 不做 agent 性能指标
- 不做历史活动搜索

### 4.7.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | Team 页面展示所有已创建 agent |
| 2 | Agent 状态实时更新（从 idle 到 working） |
| 3 | 点击卡片展示技能和规则详情 |

---

## 4.8 Process 页面（管理版）

### 4.8.1 目标

- 核心目标：查看流程定义和执行记录
- 体验原则：让用户知道流程走到哪一步、在哪里被拦截过

### 4.8.2 用户流程

1. 用户在左侧导航点击 Process
2. 主区域展示已定义的流程列表
3. 点击流程查看步骤序列和当前执行状态
4. 执行记录展示每次流程实例的完成情况和拦截记录

### 4.8.3 逻辑规则

- 数据源：Git repo 的 `processes/` 目录（YAML）+ API 层的执行记录
- 流程步骤按序列展示，高亮当前步骤
- 拦截记录显示时间、agent、被拦截的动作、原因

### 4.8.4 文案

| 场景 | 文案 |
|------|------|
| 页面标题 | "Processes" |
| 流程状态 | "In Progress" / "Completed" / "Blocked" |
| 拦截记录 | "[时间] [角色名] was blocked at [步骤名]: [原因]" |

### 4.8.5 技术说明

- 流程定义从 Git repo YAML 读取
- 执行记录从 API 层数据库读取
- 前端步骤可视化用简单的线性步骤条（不做复杂流程图）

### 4.8.6 不做的事（MVP）

- 不做流程编辑器
- 不做流程模板创建
- 不做流程统计分析

### 4.8.7 验收标准

| # | 测试用例 |
|---|---------|
| 1 | Process 页面展示预设流程列表 |
| 2 | 流程步骤序列正确展示，当前步骤高亮 |
| 3 | 拦截记录正确显示被拦截的 agent 和原因 |

---

## 5. Demo 四幕剧本

| Act | 名称 | 内容 | 时长 |
|-----|------|------|------|
| 1 | Build | 用户通过对话创建 PM + Dev + QA 团队 | 1 min |
| 2 | Work | 用户下指令做落地页，agent 在聊天中协作 | 2 min |
| 3 | Train | Juanjuan 说「给 Dev 提需求前要分 bug/需求变更，需求变更要有 PRD，我 approve 后才能开发」→ 系统自动提取多步骤流程规则 → 用户确认 | 1 min |
| 4 | Process | Juanjuan 下指令「根据用户 feedback 加 dark mode」→ Dev @PM 说需要先出 PRD → PM 写 PRD + Design → PM @Juanjuan 请求 approve → Juanjuan approve → Dev 开始 → Juanjuan 发现没考虑 mobile → @PM 更新设计 → Kill Switch 暂停全部 | 1 min |

---

## 6. 边界场景

| # | 场景 | 处理方式 |
|---|------|----------|
| 1 | 用户同时纠正多条行为 | 系统逐条生成规则提案，用户逐条审批 |
| 2 | 规则和流程冲突（规则说可以，流程说不行） | 流程优先级高于规则。MVP 不做冲突检测，靠流程拦截兜底 |
| 3 | Agent 执行长任务时用户说"stop" | Kill Switch 立即中断，不等任务完成 |
| 4 | 多个 agent 同时响应同一消息 | 只有被 @ 的 agent 响应。未 @ 时系统选择一个，其他沉默 |
| 5 | 网络断开时 agent 执行中 | Agent 本地继续执行，重连后同步结果。Kill Switch 在 Gateway 层生效不受前端网络影响 |
| 6 | 用户纠正的内容不适合作为规则 | 系统不检测为纠正，不生成规则提案。误检测时用户 Dismiss 即可 |
| 7 | 规则过多导致 context 超限 | 场景触发式召回只注入相关规则（按 trigger_scene 过滤），不加载全部规则 |
| 8 | Demo 演示中途出错 | 每幕独立，出错可跳过当前幕进入下一幕 |

---

## 7. 成功指标

| 维度 | 指标 | 目标值 | 度量方式 |
|------|------|--------|----------|
| Build 效率 | 从对话到团队上线 | 1 分钟内 | Demo 计时 |
| Train 效果 | 纠正后规则跨 session 生效 | 100% | 功能测试 |
| Process 执行 | 跳步拦截成功率 | 100% | 功能测试 |
| Kill Switch | 停止指令响应时间 | 5 秒内 | 功能测试 |
| Demo 体验 | 全程完成时间 | 5 分钟内 | Demo 计时 |
| 用户理解 | 观众理解产品价值 | 定性 | Demo 后访谈 |

---

## 8. 决策记录

| # | 决策 | 提出人 | 日期 | 备注 |
|---|------|--------|------|------|
| 1 | 交互从表单 Builder 改为对话驱动 | 团队共识 | 2026-04-13 | 更符合产品理念（人类像带员工一样） |
| 2 | 规则从手动创建改为对话自动抽取 | 团队共识 | 2026-04-13 | 核心差异化：Teach Once Remember Forever |
| 3 | Web 内嵌聊天取代 Discord 嵌入 | 团队共识 | 2026-04-13 | Demo 不依赖外部服务，体验可控 |
| 4 | Flow 改为 Process | 团队共识 | 2026-04-13 | 不做可视化流程图，用规则驱动强制执行 |
| 5 | MVP 技术栈：OpenClaw + Git Repo + AgentSkills + CLI | 团队共识 | 2026-04-11 | 继承 v1.0 决策 |
| 6 | Kill Switch 双层方案（Gateway + Proxy） | 团队共识 | 2026-04-11 | 继承 v1.0 决策 |
| 7 | MVP 自学习不用 Hermes，自研规则抽取管线 | SuperBoss | 2026-04-13 | 继承技术架构文档决策 |
| 8 | 项目改名 Hybrid Workspace（原 Hybrid Workforce） | Juanjuan | 2026-04-13 | 继承技术架构文档决策 |
| 9 | 开发顺序 M0-M1-M2-M3 围绕 Demo | Juanjuan | 2026-04-13 | 继承技术架构文档决策 |

---

*文档结束。*
