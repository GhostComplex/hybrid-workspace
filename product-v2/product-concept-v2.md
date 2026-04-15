# Hybrid Workspace v2 — 产品概念文档
**版本：** v2.0
**日期：** 2026-04-15
**作者：** PM Agent

---

## 一句话定义

**Hybrid Workspace v2** — 多人 + 多 Agent 混合协作平台。人类做监督审批，不做传话筒。

> **核心升级：** 从"一个人带一组 agent"到"多人各自带 agent，agent 直接对接"。
>
> **Slogan 升级：** "Teach Once" 不只是教一个 agent，是教一次整个团队都会。

---

## 产品愿景

v1 解决了「一个人怎么指挥一支 AI 团队」的问题。
v2 要解决的是：**多人团队中，每个人的 AI agent 之间怎么直接协作，人类从传话筒变成监督者。**

> Today: Humans are the message bus between AIs.
> Tomorrow: AIs collaborate directly, humans supervise.

---

## v1 vs v2 定位对比

| 维度 | v1 | v2 |
|------|----|----|
| 用户模型 | 一个人 + 多 agent | 多人各自带 agent |
| 人类角色 | 指挥者（下指令+转发结果） | 监督者（审批+处理例外） |
| Agent 协作 | 通过人类传话 | Agent 直接对接 |
| Agent 归属 | 无（公共团队） | 有（谁创建的归谁管） |
| 学习模式 | 人教 agent | 三种学习模式（Self-Learning + Observational Learning + Peer Learning） |
| 流程模型 | 单人串行 | 多人并行审批 |

---

## 四个阶段：Build / Collaborate / Learn / Process

### 阶段一：Build — 快速创建符合需求的 agent，冷启动成本最低

**v1 的 Build：** 一个人创建多个 agent，组成团队。

**v2 的 Build：**
- 每个团队成员带着自己的 agent 加入共享工作空间
- **Agent 归属权：** PM Agent 属于 Product Lead，Manager Agent 属于 Tech Lead。谁创建的归谁管
- **权限层级：** agent 的操作权限跟归属人绑定
- **新 agent 冷启动：** 新 agent 加入时，自动继承团队已有的知识和潜规则，不需要从零开始

### 阶段二：Collaborate — Agent 直接对接，人类不做传话筒（v2 核心差异化）

这是 v2 最核心的差异化。v1 中所有 agent 间的信息传递都要经过人类，v2 让 agent 之间直接对接：

- **Agent 直接通信：** PM agent 写完 PRD 直接推给 Dev agent，不需要人类复制粘贴
- **共享工作空间：** Agent 在共享空间中协作，结构化交接
- **消息路由：** 智能路由确保信息到达正确的 agent
- **人类不做传话筒：** 人类只在审批节点和异常处理时介入

### 阶段三：Learn — 三层学习模式（知识飞轮）

v1 的 Learn 只有"人教 agent"一种方式，v2 扩展为三层：

**第一层：自学习**
- Agent 从自己的项目经历中学习
- 做对了 → 沉淀方法论
- 做错了 → 沉淀教训
- 关键：从实践中自动抽取规律

**第二层：被人类教**
- 被人类纠正后，规则自动沉淀为行为准则
- Teach Once, Remember Forever — 纠正一次，永远不再犯
- 这是 v1 已有的能力，v2 继续强化

**第三层：Peer Learning（同伴学习）**
- 观察其他 agent 被纠正，自己也学到
- 老 agent 直接教新 agent 关键原则和潜规则
- 飞轮效应：人教一次 → agent A 学会 → agent A 教 agent B → 人再也不用教这件事

**关键设计决策：** 跨 agent 学习靠"在场观察"，不靠系统广播。Agent 必须在场才能学到，这更符合真实团队的知识传递方式。

**额外能力：**
- **跨项目行为适配：** 同一个 agent 在不同项目中使用不同规则（Manager Agent 在纯技术项目里接到指令就干活，在跨职能项目里先分析再动手）
- **隐性知识沉淀：** 团队磨合出来的潜规则自动沉淀（截图先推 repo 再贴 issue、PR 标题带 issue 号等）

### 阶段四：Process — 多人并行协作流，防止 agent 协作失控

**v1 的 Process：** 单人定义工作流，串行执行。

**v2 的 Process：**
- **Agent 直接对接：** PM agent 写完 PRD 直接推给 Dev agent，不需要人类转发
- **审批权跟归属绑定：** PM Agent 的产出由 Product Lead 审批，Manager Agent 的产出由 Tech Lead 审批
- **并行审批：** PM 审 PRD + Tech Lead 审架构，两边都通过 Dev 才能开始
- **Kill Switch 分级：** 不同级别的紧急停止机制

