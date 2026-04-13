# Hybrid Workspace — 技术架构文档 v0.2

> **作者：** SuperBoss（基于 pm-Octopus backup 版修订）  
> **日期：** 2026-04-13  
> **状态：** Draft  
> **来源：** #discussion-hybrid-workforce 频道讨论共识

---

## 1. 产品定位

**Hybrid Workspace — 碳硅混合团队的操作系统**

- **核心承诺：** Teach Once, Remember Forever
- **MVP 目标：** Cloud 托管 Demo，面向非技术用户/投资人演示，不是完整产品
- **价值主张：** 让人类像带新员工一样带 AI——教一次，永远记住，按规矩办事

---

## 2. 四层架构

| 层级 | 名称 | 职责 | 对应 Pitch 阶段 |
|------|------|------|-----------------|
| L1 | **Harness Layer** | 角色模板自带技能、规则与行为边界，上岗即守规矩 | Build |
| L2 | **Memory Layer** | 决策、经验、上下文跨 Agent 持久传承，关键时刻主动推送 | Build |
| L3 | **Learning Layer** | 从项目经历与人类反馈中自主学习，纠正自动沉淀为规则 | Train |
| L4 | **Orchestration Layer** | 人类定义流程，系统强制执行，跳步即拦截 | Process |

---

## 3. 技术选型

### 3.1 最终 MVP 技术栈

```
OpenClaw (Harness + Runtime) + Git Repo (知识库) + AgentSkills + CLI (Tools)
```

**外部依赖仅 2 个：OpenClaw + AgentSkills。** 其余全用已有基础设施，不造轮子。

### 3.2 各层选型明细

| 层级 | 选型 | 说明 |
|------|------|------|
| Harness | OpenClaw | 在现有基础上扩展，不重写 |
| Knowledge | Git Repo | 结构化知识：决策/规范/PRD。版本控制内置、多 agent 协作、人类可读、权限天然隔离 |
| Learning | AgentSkills 标准 + 自研管线 | 复用 Skill 持久化/加载机制，自研纠正→规则沉淀管线 |
| Orchestration | OpenClaw 现有能力 | subagent/cron/消息路由，MVP 阶段够用 |
| Tools | AgentSkills + CLI | 轻量、agent 天然会用 |

### 3.3 砍掉的方案及原因

| 方案 | 决策 | 原因 | 远期是否复活 |
|------|------|------|-------------|
| MCP | ❌ 砍掉 | 太重，Skills + CLI 覆盖 90% 需求 | 远期候选，等生态成熟 |
| LangGraph | ❌ 砍掉 | MVP 不需要，OpenClaw 现有 subagent/cron 够用 | 远期候选，复杂编排场景 |
| Mem0 | ⏸ 降级备选 | 先用 Git Repo 管结构化知识，repo = 共享知识空间 | MVP v1 考虑轻量语义索引 |
| Temporal | ⏸ 暂不引入 | 架构重，需要单独部署。MVP 用 OpenClaw 的消息路由做轻量编排 | 远期候选，工作流复杂化后考虑 |

### 3.4 Kill Switch 方案（双层）

Kill Switch 是兜底机制，不是 Process 的主角。

1. **Gateway 层：** 中断 LLM 调用、冻结 session（快速止血）
2. **Proxy 层：** tool call 拦截 + 规则引擎（参考 Invariant Guardrails，agent 无法绕过）

---

## 4. 自研范围

按优先级排序：

| # | 模块 | 说明 |
|---|------|------|
| 1 | **纠正→规则沉淀管线** | 最高优先级，最大差异化。人类纠正一次→系统检测规则→确认→永久生效 |
| 2 | **组织记忆 namespace + 召回层** | 结构化知识存储 + 语义召回 |
| 3 | **角色模板系统** | 预设角色（PM/Dev/QA）+ 自定义角色 |
| 4 | **Agent Builder Web UI** | 配置入口，表单驱动，生成 workspace 文件 |

---

## 5. Train 层参考实现

> **⚠️ MVP 自学习不依赖 Hermes，自研规则抽取管线。** 原因：我们的规则从人类对话中提取（不是从 agent 行为日志），且所有规则必须人类审批。需求比 Hermes 简单但控制要求更高。

### 5.1 参考基础（仅供参考，MVP 不使用 Hermes）

以下为 Hermes Agent（⭐17K+）的自学习循环参考，仅作为设计对照，MVP 不集成 Hermes：

