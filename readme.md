
# Indexed Planning Structure for AI-Assisted Software Builds

This document describes a planning structure for building large software systems with AI agents, human review, and controlled implementation stages.

The goal is simple:

> Turn a large, fragmented plan into indexed, bounded work packets that an AI agent can navigate without guessing.

This structure is inspired by the same general idea behind systems like Stripe’s “minions” approach: agents are most useful when they are given bounded, indexed, reviewable units of work instead of being thrown into an entire codebase or giant planning document with vague instructions.

The point is not to let AI run wild.

The point is to give AI the right amount of context, the right boundaries, and the right validation gates so it can help without becoming the architect, policy owner, or source of truth.

---

## Why This Structure Exists

Large projects often start as scattered planning notes:

- Versioned planning documents
- Architecture decisions
- Feature ideas
- Follow-up notes
- Testing requirements
- Security concerns
- Future-scope ideas
- Partial implementation plans
- Old drafts that still contain useful context

That is fine early in the design process.

But once implementation begins, scattered planning becomes dangerous.

AI agents especially struggle when the plan is unbounded. If all context is in one large document or spread across many old versions, the agent may:

- Pull outdated requirements
- Mix future scope into current work
- Miss validation rules
- Duplicate types or logic
- Ignore module boundaries
- Implement features without tests
- Treat archived notes as current authority
- Overload the context window with irrelevant information
- Solve the wrong part of the problem because the task was too broad

This structure solves that by giving every major domain a stable index ID and its own planning file.

The index becomes the routing layer.

Instead of asking an agent to understand the whole project every time, you point it at the exact domain it needs.

---

## The Core Idea

The planning docs are organized like this:

