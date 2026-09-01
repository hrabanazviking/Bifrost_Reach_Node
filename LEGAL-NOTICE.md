# Legal Notice and Distribution Position

**Project:** Bifröst Reach Node
**Author:** Volmarr Wyrd
**Effective:** 2026-09-01

Bifröst Reach Node is an open-source software and open-hardware project.

Unless otherwise stated:

* Bifröst software is licensed under the **Apache License, Version 2.0**.
* Bifröst open-hardware design materials are licensed under the **CERN Open Hardware Licence Version 2 — Strongly Reciprocal (`CERN-OHL-S-2.0`)**.

Nothing in this notice modifies, replaces, narrows, expands, supersedes, or overrides either license.

The authoritative licensing terms are contained in:

```text
LICENSE
LICENSES/Apache-2.0.txt
LICENSES/CERN-OHL-S-2.0.txt
NOTICE
THIRD_PARTY_NOTICES.md
```

This document describes the author's distribution position, project policies, intended use boundaries, and certain practical legal and safety considerations surrounding the Bifröst Reach Node project.

It is not itself a software or hardware license.

---

# 1. Source Code and Open-Hardware Publication

This repository is provided as a publication of source code, documentation, engineering designs, experimental concepts, reference architectures, and open-hardware materials.

Depending on the stage of development, repository materials may include:

* source code
* scripts
* firmware
* configuration examples
* system-service definitions
* networking configurations
* API definitions
* hardware schematics
* wiring diagrams
* PCB designs
* CAD designs
* enclosure concepts
* bills of materials
* antenna-system concepts
* mast designs
* antenna-rotator designs
* testing procedures
* research notes
* engineering roadmaps
* experimental features
* diagrams
* documentation

Publication of a design or concept does not constitute a representation that the design has been manufactured, certified, field-tested, verified for every environment, or approved by any governmental, telecommunications, carrier, safety, or standards organization.

The project's proof-based engineering documentation should be consulted to determine which capabilities have actually been implemented and verified.

---

# 2. No Personal-Information Gatekeeping

The author does not require users to provide age information, birth dates, government identification, biometric identifiers, or comparable personally identifying information merely to:

* obtain the source code,
* read the documentation,
* inspect the hardware designs,
* clone the repository,
* build the project,
* study the project, or
* modify the materials published through this repository.

The author does not intend access to the publicly published source and design materials to depend upon identity-verification systems.

---

# 3. Official Distribution Position

The author may choose whether, when, where, and in what form official Bifröst Reach Node distributions are provided.

Possible distribution forms may include:

* source-code releases
* packaged software
* binary releases
* Raspberry Pi images
* firmware images
* installation scripts
* hardware kits
* PCB manufacturing files
* assembled hardware
* hosted services
* mobile applications
* operating-system packages
* remote-management services

Publication of source code and hardware designs does not constitute a promise that any particular packaged form will be made available.

The author may decline to provide an official distribution channel where doing so would require identity verification, invasive personal-data collection, impractical regulatory obligations, platform restrictions, or other conditions inconsistent with the project's distribution philosophy.

---

# 4. Independent Third-Party Responsibility

Any third party who:

* forks,
* modifies,
* manufactures,
* assembles,
* packages,
* redistributes,
* sells,
* hosts,
* deploys,
* installs,
* integrates,
* republishes,
* bundles, or
* otherwise makes use of

Bifröst Reach Node software or hardware designs acts independently.

Such parties are responsible for determining and satisfying the laws, regulations, certifications, licenses, permits, platform policies, carrier requirements, safety requirements, and distribution obligations applicable to their own activities.

The author does not assume responsibility for:

* third-party manufacturing,
* third-party hardware modifications,
* third-party radio configurations,
* third-party packaging,
* third-party hosting,
* third-party deployments,
* third-party certification,
* third-party carrier agreements,
* third-party regulatory compliance,
* third-party privacy practices,
* third-party telemetry collection,
* third-party commercial products, or
* third-party representations concerning Bifröst.

---

