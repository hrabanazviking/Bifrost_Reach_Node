
# Bifröst Reach Node Software
## Mythic Engineering Proof-Gated Master Roadmap

**Document type:** implementation roadmap, verification plan, feature ledger, and engineering completion contract  
**Project:** Bifröst Reach Node  
**Method:** Mythic Engineering, architecture-first, vertical-slice, proof-driven development  
**Primary target:** Raspberry Pi 5-class Linux edge nodes  
**Reference radio:** LTE/5G QMI/MBIM/AT-capable modem such as the Quectel RM520N-GL  
**Design posture:** local-first, offline-capable, modular, recoverable, observable, hardware-safe, and container-free by default  
**Status:** living document

---

# 1. Why This Roadmap Exists

Bifröst Reach Node is not one application. It is a small communications platform containing a router, modem controller, RF measurement instrument, field notebook, failover engine, optional electromechanical antenna system, power-aware edge computer, remote-management appliance, and experimental radio-intelligence laboratory.

That breadth makes ordinary feature checklists dangerous. A subsystem can look finished while hiding stale telemetry, unsafe recovery, vendor lock-in, fragile routing, or a physical failure mode that only appears outdoors.

This roadmap therefore treats **proof as part of the implementation**.

A checkbox means nothing until a reproducible artifact shows the behavior exists under the conditions claimed.

> **No rune is carved COMPLETE until the proof exists.**

---

# 2. Mythic Engineering Forge Cycle

Every slice follows the same nine-stage cycle.

1. **Myth** — state the human purpose and field problem.
2. **Rune** — define the stable contract, schema, interface, state, event, or protocol.
3. **Hall** — assign subsystem ownership so responsibility is not smeared across the codebase.
4. **Thread** — map dependencies, consumers, events, failure propagation, and recovery.
5. **Slice** — build the smallest useful end-to-end vertical behavior.
6. **Trial** — prove the normal path.
7. **Ordeal** — deliberately induce realistic failure.
8. **Proof** — capture reproducible evidence tied to a Git commit and environment.
9. **Inscription** — update docs, tests, proof manifest, changelog, and roadmap state.

The method deliberately favors a narrow working bridge over a wide field of half-finished abstractions.

---

# 3. Proof Classes

| Code | Proof | Meaning |
|---|---|---|
| P0 | Static | build, import, lint, type, schema, packaging, licensing |
| P1 | Unit | deterministic local test |
| P2 | Integration | multiple real software components interact |
| P3 | HIL | real hardware participates |
| P4 | Fault | deliberate failure produces safe/diagnosable behavior |
| P5 | Field | realistic outdoor/mobile/campsite deployment |
| P6 | Endurance | 8h/24h/72h/7d stability evidence as required |
| P7 | Cross-platform | more than one host/modem/backend |
| P8 | Safety/Security | physical or trust-boundary behavior is proven |
| P9 | Recovery | known-good restoration is demonstrated |

## Status Symbols

- `[ ]` not started
- `[~]` prototype exists, proof incomplete
- `[T]` automated tests pass, field/HIL proof incomplete
- `[P]` reference-platform proof complete
- `[V]` verification matrix complete
- `[X]` complete for its target milestone

---

# 4. Universal Definition of Done

Unless a slice explicitly says otherwise, it cannot become `[X]` until:

- [ ] purpose is written;
- [ ] contract/schema/interface is defined;
- [ ] configuration is documented;
- [ ] normal behavior is tested;
- [ ] at least one realistic failure is induced;
- [ ] errors are actionable;
- [ ] logs reveal cause without leaking secrets;
- [ ] reboot/restart semantics are known;
- [ ] persistence behavior is known;
- [ ] security impact is reviewed;
- [ ] power impact is considered when relevant;
- [ ] physical safety is reviewed when relevant;
- [ ] mocks exist where hardware can reasonably be simulated;
- [ ] real hardware proof exists for real hardware claims;
- [ ] proof is tied to an exact Git commit;
- [ ] known limitations are written;
- [ ] docs are updated;
- [ ] proof manifest is updated.

---

# 5. Proof Artifact Layout

```text
proofs/
  <slice-id>/
    README.md
    environment.txt
    commands.txt
    expected.txt
    actual.txt
    result.json
    logs/
    screenshots/
    captures/
    fixtures/
```

Suggested `result.json`:

```json
{
  "slice": "M4-S1",
  "result": "PASS",
  "commit": "<git-commit>",
  "platform": "raspberry-pi-5",
  "proof_classes": ["P1", "P2", "P3", "P4"],
  "limitations": []
}
```

---

# 6. Architectural Laws

1. Cellular radios are replaceable peripherals, not the application.
2. Core logic speaks normalized state, never raw vendor output.
3. Hardware-facing components have mockable interfaces.
4. The router survives UI failure.
5. The router survives rotator failure.
6. Safety lives below convenience. A Linux process cannot override MCU hard limits.
7. Events describe state transitions explicitly.
8. Historical memory is local-first and inspectable.
9. Stale telemetry is never presented as current telemetry.
10. Estimates are labeled as estimates.
11. Optional AI may advise, but deterministic code owns actuation, networking safety, and recovery.
12. Recovery is a first-class feature, not an afterthought.
13. A feature claim must never be broader than its proof matrix.
14. No cloud service is required for core local operation.
15. Advanced modem knobs always have a reset-to-conservative path.

---

# 7. Feature Priority Classes

- **CORE**: required for a useful public v1.
- **IMPORTANT**: strongly recommended for dependable field operation.
- **OPTIONAL**: useful but not required for initial viability.
- **ADVANCED**: high-complexity later feature.
- **EXPERIMENTAL**: research until measurable value is demonstrated.
- **FUTURE**: architecture should permit it, implementation can wait.

---

# 8. Master Milestone Sequence

```text
Foundation
  ↓
Real modem
  ↓
Real cellular session
  ↓
Trustworthy RF telemetry
  ↓
Router/AP
  ↓
Scans and scoring
  ↓
Memory + GPS
  ↓
Multi-WAN
  ↓
Recovery + power intelligence
  ↓
Safe rotator
  ↓
Automatic aiming
  ↓
Distributed radio head
  ↓
Advanced carrier intelligence
  ↓
Experimental prediction / AI / multi-path
```

The project must resist the temptation to build the glamorous end of this chain first.

---


# M0 — Forge Foundation

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **forge foundation** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M0-S1 — Repository structure

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **repository structure** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s1/`

## M0-S2 — packaging and semantic versioning

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **packaging and semantic versioning** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s2/`

## M0-S3 — TOML configuration schema

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **toml configuration schema** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s3/`

## M0-S4 — structured logging and secret redaction

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **structured logging and secret redaction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s4/`

## M0-S5 — typed error taxonomy

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **typed error taxonomy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s5/`

## M0-S6 — internal event bus

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **internal event bus** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s6/`

## M0-S7 — aggregate system state snapshot

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **aggregate system state snapshot** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s7/`

## M0-S8 — test markers and harness

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **test markers and harness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s8/`

## M0-S9 — proof-capture tooling

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **proof-capture tooling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s9/`

## M0-S10 — SPDX/license checks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **spdx/license checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s10/`

