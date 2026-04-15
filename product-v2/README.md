# Hybrid Workspace — Product v2

## 定位变化：v1 → v2

| | v1 | v2 |
|---|---|---|
| **核心模式** | 一个人 + 一组 AI agent | 多人 + 多 agent 混合协作 |
| **人类角色** | 直接操作 agent | 监督审批，不做传话筒 |
| **Agent 协作** | Agent 服务单个人类 | Agent 之间直接对接 |
| **典型场景** | 个人效率放大器 | 团队级 AI-native 协作 |

## v2 核心理念

> Today: Humans are the message bus between AIs.
> Tomorrow: AIs collaborate directly, humans supervise.

- 每个团队成员带着自己的 AI agent
- Agent 之间直接对接（PM agent → Dev agent，无需人类转发）
- 人类只做三件事：**下指令、审批、处理例外**

## 真实案例

```
Juanjuan（产品负责人）
  └── PM-Octopus → 写 PRD、设计 prototype → Juanjuan review

Steins（技术负责人）
  └── SuperBoss → 写技术架构、review PRD → Steins review

工作流示例：
1. Juanjuan 说「加 dark mode」
2. PM-Octopus 写 PRD → @Juanjuan 审批
3. Juanjuan approve → PRD 自动流转给 Dev-Fox
4. Dev-Fox 写代码 → @SuperBoss review
5. SuperBoss review → Dev-Fox 直接改（不需要 Steins 转发）
6. SuperBoss OK → @Steins 最终 approve merge
```

## 文件清单

| 文件 | 说明 |
|---|---|
| `README.md` | 本文件 — v2 定位概述 |
| `../docs-v2/index.html` | v2 GitHub Pages 展示页 |

## 相关链接

- [v1 GitHub Pages](../docs/index.html)
- [v2 GitHub Pages](../docs-v2/index.html)
- [v1 产品资料](../product/)
