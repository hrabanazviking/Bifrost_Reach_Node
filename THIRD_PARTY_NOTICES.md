# Third-Party Notices

## Bifröst Reach Node

This document records third-party software, libraries, system components, protocols, tools, hardware interfaces, and other external works that are used by, distributed with, referenced by, or designed to interoperate with **Bifröst Reach Node**.

Bifröst Reach Node itself uses separate licenses for its software and open-hardware design materials:

* **Software:** Apache License 2.0
* **Open hardware designs:** CERN Open Hardware Licence Version 2 — Strongly Reciprocal (`CERN-OHL-S-2.0`)

See:

```text
LICENSE
NOTICE
LICENSES/Apache-2.0.txt
LICENSES/CERN-OHL-S-2.0.txt
```

This file does **not** replace the license, copyright notice, attribution requirements, or other terms of any third-party project.

Third-party works remain the property of their respective authors and copyright holders and remain subject to their own licenses.

---

# 1. Current Repository Status

At the present stage of development, the Bifröst Reach Node repository primarily contains:

* architecture documentation
* engineering specifications
* software design documentation
* implementation roadmaps
* diagrams
* project policies
* licensing materials
* design concepts
* reference information

The project has not yet established a final locked software dependency graph.

Accordingly, projects and software listed later in this document may represent:

1. planned dependencies,
2. optional system dependencies,
3. supported interfaces,
4. external operating-system facilities,
5. development tools,
6. compatibility targets, or
7. technologies referenced by Bifröst documentation.

Their appearance in this document does **not** necessarily mean their source code or binaries are redistributed by the Bifröst Reach Node repository.

---

# 2. Bundled and Vendored Third-Party Software

## Current Status

**No third-party runtime source package is intentionally vendored into the Bifröst Reach Node software source tree at this stage of development.**

No dependency should be considered bundled merely because:

* Bifröst documentation mentions it,
* Bifröst is capable of communicating with it,
* an installation guide recommends installing it from an operating-system package repository,
* Bifröst executes its command-line utility,
* Bifröst communicates with its API or service,
* Bifröst supports hardware that requires it, or
* the component happens to be installed on the same Linux system.

When Bifröst begins distributing third-party source code, compiled libraries, firmware, JavaScript packages, Python packages, binaries, fonts, datasets, or other licensed materials inside Bifröst release artifacts, this section must be updated accordingly.

---

# 3. Planned and Referenced System Components

Bifröst Reach Node is designed to operate on a Linux-based edge-computing platform and may use or interoperate with third-party system software.

Current architecture and planning documents reference technologies including, but not limited to:

## ModemManager

Bifröst may use **ModemManager** as a high-level Linux interface for discovering, configuring, monitoring, and controlling cellular modems.

Project:

```text
ModemManager
```

Role in Bifröst:

* modem discovery
* modem status
* SIM state
* network registration
* cellular bearer management
* cellular telemetry
* modem lifecycle management

ModemManager is an independent third-party project.

Bifröst does not claim ownership of ModemManager.

---

## NetworkManager

Bifröst may use **NetworkManager** for Linux network-interface configuration and connection management.

Possible roles include:

* cellular interfaces
* Ethernet interfaces
* Wi-Fi interfaces
* connection profiles
* routing integration
* interface state monitoring

NetworkManager is an independent third-party project.

---

## libqmi

Bifröst may interface with **libqmi** and QMI-capable cellular modems.

Possible roles include:

* modem control
* serving-system information
* radio telemetry
* data-session management
* modem diagnostics
* advanced modem features

libqmi is an independent third-party project.

---

## libmbim

Bifröst may interface with **libmbim** and MBIM-compatible cellular modems.

Possible roles include:

* modem management
* cellular connectivity
* SIM information
* registration information
* radio state
* bearer/session management

libmbim is an independent third-party project.

---

## gpsd

Bifröst may use **gpsd** to obtain GNSS/GPS information from compatible receivers.

Possible roles include:

* latitude
* longitude
* altitude
* heading
* speed
* fix state
* accuracy
* timestamps

gpsd is an independent third-party project.

---

## SQLite

Bifröst intends to use **SQLite** for local-first persistent storage.

Possible stored information includes:

* signal observations
* site profiles
* cellular history
* antenna bearings
* modem observations
* network-performance history
* GPS observations
* configuration metadata
* proof records
* event history

SQLite's core deliverable source code has been dedicated to the public domain by its authors.

Bifröst does not claim ownership of SQLite.

---

## Python

