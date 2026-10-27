---
name: ace-devkit-implement
description: "承接已确认的需求与设计，完成开发、自测、双轴 review、修复和用户验收，并维护术语表与 ADR。"
disable-model-invocation: true
---

Implement the work described by the user in the confirmed requirements and design, using the current conversation by default.

## 开始与恢复

If the user passes issue document paths, read the requirements and design and state the issue title before starting. If the reference is ambiguous, ask. If the requirements, design, or their confirmation is missing, state what is missing and stop.

开始或恢复工作时，完整阅读并遵循[知识库维护规范](../shared/knowledge-base/knowledge-maintenance.md)，读取根目录 `GLOSSARY.md` 和影响当前工作的 ADR；文件不存在时静默继续。

开始或恢复工作时，必须完整阅读[代码库设计参考](../shared/codebase-design/codebase-design.md)，在已确认需求、设计和当前任务范围内，将适用规则用于实现、自查和后续修订。

Before implementation, retain the starting HEAD and the initial contents of existing changes needed to distinguish this task's work for review.

Invoke `$codex-plan-tool` skill and, when its conditions apply, use `update_plan` to track development progress.

## 开发与 review

Read and follow [tdd.md](references/tdd.md) where possible, through module interfaces chosen from the confirmed design and verification strategy.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, read and follow [code-review.md](references/code-review.md) to review the work, fix in-scope problems, and re-review affected changes.

实施期间持续遵循共享规范：术语含义明确后即时更新 `GLOSSARY.md`，不延迟到最终验收；未解决的术语分歧留在会话中，不写成正式定义。

发现实现与已确认需求、重要设计约定或既有 ADR 冲突时，立即说明依据与影响。需要改变这些约定时，先与用户确认受影响的决定并更新实施依据，再继续相关修改；不得等到收尾时用补写 ADR 追认未经确认的偏离。符合 ADR 门槛的决策留待最终回顾，不另建候选文件。

## 用户验收

开发、自测、review 和必要修复完成后，向用户提供当前结果、验证情况和未解决问题，请求验收。测试或 review 通过不等于用户验收通过。

用户提出修改时，继续开发、运行受影响的检查并按 review 规则复查，再提交验收。只有用户明确确认最终验收通过，才进入知识库回顾。

## 最终知识库回顾

回顾本次实施全过程，包括开发、review、修复及用户验收提出的修改，检查相对现有知识库是否有新增或改变的结论。核对已明确术语是否即时写入、当前定义是否准确，并按共享规范筛选 ADR 候选及受影响的既有 ADR；已有记录准确覆盖时不重复创建。不重新汇总或对账各文档阶段的候选清单。

在会话中返回术语变更摘要、拟创建／替代／修订的 ADR、取舍理由及相关依据，请用户确认本轮知识库变更。候选仅在会话中展示，不写入 `requirements.md`、`design.md` 或临时候选文件；正式知识库是确认后的落盘位置。仅有术语变化时，报告实际变更即可，不再次请求术语写入许可；没有新增知识库变更时，明确报告“知识库无变更”，不为完成流程创建文件。

最终验收通过只确认实施结果，不自动批准随后提出的 ADR 候选。用户提出调整时，修订候选并确认；同一份候选已明确确认时不重复询问。确认后按共享规范创建、替代或修订正式 ADR，复读受影响文件，报告实际文件链接与处理结果。未确认、否决或延期的候选不得写成正式 ADR；延期项在会话中说明原因、影响和后续处理时机。

用户最终验收通过，知识库回顾完成，候选均有明确处理结果，且所有通过的变更完成落盘与核验后，本次实施完成。

Commit your work to the current branch only if the user has explicitly authorized it.