## M0-S11 — architecture decision records

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **architecture decision records** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s11/`

## M0-S12 — changelog and release metadata

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **changelog and release metadata** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m0-s12/`



---


# M1 — Host and Hardware Abstraction

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **host and hardware abstraction** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M1-S1 — Host inventory

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **host inventory** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s1/`

## M1-S2 — capability registry

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **capability registry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s2/`

## M1-S3 — privilege boundary

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **privilege boundary** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s3/`

## M1-S4 — pluggable modem/GNSS/rotator/power/network backends

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **pluggable modem/gnss/rotator/power/network backends** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s4/`

## M1-S5 — Raspberry Pi 5 reference support

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **raspberry pi 5 reference support** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s5/`

## M1-S6 — Raspberry Pi 4 support

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **raspberry pi 4 support** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s6/`

## M1-S7 — ARM64 portability

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **arm64 portability** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s7/`

## M1-S8 — x86-64 development support

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **x86-64 development support** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s8/`

## M1-S9 — Debian/Raspberry Pi OS native deployment

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **debian/raspberry pi os native deployment** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s9/`

## M1-S10 — optional OpenWrt compatibility

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional openwrt compatibility** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s10/`

## M1-S11 — container-free service model

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **container-free service model** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s11/`

## M1-S12 — hardware mocks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **hardware mocks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m1-s12/`



---


# M2 — Modem Discovery and Transport

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **modem discovery and transport** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M2-S1 — Modem hotplug discovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem hotplug discovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s1/`

## M2-S2 — stable logical modem identity

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **stable logical modem identity** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s2/`

## M2-S3 — manufacturer/model/firmware/capability inventory

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manufacturer/model/firmware/capability inventory** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s3/`

## M2-S4 — ModemManager backend

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modemmanager backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s4/`

## M2-S5 — QMI backend

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **qmi backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s5/`

## M2-S6 — MBIM backend

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mbim backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s6/`

## M2-S7 — guarded AT-command backend

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **guarded at-command backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s7/`

## M2-S8 — vendor firmware quirk registry

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **vendor firmware quirk registry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s8/`

## M2-S9 — raw diagnostic snapshot

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **raw diagnostic snapshot** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s9/`

## M2-S10 — multiple attached modems

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multiple attached modems** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s10/`

## M2-S11 — privacy-safe identifier handling

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **privacy-safe identifier handling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m2-s11/`



---


# M3 — SIM Registration and Bearer Lifecycle

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **sim registration and bearer lifecycle** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M3-S1 — SIM presence and readiness

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sim presence and readiness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s1/`

## M3-S2 — lock state awareness

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **lock state awareness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s2/`

## M3-S3 — APN profiles

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **apn profiles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s3/`

## M3-S4 — IPv4

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ipv4** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s4/`

## M3-S5 — IPv6

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ipv6** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s5/`

## M3-S6 — dual stack

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dual stack** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s6/`

## M3-S7 — home/roaming/searching/denied/emergency registration states

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **home/roaming/searching/denied/emergency registration states** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s7/`

## M3-S8 — bearer connect/disconnect/reconnect

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bearer connect/disconnect/reconnect** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s8/`

## M3-S9 — IP/gateway/DNS acquisition

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ip/gateway/dns acquisition** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s9/`

## M3-S10 — end-to-end Internet health

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **end-to-end internet health** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s10/`

## M3-S11 — roaming policy

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **roaming policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s11/`

## M3-S12 — optional eSIM awareness

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional esim awareness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s12/`

## M3-S13 — session byte counters

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **session byte counters** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s13/`

## M3-S14 — reconnect stress testing

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **reconnect stress testing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m3-s14/`



---


# M4 — RF Telemetry Truth Layer

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **rf telemetry truth layer** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M4-S1 — RSRP

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rsrp** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s1/`

## M4-S2 — RSRQ

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rsrq** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s2/`

## M4-S3 — RSSI

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rssi** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s3/`

## M4-S4 — SINR/SNR

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sinr/snr** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s4/`

## M4-S5 — LTE and 5G radio-access technology

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **lte and 5g radio-access technology** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s5/`

## M4-S6 — 5G NSA and SA distinction

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **5g nsa and sa distinction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s6/`

## M4-S7 — serving band

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **serving band** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s7/`

## M4-S8 — channel information

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **channel information** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s8/`

## M4-S9 — cell ID

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cell id** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s9/`

## M4-S10 — TAC/LAC/PCI where exposed

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **tac/lac/pci where exposed** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s10/`

## M4-S11 — carrier aggregation telemetry

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **carrier aggregation telemetry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s11/`

## M4-S12 — LTE anchor and NR relationship

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **lte anchor and nr relationship** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s12/`

## M4-S13 — modem temperature

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem temperature** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s13/`

## M4-S14 — metric timestamps

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **metric timestamps** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s14/`

## M4-S15 — stale-value prevention

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **stale-value prevention** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s15/`

## M4-S16 — configurable sampling rates

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **configurable sampling rates** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m4-s16/`



---


# M5 — Measurement and Signal Scan Engine

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **measurement and signal scan engine** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M5-S1 — Immutable observation schema

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **immutable observation schema** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s1/`

## M5-S2 — passive scan

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **passive scan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s2/`

## M5-S3 — latency measurement

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **latency measurement** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s3/`

## M5-S4 — DNS timing

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dns timing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s4/`

## M5-S5 — packet loss

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **packet loss** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s5/`

## M5-S6 — jitter

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **jitter** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s6/`

## M5-S7 — bounded upload/download throughput testing

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bounded upload/download throughput testing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s7/`

## M5-S8 — connection-quality score

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **connection-quality score** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s8/`

## M5-S9 — stability score

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **stability score** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s9/`

## M5-S10 — rolling variance

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rolling variance** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s10/`

## M5-S11 — scan scheduling

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **scan scheduling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s11/`

## M5-S12 — active-test data cap

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **active-test data cap** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s12/`

## M5-S13 — traffic-sensitive scanning

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **traffic-sensitive scanning** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s13/`

## M5-S14 — reliability/latency/bulk-download/low-power/data-constrained profiles

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **reliability/latency/bulk-download/low-power/data-constrained profiles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s14/`

## M5-S15 — scan provenance

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **scan provenance** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m5-s15/`



---


# M6 — Persistent Memory and Site Knowledge

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **persistent memory and site knowledge** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M6-S1 — SQLite WAL database

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sqlite wal database** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s1/`

## M6-S2 — migrations

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **migrations** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s2/`

## M6-S3 — integrity check

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **integrity check** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s3/`

## M6-S4 — transactional observation storage

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **transactional observation storage** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s4/`

## M6-S5 — named sites

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **named sites** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s5/`

## M6-S6 — coordinate clustering

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **coordinate clustering** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s6/`

## M6-S7 — known-good profiles

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **known-good profiles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s7/`

## M6-S8 — serving-cell history

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **serving-cell history** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s8/`

## M6-S9 — bearing history

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bearing history** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s9/`

## M6-S10 — retention policy

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **retention policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s10/`

## M6-S11 — aggregation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **aggregation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s11/`

