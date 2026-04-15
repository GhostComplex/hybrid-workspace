# Hybrid Workspace — Product v2

## 定位变化：v1 → v2

| | v1 | v2 |
|---|---|---|
| **用户模型** | 一个人 + 多 agent | 多人各自带 agent |
| **人类角色** | 指挥者（下指令+转发结果） | 监督者（审批+处理例外） |
| **Agent 协作** | 通过人类传话 | Agent 直接对接 |
| **Agent 归属** | 无（公共团队） | 有（谁创建的归谁管） |
| **学习模式** | 人教 agent（Teach Once） | 三层学习：自学+被教+互学 |
| **流程模型** | 单人串行 | 多人并行审批 |

## v2 核心理念

> Today: Humans are the message bus between AIs.
> Tomorrow: AIs collaborate directly, humans supervise.

- 每个团队成员带着自己的 AI agent 加入共享工作空间
- Agent 之间直接对接（PM agent → Dev agent，无需人类转发）
- 人类只做三件事：**下指令、审批、处理例外**
- Agent 归属权明确：谁创建的归谁管，审批权跟归属绑定

## 2026-04-15 讨论要点

### Build 升级
- Agent 归属权：PM-Octopus 属于 Juanjuan，SuperBoss 属于 Steins
- 新 agent 冷启动自动继承团队知识和潜规则
- 老 agent 直接教新 agent，人不需要重复教

### Train 升级（核心差异化）
三层学习模式：
1. **自学习** — 从项目经历中学（做对沉淀方法，做错沉淀教训）
2. **被人类教** — 纠正后规则自动沉淀（Teach Once, Remember Forever）
3. **跨 agent 互学** — 在场观察其他 agent 被纠正自己也学到；老 agent 教新 agent

关键决策：跨 agent 学习靠"在场观察"不靠系统广播

### Process 升级
- Agent 直接对接，人只管审批节点
- 并行审批（PM 审 PRD + Tech Lead 审架构，都通过 Dev 才开始）
- Kill Switch 分级

### Slogan 升级
"Teach Once" 不只是教一个 agent，是教一次整个团队都会

## 真实案例

```
Juanjuan（产品负责人）
  └── PM-Octopus → 写 PRD、设计 prototype → Juanjuan review

Steins（技术负责人）
  └── SuperBoss → 写技术架构、review PRD → Steins review

Agent 之间直接对接讨论，人类各管各的 agent 质量
```

## 文件清单

| 文件 | 说明 |
|---|---|
| `README.md` | 本文件 — v2 定位概述与变更记录 |
| `product-concept-v2.md` | v2 产品概念文档（完整版） |
| `../docs/v2/index.html` | v2 GitHub Pages 展示页 |

## 相关链接

- [v1 GitHub Pages](../docs/index.html)
- [v2 GitHub Pages](../docs/v2/index.html)
- [v1 产品资料](../product/)
