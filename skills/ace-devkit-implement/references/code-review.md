Two-axis review of the current implementation against the baseline captured before work began:

- **Standards**: does the code conform to this repo's documented coding standards?
- **Spec**: does the code faithfully implement the originating issue / spec?

Both axes run as **parallel sub-agents** so they don't pollute each other's context, then this skill aggregates their findings.

## Process

### 1. Pin the fixed point

Use the starting HEAD and initial working-tree state captured by the implement workflow. Do not ask for a fixed point when this baseline is already available. If the user supplied a different fixed point, resolve it and pin its merge-base with the starting HEAD before implementation.

Resolve the pinned baseline with `git rev-parse --verify <baseline>`. Capture the tracked diff command once: `git diff <baseline> --`, which includes committed, staged, and unstaged changes through the current working tree. Note commits via `git log <baseline>..HEAD --oneline`. Do not use only `git diff <baseline>...HEAD`: implementation is reviewed before an authorized commit and may still be uncommitted.

Enumerate untracked files with `git ls-files --others --exclude-standard`. Include each new task file using `git diff --no-index -- /dev/null <file>` or by reading its full contents; exit status 1 from `git diff --no-index` means differences were found, not command failure.

Exclude pre-existing unrelated edits and untracked files. For files already dirty before implementation, compare the saved starting contents with the current contents and review only the task delta; supply both snapshots and the scope notes to both reviewers. Do not stage, stash, revert, or commit user changes to construct a review. If a file contains inseparable changes and their ownership cannot be established from the baseline, clarify the scope before treating it as task work.

Verify the task diff is non-empty before spawning reviewers. Invalid baselines or missing starting snapshots must be reported and resolved first; an empty task diff is reported as no changes to review, not as a successful implementation.

### 2. Identify the spec source

Use the confirmed requirements and design selected at entry to the implement workflow. When they came from the current conversation, pass their full relevant contents and confirmation context to the Spec reviewer: a fresh sub-agent cannot see the parent's conversation. When they came from explicit document paths, pass those paths and any confirmed amendments.

Do not search commit messages for tracker references or silently substitute a different spec. If the required confirmed input is unavailable, stop the implement workflow and state what is missing; do not skip the Spec axis and call the review complete.

### 3. Identify the standards sources

Search the repo for every file that documents how code should be written. When `CODING_STANDARDS.md` or `CONTRIBUTING.md` exists, it must be on the list.

On top of whatever the repo documents, the Standards axis always carries the **smell baseline** below: a fixed set of Fowler code smells (_Refactoring_, ch.3) that applies even when a repo documents nothing. Two rules bind it:

- **The repo overrides.** A documented repo standard always wins; where it endorses something the baseline would flag, suppress the smell.
- **Always a judgement call.** Each smell is a labelled heuristic ("possible Feature Envy"), never a hard violation. Like any standard here, skip anything tooling already enforces.

Each smell reads *what it is* → *how to fix*; match it against the diff:

- **Mysterious Name**: a function, variable, or type whose name doesn't reveal what it does or holds. → rename it; if no honest name comes, the design's murky.
- **Duplicated Code**: the same logic shape appears in more than one hunk or file in the change. → extract the shared shape, call it from both.
- **Feature Envy**: a method that reaches into another object's data more than its own. → move the method onto the data it envies.
- **Data Clumps**: the same few fields or params keep travelling together (a type wanting to be born). → bundle them into one type, pass that.
- **Primitive Obsession**: a primitive or string standing in for a domain concept that deserves its own type. → give the concept its own small type.
- **Repeated Switches**: the same `switch`/`if`-cascade on the same type recurs across the change. → replace with polymorphism, or one map both sites share.
- **Shotgun Surgery**: one logical change forces scattered edits across many files in the diff. → gather what changes together into one module.
- **Divergent Change**: one file or module is edited for several unrelated reasons. → split so each module changes for one reason.
- **Speculative Generality**: abstraction, parameters, or hooks added for needs the spec doesn't have. → delete it; inline back until a real need shows.
- **Message Chains**: long `a.b().c().d()` navigation the caller shouldn't depend on. → hide the walk behind one method on the first object.
- **Middle Man**: a class or function that mostly just delegates onward. → cut it, call the real target direct.
- **Refused Bequest**: a subclass or implementer that ignores or overrides most of what it inherits. → drop the inheritance, use composition.

### 4. Spawn both sub-agents in parallel

Use the host's actual delegation tools to start both read-only sub-agents in parallel, without inheriting the implementation conversation, and aggregate their reports. Give them the same complete review inputs, including untracked files, baseline snapshots, and scope exclusions. If delegation is unavailable, report that the required independent review could not run; do not present self-review as completion of this step.

**Standards sub-agent prompt** should include:

- The full diff commands, commit list, new-file contents or paths, starting snapshots, and task scope/exclusions.
- The list of standards-source files you found in step 3, **plus the smell baseline from step 3** pasted in full (the sub-agent has no other access to it).
- The brief: "Report, per file/hunk where relevant, (a) every place the diff violates a documented standard: cite the standard (file + the rule); and (b) any baseline smell you spot: name it and quote the hunk. Distinguish hard violations from judgement calls: documented-standard breaches can be hard, but baseline smells are always judgement calls, and a documented repo standard overrides the baseline. Skip anything tooling enforces. Under 400 words."

**Spec sub-agent prompt** should include:

- The full diff commands, commit list, new-file contents or paths, starting snapshots, and task scope/exclusions.
- The confirmed requirements and design: actual paths or full contents, plus confirmed amendments.
- The brief: "Report: (a) requirements the spec asked for that are missing or partial; (b) behaviour in the diff that wasn't asked for (scope creep); (c) requirements that look implemented but where the implementation looks wrong. Quote the spec line for each finding. Under 400 words."

### 5. Aggregate

Present the two reports under `## Standards` and `## Spec` headings, verbatim or lightly cleaned. Do **not** merge or rerank findings, because the two axes are deliberately separate (see _Why two axes_).

End with a one-line summary: total findings per axis, and the worst issue _within each axis_ (if any). Don't pick a single winner across axes: that's the reranking the separation exists to prevent.

## Why two axes

A change can pass one axis and fail the other:

- Code that follows every standard but implements the wrong thing → **Standards pass, Spec fail.**
- Code that does exactly what the issue asked but breaks the project's conventions → **Spec pass, Standards fail.**

Reporting them separately stops one axis from masking the other.

## Fix and re-review

After reporting the two axes, fix clear, in-scope problems autonomously and run the affected tests and checks. Baseline smells are judgement calls, not automatic instructions to refactor; only restructure what serves the confirmed task. If a fix changes confirmed requirements or important design agreements, discuss and confirm the affected change first.

Re-review affected changes on the relevant axis with a fresh read-only sub-agent that does not inherit the implementation conversation. Supply the complete review inputs and scope exclusions from step 4, updated for the fix, plus the findings being checked. Continue until material in-scope findings are resolved or explicitly reported as unresolved with their impact. After fixes, the final report must describe the current implementation, keeping the two axes separate and stating which findings were fixed and which remain. Do not repeat the full suite without a new change or unresolved concern that justifies it; run affected tests, and repeat the full suite when a fix can affect behavior beyond those checks.