```text
current/   = current governance and delivery authority
index/     = domain-level implementation planning files
matrices/  = cross-cutting validation, test, demo, dependency, and risk artifacts
glossary/  = canonical terminology and naming rules
archive/   = older planning docs for historical reference only
````

The `index/` directory is the key.

Each major platform domain receives a stable ID:

```text
IC-00  Product Vision
IC-01  Canonical Graph Model
IC-02  Design System
IC-03  Initial Setup
IC-04  Identity / RBAC
IC-05  Canvas Editor
IC-06  Node and Edge Inspector
IC-07  Quick Actions
IC-08  Graph Versioning and Diff
IC-09  Policy / OPA
IC-10  Service Mesh / Istio
IC-11  Environment Injection
IC-12  Generator Engine
IC-13  CI and Validation Runner
IC-14  Cost Estimation
IC-15  Deployment Workflow
IC-16  Cluster Agent
IC-17  Rollback and Promotion
IC-18  Escape Hatch
IC-19  Observability and Incident Mode
IC-20  Notifications
IC-21  Audit System
IC-22  Library and Templates
IC-23  Import / Adoption Future Scope
IC-24  Testing Strategy
IC-25  Security and Tenant Isolation
```

Now an AI agent can be given a bounded task:

```text
Work only on IC-13.
Read the current governance doc.
Read the IC-13 planning file.
Read the relevant matrices.
Do not use archive as current authority.
Do not touch future scope.
```

That is much safer than:

```text
Read all the docs and build the CI system.
```

---

## Why This Is Not Overkill

At first, this structure can look like a lot.

It has multiple directories, an index, matrices, glossary files, archive rules, and pre-implementation gates.

But the purpose is not bureaucracy.

The purpose is context control.

AI agents do not need every detail of a large project for every task. In fact, giving them too much context often makes the result worse.

For example, if the agent is working on the CI validation runner, it does not need to reason deeply about the design system, visual styling, incident UI, or future import workflows.

It probably needs:

```text
current governance plan
IC-13 CI and Validation Runner
IC-21 Audit System
IC-25 Security and Tenant Isolation
test matrix
validation matrix
terminology glossary
```

That is enough.

This structure lets you feed the agent the context it needs and exclude the context it does not.

That matters because AI-assisted development works best when the task is:

* Bounded
* Indexed
* Specific
* Reviewable
* Connected to tests
* Connected to validation
* Protected by rules
* Clear about what is future scope

The structure is not about making the docs bigger.

It is about making the context smaller and cleaner for each task.

---

## Inspiration: Bounded Agent Work Packets

The inspiration comes from looking at how serious agent-driven engineering systems work.

The important lesson is not simply “use agents.”

The important lesson is:

> Agents need indexed, bounded work packets.

A human engineer can hold broad context, make judgment calls, and ask clarifying questions across a whole system.

An AI agent is more effective when the work is framed as:

```text
Here is the domain.
Here are the rules.
Here are the inputs.
Here are the outputs.
Here is what you must not do.
Here are the tests.
Here is the validation gate.
Here is the definition of done.
```

That turns AI from a vague assistant into a controlled implementer.

This structure borrows that idea and applies it to project planning.

Instead of giving the agent the entire history of the project, the plan is routed through stable domain indexes.

The result is a system like this:

```text
Unstructured request
→ Indexed domain
→ Bounded context
→ Validation gates
→ Reviewable output
```

That is the same pattern used in good infrastructure, security, and platform design:

* Limit scope
* Define interfaces
* Enforce boundaries
* Validate before execution
* Keep auditability
* Avoid hidden authority

---

## Example Directory Tree

```text
docs/plan/
|-- README.md
|-- current/
|   |-- README.md
|   `-- current-plan.md
|-- index/
|   |-- README.md
|   |-- IC-00-product-vision.md
|   |-- IC-01-canonical-graph-model.md
|   |-- IC-02-design-system.md
|   |-- IC-03-initial-setup-bootstrap.md
|   |-- IC-04-identity-sso-teams-rbac.md
|   |-- IC-05-canvas-editor.md
|   |-- IC-06-node-edge-inspector.md
|   |-- IC-07-quick-actions.md
|   |-- IC-08-graph-versioning-diff.md
|   |-- IC-09-policy-opa.md
|   |-- IC-10-service-mesh-istio.md
|   |-- IC-11-environment-injection.md
|   |-- IC-12-generator-engine.md
|   |-- IC-13-ci-validation-runner.md
|   |-- IC-14-cost-estimation.md
|   |-- IC-15-deployment-workflow.md
|   |-- IC-16-cluster-agent.md
|   |-- IC-17-rollback-promotion.md
|   |-- IC-18-escape-hatch.md
|   |-- IC-19-observability-incident-mode.md
|   |-- IC-20-notifications.md
|   |-- IC-21-audit-system.md
|   |-- IC-22-library-templates.md
|   |-- IC-23-import-adoption-future.md
|   |-- IC-24-testing-strategy.md
|   `-- IC-25-security-tenant-isolation.md
|-- matrices/
|   |-- data-flow-map.md
|   |-- demo-acceptance-matrix.md
|   |-- dependency-map.md
|   |-- module-map.md
|   |-- risk-matrix.md
|   |-- test-matrix.md
|   `-- validation-matrix.md
|-- glossary/
|   |-- canonical-naming-rules.md
|   `-- terminology-glossary.md
`-- archive/
    |-- README.md
    `-- older-planning-docs.md
```

---

## Authority Order

When documents conflict, resolve them in this order:

1. `current/` is the governance and delivery authority.
2. `index/IC-xx-*.md` files are the implementation authority for each domain.
3. `matrices/` files define cross-cutting tests, validation, risk, dependencies, and demo acceptance.
4. `glossary/` files define naming and terminology.
5. `archive/` files are historical reference only.

Archived files are not current authority.

This matters because old planning docs may still contain useful ideas, but they should not override the current plan.

The archive is memory.

The index is authority.

---

## What Goes in `current/`

The `current/` directory contains the active governance plan.

This document answers:

* What is the product or build vision?
* What are the delivery tiers?
* What gates must pass before implementation?
* What modules exist?
* What is the authority order?
* What is future scope?
* What rules cannot be violated?

This is the high-level control document.

It should not contain every implementation detail for every domain.

It should define how the work is governed.

---

## What Goes in `index/`

The `index/` directory contains one file per domain.

Each file should follow the same structure:

```md
# IC-xx — Domain Name

