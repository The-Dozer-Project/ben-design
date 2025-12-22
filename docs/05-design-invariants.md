# Design Invariants

The following invariants define properties of the Ben system that must hold regardless of implementation details, scale, or deployment context.

Violating these invariants is considered a design failure, not an acceptable tradeoff.

---

## Intent Is Authoritative

Declared intent, expressed through schemas, policy packs, playbooks, and activation epochs is the authoritative source of system behavior.

Telemetry, analytics, and derived insight are observational and non-authoritative.

---

## No Component Both Decides and Routes

Decision-making and routing responsibilities are strictly separated.

Components that:
- evaluate rules
- trigger actions
- or emit signals

do not route traffic.

Components that route traffic do not:
- interpret meaning
- evaluate policy
- or trigger action

This separation prevents hidden authority escalation and simplifies failure reasoning.

---

## No Implicit Authority

Authority is never inferred from:
- topology
- proximity
- historical behavior
- partial state
- component identity

All authority in Ben is explicitly declared and bounded.

When authority cannot be established, components must refuse to act.

---

## Re-Observation Over Reconstruction

Ben does not reconstruct system behavior from incomplete telemetry.

After failure, the system re-enters through presence observation and reconciliation against declared intent.

False continuity is treated as more dangerous than temporary loss.

---

## Loss Must Be Explicit

Ben does not guarantee zero data loss.

When loss occurs, it must be:
- visible
- bounded
- attributable
- non-corrupting

Silent loss or hidden retries that obscure system state are violations of this invariant.

---

## Hot Paths Stay Cheap

All hot-path components are designed to:
- be lightweight
- avoid expensive computation
- avoid blocking external systems

Expensive, high-risk, or iterative work is offloaded to cold paths (e.g., Lucius).

---

## Statelessness Is Preferred, State Is Justified

Components are stateless by default.

State is introduced only when:
- it is unavoidable
- it is bounded
- it is discardable
- its failure mode is well understood

State must never be required to infer intent.

---

## Refusal Is a Correct Outcome

When required information is missing, ambiguous, or invalid:
- components refuse to act
- no implicit fallback is applied

Refusal preserves legibility and prevents cascading error.

---

## Presence Is Repeatable, Commands Are Not

Presence and health signals are treated as repeatable and ephemeral.

Commands, deployments, and intent activation are not repeatable and must be explicitly acknowledged.

This distinction prevents replay-induced corruption.

---

## Humans Are Part of the System

Ben assumes human error, fatigue, and misinterpretation.

Governance, safety, and boundaries are enforced architecturally rather than procedurally.

The system must remain understandable under operational pressure.

---

## Simplicity Is Preserved by Refusal

Ben refuses to:
- infer missing behavior
- generalize prematurely
- support ambiguous extension points

Simplicity is maintained by limiting what the system is willing to do.

---

## Summary

These invariants exist to:
- prevent hidden authority
- preserve intent under failure
- keep system behavior legible
- bound complexity

Any extension or modification to Ben must preserve these properties.