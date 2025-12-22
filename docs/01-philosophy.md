# Philosophy

Ben is built on a set of beliefs that are non-negotiable by design. Not implementation details, but constraints 
that shape architectural decisions and limit what the system willingly does.

## Opinionated by Design

Ben by design limits extensibility to reduce boilerplate and eliminate ambiguity. It is not a universal security
platform or does it aim to operate in every ecosystem. Over-generalized platforms introduce complexity, unclear
ownership, policy ambiguity and introduce error surface. Ben works in a contained way by design.

## Simple Setup Through Abstraction

Ben's goal is a "if it compiles it runs" model. Behavior is better dictated through contracts than runtime 
configuration sprawl. Users that understand schemas, decorations, and can reason through Rust structs should
be able to reason Ben's behavior. Complexity does exist, but it is abstracted behind stable and explicit interfaces.

## No Drift, Only Versioning

All behavior variants are known at compile time. Runtime behavior remains static unless versioned. 
Code-as-config eliminates classes of organizational failings that include silent drift, undocumented fixes
and environmental divergence from intent.

## Collapsing Adjacent Systems

Sanitzation, playbooks, event metadata, system correction and observation are framed as a single cohesive issue.
Splitting these concerns across multiple tools increases complexity, surface area and requires deep cross-system
knowledge. Ben collapses these responsibilites into on architectural surface.

## Single-Pass Sanitization Is Insufficient

Single-pass sanitization inherently assumes perfect foresight. Ben operates under the premise data may need to be
examined, re-examined, and go through different handling routines at different stages. The ability to re-introduce
unsanitized or suspect data into controlled analysis paths is treated as a best practice.

## No Lock-In Unless Explicitly Chosen

Ben is platform and language agnostic by design. Communication is file descriptor and socket based. Ben integration
for applications require attaching a sidecar and not opening ports. No logic rewriting, complex implementation
or deeper integration unless desired.

## Zero Trust by Default

There is no line where Ben blindly accepts data. All boundaries are treated as untrusted unless validated, masked or 
constrained by explicit permission. Trust is not inferred by system topology, proximity or intent.

## Free and Open Source by Construction

Ben is fully open source. Users can inspect, reason, audit and fork Ben for their own uses. Proprietary black boxes
reduce observability and require acceptance of opacity for oft mission critical parts of a system. Licensed under
AGPL, Ben ensures improvements remain inspectable.

## Intent Is More Durable Than Telemetry

Ben's data approach is lossy, partial and failure prone. Intent by schemas, policy, playbooks and activation 
epochs are durable. Prioritizing intent preservation of history preservation, enabling correct re-observation
failure is the philosophy behind Ben's failure modes

## Re-Observation Over Replay

Ben makes no attempt to reconstruct past system behavior from incomplete data. After failure, re-observation
of reality reconciles telemetry against declared intent. This is to avoid a false continuity and hidden assumption.

## Explicit and Bounded Authority

Every Ben component has a clear responsibility and scope of authority. Components don't infer intent, policy
or silent privilege escalation. Ambiguity in authority is a design flaw.

## Loss Is Acceptable If It Is Explainable

Ben does not guarantee 0 data loss. It guarantees that when loss happens it is explicit, bounded and understandable.
Systems claiming complete continuity oft hide failure; Ben surfaces it.

## Human Boundaries Are Architectural

Ben assumes human fallibility while under operational pressure. Governance and safety are enforced through system
architecture. 

## Refusal as a Feature

Ben does not infer behavior or apply implicit fixes. It does not "do the obvious". Refusal of implicit action
preserves legibility. When Ben acts, it does because of explicit intent.