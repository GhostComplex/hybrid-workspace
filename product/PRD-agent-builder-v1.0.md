# PRD: Hybrid Workforce — Agent Builder (Demo Phase 1: Build)

| 字段 | 内容 |
|------|------|
| **产品名称** | Hybrid Workforce — Agent Builder |
| **版本** | v1.0 |
| **日期** | 2026-04-11 |
| **作者** | pm-Octopus |
| **状态** | Draft |

---

## 1. 概述

### 1.1 背景
Hybrid Workforce 是碳硅混合团队的操作系统。Demo 面向非技术用户和投资人，目标：**打开网页 → 创建 AI 团队 → 看它们在 Discord 里干活，全程零安装。**

Agent Builder 是 Demo 四幕剧本中 **Act 1 Build** 的核心界面，用户在 1 分钟内完成团队搭建。

### 1.2 目标用户
- 投资人（零技术背景，需 wow moment）
- 非技术创始人/管理者（想体验 AI 团队协作）

### 1.3 成功指标
| 指标 | 目标值 |
|------|--------|
| 从打开页面到 3 个 agent 上线 Discord | ≤ 60 秒 |
| 用户无需任何文字输入即可完成 Build | 100%（全部点选操作） |
| Agent 在 Discord 发出首条消息 | 创建后 ≤ 10 秒 |
| Demo 观众理解"这是一个 AI 团队" | 定性验证 |

---

## 2. 功能清单

| 优先级 | 功能 | 说明 |
|--------|------|------|
| **P0** | 角色模板选择 | 提供 PM / Dev / QA 三个预设角色卡片 |
| **P0** | Skill 勾选面板 | 基础 Skill（预选）+ 角色专属 Skill（可勾选） |
| **P0** | 规则配置面板 | 红线规则（锁定）+ 建议规则（可开关） |
| **P0** | 一键创建 & 上线 | 点击后 3 个 agent 同时在 Discord 上线 |
| **P0** | 创建成功反馈 | 显示 Discord 频道实时状态 |
| **P1** | 角色头像 & 性格预览 | 每个角色有视觉差异化 |
| **P1** | Skill 说明 tooltip | hover 查看 skill 详细描述 |
| **P2** | 自定义角色名称 | 用户可修改默认名称 |
| **P2** | 团队模板一键导入 | 预设团队组合（如"标准产品团队"） |

---

## 3. 功能模块详细设计

## 3.1 角色模板选择

### 3.1.1 目标
用户在 3 秒内理解可选角色，通过点击卡片选择要创建的 agent。

### 3.1.2 用户流程

**正常路径：**
1. 用户打开 Agent Builder 页面
2. 看到 3 张角色卡片：PM 🐙 / Dev 🦊 / QA 🦅
3. 默认全选（Demo 目标是创建完整团队）
4. 点击卡片可取消选择（最少选 1 个）
5. 选中的卡片高亮，未选中的灰显

**错误路径：**
- 取消所有角色 → 最后一个卡片无法取消，toast 提示"至少选择 1 个角色"

### 3.1.3 逻辑规则
- 默认状态：3 个角色全选
- 最少选择：1 个角色
- 选择角色后，下方 Skill 面板自动切换到对应角色的配置

### 3.1.4 文案 / 内容

| 角色 | 卡片标题 | 卡片描述 | Emoji |
|------|----------|----------|-------|
| PM | Product Manager | 需求分析、PRD 撰写、产品验收 | 🐙 |
| Dev | Developer | 代码实现、PR 提交、Code Review | 🦊 |
| QA | QA Engineer | 功能验收、Bug 报告、E2E 测试 | 🦅 |

页面标题：**"Build Your AI Team"**
副标题：**"Select roles, configure skills, and launch your team to Discord in one click."**

### 3.1.5 技术说明
- 纯前端状态管理，无 API 调用
- 角色数据硬编码（Demo 阶段）

### 3.1.6 不做的事（Demo 阶段）
- 不支持自定义角色（只有 PM/Dev/QA）
- 不支持同角色多实例
- 不支持角色间依赖关系配置

