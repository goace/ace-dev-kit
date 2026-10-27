# Issue 理解

通过访谈确定要解决什么问题、做到什么程度，以及如何验收。

## 工作流程

### 1. 现状调研

开始本阶段调研前，完整阅读并遵循[现状调研规范](investigation-guidelines.md)。

- 目的：理解用户要改变的现状，提出有依据的需求问题。
- 范围：相关使用场景、现有行为、已有能力和业务约束。
- 程度：足以辨明现状与期望的差距，识别需要用户澄清的需求歧义。

### 2. 访谈范围

Focus on understanding: purpose, constraints, success criteria

#### 范围预检查

Before asking detailed questions, assess scope: if the request describes multiple independent subsystems (e.g., "build a platform with chat, file storage, billing, and analytics"), flag this immediately. Don't spend questions refining details of a project that needs to be decomposed first.

范围过大时，先与用户确定本 issue 的边界及留待后续的部分。

#### 讨论内容

使用共用访谈规范，按决策依赖展开访谈，覆盖以下内容：

- 背景与目标：为什么做，要解决什么问题。
- 使用者与场景：谁在什么情况下使用，期望发生什么。
- 预期行为：主要场景和有实际影响的边界情况；用具体输入、行为和结果澄清模糊表述。
- 范围与非目标：本次做到哪里，哪些明确留到以后。
- 约束：必须满足的业务条件、兼容性和运行条件。
- 验收条件：用可观察、可验证的结果判断完成。

#### 阶段边界

技术问题只讨论会影响上述需求的部分；技术选型和具体设计留到后续阶段。尚需验证的技术可行性明确记录，不能当作已成立的事实。

### 3. 需求文档

检查 `docs/issues/` 中已有目录，分配唯一的自定 ID，并使用简短主题命名 `docs/issues/<id>-<topic>/`，例如 `docs/issues/0001-user-login/`。已有本 issue 的目录时沿用它。

将本阶段产物写入该目录的 `requirements.md`，包含：

- Issue ID、标题和状态。
- 背景与目标。
- 使用场景和预期行为。
- 范围与非目标。
- 约束。
- 验收条件。
- 待解决问题：需验证的内容、影响的决定及处理阶段；没有则明确写无。
- 知识库变更：按入口的[文档阶段维护规则](../SKILL.md#文档阶段的知识库维护)记录本阶段摘要、候选及文件链接；没有则明确写无。

#### 完成标准

影响目标、范围和验收的歧义均已解决；需求中的行为可通过具体场景理解，验收条件可验证。技术可行性问题可以带入第二阶段，但必须明确记录其对需求的影响，不能静默假设可行。

需求文档按入口的[阶段成果确认](../SKILL.md#阶段成果确认)完成自查和用户整体确认，状态为 `confirmed`，且本阶段知识库对账及确认后的处理完成后，本阶段完成。

## 贯穿规则

### 知识库维护

访谈中对照术语表和代码澄清使用者、领域概念及行为边界，按[知识库维护](../../shared/knowledge-base/knowledge-maintenance.md)记录已解决的术语和影响需求的决策候选。收尾时检查新增术语是否覆盖本阶段场景，知识库冲突是否影响目标、范围或验收。