## M6-S12 — backup and restore

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **backup and restore** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s12/`

## M6-S13 — CSV export

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **csv export** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s13/`

## M6-S14 — JSON export

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **json export** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s14/`

## M6-S15 — GeoJSON export

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **geojson export** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s15/`

## M6-S16 — human notes

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **human notes** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s16/`

## M6-S17 — mast/antenna metadata

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mast/antenna metadata** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s17/`

## M6-S18 — measurement provenance

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **measurement provenance** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m6-s18/`



---


# M7 — GPS Compass and Deployment Context

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **gps compass and deployment context** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M7-S1 — gpsd backend

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **gpsd backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s1/`

## M7-S2 — modem-integrated GNSS

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem-integrated gnss** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s2/`

## M7-S3 — optional raw NMEA backend

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional raw nmea backend** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s3/`

## M7-S4 — fix confidence and accuracy

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **fix confidence and accuracy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s4/`

## M7-S5 — altitude

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **altitude** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s5/`

## M7-S6 — speed and heading

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **speed and heading** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s6/`

## M7-S7 — electronic compass

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **electronic compass** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s7/`

## M7-S8 — compass calibration

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **compass calibration** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s8/`

## M7-S9 — optional magnetic-declination correction

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional magnetic-declination correction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s9/`

## M7-S10 — mast height

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mast height** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s10/`

## M7-S11 — antenna model

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **antenna model** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s11/`

## M7-S12 — polarization

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **polarization** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s12/`

## M7-S13 — cable profile

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cable profile** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s13/`

## M7-S14 — deployment mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **deployment mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s14/`

## M7-S15 — context linkage to scans

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **context linkage to scans** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m7-s15/`



---


# M8 — Local Router and Client Network

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **local router and client network** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M8-S1 — IPv4 forwarding

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ipv4 forwarding** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s1/`

## M8-S2 — IPv6 forwarding

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ipv6 forwarding** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s2/`

## M8-S3 — NAT

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **nat** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s3/`

## M8-S4 — DHCP

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dhcp** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s4/`

## M8-S5 — DNS forwarding/cache

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dns forwarding/cache** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s5/`

## M8-S6 — firewall baseline

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **firewall baseline** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s6/`

## M8-S7 — WAN isolation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **wan isolation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s7/`

## M8-S8 — secure Wi-Fi access point

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **secure wi-fi access point** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s8/`

## M8-S9 — Ethernet LAN

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ethernet lan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s9/`

## M8-S10 — client inventory

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **client inventory** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s10/`

## M8-S11 — lease observability

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **lease observability** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s11/`

## M8-S12 — traffic counters

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **traffic counters** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s12/`

## M8-S13 — optional guest network/VLAN

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional guest network/vlan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s13/`

## M8-S14 — Wi-Fi Calling compatibility profile and diagnostics

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **wi-fi calling compatibility profile and diagnostics** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s14/`

## M8-S15 — management-LAN isolation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **management-lan isolation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m8-s15/`



---


# M9 — Heterogeneous Multi-WAN

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **heterogeneous multi-wan** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M9-S1 — Normalized WAN abstraction

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **normalized wan abstraction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s1/`

## M9-S2 — cellular WAN

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cellular wan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s2/`

## M9-S3 — second cellular WAN

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **second cellular wan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s3/`

## M9-S4 — Ethernet upstream

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ethernet upstream** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s4/`

## M9-S5 — Wi-Fi-as-WAN

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **wi-fi-as-wan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s5/`

## M9-S6 — phone USB tether

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **phone usb tether** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s6/`

## M9-S7 — future satellite WAN

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **future satellite wan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s7/`

## M9-S8 — link/gateway/DNS/Internet/captive health model

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **link/gateway/dns/internet/captive health model** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s8/`

## M9-S9 — failover

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **failover** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s9/`

## M9-S10 — failback

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **failback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s10/`

## M9-S11 — hysteresis

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **hysteresis** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s11/`

## M9-S12 — manual priority

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manual priority** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s12/`

## M9-S13 — latency policy

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **latency policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s13/`

## M9-S14 — reliability policy

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **reliability policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s14/`

## M9-S15 — metered/cost policy

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **metered/cost policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s15/`

## M9-S16 — per-traffic routing

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **per-traffic routing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s16/`

## M9-S17 — captive-portal detection

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **captive-portal detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m9-s17/`



---


# M10 — Security and Trust Boundaries

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **security and trust boundaries** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M10-S1 — Secret storage

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **secret storage** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s1/`

## M10-S2 — redaction

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **redaction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s2/`

## M10-S3 — local authentication

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **local authentication** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s3/`

## M10-S4 — viewer/operator/admin roles

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **viewer/operator/admin roles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s4/`

## M10-S5 — CSRF/origin protection

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **csrf/origin protection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s5/`

## M10-S6 — API authorization

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **api authorization** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s6/`

## M10-S7 — rate limiting

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rate limiting** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s7/`

## M10-S8 — least privilege

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **least privilege** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s8/`

## M10-S9 — default-no-WAN-management exposure

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **default-no-wan-management exposure** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s9/`

## M10-S10 — VPN-first remote access

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **vpn-first remote access** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s10/`

## M10-S11 — audit log

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **audit log** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s11/`

## M10-S12 — secure cookies/session policy

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **secure cookies/session policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s12/`

## M10-S13 — dependency/security scanning

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dependency/security scanning** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s13/`

## M10-S14 — signed releases

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **signed releases** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s14/`

## M10-S15 — safe debug mode

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **safe debug mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m10-s15/`



---


# M11 — Rotator Protocol and Physical Safety

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **rotator protocol and physical safety** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M11-S1 — Versioned Pi-to-MCU protocol

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **versioned pi-to-mcu protocol** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s1/`

## M11-S2 — RP2040 reference controller

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rp2040 reference controller** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s2/`

## M11-S3 — heartbeat

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **heartbeat** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s3/`

## M11-S4 — safe host-loss behavior

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **safe host-loss behavior** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s4/`

## M11-S5 — physical limit switches

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **physical limit switches** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s5/`

## M11-S6 — software limits

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **software limits** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s6/`

## M11-S7 — manual move

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manual move** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s7/`

## M11-S8 — absolute/relative bearing

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **absolute/relative bearing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s8/`

## M11-S9 — home/reference

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **home/reference** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s9/`

## M11-S10 — encoder feedback

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **encoder feedback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s10/`

## M11-S11 — cable-wrap envelope

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cable-wrap envelope** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s11/`

## M11-S12 — stall detection

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **stall detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s12/`

## M11-S13 — optional motor current sensing

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional motor current sensing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s13/`

## M11-S14 — emergency stop

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **emergency stop** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s14/`

## M11-S15 — fault latch/recovery

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **fault latch/recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s15/`

## M11-S16 — firmware identity

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **firmware identity** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s16/`

## M11-S17 — firmware update/recovery procedure

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **firmware update/recovery procedure** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m11-s17/`



---


# M12 — Automated Antenna Hunting

**Phase priority:** ADVANCED  
**Proof posture:** P1/P2/P4 plus feature-specific HIL/field proof; no default enablement without recovery proof.

## Phase Myth

Establish **automated antenna hunting** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M12-S1 — Safe sweep planning

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **safe sweep planning** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s1/`

