# Ben Design


### **NOTE**
  NATS is not a god object. I feel I did a bad job with the visual diagrams.
  NATS does the following:
    - NATS  coordinates schema/config/policy pack changes by fanning out messages
    - NATS delivers signals from detectors to sidecars
    - NATS is the "go-to" component in catastrophic system failure. In the instance that the system goes down, components sends "hello" messages to NATS 
      which keeps attempting to deliver to the orchestrator until its online, where the orchestrator handles spin up
    - NATS does not handle or deliver health messages
  Ideally, NATS is generally low throughput (as opposed to the rest of the system). Data travels up through the hot path.
  I just wanted to add that because I got feedback that from the diagrams NATS looks way more involved than it is.


This repository contains the architectural paper and supporting documentation for **Ben**, an intent-driven integrated security platform (that can do telemetry).

The goal of this document set is to make Ben legible; to clearly describe what the system is, what it does, what it does not do, and why it is structured the way it is.

This is not a marketing document.  
It is a working architectural narrative.

---

## Repository Structure

This repository is organized to progressively deepen understanding of the system, from high-level boundaries to detailed technical exploration.

### `docs/assets`

Contains diagrams and visual aids used throughout the documentation.

Diagrams are treated as first-class artifacts and are used to:
- clarify system boundaries
- expose complexity rather than hide it
- support reasoning about failure modes and flows

---

### `docs/components`

Describes each major system component and its role within the broader Ben architecture.

Component documents focus on:
- purpose
- authority and boundaries
- interaction with other components
- what the component explicitly does **not** do

These documents are **not** deep technical dives. They are meant to establish mental models and architectural intent.

---

### `docs/flows`

Explains how the system behaves across common and critical workflows.

Flow documents:
- break down complex diagrams into understandable steps
- describe how components interact over time
- highlight failure modes and stress conditions
- surface known pain points and open questions

Flows are intentionally illustrative rather than exhaustive.

---

### `docs/deepdives`

Contains deeply technical exploration of specific subsystems.

Deep dives may include:
- implementation details
- algorithmic discussion
- performance considerations
- tradeoffs and alternative designs

These documents assume familiarity with the rest of the repository and are intended for readers who want to engage with Ben at an implementation level.

---

## Scope and Intent

This repository documents Ben as it exists today and as it is currently understood.

It is a **living document repo**.

As development progresses, discoveries are made, and assumptions are challenged, this documentation will evolve. Changes may occur due to:
- implementation realities
- failure analysis
- new operational insight
- improved understanding of system boundaries

Stability of intent is valued over stability of wording.

---

## What This Repository Is Not

- A tutorial
- A product manual
- A guarantee of correctness
- A complete security proof
- A promise of backward compatibility

The purpose of this repository is clarity, not completeness.

---

## Status

This document set represents an early but coherent architectural state.

Feedback, critique, and discussion are welcome, but contributions are intentionally gated to preserve architectural consistency while the system matures.

---

## Closing Note

Ben is designed to be opinionated.

If this documentation feels constrained, explicit, or uncompromising, that is by design.