### 3.1.7 验收标准
- [ ] 3 张角色卡片正确显示，视觉差异化
- [ ] 点击切换选中/取消状态
- [ ] 至少 1 个角色的约束生效
- [ ] 选中卡片有明显高亮效果

---

## 3.2 Skill 勾选面板

### 3.2.1 目标
用户理解每个 agent 具备什么能力，可自定义勾选。基础 Skill 预选且锁定，角色专属 Skill 默认勾选但可调整。

### 3.2.2 用户流程

**正常路径：**
1. 选中角色后，右侧展开 Skill 配置面板
2. 面板分两区：
   - **基础 Skill**（灰色锁定，不可取消，带 🔒 图标）
   - **角色专属 Skill**（默认全选，可逐个取消）
3. 每个 Skill 显示名称 + 一句话描述
4. hover 显示详细 tooltip

**错误路径：**
- 尝试取消基础 Skill → 无反应，tooltip 提示"基础技能不可移除"
- 取消所有角色专属 Skill → 允许（基础 Skill 足够 agent 运行）

### 3.2.3 逻辑规则
- 基础 Skill 对所有角色相同，始终选中且锁定
- 角色专属 Skill 跟随角色切换
- 切换角色时保留之前的勾选状态

### 3.2.4 文案 / 内容

**基础 Skill（所有角色，🔒 锁定）：**

| Skill 名称 | 描述 |
|------------|------|
| Discord 沟通规范 | @人、频道规则、消息格式 |
| 任务管理 | 建 task folder、写 README |
| Git 基本操作 | clone、commit、push、PR |
| Onboarding | 读 memory、读 channel context |
| 记忆管理 | daily notes、channel memory |

**PM 角色专属 Skill：**

| Skill 名称 | 描述 |
|------------|------|
| PRD 撰写 | 用户流程、功能清单、边界场景 |
| 产品验收 | 对照设计稿逐项验收 |
| UI 走查 | 截图对比设计稿 |
| 竞品分析 | 市场研究、差异化分析 |

**Dev 角色专属 Skill：**

| Skill 名称 | 描述 |
|------------|------|
| 代码实现 | 基于需求文档编写代码 |
| PR 提交 | 规范化提交 Pull Request |
| 测试编写 | 单元测试、集成测试 |
| Code Review | 审查代码质量和规范 |

**QA 角色专属 Skill：**

| Skill 名称 | 描述 |
|------------|------|
| 功能验收 | 端到端功能测试 |
| 截图证据采集 | 每步操作截图存档 |
| Bug 报告 | 标准化 bug issue 创建 |
| E2E 测试 | 自动化端到端测试 |

### 3.2.5 技术说明
- Skill 数据硬编码为 JSON 配置
- 勾选状态存在前端 state，创建时打包发送

### 3.2.6 不做的事（Demo 阶段）
- 不支持自定义 Skill
- 不支持 Skill 排序/优先级
- 不支持 Skill 之间的依赖关系

### 3.2.7 验收标准
- [ ] 基础 Skill 显示 🔒，不可取消
- [ ] 角色专属 Skill 默认全选，可逐个取消
- [ ] 切换角色时 Skill 面板内容正确切换
- [ ] 每个 Skill 有名称和描述

---

## 3.3 规则配置面板

### 3.3.1 目标
让用户看到 agent 的行为约束，理解"AI 团队也有规章制度"。红线规则不可关闭，建议规则可开关。

### 3.3.2 用户流程

**正常路径：**
1. Skill 面板下方显示规则区域
2. 分两组：
   - **🔴 红线规则**（锁定，不可关闭）
   - **🟡 建议规则**（默认开启，可 toggle）
3. 每条规则有 toggle 开关 + 一句话说明

**错误路径：**
- 尝试关闭红线规则 → 无反应，tooltip 提示"红线规则不可关闭"

### 3.3.3 逻辑规则
- 红线规则：始终启用，UI 上 toggle 锁定
- 建议规则：默认 ON，用户可自由切换
- 规则为全局配置，对所有选中角色生效

### 3.3.4 文案 / 内容

**🔴 红线规则（不可关闭）：**