# 5. Radio and Telecommunications Notice

Bifröst Reach Node is intended to improve communications primarily through lawful networking and radio-system engineering techniques such as:

* external antennas,
* directional antenna gain,
* MIMO antenna systems,
* improved antenna placement,
* increased antenna elevation,
* commercially manufactured cellular modems,
* intelligent signal measurement,
* network selection,
* connection management,
* multi-WAN routing,
* failover,
* local Wi-Fi,
* Ethernet,
* and related data-network technologies.

The project does not grant authorization to transmit on frequencies for which the user is not authorized.

The project does not grant authority to operate:

* unauthorized cellular repeaters,
* unlawful RF amplifiers,
* unapproved cellular base stations,
* unlawful jamming equipment,
* transmitters outside applicable authorization,
* or equipment configured in violation of applicable telecommunications requirements.

Users are responsible for determining whether their particular radio hardware and operating configuration are lawful where they operate it.

---

# 6. Cellular Booster Distinction

Bifröst Reach Node should not automatically be interpreted as a conventional bidirectional cellular signal booster.

The primary Bifröst architecture uses a commercially manufactured cellular modem connected to suitable antennas and then redistributes the resulting **data connection** through ordinary network technologies such as:

```text
Cellular network
      ↓
Certified/commercial cellular modem
      ↓
Bifröst networking software
      ↓
Wi-Fi / Ethernet / VPN
      ↓
User devices
```

This is fundamentally different from taking cellular RF received from a carrier and directly retransmitting amplified cellular RF toward nearby phones.

Optional documentation may discuss integration with commercially manufactured cellular booster equipment.

Where such equipment is used, responsibility for lawful selection, registration, installation, certification, operation, and carrier compliance remains with the equipment owner or operator.

---

# 7. No Authorization to Interfere With Networks

Nothing in Bifröst Reach Node authorizes interference with:

* cellular networks,
* Wi-Fi networks,
* satellite networks,
* emergency communications,
* public-safety systems,
* licensed radio services,
* private networks,
* or other communications systems.

Bifröst is intended to **find and use available connectivity more intelligently**, not degrade connectivity available to others.

---

# 8. Carrier and Network Independence

Bifröst Reach Node is an independent open-source project.

References to cellular carriers, network operators, equipment manufacturers, satellite providers, or other telecommunications companies are generally descriptive.

Unless expressly stated otherwise, Bifröst Reach Node is not sponsored, approved, certified, operated, or endorsed by any:

* cellular carrier,
* Internet provider,
* satellite provider,
* modem manufacturer,
* antenna manufacturer,
* Raspberry Pi manufacturer,
* network-equipment manufacturer,
* governmental telecommunications authority,
* or standards organization.

Carrier names and trademarks remain the property of their respective owners.

---

# 9. Hardware Manufacturer Independence

Project documentation may reference compatible or potentially compatible hardware manufactured by companies such as modem, antenna, SBC, networking, GNSS, power-system, and electronics manufacturers.

Such references are made for technical identification and interoperability purposes.

They do not imply sponsorship or endorsement.

Bifröst does not relicense manufacturer firmware, proprietary hardware designs, trademarks, documentation, or other intellectual property merely by supporting or discussing a manufacturer's product.

---

# 10. Open Hardware Does Not Mean Certified Hardware

The availability of hardware source designs under an open-hardware license does not itself establish regulatory or safety certification.

A person or organization manufacturing Bifröst-derived hardware may have independent obligations relating to matters such as:

* radio certification,
* electromagnetic compatibility,
* electrical safety,
* product safety,
* environmental requirements,
* labeling,
* consumer-product regulations,
* import requirements,
* commercial distribution,
* or telecommunications equipment.

Those obligations may differ substantially by jurisdiction and deployment.

---

# 11. Antenna and Mast Safety

Bifröst may use elevated antennas, directional antennas, telescoping poles, portable masts, permanent masts, guy lines, mounting hardware, or motorized antenna systems.

These structures can create serious hazards if improperly installed.