Python is currently contemplated as a principal implementation language for portions of the Bifröst software stack.

Python is developed by the Python community and supported by the Python Software Foundation.

Python itself is subject to its own licensing terms.

The Apache-2.0 license applying to Bifröst source code does not replace or modify Python's license.

---

## systemd

On compatible Linux distributions, Bifröst may use **systemd** for functions including:

* service startup
* service supervision
* watchdogs
* restart policies
* logging integration
* dependency ordering
* startup recovery

systemd is an independent open-source project and remains subject to its own licensing terms.

---

## iproute2

Bifröst may use Linux networking utilities provided by **iproute2**.

Possible uses include:

* routing
* policy routing
* interface inspection
* network namespaces
* link configuration
* traffic management

iproute2 is an independent third-party project.

---

## nftables

Bifröst may use Linux **nftables** infrastructure for:

* firewall rules
* packet filtering
* network address translation
* interface isolation
* forwarding policy
* security boundaries

nftables and associated utilities are independent third-party works.

---

## hostapd

Bifröst may use **hostapd** to provide Wi-Fi access-point functionality on supported Linux hardware.

Possible uses include:

* Wi-Fi access-point operation
* authentication
* WPA/WPA2/WPA3 configuration where supported
* local wireless networking

hostapd remains subject to its own upstream licensing terms.

---

## dnsmasq

Bifröst may use **dnsmasq** for lightweight local-network services including:

* DHCP
* DNS forwarding
* DNS caching
* local network configuration

dnsmasq is an independent third-party project.

---

# 4. Optional Platforms and Integrations

The Bifröst architecture may optionally support or interoperate with additional third-party platforms.

These may include:

## OpenWrt

Bifröst may support OpenWrt-based deployments or networking components.

OpenWrt is an independent open-source Linux operating system/project and remains subject to its own licenses and package-specific licensing terms.

---

## Raspberry Pi OS

Bifröst may run on Raspberry Pi OS.

Raspberry Pi OS and the software distributed with it are separate from Bifröst and contain components under numerous independent licenses.

Installing Bifröst on Raspberry Pi OS does not cause those operating-system components to become licensed under the Bifröst licenses.

---

## Debian

Bifröst may run on Debian or Debian-derived systems.

Debian contains thousands of independently licensed software packages.

Bifröst does not alter their respective licenses.

---

## Ubuntu

Bifröst may run on Ubuntu Server and compatible Ubuntu systems.

Ubuntu and its packages remain subject to their respective upstream and distribution licensing terms.

---

## Tailscale

Bifröst may optionally integrate with or detect Tailscale for secure remote connectivity.

Tailscale is an independent project/service.

Bifröst's licensing does not grant rights to Tailscale software, services, names, trademarks, or infrastructure.

Use of Tailscale remains subject to the applicable Tailscale software licenses and service terms.

---

# 5. Cellular Modem and Hardware Vendors

Bifröst is intended to support commercially manufactured cellular modem hardware.

Architecture and development documentation may reference hardware manufactured by companies including:

* Quectel
* Sierra Wireless / Semtech
* Fibocom
* Telit Cinterion
* Raspberry Pi Ltd.
* Waveshare
* Poynting
* and other compatible manufacturers

References to these companies or their products identify potential compatible hardware only.

Unless explicitly stated otherwise:

* no manufacturer sponsors Bifröst Reach Node,
* no manufacturer endorses Bifröst Reach Node,
* Bifröst Reach Node is not affiliated with those manufacturers,
* manufacturer firmware is not relicensed by Bifröst,
* manufacturer documentation remains subject to its original terms, and
* trademarks remain the property of their respective owners.

---

# 6. Communications Protocols and Standards

Bifröst may implement, invoke, or interface with standardized or vendor-defined communications mechanisms including:

* QMI
* MBIM
* AT command interfaces
* USB
* PCI Express
* Ethernet
* IPv4
* IPv6
* TCP
* UDP
* DNS
* DHCP
* Wi-Fi
* GNSS/NMEA
* HTTP
* HTTPS
* REST-style APIs
* serial/UART interfaces

Support for or implementation of a protocol does not imply ownership of that protocol, standard, specification, trademark, or related intellectual property.

Implementers are responsible for reviewing any applicable standards, patent requirements, certification requirements, or vendor terms relevant to their specific implementation.

---

# 7. Linux Kernel and Operating-System Components

Bifröst runs on or is intended to run on Linux systems.

The Linux kernel and operating-system components are separate third-party works.

