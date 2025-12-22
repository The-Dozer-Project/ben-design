# Non-Goals

This section lists behaviors Ben explicitly **does not** guarantee or require. While not statements about what is impossible, these are descriptions of what Ben does not assume as a prerequisite for correctness.

## Ben Is Not a Required System of Record for History

Complete replay or backfilled timelines are not required for Ben to function. Telemetry can be
partial and/or lossy. Correctness is derived from intent rather than event history.

Replay and/or forensic capabilities may exist as optional planes in the future, however Ben will operate independent
of them.


## Ben Is Not an Implicitly Self-Healing Control Plane

Ben does not perform silent remediation, inferred corrections, or unbounded autonomous action. All corrective behavior is driven by declared intent in the form of policies and playbooks.

While Ben supports bounded autonomy, including ML-driven detectors and automated playbook execution, autonomy is explicit, scoped, attributable, and reversible. Ben does not infer goals or act beyond declared intent.

## Ben Is Not a Configuration Mutation Engine

Ben is not a general-purpose configuration management system. It does not mutate runtime configuration or reconcile imperative state drift.

Behavior is declared through versioned contracts and intent, not through ongoing mutable configuration.

## Ben Is Not a UI-Authored Source of Truth

UX interfaces in Ben are not authoritative of system state. Interactive tooling is intended for observation, exploration, and validation.

Authorship of schemas, policies, and playbooks occurs through explicit tooling and contracts (benctl).

## Ben Does Not Assume Completeness

Ben **does not** assume complete telemetry coverage, perfect observability, or exhaustive system knowledge. Absence of data is treated as a first-class condition rather than an error to be hidden.

Where more completeness is needed, it must be achieved through explicit design, coupling and intent. 

## Ben Is Not a Universal Integration Layer

Ben **does not** integrate with every platform/protocol/ecosystem. It is opinionated and bounded.

Interoperability is achieved through stable, minimal boundaries rather than exhaustive compatibility.

## Summary

Ben **does not** to infer correctness from history, topology, or heuristics.  
It **does not** hide loss, infer intent, or silently assume authority.  

These refusals are intentional and foundational to its design.