## M12-S2 — mechanical settle time

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mechanical settle time** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s2/`

## M12-S3 — fresh-metric synchronization

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **fresh-metric synchronization** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s3/`

## M12-S4 — coarse azimuth sweep

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **coarse azimuth sweep** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s4/`

## M12-S5 — candidate ranking

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **candidate ranking** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s5/`

## M12-S6 — fine sweep

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **fine sweep** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s6/`

## M12-S7 — return-to-winner

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **return-to-winner** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s7/`

## M12-S8 — post-move verification

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **post-move verification** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s8/`

## M12-S9 — fallback to next candidate

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **fallback to next candidate** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s9/`

## M12-S10 — movement hysteresis

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **movement hysteresis** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s10/`

## M12-S11 — traffic-aware scan deferral

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **traffic-aware scan deferral** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s11/`

## M12-S12 — multi-objective aiming

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multi-objective aiming** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s12/`

## M12-S13 — band-aware bearing memory

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **band-aware bearing memory** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s13/`

## M12-S14 — carrier-aware bearing memory

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **carrier-aware bearing memory** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s14/`

## M12-S15 — bounded continuous micro-tracking research

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bounded continuous micro-tracking research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m12-s15/`



---


# M13 — Power Solar and Thermal Intelligence

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **power solar and thermal intelligence** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M13-S1 — Battery voltage/current/power/SOC abstraction

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **battery voltage/current/power/soc abstraction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s1/`

## M13-S2 — charging state

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **charging state** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s2/`

## M13-S3 — solar input where measurable

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **solar input where measurable** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s3/`

## M13-S4 — CPU temperature/throttling

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cpu temperature/throttling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s4/`

## M13-S5 — modem temperature

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem temperature** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s5/`

## M13-S6 — motor-energy awareness

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **motor-energy awareness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s6/`

## M13-S7 — full/balanced/low-power/reserve/emergency modes

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **full/balanced/low-power/reserve/emergency modes** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s7/`

## M13-S8 — threshold hysteresis

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **threshold hysteresis** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s8/`

## M13-S9 — action energy accounting

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **action energy accounting** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s9/`

## M13-S10 — solar-aware scheduling

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **solar-aware scheduling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s10/`

## M13-S11 — thermal throttling policy

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **thermal throttling policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s11/`

## M13-S12 — low-voltage handling

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **low-voltage handling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s12/`

## M13-S13 — runtime forecasting research

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **runtime forecasting research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m13-s13/`



---


# M14 — Watchdogs Recovery and Self-Repair

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **watchdogs recovery and self-repair** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M14-S1 — Service watchdog

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **service watchdog** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s1/`

## M14-S2 — modem disappearance recovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem disappearance recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s2/`

## M14-S3 — no-Internet recovery ladder

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **no-internet recovery ladder** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s3/`

## M14-S4 — DNS-specific recovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dns-specific recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s4/`

## M14-S5 — route recovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **route recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s5/`

## M14-S6 — bearer recovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bearer recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s6/`

## M14-S7 — modem reset escalation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem reset escalation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s7/`

## M14-S8 — alternate-WAN escalation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **alternate-wan escalation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s8/`

## M14-S9 — database integrity recovery

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **database integrity recovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s9/`

## M14-S10 — rotator fault isolation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rotator fault isolation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s10/`

## M14-S11 — boot self-test

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **boot self-test** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s11/`

## M14-S12 — safe-mode boot

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **safe-mode boot** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s12/`

## M14-S13 — known-good configuration rollback

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **known-good configuration rollback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s13/`

## M14-S14 — crash-loop detection

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **crash-loop detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s14/`

## M14-S15 — recovery event history

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **recovery event history** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s15/`

## M14-S16 — rescue command

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rescue command** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m14-s16/`



---


# M15 — CLI REST API and Dashboard

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **cli rest api and dashboard** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M15-S1 — Human CLI

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **human cli** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s1/`

## M15-S2 — JSON CLI

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **json cli** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s2/`

## M15-S3 — status

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **status** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s3/`

## M15-S4 — modem

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s4/`

## M15-S5 — signal

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **signal** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s5/`

## M15-S6 — scan

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **scan** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s6/`

## M15-S7 — network

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **network** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s7/`

## M15-S8 — GPS

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **gps** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s8/`

## M15-S9 — history

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **history** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s9/`

## M15-S10 — antenna

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **antenna** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s10/`

## M15-S11 — power

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **power** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s11/`

## M15-S12 — diagnostics

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **diagnostics** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s12/`

## M15-S13 — REST API v1

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rest api v1** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s13/`

## M15-S14 — schema/OpenAPI

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **schema/openapi** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s14/`

## M15-S15 — live event stream

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **live event stream** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s15/`

## M15-S16 — dashboard overview

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dashboard overview** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s16/`

## M15-S17 — scan workflow

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **scan workflow** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s17/`

## M15-S18 — historical charts

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **historical charts** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s18/`

## M15-S19 — configuration editor

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **configuration editor** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s19/`

## M15-S20 — risky-change confirmation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **risky-change confirmation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s20/`

## M15-S21 — mobile-first layout

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mobile-first layout** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s21/`

## M15-S22 — optional offline PWA shell

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional offline pwa shell** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m15-s22/`



---


# M16 — Advanced Cellular Policy

**Phase priority:** ADVANCED  
**Proof posture:** P1/P2/P4 plus feature-specific HIL/field proof; no default enablement without recovery proof.

## Phase Myth

Establish **advanced cellular policy** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M16-S1 — Auto/LTE-only/5G-preferred/5G-only policy where supported

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **auto/lte-only/5g-preferred/5g-only policy where supported** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s1/`

## M16-S2 — NSA/SA preference

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **nsa/sa preference** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s2/`

## M16-S3 — optional band preference and locking

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional band preference and locking** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s3/`

## M16-S4 — safe reset-to-auto

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **safe reset-to-auto** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s4/`

## M16-S5 — manual carrier selection

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manual carrier selection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s5/`

## M16-S6 — carrier aggregation-aware scoring

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **carrier aggregation-aware scoring** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s6/`

## M16-S7 — MTU/MSS diagnostics

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mtu/mss diagnostics** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s7/`

## M16-S8 — modem firmware capability flags

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem firmware capability flags** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s8/`

## M16-S9 — experimental vendor tuning profiles

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **experimental vendor tuning profiles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s9/`

## M16-S10 — guardrails and rollback

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **guardrails and rollback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m16-s10/`



---


# M17 — Carrier Diversity Cost and Data Budgets

**Phase priority:** ADVANCED  
**Proof posture:** P1/P2/P4 plus feature-specific HIL/field proof; no default enablement without recovery proof.

## Phase Myth

Establish **carrier diversity cost and data budgets** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M17-S1 — Per-WAN cost metadata

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **per-wan cost metadata** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s1/`

## M17-S2 — metered/unmetered state

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **metered/unmetered state** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s2/`

## M17-S3 — daily/monthly byte budgets

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **daily/monthly byte budgets** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s3/`

## M17-S4 — soft/hard thresholds

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **soft/hard thresholds** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s4/`