Users are responsible for appropriate precautions involving:

* overhead electrical lines,
* lightning,
* grounding,
* wind,
* structural loading,
* guy-line placement,
* falling equipment,
* vehicle traffic,
* pedestrians,
* trees,
* ice,
* snow,
* unstable terrain,
* mast collapse,
* moving rotator mechanisms,
* and local installation requirements.

**Never place or erect an antenna, pole, mast, guy line, or conductive structure where it could contact or fall into an electrical power line.**

Mechanical safety systems should not rely exclusively on high-level Raspberry Pi software.

Where motorized antenna positioning is used, independent limit switches, controllers, stops, watchdogs, or equivalent safeguards are strongly recommended.

---

# 12. Lightning and Weather

Outdoor antennas and elevated structures may increase exposure to lightning and severe-weather hazards.

No Bifröst design should be interpreted as creating immunity from:

* lightning strikes,
* electrical surges,
* static discharge,
* rain,
* flooding,
* condensation,
* snow,
* ice,
* heat,
* wind,
* or other environmental hazards.

Users must determine suitable grounding, surge protection, weatherproofing, installation practices, and shutdown procedures for their installation.

---

# 13. Battery, Solar, and Electrical Safety

Bifröst deployments may use:

* USB power banks,
* DC power systems,
* lithium batteries,
* LiFePO4 batteries,
* solar panels,
* charge controllers,
* DC-DC converters,
* USB Power Delivery,
* vehicle electrical systems,
* or other energy sources.

Electrical and battery systems can create hazards including:

* fire,
* overheating,
* short circuits,
* burns,
* battery damage,
* equipment damage,
* electrical shock,
* and release of hazardous energy.

Users are responsible for appropriate:

* fusing,
* wire sizing,
* connectors,
* insulation,
* polarity,
* voltage regulation,
* charge control,
* battery protection,
* thermal management,
* and enclosure design.

Software monitoring is not a substitute for appropriate electrical protection.

---

# 14. Motorized Hardware Safety

Future Bifröst configurations may include motorized antenna rotators or other electromechanical systems.

Software bugs, communication failures, mechanical jams, sensor failures, power faults, or configuration errors may cause unexpected motion.

For this reason, the intended architecture places critical physical safety protections below the primary Bifröst application layer.

Examples include:

* physical stops,
* limit switches,
* current limits,
* independent microcontroller safeguards,
* watchdogs,
* emergency-stop systems,
* movement envelopes,
* and cable-wrap limits.

No software control interface should be treated as the sole physical safety mechanism.

---

# 15. Experimental Features

Bifröst Reach Node intentionally includes research, advanced, optional, and experimental concepts.

Project documentation may discuss capabilities that:

* have not yet been implemented,
* have been implemented only as prototypes,
* have not been field-tested,
* depend on particular hardware,
* depend on particular carriers,
* depend on external services,
* may later be abandoned,
* or may never become production features.

Terms such as:

```text
PROPOSED
OPTIONAL
ADVANCED
EXPERIMENTAL
FUTURE
```

should be interpreted accordingly.

Only features supported by appropriate project proof records should be regarded as verified Bifröst capabilities.

---

# 16. Proof-Based Engineering Claims

Bifröst uses a proof-oriented Mythic Engineering development process.

The project attempts to distinguish among:

* proposed,
* implemented,
* tested,
* hardware-tested,
* field-tested,
* endurance-tested,
* cross-platform verified,
* and release-qualified

features.

A design described in documentation should not automatically be assumed to have passed every level of proof.

Users making safety-critical, commercial, emergency, or infrastructure decisions should independently validate the relevant configuration.

---

# 17. No Guarantee of Connectivity

Bifröst cannot create terrestrial cellular service where no usable carrier signal exists.

Performance can be affected by factors outside the project's control, including:

* terrain,
* distance,
* vegetation,
* buildings,
* antenna placement,
* weather,
* radio interference,
* tower congestion,
* carrier configuration,
* frequency bands,
* modem capability,
* SIM provisioning,
* account restrictions,
* network outages,
* tower maintenance,
* backhaul limitations,
* and regulatory restrictions.