## Purpose

## Scope

## Authoritative Sources

## Owned Schemas / Models

## Inputs

## Outputs

## Owner Package

## Dependencies

## Must Never Do

## RBAC Requirements

## Audit Requirements

## Validation Rules

## Test Requirements

## Demo Acceptance

## Future Scope

## Traceability

## Open Questions
```

This consistency matters.

It lets humans and agents find the same kind of information in the same place every time.

If the agent is working on `IC-13`, it knows exactly where to look for:

* Inputs
* Outputs
* Validation rules
* Tests
* Audit requirements
* Future-scope boundaries
* Related domains

This prevents the agent from guessing.

---

## What Goes in `matrices/`

Matrices are cross-cutting control artifacts.

They answer questions that span multiple domains.

Recommended files:

```text
test-matrix.md
validation-matrix.md
demo-acceptance-matrix.md
risk-matrix.md
dependency-map.md
module-map.md
data-flow-map.md
```

These help prevent disconnected planning.

For example:

* A feature should map to a test.
* A deploy action should map to validation rules.
* A module should have clear inputs and outputs.
* A risk should have a preventive control.
* A tier should have a demo acceptance story.
* A dependency should have an allowed direction.
* A privileged action should have audit expectations.

The matrices are how you catch gaps before code exists.

---

## What Goes in `glossary/`

The glossary defines language that must stay consistent across the project.

Examples:

```text
Use "kind", not "type", for graph discriminators.
Use "consumer" and "dependency" for environment injection.
Use "guided" and "manual" for setup paths.
Use "managed", "external", and "none" for integration modes.
Keep graph version, generated bundle, checksum, and deployment record separate.
```

This prevents slow drift where different parts of the plan start using different words for the same thing.

Terminology drift becomes implementation drift.

Implementation drift becomes bugs.

The glossary prevents that.

---

## What Goes in `archive/`

The `archive/` directory contains older planning files.

These are useful for history, but they are not implementation authority.

Agents should only read archive files when an active indexed file references them.

Every archived file should clearly say:

```text
Archived planning reference.
This is not current authority.
Use current/ and index/ for implementation planning.
```

The archive should not be deleted, because old notes often explain why a decision was made.

But the archive should not be treated as the current plan.

---

## How an AI Agent Should Use This Structure

An agent should follow this order:

1. Read the root planning README.
2. Read the current governance document.
3. Read the index README.
4. Identify the requested delivery tier and `IC-xx` domains.
5. Read the relevant `IC-xx` files.
6. Read the relevant matrix files.
7. Read the glossary.
8. Only read archive files if an active document points to them.

The agent should not start in archive.

The agent should not implement from old notes.

The agent should not pull unrelated domains into the current task unless the indexed file says they are dependencies.

---

## Required Pre-Implementation Questions

Before implementation begins, every relevant domain should answer:

* What is the index ID?
* What package or module owns it?
* What does it consume?
* What does it produce?
* What must it never do?
* What validates its output?
* What depends on it?
* What happens when it fails?
* What tests prove success and failure?
* What permissions are required?
* What audit events are required?
* What is the demo acceptance story?

If those answers are missing, the correct action is to update the plan, not invent implementation behavior.

This is the key rule:

> Missing planning detail is not permission for the agent to improvise.

---

## Example Agent Task

Instead of this:

```text
Build the validation system from the planning docs.
```

Use this:

```text
Implement only IC-13 — CI and Validation Runner.

Read:
- current/current-plan.md
- index/IC-13-ci-validation-runner.md
- index/IC-21-audit-system.md
- index/IC-25-security-tenant-isolation.md
- matrices/test-matrix.md
- matrices/validation-matrix.md
- glossary/terminology-glossary.md

Do not read archive unless one of those files references it.
Do not implement future scope.
Do not write code until the pre-implementation gate is answered.
Return any missing plan details before coding.
```

That gives the agent bounded context.

The agent does not need the entire project history.

It needs the right slice of the project.

---

## Another Example: Frontend Task

Instead of:

```text
Build the UI.
```

Use:

```text
Implement the Tier 1 static canvas foundation.