## M17-S5 — roaming cost class

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **roaming cost class** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s5/`

## M17-S6 — carrier diversity score

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **carrier diversity score** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s6/`

## M17-S7 — SIM profile catalog

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sim profile catalog** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s7/`

## M17-S8 — manual save-data mode

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manual save-data mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s8/`

## M17-S9 — speed-test suppression

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **speed-test suppression** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s9/`

## M17-S10 — optional client traffic budget

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional client traffic budget** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s10/`

## M17-S11 — predictive data-budget forecasting

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **predictive data-budget forecasting** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s11/`

## M17-S12 — policy explanations

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **policy explanations** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m17-s12/`



---


# M18 — Mapping Route Surveys and Site Intelligence

**Phase priority:** ADVANCED  
**Proof posture:** P1/P2/P4 plus feature-specific HIL/field proof; no default enablement without recovery proof.

## Phase Myth

Establish **mapping route surveys and site intelligence** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M18-S1 — GeoJSON

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **geojson** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s1/`

## M18-S2 — offline-friendly map

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **offline-friendly map** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s2/`

## M18-S3 — site pins

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **site pins** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s3/`

## M18-S4 — route-survey mode

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **route-survey mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s4/`

## M18-S5 — GPS/RF logging while moving

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **gps/rf logging while moving** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s5/`

## M18-S6 — coverage heatmap

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **coverage heatmap** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s6/`

## M18-S7 — measured-vs-estimated distinction

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **measured-vs-estimated distinction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s7/`

## M18-S8 — site comparison

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **site comparison** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s8/`

## M18-S9 — time-of-day congestion patterns

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **time-of-day congestion patterns** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s9/`

## M18-S10 — terrain/context annotations

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **terrain/context annotations** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s10/`

## M18-S11 — antenna height comparisons

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **antenna height comparisons** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s11/`

## M18-S12 — offline elevation/terrain hints

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **offline elevation/terrain hints** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s12/`

## M18-S13 — privacy controls

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **privacy controls** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s13/`

## M18-S14 — route export

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **route export** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m18-s14/`



---


# M19 — Distributed Bifröst and Remote Radio Head

**Phase priority:** ADVANCED  
**Proof posture:** P1/P2/P4 plus feature-specific HIL/field proof; no default enablement without recovery proof.

## Phase Myth

Establish **distributed bifröst and remote radio head** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M19-S1 — Node identity

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **node identity** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s1/`

## M19-S2 — radio-head/router/controller/observer roles

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **radio-head/router/controller/observer roles** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s2/`

## M19-S3 — secure encrypted node RPC

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **secure encrypted node rpc** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s3/`

## M19-S4 — capability negotiation

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **capability negotiation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s4/`

## M19-S5 — remote modem telemetry

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote modem telemetry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s5/`

## M19-S6 — remote WAN handoff over Ethernet

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote wan handoff over ethernet** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s6/`

## M19-S7 — control/data-link health

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **control/data-link health** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s7/`

## M19-S8 — link-loss behavior

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **link-loss behavior** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s8/`

## M19-S9 — remote power/brownout telemetry

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote power/brownout telemetry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s9/`

## M19-S10 — remote updates

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote updates** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s10/`

## M19-S11 — staged rollback

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **staged rollback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s11/`

## M19-S12 — multiple remote-radio-head research

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multiple remote-radio-head research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s12/`

## M19-S13 — node discovery

**Priority:** ADVANCED  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **node discovery** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m19-s13/`



---


# M20 — Satellite and Non-Cellular Expansion

**Phase priority:** FUTURE  
**Proof posture:** Define interfaces and non-goals now; implementation proof is not required until promoted.

## Phase Myth

Establish **satellite and non-cellular expansion** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M20-S1 — Satellite WAN abstraction

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **satellite wan abstraction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s1/`

## M20-S2 — high-latency health profile

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **high-latency health profile** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s2/`

## M20-S3 — metered/high-energy metadata

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **metered/high-energy metadata** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s3/`

## M20-S4 — cellular-before-satellite policy

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cellular-before-satellite policy** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s4/`

## M20-S5 — emergency satellite override

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **emergency satellite override** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s5/`

## M20-S6 — Ethernet satellite terminals

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **ethernet satellite terminals** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s6/`

## M20-S7 — Wi-Fi satellite terminals

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **wi-fi satellite terminals** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s7/`

## M20-S8 — multi-path cellular-plus-satellite research

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multi-path cellular-plus-satellite research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s8/`

## M20-S9 — outage representation when terrestrial RF is absent

**Priority:** FUTURE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **outage representation when terrestrial rf is absent** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m20-s9/`



---


# M21 — Remote Administration and Multi-Node Operations

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **remote administration and multi-node operations** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M21-S1 — Tailscale/overlay awareness

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **tailscale/overlay awareness** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s1/`

## M21-S2 — remote diagnostics bundle

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote diagnostics bundle** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s2/`

## M21-S3 — sanitized logs

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sanitized logs** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s3/`

## M21-S4 — remote read-only dashboard

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote read-only dashboard** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s4/`

## M21-S5 — guarded remote actions

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **guarded remote actions** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s5/`

## M21-S6 — multiple-node dashboard

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multiple-node dashboard** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s6/`

## M21-S7 — remote proof retrieval

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote proof retrieval** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s7/`

## M21-S8 — store-and-forward telemetry sync

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **store-and-forward telemetry sync** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s8/`

## M21-S9 — offline queue

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **offline queue** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s9/`

## M21-S10 — reconnect deduplication

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **reconnect deduplication** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s10/`

## M21-S11 — remote health checks

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **remote health checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s11/`

## M21-S12 — optional notification hooks

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional notification hooks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m21-s12/`



---


# M22 — Deployment Updates Backup and Rescue

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **deployment updates backup and rescue** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M22-S1 — Native installer

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **native installer** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s1/`

## M22-S2 — systemd service units

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **systemd service units** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s2/`

## M22-S3 — dependency checks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dependency checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s3/`

## M22-S4 — idempotent install

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **idempotent install** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s4/`

## M22-S5 — clean uninstall

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **clean uninstall** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s5/`

## M22-S6 — configuration backup

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **configuration backup** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s6/`

## M22-S7 — database backup

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **database backup** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s7/`

## M22-S8 — migration dry-run

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **migration dry-run** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s8/`

## M22-S9 — application upgrade

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **application upgrade** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s9/`

## M22-S10 — rollback

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rollback** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s10/`

## M22-S11 — A/B or previous-version recovery where feasible

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **a/b or previous-version recovery where feasible** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s11/`

## M22-S12 — offline update bundle

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **offline update bundle** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s12/`

## M22-S13 — signature/checksum verification

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **signature/checksum verification** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s13/`

## M22-S14 — rescue mode

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rescue mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s14/`

## M22-S15 — clean factory-ish software reset without erasing proof archives unless requested

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **clean factory-ish software reset without erasing proof archives unless requested** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m22-s15/`



---


# M23 — Diagnostics and Deep Observability

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **diagnostics and deep observability** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M23-S1 — Unified health report

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **unified health report** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s1/`

## M23-S2 — correlated timeline

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **correlated timeline** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s2/`