Bifröst's Apache-2.0 software license does not relicense:

* the Linux kernel,
* kernel modules,
* device drivers,
* operating-system packages,
* system libraries,
* bootloaders,
* firmware,
* networking utilities, or
* distribution-specific components.

Each remains subject to its applicable license.

---

# 8. Hardware Firmware

Some hardware used with Bifröst may contain proprietary, redistributable, or open-source firmware supplied by the hardware manufacturer.

Examples may include firmware for:

* cellular modems
* Wi-Fi chipsets
* GNSS receivers
* microcontrollers
* USB adapters
* power-management devices

Unless explicitly included in a Bifröst release, such firmware is **not part of the Bifröst Reach Node project**.

Users must obtain and use firmware according to the terms supplied by the applicable manufacturer or upstream project.

---

# 9. Future Python Dependencies

When Python package dependencies are introduced, their authoritative versions and licenses must be determined from the project's locked dependency graph rather than from this planning document.

The project should eventually generate dependency notices from files such as:

```text
pyproject.toml
requirements.txt
requirements.lock
uv.lock
poetry.lock
```

or whichever package-management system Bifröst ultimately adopts.

A package should not be added to this notice merely because it was evaluated experimentally.

---

# 10. Future JavaScript / Web Dependencies

If the Bifröst dashboard later uses third-party JavaScript, TypeScript, CSS, icon, charting, mapping, or frontend packages, those dependencies must be reviewed and listed according to the exact versions actually distributed.

Potential categories include:

* web frameworks
* visualization libraries
* mapping libraries
* icon sets
* fonts
* CSS frameworks
* browser-side utilities

The final release artifact should contain the notices required by the exact dependency versions included in that release.

---

# 11. Maps, Geographic Data, and Terrain Data

Future Bifröst mapping and radio-analysis capabilities may use external:

* map tiles
* vector maps
* terrain datasets
* elevation data
* geographic databases
* tower databases
* geospatial libraries

These resources may have licenses or attribution requirements that differ substantially from software licenses.

Any such dataset included or downloaded by Bifröst must be documented separately.

Bifröst should distinguish:

```text
MEASURED DATA
```

from:

```text
THIRD-PARTY DATA
```

and:

```text
BIFRÖST-GENERATED ESTIMATES
```

where practical.

---

# 12. AI and Machine-Learning Components

Future optional Bifröst features may interface with local or remote artificial-intelligence systems.

Possible functions include:

* natural-language diagnostics
* signal-condition explanations
* troubleshooting suggestions
* historical summaries
* predictive connection analysis
* antenna-bearing recommendations
* anomaly detection

AI models, inference engines, tokenizers, datasets, model weights, and associated software are separate works and may each carry different licenses.

No AI model should be considered part of Bifröst merely because Bifröst can communicate with it.

If Bifröst later distributes model weights or AI runtimes, their exact licenses and required notices must be added to this document before release.

---

# 13. External Services

Bifröst is designed around a **local-first architecture** and should not require a particular cloud service for basic operation.

Optional integrations may nevertheless involve external services.

Third-party services retain their own:

* terms of service,
* privacy policies,
* acceptable-use policies,
* rate limits,
* licensing terms, and
* availability conditions.

The Bifröst open-source licenses do not override those terms.

---

# 14. Trademarks

Product names, project names, company names, service names, logos, and trademarks mentioned by Bifröst remain the property of their respective owners.

References are generally descriptive and are intended to identify:

* compatible hardware,
* supported software,
* possible integrations,
* development tools, or
* relevant technical standards.

Such references do not imply endorsement, sponsorship, certification, partnership, or affiliation.

---

# 15. Distribution Policy

Before a Bifröst release containing executable software is published, maintainers should review the complete dependency graph.

For every third-party component actually distributed with Bifröst, record:

```text
Component:
Version:
Upstream project:
Copyright holder(s):
License:
SPDX identifier:
Source location:
Bundled or dynamically/system provided:
Modifications by Bifröst:
Required attribution:
Required source offer:
Required license text:
Release artifact containing component:
```

Where a license requires reproduction of its complete text, that text should be included in an appropriate location such as:

```text
LICENSES/THIRD_PARTY/
```

or in an automatically generated release-license bundle.

---

# 16. System Dependencies vs. Bundled Dependencies

Bifröst intentionally distinguishes between these categories.

## System Dependency

Example:

```text
sudo apt install modemmanager
```

If the user's Linux distribution independently supplies ModemManager, Bifröst is not necessarily redistributing ModemManager.

