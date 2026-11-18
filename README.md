# agent-project-template

用 coding agent（Claude Code、Codex 等）做长期项目时的一套项目骨架：文档怎么分、规则怎么写、长任务怎么在多个 agent 之间分工。没有可运行的代码，只有文档和约定。

核心只有一条：**协调者的 context 只放目标、决定和结论；执行、解决合并冲突、验证，都交给其他独立的 agent。**

## 目录

```
AGENTS.md                 项目知识和共用规则，每个会话都会读
docs/HANDOFF.md           当前状态，接手的入口
docs/roles/               协调者、执行者、验证者各自的规则
docs/tasks/_template.md   任务文件：目标、范围、验收标准、交回、验证
experiments/_template/    实验登记：results.md 首屏是结论，runs.md 记每次运行
notes/                    读代码、读资料时的发现
.tmp/                     临时文件，不入库
.claude/agents/           Claude Code 的 agent 定义，正文指向 docs/roles/
.codex/agents/            Codex 的 agent 定义，正文指向 docs/roles/
```

## 怎么用

1. 复制这个仓库。文件里用尖括号括起来的说明（如 `<一句话说明这个项目做什么。>`）是写给你的，用之前替换或删掉。路径里的 `<名字>` 是占位，保留。
2. 改 `AGENTS.md`：开头写这个项目做什么，「开发」一节换成你自己项目的规则。
3. 要不要分工由你决定。一个会话能做完的任务，直接做。
4. 任务长到一个会话做不完时，开一个会话当协调者，第一句话告诉它：

   > 你是协调者，按 docs/roles/coordinator.md 工作。

   之后：

   - 和它对齐目标，让它把工作拆成任务，每个任务写一份 `docs/tasks/<名字>.md`。
   - 它把任务交给 `executor`。执行者在自己的 worktree 里做，做完只交回结果和证据。
   - 它把交回的任务交给 `verifier`。验证者只看任务文件和改动，给出结论。
   - 你根据验证者的结论决定合并还是退回。

## 角色规则放在哪

角色规则只写在 `docs/roles/` 一处。`AGENTS.md` 每个会话和 subagent 都会读到，所以里面不放某个角色专属的规则。`.claude/agents/` 和 `.codex/agents/` 里的文件只负责让工具按名字启动执行者和验证者，并设置各自的工具和 worktree。

## 来源

这些做法整理自下面几篇公开的文章，以及自己在项目里遇到的问题。文章的质量都很高，建议直接读原文。

- OpenAI
  - [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)（2026-02）
  - [Symphony](https://github.com/openai/symphony)（README 与 SPEC，2026-02）
- Anthropic
  - [Building effective agents](https://www.anthropic.com/research/building-effective-agents)（2024-12）
  - [Harness design for long-running apps](https://www.anthropic.com/research/harness-design-long-running-apps)（2026-03）
  - [Multi-agent coordination patterns](https://claude.com/resources/articles/multi-agent-coordination-patterns)（2026-04）
- Google
  - [Google Antigravity built an OS (and more)](https://antigravity.google/blog/google-antigravity-built-an-os)（2026-05）
  - Google Research：[Towards a science of scaling agent systems](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)（2026-01）
- DeepSeek
  - [Exploring Super-agent Design: Towards a More Autonomous Agent System](https://x.com/DeepSeekHarness/status/2116857257245987236)（2026-11）
  - 原型仓库 [dsh-superagent-prototype](https://github.com/deepseek-ai/dsh-superagent-prototype)。`docs/roles/` 里角色规则的部分写法参考了它。
- Cursor
  - [Towards self-driving codebases](https://cursor.com/blog/self-driving-codebases)（2026-02）
- Cognition
  - [Devin can now Manage Devins](https://cognition.com/blog/devin-can-now-manage-devins)（2026-03）
  - [Multi-Agents: What's Actually Working](https://cognition.com/blog/multi-agents-working)（2026-04）

这些做法变化很快，要在实践中不断迭代。