## M23-S3 — bounded packet-capture helper

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **bounded packet-capture helper** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s3/`

## M23-S4 — privacy-safe modem diagnostics

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **privacy-safe modem diagnostics** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s4/`

## M23-S5 — before/after RF report

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **before/after rf report** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s5/`

## M23-S6 — standardized benchmark

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **standardized benchmark** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s6/`

## M23-S7 — performance regression tracking

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **performance regression tracking** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s7/`

## M23-S8 — resource telemetry

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **resource telemetry** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s8/`

## M23-S9 — file-descriptor/memory leak checks

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **file-descriptor/memory leak checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s9/`

## M23-S10 — database growth checks

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **database growth checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s10/`

## M23-S11 — network namespace lab

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **network namespace lab** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s11/`

## M23-S12 — support bundle

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **support bundle** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s12/`

## M23-S13 — human-readable failure explanations

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **human-readable failure explanations** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m23-s13/`



---


# M24 — Optional AI Seer Layer

**Phase priority:** EXPERIMENTAL  
**Proof posture:** Benchmark against a simpler baseline; document resource cost, failure mode, false positives, and a clean disable path.

## Phase Myth

Establish **optional ai seer layer** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M24-S1 — Read-only sanitized AI context

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **read-only sanitized ai context** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s1/`

## M24-S2 — natural-language status explanation grounded in metrics

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **natural-language status explanation grounded in metrics** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s2/`

## M24-S3 — recommendation generator

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **recommendation generator** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s3/`

## M24-S4 — historical pattern summarizer

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **historical pattern summarizer** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s4/`

## M24-S5 — troubleshooting assistant

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **troubleshooting assistant** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s5/`

## M24-S6 — capability-aware safety filter

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **capability-aware safety filter** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s6/`

## M24-S7 — local-model adapter

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **local-model adapter** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s7/`

## M24-S8 — backend-neutral model interface

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **backend-neutral model interface** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s8/`

## M24-S9 — optional local STT/TTS voice interface

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **optional local stt/tts voice interface** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s9/`

## M24-S10 — explicit confirmation for any future gated action

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **explicit confirmation for any future gated action** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s10/`

## M24-S11 — no AI ownership of safety-critical actuation

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **no ai ownership of safety-critical actuation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m24-s11/`



---


# M25 — Predictive Radio Intelligence

**Phase priority:** EXPERIMENTAL  
**Proof posture:** Benchmark against a simpler baseline; document resource cost, failure mode, false positives, and a clean disable path.

## Phase Myth

Establish **predictive radio intelligence** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M25-S1 — Connection forecasting

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **connection forecasting** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s1/`

## M25-S2 — best-bearing prediction

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **best-bearing prediction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s2/`

## M25-S3 — tower/cell behavior modeling

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **tower/cell behavior modeling** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s3/`

## M25-S4 — time-of-day performance prediction

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **time-of-day performance prediction** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s4/`

## M25-S5 — deployment recommendation

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **deployment recommendation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s5/`

## M25-S6 — mast-height experiment suggestion

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mast-height experiment suggestion** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s6/`

## M25-S7 — contextual band-policy research

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **contextual band-policy research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s7/`

## M25-S8 — anomaly detection

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **anomaly detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s8/`

## M25-S9 — thermal anomaly detection

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **thermal anomaly detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s9/`

## M25-S10 — modem-reset anomaly detection

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **modem-reset anomaly detection** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s10/`

## M25-S11 — held-out evaluation

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **held-out evaluation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s11/`

## M25-S12 — naive-baseline comparison

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **naive-baseline comparison** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s12/`

## M25-S13 — confidence calibration

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **confidence calibration** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m25-s13/`



---


# M26 — Experimental Networking Laboratory

**Phase priority:** EXPERIMENTAL  
**Proof posture:** Benchmark against a simpler baseline; document resource cost, failure mode, false positives, and a clean disable path.

## Phase Myth

Establish **experimental networking laboratory** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M26-S1 — MPTCP research

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **mptcp research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s1/`

## M26-S2 — VPN bonding research

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **vpn bonding research** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s2/`

## M26-S3 — cellular multi-path

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **cellular multi-path** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s3/`

## M26-S4 — satellite multi-path

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **satellite multi-path** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s4/`

## M26-S5 — SQM/CAKE/fq_codel bufferbloat control

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **sqm/cake/fq_codel bufferbloat control** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s5/`

## M26-S6 — emergency traffic reservation

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **emergency traffic reservation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s6/`

## M26-S7 — application-aware queueing

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **application-aware queueing** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s7/`

## M26-S8 — opportunistic safe cache

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **opportunistic safe cache** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s8/`

## M26-S9 — delay-tolerant/store-and-forward jobs

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **delay-tolerant/store-and-forward jobs** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s9/`

## M26-S10 — multi-interface DNS experiments

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **multi-interface dns experiments** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s10/`

## M26-S11 — explicit experimental feature flags and revert paths

**Priority:** EXPERIMENTAL  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **explicit experimental feature flags and revert paths** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m26-s11/`



---


# M27 — Field Operating Modes

**Phase priority:** IMPORTANT  
**Proof posture:** P1/P2 required; field-facing behavior normally needs P3/P4/P5 and often P6.

## Phase Myth

Establish **field operating modes** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M27-S1 — Station mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **station mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s1/`

## M27-S2 — survey mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **survey mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s2/`

## M27-S3 — auto-aim mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **auto-aim mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s3/`

## M27-S4 — travel mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **travel mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s4/`

## M27-S5 — low-power mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **low-power mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s5/`

## M27-S6 — emergency mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **emergency mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s6/`

## M27-S7 — silent/night mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **silent/night mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s7/`

## M27-S8 — manual-control mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **manual-control mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s8/`

## M27-S9 — known-site fast-start

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **known-site fast-start** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s9/`

## M27-S10 — no-service mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **no-service mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s10/`

## M27-S11 — alternate-WAN-only mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **alternate-wan-only mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s11/`

## M27-S12 — maintenance mode

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **maintenance mode** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s12/`

## M27-S13 — field workflow proofs with real clients

**Priority:** IMPORTANT  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **field workflow proofs with real clients** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m27-s13/`



---


# M28 — CI Simulation HIL and Endurance

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **ci simulation hil and endurance** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M28-S1 — Unit CI

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **unit ci** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s1/`

## M28-S2 — formatting/lint/type checks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **formatting/lint/type checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s2/`

## M28-S3 — schema checks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **schema checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s3/`

## M28-S4 — dependency/license checks

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **dependency/license checks** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s4/`

## M28-S5 — network namespaces

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **network namespaces** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s5/`

## M28-S6 — injected latency/loss/DNS failure

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **injected latency/loss/dns failure** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s6/`

## M28-S7 — recorded modem fixtures

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **recorded modem fixtures** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s7/`

## M28-S8 — HIL runner

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **hil runner** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s8/`

## M28-S9 — GPS fixtures

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **gps fixtures** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s9/`

## M28-S10 — rotator fixtures

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **rotator fixtures** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s10/`

## M28-S11 — power fixtures

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **power fixtures** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s11/`