No specific range, bandwidth, latency, reliability, or signal improvement is guaranteed.

---

# 18. Emergency and Disaster Use

Bifröst may prove useful for:

* rural communications,
* remote camps,
* nomadic travel,
* emergency preparedness,
* disaster recovery,
* temporary deployments,
* field research,
* and resilient networking.

However, Bifröst Reach Node is **not represented as a certified emergency communications system, life-safety system, medical system, public-safety radio system, or guaranteed emergency service**.

Users should not assume that Bifröst will remain functional during an emergency.

Where life or safety depends upon communications, appropriate redundant and professionally suitable communications systems should be considered.

---

# 19. GPS and Location Data

Bifröst may optionally collect or store location information for features such as:

* site memory,
* signal surveys,
* route surveys,
* tower observations,
* antenna-bearing history,
* mapping,
* or network-performance analysis.

The reference project philosophy is **local first**.

Location information should remain local unless the operator deliberately configures an export, synchronization, remote-management, sharing, or external-service function.

A third-party fork or deployment may behave differently.

Operators who collect location data belonging to other persons, users, vehicles, or devices are responsible for their own privacy and legal obligations.

---

# 20. Network Telemetry and Privacy

Bifröst may observe network information needed for operation and diagnostics, including:

* cellular signal metrics,
* serving-cell identifiers,
* modem state,
* network addresses,
* connection quality,
* latency,
* throughput,
* connected-device information,
* data usage,
* network failures,
* and system logs.

The reference implementation should avoid unnecessary collection of personal information.

Sensitive identifiers should be minimized, masked, redacted, or stored only when technically necessary.

This policy does not control independent forks, third-party distributions, or modified deployments.

---

# 21. Security

Bifröst may function as a network router, access point, gateway, remote-management host, or VPN endpoint.

No networking software can be assumed permanently immune from security defects.

Users and distributors are responsible for:

* installing security updates,
* using appropriate credentials,
* maintaining firewall rules,
* protecting remote access,
* controlling administrative interfaces,
* protecting API credentials,
* protecting Wi-Fi credentials,
* securing physical access,
* and reviewing the security consequences of modifications.

Experimental features should not be exposed directly to untrusted networks without appropriate review.

---

# 22. AI-Assisted Features

Future Bifröst releases may contain optional AI-assisted functionality for purposes such as:

* interpreting signal conditions,
* explaining diagnostics,
* suggesting antenna directions,
* analyzing site history,
* forecasting connectivity,
* detecting anomalies,
* or assisting troubleshooting.

AI-generated recommendations may be incomplete or incorrect.

The intended Bifröst architecture keeps AI assistance subordinate to deterministic safety, networking, and hardware-control systems.

AI output should not be treated as:

* regulatory approval,
* professional engineering certification,
* legal advice,
* emergency instruction,
* or an independent hardware safety mechanism.

---

# 23. Third-Party Components

Bifröst may depend upon or interoperate with independent third-party software, operating-system components, libraries, firmware, hardware, services, standards, and protocols.

Such components remain subject to their respective licenses and terms.

See:

```text
THIRD_PARTY_NOTICES.md
```

for additional information.

Nothing in the Apache-2.0 or CERN-OHL-S-2.0 licensing of Bifröst replaces the license of an independent third-party work.

---

# 24. Third-Party Forks and Derivative Hardware

Open-source and open-hardware licensing permits forms of modification and redistribution subject to the applicable license terms.

A fork, modified product, derivative hardware implementation, manufactured board, commercial kit, or third-party service may differ substantially from the original Bifröst project.

The author does not control and cannot warrant such derivatives.

Third parties should not represent their products or services as officially approved by the original Bifröst Reach Node project unless such approval has actually been granted.

---

# 25. Project Name and Identity

The licenses governing Bifröst software and hardware source materials do not necessarily grant a right to imply endorsement by the original project.

A derivative project may truthfully state that it is:

