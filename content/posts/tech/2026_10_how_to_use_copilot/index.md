+++
date = '2026-10-15T09:21:09+08:00'
draft = false
title = 'GitHub Copilot 使用指南'
categories = ["技术"]
tags = ["Copilot","工具"]
+++

# GitHub Copilot 使用指南

适用环境：VS Code / Visual Studio / JetBrains IDE / Xcode / GitHub Web / CLI

核心形态：Inline Completion（补全） + Inline Chat（行内对话） + Copilot Chat（侧边栏对话） + Edit Mode（多文件编辑） + Agent Mode（自主代理） + Cloud Coding Agent（云端建 PR）

---

## 1. 先建立心智模型

GitHub Copilot 不是“一个聊天框”，而是一套分层能力：

| 层级 | 名字 | 你在干嘛 | Copilot 在干嘛 |
| --- | --- | --- | --- |
| L0 | Autocomplete | 写代码 | 在光标处补下一行/下一段 |
| L1 | Inline Chat | 选中代码改代码 | 在当前文件里做定向修改 |
| L2 | Copilot Chat | 问项目/问报错/问架构 | 读上下文，给解释、代码、命令 |
| L3 | Edit Mode | 改几个文件 | 按你给的范围做受控编辑 |
| L4 | Agent Mode | 给目标，不管步骤 | 自己读文件、跑命令、修错、迭代 |
| L5 | Coding Agent | 给 issue / PR 描述 | 云端开分支、改代码、跑测试、提 PR |

一句话总结：

- 写代码 → 用补全
- 改当前文件 → Inline Chat
- 问项目/调试 → Copilot Chat
- 多文件功能 → Edit / Agent Mode
- 团队异步流程 → Coding Agent

---

## 2. 安装与登录

### VS Code

扩展市场装两个：

- GitHub Copilot
- GitHub Copilot Chat

登录 GitHub 账号，并根据需要订阅相应服务

### JetBrains

设置 → Plugins → 搜索 GitHub Copilot

### CLI

```bash
gh copilot
gh copilot explain "git rebase --onto"
gh copilot suggest "find large files in repo"
```

---

## 3. 基础用法：Inline Completion

边写边出灰色建议，这是 Copilot 的起点。

### 快捷键（VS Code）