## M28-S12 — browser automation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **browser automation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s12/`

## M28-S13 — weak-signal lawful lab scenarios

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **weak-signal lawful lab scenarios** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s13/`

## M28-S14 — 24-hour soak

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **24-hour soak** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s14/`

## M28-S15 — 72-hour soak

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **72-hour soak** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s15/`

## M28-S16 — 7-day release soak

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **7-day release soak** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s16/`

## M28-S17 — controlled power-cut resilience

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **controlled power-cut resilience** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m28-s17/`



---


# M29 — Release Gates and Public Proof Manifest

**Phase priority:** CORE  
**Proof posture:** P0/P1/P2 required; add P3/P4/P8/P9 whenever hardware, networking, security, or recovery is claimed.

## Phase Myth

Establish **release gates and public proof manifest** as measured, recoverable behavior. Start with the narrowest useful vertical slice and widen only after its proof is stable.

## Phase Exit Gate

- [ ] Required slices are `[X]`.
- [ ] Proofs name the exact commit and test matrix.
- [ ] Relevant failure/safety proofs exist.
- [ ] Known limitations are explicit.

## M29-S1 — v0.1 developer preview

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.1 developer preview** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s1/`

## M29-S2 — v0.2 real modem proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.2 real modem proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s2/`

## M29-S3 — v0.3 router proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.3 router proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s3/`

## M29-S4 — v0.4 site-memory survey proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.4 site-memory survey proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s4/`

## M29-S5 — v0.5 multi-WAN proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.5 multi-wan proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s5/`

## M29-S6 — v0.6 rotator safety proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.6 rotator safety proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s6/`

## M29-S7 — v0.7 auto-aim proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.7 auto-aim proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s7/`

## M29-S8 — v0.8 field reliability proof

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.8 field reliability proof** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s8/`

## M29-S9 — v0.9 release candidate

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v0.9 release candidate** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s9/`

## M29-S10 — v1.0 public release

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v1.0 public release** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s10/`

## M29-S11 — v1.x hardware breadth

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v1.x hardware breadth** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s11/`

## M29-S12 — v2 research graduation

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **v2 research graduation** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s12/`

## M29-S13 — machine-readable proof manifest

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **machine-readable proof manifest** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s13/`

## M29-S14 — known-limitations ledger

**Priority:** CORE  **Status:** `[ ]`

**Myth:** Prove Bifröst can provide **known-limitations ledger** through its normal architecture, with truthful unavailable/stale states and without backend leakage.

**Implementation:**
- [ ] Define contract, config, events, errors, capability detection, and ownership.
- [ ] Implement normalized behavior plus mock/fixture support; use real hardware when the claim touches hardware.
- [ ] Document dependencies, restart/persistence semantics, resource cost, and security/safety impact.

**Proof gate:**
- [ ] Happy-path test passes through the production-facing interface.
- [ ] At least one realistic dependency/data/fault ordeal is induced and recovery or safe failure is demonstrated.
- [ ] Evidence records commit, platform, hardware/firmware, redacted config, commands, actual result, logs, proof classes, and limits.
- [ ] Another developer can reproduce the claim and the claim is no broader than the tested matrix.

**Evidence:** `proofs/m29-s14/`



---



# 30. Cross-Cutting Proof Scenarios

These scenarios prove whole workflows across phase boundaries.

## FP-1 — Weak Rural Cellular Site

- [ ] boot from cold;
- [ ] discover modem;
- [ ] register normally;
- [ ] acquire IP and DNS;
- [ ] measure RSRP/RSRQ/SINR;
- [ ] route at least a phone and laptop;
- [ ] persist site observation;
- [ ] induce one bearer interruption;
- [ ] recover without host reboot;
- [ ] retain coherent timeline.

**Required:** P2, P3, P4, P5.

## FP-2 — Directional Antenna Survey

- [ ] deploy mast and directional antenna;
- [ ] record mast/antenna metadata;
- [ ] sample multiple bearings;
- [ ] rank candidates;
- [ ] verify selected winner after returning to it;
- [ ] store the full scan;
- [ ] repeat at least once to quantify repeatability.

## FP-3 — Cellular Failure With Backup WAN

- [ ] begin active client traffic;
- [ ] remove primary cellular path;
- [ ] confirm hysteresis and failover;
- [ ] preserve LAN and management access;
- [ ] restore primary;
- [ ] prove configured failback behavior;
- [ ] verify no route thrash.

## FP-4 — No Terrestrial Service

- [ ] represent no-service honestly;
- [ ] do not fabricate metrics;
- [ ] do not enter destructive reconnect loop;
- [ ] use alternate WAN if present;
- [ ] store the no-service observation;
- [ ] continue local dashboard and diagnostics.

## FP-5 — Power-Constrained Night

- [ ] enter low-power/silent mode;
- [ ] preserve basic connectivity;
- [ ] suppress throughput tests;
- [ ] suppress motor movement where policy requires;
- [ ] reduce sampling/UI refresh;
- [ ] recover normal mode after charge threshold plus hysteresis.

## FP-6 — Host Crash During Antenna Motion

- [ ] start legal/safe movement;
- [ ] kill host controller process;
- [ ] MCU detects heartbeat loss;
- [ ] motion stops within stated bound;
- [ ] router remains unaffected;
- [ ] restart requires sane position reconciliation.

## FP-7 — Modem Hotplug Torture

Perform repeated attach/remove cycles while the core remains running. The proof must state the cycle count and show there are no leaked logical modem objects or unrecoverable bearer state.

## FP-8 — DNS Failure Without Cellular Failure

Prove Bifröst identifies DNS as the fault and does not reboot/reset the modem unnecessarily.

## FP-9 — 24-Hour Campsite Soak

Use realistic clients and traffic. Monitor memory, file descriptors, database size, temperatures, modem reconnects, WAN state, and event volume. Require at least one induced fault.

## FP-10 — 72-Hour Release Soak

Same as FP-9 but with automated proof collection and threshold alarms. Required before late release candidates.

## FP-11 — Travel Survey

- [ ] directional movement disabled;
- [ ] GPS/cell transitions recorded;
- [ ] no unsafe driver interaction required;
- [ ] route can be exported after stopping;
- [ ] missing GPS segments are represented as missing.

## FP-12 — Remote Radio Head

Place the modem/radio head near the antenna with short RF leads. Carry digital traffic over the remote-head link. Prove link loss, head reboot, router reboot, and recovery independently.

---

# 31. Negative-Proof Ledger

Bifröst must explicitly prove that:

- [ ] WAN cannot reach management services by default.
- [ ] guest clients cannot reach the management LAN.
- [ ] viewer users cannot change network or physical state.
- [ ] stale RF metrics cannot masquerade as live readings.
- [ ] unsupported modem capabilities are not guessed.
- [ ] a bad APN does not permanently wedge the modem service.
- [ ] a DNS outage does not automatically escalate to destructive modem reset.
- [ ] a dead rotator does not kill Internet routing.
- [ ] a dashboard crash does not kill routing.
- [ ] an invalid bearing cannot defeat MCU limits.
- [ ] host crash cannot leave the rotator moving indefinitely.
- [ ] low-power mode cannot silently start prohibited high-energy operations.
- [ ] a capped throughput test cannot exceed its configured byte/time budget.
- [ ] estimated map coverage cannot be visually confused with measured observations.
- [ ] AI output cannot directly bypass deterministic safety gates.
- [ ] experimental band/modem tuning always has a reset-to-conservative path.
- [ ] failed software updates have a documented recovery path.
- [ ] cloud/Internet loss does not disable local management.