The operating-system distributor provides that component under its applicable license.

## Bundled Dependency

Example:

```text
bifrost-release/
└── third_party/
    └── example-library/
```

If Bifröst distributes another project's source or binary material inside its own release, the corresponding license obligations must be satisfied by that Bifröst release.

## Downloaded Dependency

If Bifröst automatically downloads an external binary, model, firmware image, dataset, or package, the user should be informed that the material originates from a third party and remains subject to that party's terms.

---

# 17. Source Availability

Where a third-party license requires source-code availability, corresponding source, modifications, notices, or an appropriate source offer must be supplied as required by that license.

The Bifröst project's Apache-2.0 license must never be presented as replacing copyleft or other obligations applying to third-party code.

Similarly, the CERN-OHL-S-2.0 license used for Bifröst open-hardware designs does not automatically apply to independent third-party hardware designs.

---

# 18. Dependency Review Policy

Before introducing a new dependency, maintainers should evaluate:

* technical necessity
* maintenance status
* security history
* architecture support
* ARM64 support
* offline operation
* power and resource requirements
* license
* redistribution requirements
* patent clauses
* attribution requirements
* source-disclosure requirements
* interaction with Apache-2.0
* long-term availability

A dependency should not be selected merely because it is convenient.

Where a Linux system component can provide the necessary capability without bundling another library, system integration may be preferred.

---

# 19. Release Automation

As the implementation matures, Bifröst should automatically generate or validate third-party notices from the **exact dependency lockfiles used for each release**.

Recommended release proof:

```text
dependency lockfile
        ↓
license scanner
        ↓
dependency manifest
        ↓
THIRD_PARTY_NOTICES validation
        ↓
release artifact
        ↓
proof record
```

A release should fail its licensing proof if:

* an included dependency has an unknown license,
* a required notice is missing,
* a license text required for redistribution is absent,
* a bundled dependency is not represented in the notice manifest, or
* the notice was generated from a dependency graph different from the release artifact.

---

# 20. Authority of Upstream Licenses

This document is an attribution and compliance aid.

It is **not itself a license for third-party software**.

For any third-party component, the authoritative terms are the license and notices supplied by that project's copyright holders.

If information in this file conflicts with an authoritative upstream license, the upstream license controls for that third-party work.

---

# 21. No Warranty Concerning Third-Party Components

Third-party software, hardware, services, firmware, data, and other components are provided by their respective authors or vendors.

The Bifröst Reach Node project makes no additional warranty concerning those independent works.

Consult the applicable upstream project or vendor for its warranty and support terms.

---

# 22. Maintaining This File

This document must evolve with the implementation.

Whenever one of the following occurs:

* a new runtime dependency is added,
* a library becomes bundled,
* a binary is redistributed,
* frontend dependencies are added,
* model weights are distributed,
* a dataset is included,
* firmware is distributed,
* an external component is modified and redistributed,
* a dependency is removed, or
* a dependency's license changes,

the corresponding entry in this file should be reviewed.

A dependency notice should describe **what the repository actually distributes**, rather than what the architecture merely proposes.

---

# 23. Current Summary

At the present project stage:

| Category                              | Status                    |
| ------------------------------------- | ------------------------- |
| Bifröst software                      | Apache-2.0                |
| Bifröst open-hardware designs         | CERN-OHL-S-2.0            |
| Bundled third-party runtime libraries | None presently identified |
| Vendored third-party source           | None presently identified |
| Locked Python dependency graph        | Not yet established       |
| Locked frontend dependency graph      | Not yet established       |
| Bundled AI models                     | None                      |
| Bundled external datasets             | None                      |
| System integrations                   | Planned / evolving        |
| Linux networking dependencies         | Planned / evolving        |
| Cellular modem backends               | Planned / evolving        |
| Hardware support                      | Planned / evolving        |

---

# 24. Reporting an Attribution or License Issue

If a third-party component, attribution, copyright notice, license, or source requirement has been omitted or incorrectly represented, please open an issue in the Bifröst Reach Node repository.

Include, if possible:

* component name
* version
* upstream source
* applicable license
* affected Bifröst file or release
* description of the correction needed

Licensing errors should be treated as engineering defects and corrected transparently.

---

## Final Principle

Bifröst Reach Node is built on open technology and the work of many independent communities.

The project's own licenses protect the Bifröst software and hardware commons.

They do not erase the lineage of the tools beneath them.

**Honor the source. Preserve the notice. Keep the bridge open.**