| 操作 | Windows / Linux | macOS |
| --- | --- | --- |
| 接受建议 | Tab | Tab |
| 拒绝建议 | Esc | Esc |
| 下一条 | Alt + ] | Option + ] |
| 上一条 | Alt + [ | Option + [ |
| 手动触发 | Alt + \ | Option + \ |
| 打开补全面板 | Ctrl + Enter | Cmd + Enter |

### 最佳实践

不要只写变量名等让模型去猜测，而是写详细的意图注释，给出明确工作建议：

```python
# Load CSV, drop rows with missing target, normalize features,
# split 80/20, return X_train, y_train, X_test, y_test
def prepare_dataset(path: str):
    ...
```

相比：

```python
def prepare_dataset(path):
    ...
```

明显质量更高。

### Next Edit Suggestions（NES）

NES 不是“接着写”，而是“下一步该改哪”：

- 重命名函数 → Copilot 提示调用点也要改
- 改接口签名 → 提示实现类要同步
- 侧边出现箭头 → Tab 跳过去接受

适合重构，不只是写新代码。

---

## 4. Inline Chat：在当前文件里对话

### 快捷键

- VS Code：Ctrl + I / Cmd + I
- JetBrains：Ctrl + Shift + G（可自定义）

### 示例

- 把这个函数改成 async/await
- 给这段代码加输入校验
- 提取成独立模块，导出默认函数
- 用策略模式重构这段 if-else

### 特点

- 作用域是当前文件 / 当前选择
- 出 diff，可逐块接受
- 不离开编辑器

---

## 5. Copilot Chat：项目级对话

### 打开方式

- VS Code：Ctrl + Alt + I / Cmd + Shift + I
- 侧边栏点击 Copilot 图标

### 能干啥

- 解释代码
- 找 bug
- 写测试
- 设计模块
- 看架构
- 解读终端报错
- 生成 SQL / 正则 / Dockerfile / CI

### 示例

- 解释一下这个 Tokenizer 的分词逻辑
- 为什么这个 React effect 会死循环？
- 给 UserService 写 JUnit 5 测试，覆盖空指针和并发调用

---

## 6. @：Chat Participants（聊天参与者）

`@` 用来切换“谁来回答你”。这不是普通关键词，而是把对话交给某个专用角色/域。

### 常用内置参与者

#### @workspace

把整个工作区当上下文：

- `@workspace 用户登录失败时在哪些文件里打了日志？`
- `@workspace 这个项目用了哪种认证流程？`
- `@workspace 为什么通知没有按时发送？`

适合：

- 跨文件定位
- 架构理解
- “这个项目里 X 是在哪里实现的”

#### @terminal

读取集成终端内容：

- `@terminal 刚才那个 npm error 是什么意思？`
- `@terminal 怎么把上一条命令的报错修掉？`

#### @vscode

问 VS Code 本身：

- `@vscode 怎么把默认终端改成 fish？`
- `@vscode 怎么给 Copilot 配置快捷键？`

#### @github

问 GitHub 侧对象（PR / issue / Actions / release 等，常配合 GitHub 技能或 MCP）：

- `@github 列出我被分配的 open PR`
- `@github 这个 issue #142 的复现步骤是什么？`

### IDE 特有参与者

Visual Studio 中还有：

- `@debug` / `@debugger`：结合调用栈、变量、断点排错
- `@profiler`：性能瓶颈分析
- `@test`：按项目测试框架生成单测
- `@git`：审本地未提交改动
- `@modernize`：.NET / C++ 升级迁移

### 自定义 agent

- `@security-reviewer 审计这段 SQL 拼接代码`
- `@tdd 用测试驱动方式实现购物车折扣逻辑`
- `@api-designer 设计一个分页 + 过滤的 REST 接口`

---

## 7. #：Chat Variables / Context References（上下文锚点）

`#` 不是装饰，而是把“具体上下文”钉进 prompt。

### 常用变量

| 变量 | 含义 |
| --- | --- |
| `#file:src/auth.ts` | 把某个文件内容作为上下文 |
| `#selection` | 当前编辑器里选中的代码 |
| `#editor` | 当前打开的文件 |
| `#codebase` | 对整个代码库做语义检索 |
| `#terminalLastCommand` | 上一条终端命令及其输出 |
| `#terminalSelection` | 终端里框选的文本 |
| `#problems` | Problems 面板里的错误/警告 |
| `#git` | Git 状态 / diff / 变更 |
| `#changes` | 当前未提交改动 |
| `#web` | 联网搜索（部分环境/技能支持） |
| `#fetch:https://...` | 抓取某个网页内容 |

### 组合使用的关键价值

```text
@workspace /fix
#file:src/order_service.py
#terminalLastCommand
#problems
```

含义：在整个工作区语境下修 bug，重点看 `order_service.py`，并结合刚才终端报错和 Problems 面板。

再比如：

- 给 `#selection` 写单元测试，测试框架参考 `#file:tests/conftest.py`
- `#file:src/schema.sql` 根据这张表，生成 SQLAlchemy 模型
- 解释 `#file:src/cache/lru.go` 和 `#file:src/cache/lfu.go` 的差异

---

## 8. /：Slash Commands（斜杠命令）

`/` 是快捷动作，用于避免你写长 prompt。

### 通用命令

| 命令 | 作用 |
| --- | --- |
| `/explain` | 解释选中代码 |
| `/fix` | 修选中代码里的错误 |
| `/tests` | 生成单测 |
| `/doc` | 补注释 / docstring / JSDoc |
| `/simplify` | 简化代码 |
| `/new` | 脚手架新项目 / 新文件 |
| `/newNotebook` | 新建 Jupyter Notebook |
| `/clear` | 清聊天历史 |

### 示例

```text
/explain
/tests
/fix
/doc
/new react app with typescript and zustand
/new python fastapi project with sqlite
```

### 和 `@` / `#` 组合

- `@workspace /tests #file:src/payment.py`
- `@workspace /explain #file:src/scheduler/cron.go #file:src/scheduler/queue.go`

---

## 9. Chat Modes：Ask / Edit / Plan / Agent

现代 Copilot Chat 底部通常有模式选择器。

### Ask Mode

只说不做：

- 解释
- 调研
- 给方案
- 不改文件

### Edit Mode

- 你指定文件 / 选择 / `#file`
- Copilot 改几个文件
- 出 diff，你逐文件接受
- 适合“我知道要动哪些文件”

### Plan Mode

- 只读探索代码
- 输出实现计划
- 不写代码
- 适合大改动前的方案确认

常见产物：

- `plan.md`
- 修改文件清单
- 风险点
- 验证方式

### Agent Mode

最像“编程同事”：

1. 你给目标
2. 它自己找文件
3. 改代码
4. 跑命令
5. 看报错
6. 再修
7. 直到任务完成或等你确认

示例：

> 给 Express 项目加滑动窗口限流中间件，支持 Redis，写单元测试，更新 README 的 API 说明

它会：

- 找路由文件
- 建 `middleware/rateLimit.ts`
- 加 Redis client
- 写 `*.test.ts`
- 跑 `npm test`
- 失败就自己看堆栈改

### Agent 权限

终端命令默认要你批准：

- Allow once
- Allow for session
- Allow for workspace
- Deny

可配置自动批准安全命令：

```json
{
  "chat.tools.terminal.autoApprove": {
    "git status": true,
    "npm test": true,
    "pytest": true,
    "rm -rf": false,
    "sudo": false
  }
}
```

---

## 10. Cloud Coding Agent（GitHub 侧）

不是本地 Agent，而是 GitHub 云端编码代理。

### 触发方式

- 把 GitHub Issue 分配给 Copilot
- PR 里 `@github-copilot` 叫它改
- VS Code / GitHub Web 里开 coding agent session

### 它会做什么

1. 克隆仓库到沙箱
2. 读 issue / PR / instructions
3. 改代码
4. 跑测试 / lint
5. 开分支
6. 提 PR
7. 你评论后继续改

### 适合场景

- 修这个 bug
- 给这个模块补测试
- 把这段代码迁移到新 SDK
- 根据 issue 实现功能

### 不适合场景

- 涉及密钥
- 要改生产环境
- 架构还没定
- 业务规则模糊

---

## 11. MCP：让 Copilot 用外部工具

MCP = Model Context Protocol。

Copilot 本来只会读代码，MCP 让它：

- 查数据库
- 调内部 API
- 读 Postgres
- 搜 GitHub Repo
- 抓网页
- 调 Jira / Notion / Figma
- 跑公司内部工具

### VS Code 配置示例

`.vscode/mcp.json`：

```json
{
  "mcp": {
    "servers": {
      "filesystem": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
      },
      "github": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}"
        }
      },
      "postgres": {
        "type": "stdio",
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-postgres", "${env:DATABASE_URL}"]
      }
    }
  }
}
```

### Agent Mode 里可以直接用

- 查一下 postgres 里 `users` 表有多少重复 email
- 从 GitHub repo `microsoft/vscode` 里找 terminal 相关 issue

---

## 12. 自定义项目规则：让 Copilot 像团队成员

很多平台会读取项目级说明文件：

- `.github/copilot-instructions.md`
- `.github/instructions/*.instructions.md`
- `.github/prompts/*.prompt.md`
- `.github/agents/*.agent.md`
- `AGENTS.md`
- `CLAUDE.md`

这些内容会自动进入 Copilot 的上下文，给它“团队规范”。

### `.github/copilot-instructions.md`

这是最常见的项目规则入口：

```md
# Copilot Instructions

- 后端用 Python 3.12 + FastAPI
- 所有接口返回统一结构：{ code, message, data }
- 数据库访问必须用 repository 层，不许在 route 里写 SQL
- 测试用 pytest，单测不连真实数据库
- 提交信息用 Conventional Commits
- 中文注释可以，公开 API 必须有英文 docstring
```

### `.github/instructions/*.instructions.md`

按文件类型自动加载：

```md
# .github/instructions/react-ts.instructions.md
applyTo: "**/*.tsx"

- 用函数组件 + hooks
- props 必须显式类型
- 不允许 any
- 状态管理优先 zustand，不用 redux
```

### `.github/prompts/*.prompt.md`

可复用 prompt 模板：

```md
# .github/prompts/write-adr.prompt.md
---
description: Write an architecture decision record
---

请为下面的决策写一份 ADR，包含：
1. 背景
2. 决策
3. 备选方案
4. 后果
5. 后续动作
```

调用方式类似：

```text
/prompt:write-adr 我们要从 JWT 换成 OAuth2
```

### `.github/agents/*.agent.md`

自定义 agent：

```md
# .github/agents/security-reviewer.agent.md
---
name: security-reviewer
description: 审查代码中的安全漏洞，用于 PR 审查和代码审计
tools:
  - codeSearch
  - readFile
  - listFiles
---

你是一名资深安全工程师。审查时重点关注：
- SQL 注入
- XSS
- CSRF
- 硬编码密钥
- 权限绕过
- 不安全的反序列化
```

使用方式：

```text
@security-reviewer 审计 src/auth/
```

---

## 13. 模型选择

Chat / Agent 里通常可以切模型：

- GPT-4o / GPT-5.x（Copilot 默认系列）
- Claude Sonnet
- Gemini
- o-series / 推理模型

### 经验建议

- 写业务代码：默认模型就够
- 复杂重构：强指令跟随模型
- 算法/架构：推理模型
- 大批量改文件：Agent + 强指令模型
- 小补全：别用重模型，延迟重要

---

## 14. 高频场景速查

### 读不懂老代码

```text
@workspace 解释 OrderService.process() 的完整调用链
/explain
```

### 修报错

```text
@workspace /fix
#terminalLastCommand
#problems
```

### 写测试

```text
/tests
@workspace /tests #file:src/account.py
```

### 加功能

```text
Agent:
给用户表加 soft delete，
所有查询默认过滤已删除数据，
更新相关 repository 和 service，
并补测试
```

### 重构

```text
Plan:
把 utils/helpers.py 里 40 个函数按职责拆到
string/, date/, validation/ 三个模块，
保证现有测试通过
```

### 代码评审

- `@security-reviewer 审这次 PR 的 diff`
- `@github-copilot review 这个 PR 的性能和错误处理`

### 数据库相关

- `#file:models/user.py` 根据这些模型生成 Alembic migration
- 查 postgres 里最近 7 天登录失败最多的用户（MCP）

### DevOps

生成一个 GitHub Actions：

- lint
- type check
- pytest
- 仅在 `main` 和 PR 上跑

---

## 15. Prompt 写法：别写“人话”，写“规格”

### 差的写法

```text
帮我优化一下这个函数
```

### 好的写法

```text
优化 src/cache/lru.py 的 get() 方法：
- 时间复杂度保持 O(1)
- 不要用额外 dict
- 保留线程安全注释
- 不改变公开接口
- 给出前后 benchmark 说明
```

### 公式

`[动作] + [目标对象] + [约束] + [不动什么] + [验证方式] + [上下文]`

### 示例

```text
动作：重构
对象：PaymentService.charge()
约束：不用第三方库，保持同步接口
不动：数据库表结构、公开方法签名
验证：现有 ChargeTest 全绿
上下文：#file:src/payment.py #file:tests/test_charge.py
```

---

## 16. 工作流推荐

### 个人开发

1. 写注释 / 类型 / 接口 → 用补全
2. 小改动 → Inline Chat
3. 不懂 → `@workspace /explain`
4. 单文件改 → Edit Mode
5. 跨文件功能 → Agent Mode
6. 跑测试 → 让 agent 自己修红

### 团队开发

1. Issue 写清楚验收标准
2. 本地用 Agent 做实现
3. PR 里让 Copilot 做 review
4. 重复坏味道写进 `copilot-instructions.md`
5. 安全/性能/测试各有专用 agent
6. 大特性用 Plan Mode 先对齐

### Vibe Coding 模式

做一个 Todo 应用：

- 前端 React + Tailwind
- 可以增删改、标记完成
- 用 localStorage
- 深色模式
- 动画别太花

然后：

- 看效果
- 说“卡片间距再大点”
- 说“删除要有确认”
- 说“加个空状态插画”

你负责审美和判断，Copilot 负责码字。

---

## 17. 常见坑

### 1. 它很会编，但不一定对

- 不存在的 API
- 假参数
- 假库版本
- 假 SQL 函数

结论：跑之前看 import、看签名、看版本。

### 2. 大仓库不会“真的懂全部”

`@workspace` 不是无限上下文：

- 索引有上限
- 打开相关文件会更好
- 用 `#file` 比靠它猜更稳

### 3. Agent 会乱跑

- 改了不该改的文件
- 顺手改 lint 配置
- 把 seed 数据删了
- 自动加依赖

结论：Agent 跑完一定看 diff，不要无脑 Accept All。

### 4. 测试通过了但测了个寂寞

Copilot 可能：

- `assert` 写错
- 只测 happy path
- mock 掉关键逻辑
- 用 `assert True`

结论：看测试到底验证了什么。

### 5. 密钥/配置别喂给它

- `.env`
- 生产连接串
- AWS key
- 用户数据

结论：用 `#file` 时别把 secrets 文件拖进去。

---

## 18. 权限与安全清单

提交 Copilot 代码前检查：

- 没有硬编码 secret
- SQL 不是字符串拼接
- 用户输入做了校验
- 权限/鉴权没被绕开
- 外部命令没裸拼 shell
- 依赖不是瞎加的
- 测试不是假通过
- 日志没打敏感信息
- 迁移脚本可回滚

---

## 19. 速记口诀

- 写代码：等补全，Tab 接受
- 改代码：Ctrl + I
- 问项目：`@workspace`
- 问终端：`@terminal`
- 问 VS Code：`@vscode`
- 问 GitHub：`@github`
- 钉文件：`#file:`
- 钉报错：`#terminalLastCommand`
- 钉问题：`#problems`
- 快捷动作：`/explain /fix /tests /doc`
- 多文件：Edit Mode
- 大任务：Agent Mode
- 先别改：Plan Mode
- 异步 PR：Coding Agent
- 扩能力：MCP
- 定规矩：`copilot-instructions.md`
- 建专家：`.github/agents/*.agent.md`

---

## 20. 一个完整例子

你说：

```text
@workspace
Agent:
给项目加“导出订单为 CSV”的功能：
- 仅管理员可导出
- 支持按日期范围过滤
- 使用仓库层查询，不写裸 SQL
- 生成 FastAPI 路由 + service + 单测
- 更新 API 文档
上下文：
#file:src/orders/repository.py
#file:src/auth/dependencies.py
#file:tests/test_orders.py
```

Copilot Agent 会：

1. 读 order repository
2. 看鉴权依赖
3. 加 `export_orders_csv()`
4. 加路由 `/orders/export`
5. 加 admin 权限检查
6. 写 `test_export_orders_csv.py`
7. 跑 pytest
8. 失败就修
9. 最后给你 diff + 总结

你只需要：

- 看 diff
- 改文案
- 决定要不要合并

---

## 21. 总结

GitHub Copilot 的用法可以分三层：

### 新手层

- Tab 补全
- `/explain /fix /tests`
- 聊天问问题

### 进阶层

- `@workspace` 问整个项目
- `#file #selection #problems` 精确喂上下文
- Edit Mode 做受控多文件修改

### 专业层

- Agent Mode 自主实现功能
- Plan Mode 先出方案
- MCP 接数据库/内部系统
- 自定义 agents / instructions / prompts
- Coding Agent 在 GitHub 上异步出 PR

Copilot 不是代替你写代码，而是把“从想法到代码”的摩擦降到最低。
但架构、安全、业务正确性，仍然必须是人的责任。

---

> 结论：GitHub Copilot 的价值，不是“自动生成代码”，而是让开发者在思考、设计和验证中更高效、更稳健。