Read:
- current/current-plan.md
- index/IC-01-canonical-graph-model.md
- index/IC-02-design-system.md
- index/IC-05-canvas-editor.md
- index/IC-06-node-edge-inspector.md
- index/IC-07-quick-actions.md
- index/IC-08-graph-versioning-diff.md
- index/IC-24-testing-strategy.md
- matrices/module-map.md
- matrices/test-matrix.md
- matrices/demo-acceptance-matrix.md
- glossary/terminology-glossary.md

Do not implement deployment.
Do not implement cluster agent work.
Do not implement future import.
Do not create local domain types that belong in shared schemas.
Return the Tier 1 readiness check before coding.
```

This prevents the agent from turning a canvas task into a full platform implementation.

---

## Why This Works

This structure gives agents what they need most:

* Bounded context
* Stable task IDs
* Clear authority order
* Module ownership
* Validation rules
* Test expectations
* Future-scope boundaries
* Consistent terminology
* Traceability back to the plan

It turns a large project into smaller, reviewable work packets.

The goal is not to make the agent autonomous.

The goal is to make the agent controllable.

---

## The Context Window Benefit

AI agents have limited working context.

Even when the context window is large, more context is not always better.

A giant context can cause the agent to:

* Over-prioritize irrelevant details
* Resurrect old requirements
* Confuse future plans with current scope
* Miss the exact section that matters
* Produce broad but shallow output
* Implement across too many modules at once

Indexed planning reduces that problem.

Each task can pull only the relevant files:

```text
Governance file
Relevant IC domain files
Relevant matrices
Glossary
Maybe one archived source if referenced
```

That is the right context shape.

Not too small.

Not too broad.

Just enough.

---

## Recommended Guardrails

Agents working from this structure should follow these rules:

* Do not implement from archived docs.
* Do not add new platform capabilities unless the current plan allows them.
* Do not bypass module boundaries.
* Do not redefine shared domain types locally.
* Do not treat UI checks as security.
* Do not create functionality without validation.
* Do not create privileged actions without audit.
* Do not implement future-scope items early.
* Do not continue if the relevant IC file is missing required sections.
* Do not write code when the request is for planning or readiness review.
* Do not start implementation until the relevant pre-code gate is answered.

---

## What Good Output Looks Like

When an agent receives a task, good output should look like this:

```text
Relevant IC domains:
- IC-13 CI and Validation Runner
- IC-21 Audit System
- IC-25 Security and Tenant Isolation

Relevant matrices:
- test-matrix.md
- validation-matrix.md
- dependency-map.md

Owner package:
- packages/ci-runner

Inputs:
- generated bundle
- trace metadata
- graph version
- environment
- policy profile

Outputs:
- normalized CI report
- findings
- blocking decision
- audit evidence

Must never do:
- deploy
- mutate graph versions
- render frontend-specific findings
- bypass policy

Missing details:
- exact timeout behavior for failed tool execution is not defined

Recommendation:
- resolve timeout behavior before implementation
```

That is controlled agent behavior.

The agent is not just producing code.

It is proving it understands the bounded work packet.

---

## Design Principle

Large AI-assisted builds need an indexed planning layer.

The pattern is:

```text
Unstructured notes
→ Current governance plan
→ Indexed domain files
→ Validation matrices
→ Bounded implementation tasks
→ Reviewable agent output
```

This is how you keep AI-assisted development from turning into uncontrolled feature generation.

The plan becomes the control surface.

The index becomes the routing layer.

The matrices become the gates.

The glossary becomes the language contract.

The archive becomes memory, not authority.

The agent becomes an implementer inside boundaries, not the architect of the whole system.

---

## Final Summary

This structure may look heavy, but it exists to make the actual work lighter.

It keeps each task focused.

It keeps old plans from polluting new work.

It keeps future scope from sneaking into current implementation.

It gives humans a clean review path.

It gives AI agents bounded context.

It creates a repeatable way to move from planning to implementation without losing control.

The goal is not more documentation for its own sake.

The goal is controlled execution.

```
```