* based on Bifröst Reach Node,
* derived from Bifröst Reach Node,
* compatible with Bifröst Reach Node,
* or a fork of Bifröst Reach Node,

subject to applicable license and trademark principles.

It should not falsely represent itself as the official upstream project.

---

# 26. Commercial Use

The project's open-source and open-hardware licenses permit commercial activities subject to their respective terms.

Commercial manufacture or distribution may, however, create legal, certification, warranty, taxation, safety, consumer-protection, telecommunications, or other obligations that do not apply merely to publication of source materials.

Anyone commercially manufacturing or distributing Bifröst-derived products is responsible for evaluating those obligations.

The author's publication of open designs does not constitute regulatory certification of a third party's commercial product.

---

# 27. No Representation of Universal Availability

Nothing in this repository should be interpreted as a representation that Bifröst Reach Node:

* can legally be operated in every jurisdiction,
* supports every carrier,
* supports every modem,
* supports every radio band,
* supports every Raspberry Pi or SBC,
* works with every antenna,
* will be officially distributed through every platform,
* or will function in every deployment environment.

Availability of source materials does not imply universal regulatory or technical compatibility.

---

# 28. No Professional Engineering Representation

Project documents may contain substantial engineering analysis, calculations, specifications, architecture, diagrams, testing methods, and recommendations.

Unless expressly stated otherwise, publication of these materials does not constitute a stamped or certified professional engineering service.

Users are responsible for obtaining qualified professional review where required by law or appropriate to the risk of a deployment.

---

# 29. No Carrier Service Guarantee

Bifröst does not itself provide cellular service.

Use of a cellular modem generally depends upon an independent carrier relationship, compatible SIM or eSIM configuration, network availability, supported bands, account status, and applicable carrier policies.

The project cannot guarantee:

* carrier activation,
* network access,
* roaming,
* Wi-Fi Calling,
* particular frequency access,
* specific modem functionality,
* or continued compatibility with carrier networks.

---

# 30. No Warranty Beyond Applicable Licenses

Bifröst Reach Node is provided without warranty except as expressly provided by the applicable licenses.

This includes software, documentation, hardware designs, examples, scripts, diagrams, calculations, experimental concepts, and reference configurations.

Users remain responsible for evaluating whether a particular implementation is appropriate for their intended purpose.

---

# 31. Reservation of Distribution Choices

The author reserves the right to determine whether, when, where, and how official:

* source releases,
* binaries,
* installers,
* firmware,
* operating-system images,
* hardware kits,
* assembled devices,
* hosted services,
* remote services,
* mobile applications,
* or platform-specific releases

will be provided.

Nothing in the open-source or open-hardware publication obligates the author to provide every possible distribution form.

---

# 32. Changes to Project Policy

This notice may be revised as Bifröst Reach Node evolves.

Changes to this policy notice do not retroactively alter the licenses under which already distributed software or hardware designs were released.

The applicable `LICENSE`, file-level SPDX identifiers, and authoritative license texts remain controlling for licensed materials.

---

# 33. No Legal Advice

This document is a statement of project policy, author intent, technical boundaries, and general caution.

It is **not legal advice**.

Nothing in this document should be interpreted as determining the legal requirements applicable to a specific:

* user,
* jurisdiction,
* radio installation,
* telecommunications system,
* manufactured device,
* commercial product,
* carrier relationship,
* or distribution model.

Users, manufacturers, distributors, deployers, and commercial operators should obtain appropriate professional advice where necessary.

---

# 34. Governing Principle

Bifröst Reach Node exists to explore an open, resilient, local-first approach to communications engineering.

The project is intended to:

* listen rather than interfere,
* optimize rather than overpower,
* use lawful communications hardware,
* preserve user autonomy,
* minimize unnecessary data collection,
* make engineering claims through proof,
* keep software open,
* keep covered hardware lineage open,
* and enable experimentation without pretending experimentation is certification.

**Build the bridge. Measure reality. Respect the spectrum. Keep Bifröst open.**

---