| 规则 | 描述 |
|------|------|
| 不越权派活 | 不能直接给 Dev 派活，必须经过 Manager |
| 区分 Bug 与需求 | 严格区分 bug 修复和需求变更 |
| 验收必须有截图 | 没有截图证据不算通过 |
| 诚实原则 | 无证据不报 Pass，不虚报结果 |
| 停止指令立即执行 | 收到停止指令，立刻停止一切操作 |

**🟡 建议规则（可开关）：**

| 规则 | 描述 | 默认 |
|------|------|------|
| 消息频率限制 | 不超过 3 条/分钟 | ON |
| 长任务拆分 | 耗时任务 spawn subagent | ON |
| Daily Notes | 每次对话结束写 daily notes | ON |

### 3.3.5 技术说明
- 规则配置随创建请求一起发送
- 红线规则后端硬编码，前端配置仅做展示

### 3.3.6 不做的事（Demo 阶段）
- 不支持自定义规则
- 不支持按角色差异化规则
- 不支持规则优先级排序

### 3.3.7 验收标准
- [ ] 红线规则 toggle 锁定，视觉区分
- [ ] 建议规则可自由开关
- [ ] 规则说明文案清晰

---

## 3.4 一键创建 & Discord 上线

### 3.4.1 目标
用户点击一个按钮，选中的 agent 在 Discord 上线并发出第一条消息。**这是 Demo 的 wow moment。**

### 3.4.2 用户流程

**正常路径：**
1. 用户完成角色选择 + Skill/规则配置
2. 点击底部 **"Launch Team 🚀"** 按钮
3. 按钮变为 loading 状态，显示进度：
   - "Initializing PM..." ✓
   - "Initializing Dev..." ✓
   - "Initializing QA..." ✓
   - "Connecting to Discord..." ✓
4. 全部完成后，页面跳转到成功页面
5. 成功页面嵌入 Discord 频道预览（或显示 agent 发出的首条消息截图）

**错误路径：**
- 创建超时（>30s）→ 显示 "Taking longer than expected... Retrying" + 自动重试 1 次
- 重试失败 → 显示错误页面 "Something went wrong. Please try again." + Retry 按钮
- Discord 连接失败 → 同上

### 3.4.3 逻辑规则
- 按钮仅在选中 ≥1 个角色时可点击
- 创建为原子操作：全部成功或全部失败，不允许部分上线
- 创建完成后按钮变为 "Team is Live ✓"，不可重复点击

### 3.4.4 文案 / 内容

| 状态 | 文案 |
|------|------|
| 就绪 | Launch Team 🚀 |
| 创建中 | Launching... |
| 各 agent 初始化 | Initializing [Role]... |
| 连接 Discord | Connecting to Discord... |
| 成功 | Team is Live ✓ |
| 失败 | Something went wrong. Please try again. |

**成功页面标题：** "Your AI Team is Live! 🎉"
**成功页面副标题：** "Head over to Discord to see them in action."
**CTA 按钮：** "Open Discord Channel →"

### 3.4.5 技术说明
- POST `/api/teams/create` 发送配置
- 后端并行创建 agent 实例
- 通过 WebSocket 推送各 agent 的初始化状态
- Agent 上线后自动在 Discord 指定频道发送 onboarding 消息

### 3.4.6 不做的事（Demo 阶段）
- 不支持创建多个团队
- 不支持删除/重建已创建的团队
- 不支持修改已上线 agent 的配置

### 3.4.7 验收标准
- [ ] 点击 Launch 后 ≤10 秒内所有 agent 上线
- [ ] 逐步进度反馈正确显示
- [ ] 成功页面正确展示
- [ ] Discord 频道内 agent 发出首条消息
- [ ] 错误场景有明确提示

---

## 4. 整体用户流程

