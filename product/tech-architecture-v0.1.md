# Hybrid Workforce — 技术架构文档 v0.1

> **作者：** pm-Octopus  
> **日期：** 2026-04-11  
> **状态：** Draft  
> **来源：** #discussion-hybrid-workforce 频道讨论共识

---

## 1. 产品定位

**Hybrid Workforce — 碳硅混合团队的操作系统**

- **核心承诺：** Teach Once, Remember Forever
- **MVP 目标：** Cloud 托管 Demo，面向非技术用户/投资人演示，不是完整产品
- **价值主张：** 让人类像带新员工一样带 AI——教一次，永远记住，按规矩办事

---

## 2. 四层架构

| 层级 | 名称 | 职责 | 对应 Pitch 阶段 |
|------|------|------|-----------------|
| L1 | **Harness Layer** | 行为准则、权限边界、角色定义 | Build |
| L2 | **Memory Layer** | 组织记忆、知识传承、跨 agent 共享 | Build |
| L3 | **Learning Layer** | 自学习、人类纠正→规则沉淀 | Train |
| L4 | **Orchestration Layer** | 碳硅分工、流程调度、强制执行 | Flow |

---

## 3. 技术选型

### 最终 MVP 技术栈

```
OpenClaw/Hermes (Harness) + Git Repo (知识库) + AgentSkills + CLI (Tools)
```

### 砍掉的方案及原因

| 方案 | 决策 | 原因 | 提出人 |
|------|------|------|--------|
| MCP | ❌ 砍掉 | 太重，Skills + CLI 覆盖 90% 需求 | Juanjuan |
| LangGraph | ❌ 砍掉 | MVP 不需要，OpenClaw 现有 subagent/cron 够用 | Juanjuan |
| Mem0 | ⏸ 降级备选 | 先用 Git Repo 管结构化知识，repo = 共享知识空间 | Juanjuan |

### Kill Switch 方案（双层）

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

**基础：** 参考 Hermes Agent 的自学习循环，复用 AgentSkills 标准的 Skill 持久化和加载机制。

**关键改造（与 Hermes 原生的区别）：**

| 维度 | Hermes 原生 | Hybrid Workforce 改造 |
|------|------------|----------------------|
| 触发方式 | Agent 自主学习 | 人类纠正触发 |
| 审批环节 | 无 | 加人类审批 |
| 共享范围 | 单 agent | 跨 agent 团队共享 |

**四层记忆保障：**

1. **场景触发** — 识别当前场景，匹配已有规则
2. **行为前检查** — 执行前检查是否有相关规则约束
3. **失败记忆召回** — 召回历史上类似场景的失败经验
4. **渐进式强化** — 规则随纠正次数增强权重

---

## 6. Flow 层定义（修正版）

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
- **Demo 标准：** 打开网页 → 创建 PM agent → 看它在 Discord 里干活，全程零安装
- **不需要：** 多租户、计费、数据隔离、高可用

---

## 8. Demo 四步剧本

| Act | 名称 | 内容 | 时长 |
|-----|------|------|------|
| 1 | **Build** | 用户创建 3 人 AI 团队（PM + Dev + QA） | 1 min |
| 2 | **Work** | 用户下指令做落地页，agent 协作干活 | 2 min |
| 3 | **Train** | 用户纠正一次→系统检测规则→人类确认→永久生效（Teach Once Remember Forever） | 1 min |
| 4 | **Flow** | 演示流程强制执行（Dev 试图跳步被拦截）+ Kill Switch 兜底 | 1 min |

> **注意：** Pitch deck 三阶段（Build/Train/Flow）不变，demo 四步骤独立。

---

## 9. 共享知识库

| 知识类型 | 存储方案 | 特点 |
|----------|----------|------|
| 结构化知识（决策、规范、PRD） | Git Repo | 确定性，可追溯 |
| 模糊记忆（经验、教训、上下文） | 远期加语义索引（Mem0 或轻量 embedding + 搜索） | 语义匹配 |

**已知局限：** repo 只解决存储不解决召回，跨 session / 跨项目失忆问题仍在。

---

## 10. 已知局限 + 演进路径

| 阶段 | 方案 | 解决的问题 |
|------|------|-----------|
| MVP v0 | Git Repo | 先跑起来，结构化知识存储 |
| MVP v1 | Git Repo + 轻量语义索引 | 解决召回问题 |
| 远期 | 完整 Memory Layer（Mem0 或自建） | 全面记忆管理 |

---

## 11. 决策记录

| # | 决策 | 提出人 | 达成共识时间 | 备注 |
|---|------|--------|-------------|------|
| 1 | MVP 技术栈：OpenClaw/Hermes + Git Repo + AgentSkills + CLI | 团队共识 | 2026-04-11 下午 | — |
| 2 | 砍掉 MCP，Skills + CLI 覆盖 90% 需求 | Juanjuan | 2026-04-11 下午 | 太重 |
| 3 | 砍掉 LangGraph，OpenClaw subagent/cron 够用 | Juanjuan | 2026-04-11 下午 | MVP 不需要 |
| 4 | Mem0 降级备选，先用 Git Repo | Juanjuan | 2026-04-11 下午 | repo = 共享知识空间 |
| 5 | Kill Switch 双层方案（Gateway + Proxy） | 团队共识 | 2026-04-11 下午 | 参考 Invariant Guardrails |
| 6 | 纠正→规则沉淀管线为最高优先级自研项 | 团队共识 | 2026-04-11 下午 | 最大差异化 |
| 7 | MVP 形态为 Cloud 托管 Demo，不做完整产品 | 团队共识 | 2026-04-11 下午 | 面向非技术用户/投资人 |
| 8 | Demo 四步剧本（Build/Work/Train/Flow） | 团队共识 | 2026-04-11 下午 | 总计 5 分钟 |
| 9 | Flow 层核心：人类定义规则，系统强制执行，违反即拦截 | 团队共识 | 2026-04-11 下午 | 修正版定义 |
| 10 | Agent Builder Web UI 为配置入口，表单驱动 | 团队共识 | 2026-04-11 下午 | 生成 workspace 文件 |

---

*文档结束。如有遗漏或需修正，请在 #discussion-hybrid-workforce 提出。*
