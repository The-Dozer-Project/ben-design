# bux (Ben UX)

## Purpose

bux is the human-facing user interface for observing, inspecting, and testing Ben system intent and behavior.

Its purpose is to provide visibility into system state, telemetry, health, and bounded actions, and to offer a safe environment for testing and exploration without granting authority to mutate or deploy system intent.

bux exists to help humans understand Ben, not to control it.

---

## Authority and Boundaries

bux is **not authoritative** for any part of the Ben system.

It has authority only to:
- View and explore system telemetry and analytics
- Inspect system health and derived signals
- Observe bounded actions taken by detectors and playbooks
- Test and validate declared intent in a non-deploying context
- Do artifact handoff from user schemas
- View/test/explore Lucius "light scrub" tooling.

bux does **not** implement, deploy, or activate:
- Schemas
- Policies
- Playbooks
- Signals
- Runtime configuration

All authoritative changes to the system occur outside of bux.

---

## Interaction Model

bux interacts with Ben exclusively through **ben_api**.

It does not communicate directly with:
- ben_ledger
- benctl
- runtime components

bux uses a push-based model to receive updates and system state, favoring responsiveness and exploration over strict real-time guarantees.

---

## State Model

bux is stateless with respect to system authority.

It may retain **local or workspace-scoped state** for user convenience, including:
- Saved test cases
- Draft schemas or playbooks used for validation
- Aggregated views or exploratory artifacts

Any state retained by bux is:
- Non-authoritative
- Non-binding
- Never deployed directly into the system

Persisted bux state is treated as user workspace data, not system intent.

---

## Testing and Exploration Role

bux provides a testing plane for declared intent.

Through bux, users may:
- Validate schemas and contracts
- Simulate or inspect the effects of playbooks and signals
- Explore aggregated telemetry and detector outputs
- Export tested aggregates or exploratory results into files

bux cannot deploy, activate, or publish these artifacts. Deployment requires explicit action through benctl.

---

## Collaboration and Workspaces

bux supports collaborative workflows through team workspaces.

Workspaces allow users to:
- Share views, tests, and exploratory artifacts
- Collaborate on understanding system behavior
- Align on interpretation without modifying system state

Workspaces do not grant shared authority over Ben itself.

---

## What This Component Does Not Do

bux does not:
- Write to ben_ledger
- Interact with benctl
- Deploy or activate intent
- Issue runtime commands
- Serve as a system of record
- Infer or enforce policy

It is a viewer, tester, and exploratory surface only.