---

# 32. Proof Review Checklist

Before a reviewer promotes any slice to `[X]`, answer:

1. What exact claim is being made?
2. What exact commit was tested?
3. What platform and kernel were used?
4. What hardware and firmware were used?
5. Which proof classes were satisfied?
6. What was deliberately broken?
7. What happened when it broke?
8. Did the system recover automatically, manually, or not at all?
9. Could stale state create a false pass?
10. Could a mock hide a hardware-specific failure?
11. Did the test exercise the same interface used in production?
12. Are secrets absent from evidence?
13. Is physical safety proven where motion/power is involved?
14. Is network exposure proven where firewall/API behavior is involved?
15. Is persistence correct across reboot where relevant?
16. Can another developer reproduce it?
17. Are limitations narrower than the README claim?
18. Did a simpler baseline perform just as well for an experimental feature?
19. What would make this proof invalid after a future change?
20. Is there an automated regression test preserving the discovery?

---

# 33. Architecture Decision Record Requirements

Major choices belong in `docs/adr/`.

Suggested ADRs:

```text
0001-normalized-modem-interface.md
0002-sqlite-local-first-memory.md
0003-native-systemd-deployment.md
0004-mcu-owns-rotator-safety.md
0005-dashboard-does-not-own-routing.md
0006-stale-telemetry-is-explicit.md
0007-ai-is-advisory-only.md
0008-remote-radio-head-architecture.md
0009-multi-wan-health-model.md
0010-proof-manifest-is-release-artifact.md
```

Each ADR must state context, decision, alternatives, consequences, reversal strategy, and proof required.

---

# 34. Experimental Feature Graduation

An EXPERIMENTAL feature may become ADVANCED only if:

- [ ] interface is stable enough to test;
- [ ] feature can be disabled completely;
- [ ] deterministic fallback exists;
- [ ] measurable benefit beats a simpler baseline;
- [ ] false-positive/failure rate is reported;
- [ ] CPU/RAM/data/power cost is measured;
- [ ] security and safety impact are reviewed;
- [ ] field proof exists where relevant.

An ADVANCED feature may become IMPORTANT only after repeated field evidence and endurance testing show that it reduces real operational burden.

No AI/ML feature graduates because its demo looks impressive. It must outperform a deterministic baseline on held-out or field data.

---

# 35. First 30 GitHub Issues

1. Create package skeleton and `bifrost` CLI.
2. Add TOML config loader and typed schema.
3. Add structured logs and secret redaction.
4. Define error taxonomy.
5. Define aggregate `SystemSnapshot`.
6. Define internal event contracts.
7. Define modem backend interface.
8. Implement mock modem backend.
9. Implement ModemManager modem discovery.
10. Implement normalized modem identity.
11. Implement SIM state.
12. Implement registration state.
13. Implement APN profile handling.
14. Implement bearer connect/disconnect.
15. Implement IP/gateway/DNS acquisition.
16. Implement layered Internet health checks.
17. Implement RSRP parser/normalization.
18. Implement RSRQ parser/normalization.
19. Implement SINR parser/normalization.
20. Implement RAT/band/cell state.
21. Implement `bifrost status --json`.
22. Create SQLite schema and migration framework.
23. Persist passive observations.
24. Implement latency and packet-loss scanner.
25. Implement transparent quality score v1.
26. Expose REST `/api/status`.
27. Create systemd service.
28. Build first real-HIL proof command.
29. Route one wired LAN client through cellular.
30. Capture and publish first end-to-end proof bundle.

Issue 31 should be chosen from what real hardware teaches, not from aesthetics.

---

# 36. Release Ladder

## v0.1 — Developer Forge

Required: repository, config, logs, state, mocks, CLI, CI. No broad hardware claims.

## v0.2 — Cellular Spark

Required: real modem detection, SIM/registration, data session, normalized RF telemetry, HIL proof.

## v0.3 — Bridge Router

Required: LAN/Wi-Fi clients through cellular, DHCP/DNS/NAT/firewall, security proof.

## v0.4 — Memory of the Land

Required: GPS, observations, site memory, scoring, survey workflow, export, field proof.

## v0.5 — Many Roads

Required: at least two WAN types, failover/failback, hysteresis, endurance proof.

## v0.6 — Turning Eye

Required: rotator protocol, MCU safety, limits, home, manual positioning, crash/limit proofs.

## v0.7 — Signal Hunter

Required: coarse/fine automatic aiming, winner verification, movement hysteresis, field proof.

## v0.8 — Off-Grid Keeper

Required: power modes, thermal telemetry, recovery ladder, watchdogs, safe mode, 24h field proof.

## v0.9 — Release Candidate

Required: diagnostics, updater/rollback, remote admin, security review, 72h soak, documentation.

## v1.0 — Proven Bridge

Required: all CORE slices and designated field-critical IMPORTANT slices are `[X]`; clean installation and recovery are reproducible; proof manifest is public; limitations are explicit.

## v1.x

Expand modem/platform support, advanced carrier policy, mapping/site intelligence, and multi-carrier breadth.

## v2 Research Track

Distributed radio heads, satellite integration, bounded AI seer, predictive radio intelligence, multi-path networking, and other experiments graduate only after evidence.

---

# 37. Completion Ledger Template

```markdown
## Slice M?-S? — Name

Status: [ ]
Priority:
Owner:
Target release:

### Myth
Human purpose and field problem.

### Rune
Stable interface/schema/protocol/state.

### Hall
Owning subsystem.

### Threads
Dependencies, consumers, events, failure propagation.

### Slice
Smallest end-to-end behavior implemented.

### Trial
Normal test procedure.

### Ordeal
Deliberate failure procedure.

### Proof Classes
- [ ] P0
- [ ] P1
- [ ] P2
- [ ] P3
- [ ] P4
- [ ] P5
- [ ] P6
- [ ] P7
- [ ] P8
- [ ] P9

### Evidence
Commit:
Proof directory:
Platform:
Hardware:
Firmware:
Date:

### Known Limits
Explicitly unproven cases.

### Inscription
- [ ] docs updated
- [ ] changelog updated
- [ ] proof manifest updated
- [ ] roadmap promoted
```

---

# 38. Final Law of the Forge

Bifröst must never confuse:

- implemented with proven;
- proven once with reliable;
- reliable on one device with portable;
- a modem capability with a Bifröst-supported capability;
- an estimate with a measurement;
- a recommendation with safe permission to act;
- an attractive experiment with a production feature.

Every important claim must be able to answer one question:

> **Where is the proof?**

If the repository cannot point to it, the rune remains unfinished.

> Build vertically. Measure reality. Fail deliberately. Recover cleanly. Preserve evidence. Let experiments earn their place.

**Find the signal. Forge the path. Prove the bridge.**
