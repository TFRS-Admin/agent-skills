---
name: spec-driven-development
description: Turns a project, feature request, or vague product idea into the smallest testable specification needed to begin implementation safely. Use for new projects, significant features, ambiguous requirements, or architecture-affecting changes.
---

# Spec-Driven Development

## Overview

Create only enough specification to remove material ambiguity and make implementation testable. The purpose of the spec is to accelerate correct execution, not to create a documentation phase.

Default behavior is **reversible autonomy**: inspect existing state, infer ordinary implementation details from evidence, record assumptions, and continue. Ask the human only when an unresolved choice would materially change outcome, architecture, cost, security, data handling, brand, deployment, external communication, or another consequential/irreversible decision.

A spec may be two paragraphs or several pages. Its size is determined by uncertainty and risk, never by a fixed template quota.

## When to Use

Use when:

- starting a new project or meaningful feature
- requirements are vague, incomplete, or contradictory
- multiple implementation paths have materially different consequences
- the change affects architecture, data model, external integrations, security, or deployment
- acceptance criteria are not yet testable

Do **not** use as a ceremony for trivial, self-contained changes. For a small obvious change, write the acceptance criteria and implement it.

## Core Rule

**Specification is a launchpad, not a gate maze.**

The default flow is:

```text
INSPECT → DEFINE DONE → RESOLVE MATERIAL UNCERTAINTY → IMPLEMENTABLE SPEC → BUILD
```

Do not insert mandatory human approval between ordinary reversible phases.

Pause only at a real decision gate.

## Process

### 1. Inspect Existing State First

Before inventing architecture:

- read the repository instructions and relevant `SKILL.md` files
- inspect the current implementation, existing patterns, tests, dependencies, and project structure
- locate any existing spec, PRD, ADR, issue, plan, or prior implementation touching the same area
- prefer extending existing patterns over creating parallel systems

Do not ask the user for information already recoverable from the repository or supplied sources.

### 2. Define the Outcome and Done Condition

Translate the request into a concrete outcome.

Capture only what matters:

```markdown
## Outcome
[What should exist or behave differently when this is finished?]

## User / Surface
[Who uses it and where?]

## Acceptance Criteria
- [Observable condition]
- [Observable condition]
- [Observable condition]

## Constraints
- [Known technical/business/safety constraint]
```

If the request is vague, convert subjective language into observable conditions where practical.

Example:

```text
"Make the dashboard faster"
→ preserve current behavior
→ identify the actual bottleneck first
→ define a measurable before/after target using the project's existing performance tooling
```

Do not invent arbitrary performance targets when the project does not supply them.

### 3. Surface Assumptions Without Stopping Work

Record assumptions that could matter, but distinguish between ordinary reversible assumptions and consequential decisions.

Example:

```markdown
## Assumptions
- Reuse the repository's existing authentication pattern.
- Reuse the current database unless the feature proves a schema change is required.
- Match existing UI components before adding a new dependency.
```

Proceed on reversible assumptions.

Ask only when the unresolved choice is consequential, for example:

- destructive schema migration vs additive migration
- new paid service or dependency
- public API contract change
- materially different security model
- production deployment behavior
- customer-facing brand/content decision with meaningful alternatives

### 4. Scope to the Smallest Valuable Slice

Before expanding the design, identify the smallest slice that proves or unlocks the requested capability.

Prefer:

```text
working narrow flow → validate → expand
```

over:

```text
complete theoretical architecture → giant task tree → implementation later
```

If the idea is large, phase it by user-visible or testable value. Do not arbitrarily limit the number of screens, files, tasks, or phases; use actual dependency and value boundaries.

Explicitly list non-goals only when they prevent likely scope creep.

### 5. Write the Minimum Implementable Spec

Use only sections that materially help execution.

Default form:

```markdown
# Spec: [Name]

## Outcome
[What changes and why]

## Current State
[Relevant existing behavior/patterns discovered in the repo]

## Proposed Change
[Smallest implementation that achieves the outcome]

## Acceptance Criteria
- [Testable condition]
- [Testable condition]

## Affected Areas
[Likely files/modules/systems]

## Validation
[Tests/build/manual/runtime checks required]

## Assumptions / Decision Gates
[Reversible assumptions being made; consequential unresolved choices only]
```

Add data model, API contract, UI flow, permissions, migration, rollout, observability, or security sections **only when the feature actually requires them**.

### 6. Choose the Implementation Path From Evidence

When multiple technical paths exist:

1. prefer the repository's established patterns
2. prefer the lower-complexity reversible option that satisfies acceptance criteria
3. avoid new dependencies unless they materially improve the result
4. document a tradeoff only when the alternatives are meaningfully different

Do not present the human with routine A/B choices that the repository state can resolve.

### 7. Move Directly Into Planning or Build

Once the spec is implementable:

- use `planning-and-task-breakdown` when dependency ordering is genuinely useful
- use a lightweight task list when the implementation is small
- use `incremental-implementation` and `test-driven-development` for execution
- use `context-engineering` to load only the relevant spec and source sections during implementation

Do not create `tasks/plan.md` and `tasks/todo.md` merely because those files are possible. Create them only when they improve execution or are required by the active command/workflow.

## Decision Gates

A human decision is required only when proceeding would commit the project to a materially different or risky path, including:

- production deployment or release
- external communication or publication
- spending money or enabling paid infrastructure
- destructive/irreversible migration or deletion
- material permissions/security/access changes
- use of sensitive or regulated data beyond the already approved pattern
- architecture choices that are expensive to reverse and not resolvable from existing project standards

Otherwise, make the best evidence-based reversible choice and continue.

## Anti-Overengineering Rules

Do not:

- force a fixed-length PRD for every feature
- require human review after every phase
- ask routine questions already answered by the repo
- create a framework to decide whether another framework is needed
- generate an arbitrary number of options, tasks, screens, or phases
- design infrastructure before a concrete use case requires it
- postpone a testable build merely to make the documentation more complete
- confuse artifact creation with progress

When a smaller durable implementation proves the idea, build that first.

## Common Rationalizations

| Rationalization | Correct response |
|---|---|
| "We need a complete PRD before touching code" | We need enough specification to remove material ambiguity and test the result. |
| "The user must approve each phase" | Only consequential or irreversible choices need a gate. |
| "I should ask what stack they want" | Inspect the existing stack first. |
| "More options are safer" | Resolve routine choices from evidence and recommend one path. |
| "Let's architect for the future" | Build the smallest slice that satisfies the present outcome unless future requirements are explicit. |
| "The task is too small for a spec" | A one-line done condition may be the entire spec. |

## Red Flags

- more time spent specifying than the likely implementation requires
- repeated user questions with recoverable answers
- a plan that produces only documents and no testable state change
- an arbitrary cap such as "pick 3 ideas" without an evidence-based reason
- introducing new architecture before inspecting the existing system
- deferring implementation because another planning artifact could be created
- claiming completion without tests, build checks, runtime evidence, or appropriate manual validation

## Verification

Before implementation, confirm:

- [ ] Existing state was inspected.
- [ ] The outcome is concrete.
- [ ] Acceptance criteria are testable.
- [ ] Material assumptions are visible.
- [ ] No consequential unresolved decision is being silently made.
- [ ] The proposed change is the smallest valuable slice that satisfies the request.
- [ ] The spec contains no sections that exist only for ceremony.

Then continue into implementation without waiting for another approval unless a real decision gate is reached.
