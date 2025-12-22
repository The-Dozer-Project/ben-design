# Proxy

## Purpose

Proxies form the boundary between application-facing sidecars and Ben’s internal processing components.

Their purpose is to route, buffer, and regulate data flow into the Ben system, ensuring that internal components are protected from bursty, malformed, or unbounded traffic while preserving declared intent and system stability.

Proxies exist to manage flow, not to interpret meaning.

---

## Authority and Boundaries

Proxies are authoritative only for:
- Routing data between sidecars and internal components
- Regulating traffic flow and applying backpressure
- Enforcing structural boundaries between application-facing and internal planes
- Routing eligible data toward Lucius for sanitization
- Routing data deemed okay by lucius to the system

Proxies are **not** authoritative for:
- Policy or rule evaluation not pertaining to Lucius routing
- Signal generation
- Telemetry interpretation
- Schema or intent declaration
- Component orchestration

Proxies do not make decisions about *what* data means or *what* actions should occur.

---

## Routing and Boundary Role

Proxies act as the primary ingress boundary into Ben’s internal systems.

They:
- Accept data from one or many sidecars
- Route data toward spines for evaluation
- Divert marked or eligible data toward Lucius
- Isolate internal components from application-level traffic behavior

This boundary prevents application-side variability from leaking into Ben’s internal processing paths.

---

## Configuration Model

Proxies operate under a unified configuration model.

A single configuration applies across proxy instances to:
- Simplify reasoning about system behavior
- Support one-to-many relationships with sidecars
- Avoid per-proxy drift and implicit specialization

Configuration is expressed through Ben’s in-house DSL and is treated as code-as-configuration.

While future versions may support more granular control, uniform configuration is the default and preferred model.

---

## Flow Control and Backpressure

Proxies are responsible for:
- Managing bursty or uneven traffic
- Applying backpressure toward sidecars when internal capacity is constrained
- Protecting spines and downstream systems from overload

Flow control decisions are mechanical and structural. Proxies do not drop, modify, or reinterpret data based on semantic content.

---

## State Model

Proxies are semi-stateless.

They may maintain transient state related to:
- In-flight connections
- Buffering and flow control
- Load-dependent routing decisions

All such state is local, bounded, and discardable. Restarting a proxy does not invalidate system intent or historical data.

---

## Interaction With Other Components

- **Sidecars** send application-derived data to proxies.
- **Spines** receive regulated and routed data from proxies.
- **Lucius** receives data explicitly routed for sanitization. **Lucius** returns okay data to proxies.
- **Orchestrator** manages proxy lifecycle but does not influence routing behavior.
- **Configuration sources** provide declarative proxy configuration.

Proxies do not interact directly with detectors or user-facing components.

---

## What This Component Does Not Do

Proxies do not:
- Evaluate rules or policies not pertaining to lucius routing
- Emit signals
- Interpret telemetry
- Store authoritative data
- Participate in intent declaration or deployment

They exist to keep traffic honest, bounded, and survivable.
