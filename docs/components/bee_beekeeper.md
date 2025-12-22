# BEE and Bee-Keeper

## Purpose

BEE (Ben Escrow Emissary) and Bee-Keeper together define the trust boundary between applications and the Ben telemetry ecosystem.

Their purpose is to verify identity, communication integrity, and policy compliance at connection time, without embedding trust logic into Ben’s core data path.

BEE performs short-lived verification. Bee-Keeper maintains long-lived oversight and enforcement.

---

## Authority and Boundaries

BEE and Bee-Keeper have authority only over **connection verification and session admission**.

They are authoritative for:
- Verifying the identity of connecting processes
- Validating communication lanes before telemetry begins
- Enforcing policy at the boundary between applications and sidecars
- Accepting or refusing session establishment with auditable reasoning

They are **not** authoritative for:
- Telemetry interpretation
- Policy definition
- Signal generation
- Runtime decision-making
- Data persistence beyond session evidence

BEE and Bee-Keeper do **not** infer trust. All trust decisions are explicitly verified or refused.

---

## Lifecycle and State

BEE instances are ephemeral. Each instance exists only long enough to perform verification and then terminates.

Bee-Keeper is long-running and maintains minimal, derived state required to:
- Track active sessions
- Enforce admission policy
- Preserve an auditable record of verification outcomes

All state maintained by Bee-Keeper is discardable and recomputable. No durable system intent is stored here.

---

## Interaction With Other Components

- Applications initiate contact through Bee-Keeper to establish a verified session.
- Upon successful verification, communication lanes are handed off to sidecars.
- Sidecars rely on BEE-verified connections and do not perform independent trust verification.
- Policy and intent used during verification are read from authoritative sources, not authored locally.

BEE and Bee-Keeper do not participate in telemetry flow, signal routing, or analysis pipelines.

---

## What This Component Does Not Do

BEE and Bee-Keeper do not:
- Mutate application or system state
- Execute playbooks or corrective actions
- Act as a message broker or telemetry processor
- Persist or replay telemetry
- Serve as a system of record for intent

They exist solely to ensure that observation begins from a verified and explainable boundary.

---

## Related Documents

For detailed discussion of BEE’s security model, kernel-level identity verification, lease semantics, and write-ahead logging guarantees, see [BEE deepdive](../deepdives/bee_deepdive.md)