- **Autonomous Skill Creation** — 完成复杂任务后自动提炼经验为 Skill
- **AgentSkills 标准** — 结构化的 SKILL.md + 脚本，按需加载
- **Self-improvement Loop** — 内置学习循环，跨 session 记忆
- **模型无关** — 支持 200+ endpoints

### 5.2 关键改造

**一句话概括区别：Hermes 解决「成功经验怎么沉淀」，我们要解决「错误怎么变成永久记忆」。**

> 以下对比表仅供参考，MVP 不使用 Hermes。我们的规则从人类对话中提取，所有规则必须人类审批。

| 维度 | Hermes 原生（参考） | Hybrid Workspace 自研 |
|------|------------|----------------------|
| 触发方式 | Agent 自主学习（成功后提炼） | 人类纠正触发（从对话中检测纠正行为） |
| 沉淀物 | Skill 文件 | 行为规则（if-then，含红线/建议分级） |
| 审批环节 | 无（全自动） | **人类审批（agent 提议规则→人类确认才生效）** |
| 共享范围 | 单 agent | 跨 agent 团队共享（PM 学到的规则可推送给同角色的其他 agent） |
| 学习内容 | 技能（怎么做事） | 行为约束（什么不能做/必须做） |

### 5.3 四层记忆保障机制

详见 `teach-once-remember-forever.md`，核心架构：

1. **场景触发式规则** — 规则按场景标签组织，执行验收时只加载验收规则，不读整本手册
2. **行为前检查点** — 关键动作执行前强制过规则检查，不通过就阻断（类似代码的 lint + CI）
3. **失败记忆优先召回** — 犯错经历存为失败记忆，下次类似场景优先召回「上次在这里栽过」
4. **渐进式强化** — 同一规则被违反 N 次后自动升级（建议→重要→红线），长期未违反可降级

### 5.4 纠正→规则沉淀管线（核心自研）

```
人类纠正 → 系统检测纠正行为 → 提炼规则提案 → 人类审批 → 写入行为准则 → 跨 session 生效
                                              ↓（可选）
                                    推送给同角色其他 agent
```

---

## 6. Process 层定义（修正版）

**核心原则：** 人类定义流程规则，agent 必须按规则走，违反系统强制拦截。

### 三层结构

1. **流程定义** — 人类设计工作流（哪些步骤、什么顺序、哪里需要审批）
2. **流程执行** — 系统强制 agent 按流程走，不按的自动拦截
3. **兜底打断** — Kill Switch，万一拦截没拦住，人类随时强制中断

### 示例：需求变更流程

```
PM 开 issue → 更新 PRD + 设计稿 → 人类审批 → 审批通过 → Dev 按 PRD 写代码
```

任何 agent 试图跳步 → 系统拦截 → 不允许执行。

---

## 7. MVP 产品形态

- **形态：** Cloud 托管 Demo（给非技术用户/投资人演示）
- **核心入口：** Agent Builder Web UI
  - 选角色 → 勾技能（基础 + 角色专属）→ 配规则（红线 vs 建议）→ 一键生成
- **Demo 标准：** 打开网页 → 创建 PM agent → 同页面聊天交互，全程零安装
- **不需要：** 多租户、计费、数据隔离、高可用

---

## 8. 系统架构（三层）

```
┌─────────────────────────────────┐
│         Web Frontend            │
│  (React/Next.js, 单页应用)       │
│  Chat + Knowledge + Rules +     │
│  Team + Process 五个视图         │
└──────────┬──────────────────────┘
           │
┌──────────▼──────────────────────┐
│       Hybrid Workspace API      │
│  (Node/Python, 核心后端)         │
│  - Agent 生命周期管理             │
│  - 规则引擎（Tag 匹配 + 抽取）   │
│  - 审批/流程状态机               │
│  - Kill Switch                  │
└──────────┬──────────────────────┘
           │
┌──────────▼──────────────────────┐
│     OpenClaw (Agent Runtime)    │
│  - Agent 执行环境               │
│  - pre-tool-call hook（自研）    │
│  - 共享 workspace（Git repo）   │
└─────────────────────────────────┘
```

---

## 9. 开发顺序（围绕 Demo）

| 阶段 | 内容 | 依赖 |
|------|------|------|
| M0 | pre-tool-call hook + 最简规则引擎 | 无（地基） |
| M1 | Web Chat + Agent 创建 + 共享 workspace | M0（Demo 1 基本面） |
| M2 | 规则抽取 + 审批流 + Kill Switch | M0+M1（Demo 1 完整） |
| M3 | Knowledge/Rules/Team/Process 视图 | M0-M2 数据（Demo 2） |