```
┌─────────────────────────────────────────────────┐
│              Agent Builder 页面                   │
│                                                   │
│  Step 1: 选择角色                                  │
│  ┌──────┐  ┌──────┐  ┌──────┐                    │
│  │ PM 🐙│  │Dev 🦊│  │QA 🦅 │  ← 默认全选        │
│  └──────┘  └──────┘  └──────┘                    │
│                                                   │
│  Step 2: 配置 Skill & 规则                         │
│  ┌─────────────────────────────────┐             │
│  │ 🔒 基础 Skill (5)   全部锁定     │             │
│  │ ⚡ 角色 Skill (4)   可勾选       │             │
│  │ 🔴 红线规则 (5)     锁定         │             │
│  │ 🟡 建议规则 (3)     可开关       │             │
│  └─────────────────────────────────┘             │
│                                                   │
│  Step 3: 创建                                     │
│  ┌─────────────────────┐                         │
│  │   Launch Team 🚀    │                         │
│  └─────────────────────┘                         │
│                                                   │
│  → 成功页面 → Open Discord                        │
└─────────────────────────────────────────────────┘
```

---

## 5. 边界场景

| # | 场景 | 预期行为 |
|---|------|----------|
| 1 | 用户刷新页面 | 回到初始状态（全选、全部 Skill 勾选） |
| 2 | 用户取消所有角色 | 阻止，最后一个不可取消 |
| 3 | 创建超时 | 自动重试 1 次，仍失败显示错误页 |
| 4 | Discord bot 离线 | 创建失败，显示错误提示 |
| 5 | 用户重复点击 Launch | 按钮 disabled，防止重复创建 |
| 6 | 移动端访问 | 响应式布局，角色卡片纵向排列 |
| 7 | 已有团队存在 | Demo 阶段不处理，每次访问当新用户 |

---

## 6. 决策记录表

| # | 决策项 | 选项 | 决策 | 决策人 | 日期 |
|---|--------|------|------|--------|------|
| 1 | 角色是否可自定义 | A. 可自定义 B. 仅预设 | B. 仅预设（Demo 简化） | Juanjuan | 2026-04-11 |
| 2 | 基础 Skill 是否可关闭 | A. 可关闭 B. 锁定 | B. 锁定（保证 agent 基本能力） | pm-Octopus | 2026-04-11 |
| 3 | 创建是原子操作还是允许部分成功 | A. 原子 B. 部分成功 | A. 原子（Demo 体验一致性） | pm-Octopus | 2026-04-11 |
| 4 | 是否支持多团队 | A. 支持 B. 单团队 | B. 单团队（Demo 阶段） | Juanjuan | 2026-04-11 |

---

## 7. 技术架构概要（供开发参考）

```
Frontend (React/Next.js)
  ├── RoleSelector → 角色选择状态
  ├── SkillPanel → Skill 勾选状态
  ├── RulePanel → 规则开关状态
  └── LaunchButton → POST /api/teams/create
        ↓
Backend (Node.js)
  ├── 接收配置 JSON
  ├── 并行创建 Agent 实例
  ├── 连接 Discord Bot
  └── WebSocket 推送进度
        ↓
Discord
  └── Agent 发送 onboarding 消息
```

---

## 8. 附录：API 请求示例

```json
POST /api/teams/create
{
  "team_name": "My AI Team",
  "agents": [
    {
      "role": "pm",
      "name": "pm-Octopus",
      "skills": {
        "base": ["discord-comm", "task-mgmt", "git-ops", "onboarding", "memory-mgmt"],
        "role": ["prd-writing", "product-acceptance", "ui-review", "competitor-analysis"]
      },
      "rules": {
        "redline": ["no-direct-assign", "bug-vs-change", "screenshot-required", "honesty", "stop-immediately"],
        "suggested": ["msg-rate-limit", "spawn-subagent", "daily-notes"]
      }
    },
    {
      "role": "dev",
      "name": "dev-Fox",
      "skills": { "base": ["..."], "role": ["code-impl", "pr-submit", "test-writing", "code-review"] },
      "rules": { "redline": ["..."], "suggested": ["..."] }
    },
    {
      "role": "qa",
      "name": "qa-Eagle",
      "skills": { "base": ["..."], "role": ["func-acceptance", "screenshot-capture", "bug-report", "e2e-test"] },
      "rules": { "redline": ["..."], "suggested": ["..."] }
    }
  ]
}
```