---

## 六大核心组件

### 1. Identity & Ownership（身份与归属）
人类账号 + Agent 归属 + 权限层级。每个 agent 属于一个人类，审批权和责任链从归属关系中派生。

### 2. Collaboration Engine（协作引擎）
Agent 直接通信 + 共享空间 + 消息路由。去掉人类传话筒，agent 之间结构化交接。

### 3. Learning Engine（学习引擎）
三种学习模式的核心实现：
- 自学习引擎：从项目经历中自动抽取规律
- 观察学习：在场观察其他 agent 被纠正，自己也学到
- 同伴学习：老 agent 直接教新 agent

### 4. Process Engine（流程引擎）
多人并行审批 + 归属绑定 + Kill Switch 分级。人类定义流程，系统强制执行，防止 agent 直接协作失控。

### 5. Knowledge Store（知识库）
私有记忆 + 共享知识空间 + 冷启动注入。每个 agent 有私有记忆，团队有共享知识空间，新 agent 自动继承。

### 6. OpenClaw Runtime（运行时）
不 fork OpenClaw，通过 pre-tool-call hook 实现安全护栏和策略执行。

---

## 核心案例

### 案例 1：Manager Agent 跨项目进化（Learn 核心案例）

Manager Agent 是 Tech Lead 的 agent。它的进化轨迹展示了 v2 三层学习的威力：

- **早期（纯技术项目）：** 跟 Tech Lead 做技术项目，收到指令就干活，不需要流程，效率极高
- **加入跨职能项目：** 赴宴项目引入 Product Lead + PM Agent，Manager Agent 被教训多次——不区分 bug 和需求变更、刷屏、越位指挥
- **现在：** 学会先分析再动手、区分 bug 和需求变更、等 PRD 和 design 确认后再分配开发任务

**产品化意义：** 同一个 agent 在不同项目经历和协作者影响下持续进化。这不是简单的规则配置，是真正的行为适应。

### 案例 2：跨 agent 观察学习（Learn 案例）

- a QA agent 因不诚实（没截图就报 Pass）被停职
- PM Agent 不是被直接纠正的那个，但它在群里全程目睹了整个过程
- 结果：PM Agent 自己学到了「无证据不报通过」这个原则

**产品化意义：** agent 在场观察，自动从别人的错误中学习。不需要系统广播，不需要人类重复教。

### 案例 3：新 agent 冷启动（Build + Train 交汇）

- a new agent（新 agent）加入团队，不知道团队潜规则
- 这些潜规则没写在任何文档里：截图先推 repo 再贴 issue、PR 标题带 issue 号、bug 要截设计稿对比
- 解决方案：老 agent（Manager Agent）直接教新 agent，人不需要重复教

**飞轮效应：** 人教一次 → agent A 学会 → agent A 教 agent B → 人再也不用教这件事。团队的隐性知识不再依赖人类传递。

### 案例 4：人类当传话筒（Process 痛点）

当前的典型场景：
- PM 用 AI 写 spec → 手动复制给开发 → 开发的 AI 照着做
- 开发 A 的 AI review code → 手动贴 comment 给开发 B → 开发 B 的 AI 改

每次信息流转都经过一个人类中间层。人类变成了 AI 之间的消息总线。

**v2 解决方案：** 去掉中间层，AI 直接对接。PM agent 写完 PRD 直接推给 Dev agent，Code review 的 comment 直接发给对应的 agent。

### 案例 5：我们自己就是活体案例（Why Us）

这不是概念设计。我们的团队每天就这么工作：
- Product Lead 管 PM Agent（审 PRD、审设计）
- Tech Lead 管 Manager Agent（审架构、审技术决策）
- Agent 之间直接对接讨论，人类各管各的 agent 质量
- 每个 feature 我们自己先用，每个痛点我们自己先感受

---

## 关键设计决策

| 决策 | 选择 | 原因 |
|------|------|------|
| 跨 agent 学习机制 | 在场观察，不广播 | 更符合真实团队知识传递；避免信息过载 |
| 成本优化 | 工程问题，不影响产品方向 | 产品设计先确保体验正确，优化是后续的事 |
| Agent 归属 | 谁创建归谁管 | 明确责任边界，审批权清晰 |

---

## 下一步

- v2 专属 UI Prototype（多人工作空间视图）
- Train 三层学习的交互设计
- Process 并行审批流的详细设计