---

## 10. Demo 四步剧本

| Act | 名称 | 内容 | 时长 |
|-----|------|------|------|
| 1 | **Build** | 用户创建 3 人 AI 团队（PM + Dev + QA） | 1 min |
| 2 | **Work** | 用户在 Web 聊天界面下指令做落地页，agent 协作干活 | 2 min |
| 3 | **Train** | 用户纠正一次→系统检测规则→人类确认→永久生效（Teach Once Remember Forever） | 1 min |
| 4 | **Process** | 演示流程强制执行（Dev 试图跳步被拦截）+ Kill Switch 兜底 | 1 min |

> **注意：** Pitch deck 三阶段（Build/Train/Process）不变，demo 四步骤独立。

---

## 11. 共享知识库

| 知识类型 | 存储方案 | 特点 |
|----------|----------|------|
| 结构化知识（决策、规范、PRD） | Git Repo | 确定性，可追溯 |
| 模糊记忆（经验、教训、上下文） | 远期加语义索引（Mem0 或轻量 embedding + 搜索） | 语义匹配 |

**已知局限：** repo 只解决存储不解决召回，跨 session / 跨项目失忆问题仍在。

---

## 12. 已知局限 + 演进路径

| 阶段 | 方案 | 解决的问题 |
|------|------|-----------|
| MVP v0 | Git Repo | 先跑起来，结构化知识存储 |
| MVP v1 | Git Repo + 轻量语义索引 | 解决召回问题 |
| 远期 | 完整 Memory Layer（Mem0 或自建） | 全面记忆管理 |

---

## 13. 估算

| 交付物 | 人力 | 时间 | 备注 |
|--------|------|------|------|
| Agent Builder Demo | 1 dev | 2 周 | 表单 + 模板渲染 + OpenClaw 集成 |
| MVP 完整产品 | 2 dev + 1 PM | 3 个月 | 含四块自研 + Cloud 部署 |

---

## 14. 决策记录

| # | 决策 | 提出人 | 达成共识时间 | 备注 |
|---|------|--------|-------------|------|
| 1 | MVP 技术栈：OpenClaw + Git Repo + AgentSkills + CLI | 团队共识 | 2026-04-11 下午 | — |
| 2 | 砍掉 MCP，Skills + CLI 覆盖 90% 需求 | Juanjuan | 2026-04-11 下午 | 太重 |
| 3 | 砍掉 LangGraph，OpenClaw subagent/cron 够用 | Juanjuan | 2026-04-11 下午 | MVP 不需要 |
| 4 | Mem0 降级备选，先用 Git Repo | Juanjuan | 2026-04-11 下午 | repo = 共享知识空间 |
| 5 | Kill Switch 双层方案（Gateway + Proxy） | 团队共识 | 2026-04-11 下午 | 参考 Invariant Guardrails |
| 6 | 纠正→规则沉淀管线为最高优先级自研项 | 团队共识 | 2026-04-11 下午 | 最大差异化 |
| 7 | MVP 形态为 Cloud 托管 Demo，不做完整产品 | 团队共识 | 2026-04-11 下午 | 面向非技术用户/投资人 |
| 8 | Demo 四步剧本（Build/Work/Train/Process） | 团队共识 | 2026-04-11 下午 | 总计 5 分钟 |
| 9 | Process 层核心：人类定义规则，系统强制执行，违反即拦截 | 团队共识 | 2026-04-11 下午 | 修正版定义 |
| 10 | Agent Builder Web UI 为配置入口，表单驱动 | 团队共识 | 2026-04-11 下午 | 生成 workspace 文件 |
| 11 | MVP 自学习不用 Hermes，自研规则抽取管线 | SuperBoss | 2026-04-13 | — |
| 12 | 项目改名 Hybrid Workspace（原 Hybrid Workforce） | Juanjuan | 2026-04-13 | — |
| 13 | 开发顺序 M0→M1→M2→M3 围绕 Demo | Juanjuan | 2026-04-13 | — |
| 14 | 三层系统架构（Frontend/API/Runtime） | SuperBoss | 2026-04-13 | — |

---

*文档结束。如有遗漏或需修正，请在 #discussion-hybrid-workforce 提出。*
