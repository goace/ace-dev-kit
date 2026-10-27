---
name: ace-devkit-init
description: 初始化当前项目的 Ace DevKit 约定
disable-model-invocation: true
---

# Ace DevKit 初始化

为当前 Git 仓库安装Agent行为规范，采用两层文档结构：

* 项目根目录`AGENTS.md` 只保留始终生效的入口规则
* `docs/agents/knowledge-fetching.md` 保存术语表和ADR 的详细消费规则。

本技能不负责创建或维护具体的术语表、ADR、模块地图或其他工作流文档。

开始写入前，先完整阅读 [knowledge-fetching.md](references/knowledge-fetching.md)，将它作为`docs/agents/knowledge-fetching.md` 的基线。

## 执行步骤

1. 确认当前工作目录是目标项目的 Git 根目录。
2. 读取现有的根目录 `AGENTS.md`（如果存在），以及现有的`docs/agents/knowledge-fetching.md`（如果存在），保留其中所有非本技能管理的内容。
3. 创建或更新 `docs/agents/knowledge-fetching.md`：
   - 以 [knowledge-fetching.md](references/knowledge-fetching.md) 为基线。
   - 如果目标文件已存在，保留不冲突的用户补充，只合并或更新本技能负责的规则。
   - 不因为初始化而创建 `GLOSSARY.md` 或任何 ADR。
4. 将下面的短入口作为一个独立的 `## Agent Rules` 区块写入 `AGENTS.md`：
   - 如果该区块已存在，原地更新，不能追加重复区块。
   - 保留该区块中已有且不冲突的用户补充，只合并或更新本技能负责的规则。
   - 如果 `AGENTS.md` 不存在，创建它。
   - 不修改 `CLAUDE.md`；本技能的目标文件固定为 `AGENTS.md`。
5. 写入后重新读取两个目标文件，确认 `## Agent Rules` 只出现一次，且入口指向`docs/agents/knowledge-fetching.md`。

## AGENTS.md 注入内容

```markdown
## Agent Rules

### 项目知识库

每次开始分析、设计、修改代码或执行项目操作前，先阅读
`docs/agents/knowledge-fetching.md`，并遵循其中关于术语表和 ADR 的读取规则。
```

## 边界

- 不要求术语表或 ADR 在初始化时已经存在；它们由后续工作按需创建。
- `## Agent Rules` 是入口，`docs/agents/knowledge-fetching.md` 是详细规则；不要把完整领域文档重新复制回 `AGENTS.md`。
- 不把“阅读规则”扩展到与当前任务无关的全部文档；agent 只需阅读相关 context 和相关 ADR。
