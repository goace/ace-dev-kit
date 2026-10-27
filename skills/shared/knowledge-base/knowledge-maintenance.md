# 知识库维护

本规范定义术语表和 ADR 的通用维护规则。术语含义明确后即时更新；ADR 候选的提出时机、载体与确认方式由调用流程规定。

Actively build and sharpen the project's domain model as you work: challenge terms, discuss concrete edge cases, update resolved glossary entries, and identify decisions worth preserving. Merely reading `GLOSSARY.md` for vocabulary is consumption; maintenance also resolves gaps and conflicts in that knowledge.

## File structure

```text
/
├── GLOSSARY.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Create files lazily: only when you have something to write. If no `GLOSSARY.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

开始或恢复工作时，读取根目录 `GLOSSARY.md` 和 `docs/adr/` 中影响当前工作的 ADR；文件不存在时静默继续。使用已有的规范术语，发现与已有决策冲突时明确指出原因，交由用户决定如何处理。

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `GLOSSARY.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account': do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible. Which is right?"

区分现有行为与已确认但尚未实现的目标，明确标出差异。代码尚未实现目标不等于决策无效；未经确认的实现偏离也不能通过补写知识库变成既定决策。

### Update GLOSSARY.md inline

When a term is resolved, update `GLOSSARY.md` right there. Don't batch these up: capture them as they happen. Before creating or updating `GLOSSARY.md`, read [GLOSSARY-FORMAT.md](GLOSSARY-FORMAT.md) for the format.

`GLOSSARY.md` should be totally devoid of implementation details. Do not treat `GLOSSARY.md` as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

术语表始终表达当前有效词汇。定义改变时原地修订；暂未解决的分歧明确保留为待解决问题，载体由调用流程决定，不写成已确定的定义。

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse**: the cost of changing your mind later is meaningful
2. **Surprising without context**: a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off**: there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. Before creating, superseding, or editing an ADR, read [ADR-FORMAT.md](ADR-FORMAT.md) for the format.

候选 ADR 说明背景、拟记录的决策、备选方案及取舍理由、依据与受影响的既有 ADR。按调用流程提交用户确认，满足其确认条件后才创建状态为 `accepted` 的正式 ADR；同一份候选已明确确认时，不再重复请求创建确认。未达到上述门槛的选择不创建 ADR。

已接受的决策改变时，保留旧 ADR 的决策与理由，创建下一个编号的替代 ADR，新记录链接旧记录，旧记录标记为 `superseded by ADR-NNNN` 并链接新记录。只修正文字或补充不改变决策的说明时，可直接修订原 ADR。

## 写入与核验

- 正式定义和已接受决策以知识库文件为准。候选与待解决问题不能表述为已经生效的知识；已有记录准确覆盖结论时，不重复创建。
- 写入后复读受影响的文件，核对内容与明确的术语含义、已确认决策及相关证据一致；候选未通过确认时，不写成正式 ADR。
- 替代 ADR 时检查历史保留、编号和双向链接。对外提供的链接必须指向实际文件，未创建的候选不填虚构链接。
