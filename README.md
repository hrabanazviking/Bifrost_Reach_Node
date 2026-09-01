| [Bifröst Reach Node v1 Engineering Design](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/Bifrost_Reach_Node_v1_Engineering_Design.md) | [Software Readme](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/software/README.md) | [Glossary](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/GLOSSARY.md) | [Software Roadmap](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/software/BIFROST_REACH_NODE_SOFTWARE_PROOF_ROADMAP.md) | [Engineering Doctrine](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/ENGINEERING_DOCTRINE.md) | [Notice](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/NOTICE) | [License](https://github.com/hrabanazviking/Bifrost_Reach_Node/blob/main/LICENSE) |

---

# RuneForgeAI: Bifröst Reach Node v1
## A Raspberry Pi–Powered Long-Range Cellular Gateway, Signal Surveyor, Smart Antenna Controller, and Nomadic Connectivity Platform

**Document type:** Engineering architecture / build specification  
**Version:** 1.0  
**Date:** 2026-09-01  
**Primary platform:** Raspberry Pi 5  
**Secondary control options:** Raspberry Pi Zero 2 W / RP2040-class microcontroller  
**Primary modem target:** Quectel RM520N-GL 5G/LTE M.2 module  
**Primary antenna target:** 4×4 MIMO directional wideband cellular panel, exemplified by Poynting XPOL-24  
**Operating philosophy:** legal, carrier-safe, field-repairable, modular, Docker-free, power-aware, and optimized for weak-signal nomadic use

---

# Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What the Bifröst Reach Node Is](#2-what-the-bifröst-reach-node-is)
3. [Design Goals](#3-design-goals)
4. [Non-Goals and Legal Boundaries](#4-non-goals-and-legal-boundaries)
5. [Why This Can Beat a Phone in Weak-Signal Country](#5-why-this-can-beat-a-phone-in-weak-signal-country)
6. [RF Fundamentals That Matter in the Field](#6-rf-fundamentals-that-matter-in-the-field)
7. [Core System Architecture](#7-core-system-architecture)
8. [The Critical Design Decision: Keep RF Cables Short](#8-the-critical-design-decision-keep-rf-cables-short)
9. [Build Tiers](#9-build-tiers)
10. [Recommended v1 Hardware Architecture](#10-recommended-v1-hardware-architecture)
11. [Raspberry Pi 5 Subsystem](#11-raspberry-pi-5-subsystem)
12. [5G/LTE Modem Subsystem](#12-5glte-modem-subsystem)
13. [Antenna Subsystem](#13-antenna-subsystem)
14. [Mast and Mechanical System](#14-mast-and-mechanical-system)
15. [Motorized Antenna Rotator](#15-motorized-antenna-rotator)
16. [Power System](#16-power-system)
17. [Weatherproof Remote Radio Head](#17-weatherproof-remote-radio-head)
18. [Vehicle and Camp Deployment](#18-vehicle-and-camp-deployment)
19. [Network Architecture](#19-network-architecture)
20. [Operating System Choices](#20-operating-system-choices)
21. [Software Architecture](#21-software-architecture)
22. [Signal Survey Engine](#22-signal-survey-engine)
23. [Automatic Antenna Aiming](#23-automatic-antenna-aiming)
24. [Connection Scoring Algorithm](#24-connection-scoring-algorithm)
25. [Tower and Campsite Memory](#25-tower-and-campsite-memory)
26. [Band and Network Strategy](#26-band-and-network-strategy)
27. [Multi-WAN, Failover, and Multiple Carriers](#27-multi-wan-failover-and-multiple-carriers)
28. [Phone Integration](#28-phone-integration)
29. [Optional Certified Cellular Booster Co-Pilot](#29-optional-certified-cellular-booster-co-pilot)
30. [Security Architecture](#30-security-architecture)
31. [Thermal Design](#31-thermal-design)
32. [Power Budget and Runtime Estimates](#32-power-budget-and-runtime-estimates)
33. [Solar Operation](#33-solar-operation)
34. [Bill of Materials](#34-bill-of-materials)
35. [Cost Estimates by Build Tier](#35-cost-estimates-by-build-tier)
36. [Wiring Diagrams](#36-wiring-diagrams)
37. [Connector and Cable Plan](#37-connector-and-cable-plan)
38. [Software Directory Layout](#38-software-directory-layout)
39. [Database Schema](#39-database-schema)
40. [Configuration File Example](#40-configuration-file-example)
41. [Service Architecture](#41-service-architecture)
42. [Installation Outline: Raspberry Pi OS Lite](#42-installation-outline-raspberry-pi-os-lite)
43. [Installation Outline: OpenWrt](#43-installation-outline-openwrt)
44. [Signal Measurement Commands](#44-signal-measurement-commands)
45. [Survey and Aiming Pseudocode](#45-survey-and-aiming-pseudocode)
46. [Field Operating Procedure](#46-field-operating-procedure)
47. [Fast Deployment Procedure](#47-fast-deployment-procedure)
48. [Storm, Wind, and Lightning Procedure](#48-storm-wind-and-lightning-procedure)
49. [Troubleshooting](#49-troubleshooting)
50. [Failure Modes and Graceful Degradation](#50-failure-modes-and-graceful-degradation)
51. [Testing Plan](#51-testing-plan)
52. [Performance Expectations](#52-performance-expectations)
53. [Future Upgrades](#53-future-upgrades)
54. [Bifröst Reach Node v2 Possibilities](#54-bifröst-reach-node-v2-possibilities)
55. [Recommended Build Sequence](#55-recommended-build-sequence)
56. [Final Recommended Configuration](#56-final-recommended-configuration)
57. [Reference Sources](#57-reference-sources)

---

# 1. Executive Summary

The **Bifröst Reach Node** is a portable, Raspberry Pi–controlled communications system designed to extract the best possible usable Internet connection from weak terrestrial cellular coverage without attempting to build an illegal homebrew cellular repeater.

The core concept is simple:

> **Do not try to make a phone into a better radio. Put a better radio and a better antenna where radio conditions are best, then bring the resulting Internet connection back to the phone over Wi-Fi or Ethernet.**

A normal smartphone is constrained by pocket-sized antennas, body absorption, vehicle shielding, battery limits, thermal limits, and the need to support many bands without external antenna connectors. The Bifröst system replaces those compromises with:

- a dedicated 4G/5G modem;
- four external cellular antenna paths for 4×4 MIMO where supported;
- a physically large directional antenna;
- antenna placement several meters above the ground;
- a weatherproof remote radio head that keeps RF cable runs short;
- Raspberry Pi software that continuously measures real radio quality;
- optional automatic antenna rotation;
- actual throughput and latency testing rather than trusting “bars”;
- campsite and tower memory;
- optional multi-carrier failover;
- Wi-Fi distribution to phones, laptops, VR systems, other Raspberry Pis, and local servers;
- optional integration with a certified consumer cellular booster when direct phone service is desired.

The result is not magic and cannot create RF energy where absolutely no tower signal reaches the site. What it can do is exploit the large gray zone between “good phone service” and “true radio dead zone.” In that gray zone, antenna gain, height, orientation, polarization, lower cable loss, better modem diagnostics, and intelligent network selection can be decisive.

The recommended v1 configuration is:

1. **Raspberry Pi 5** as router, controller, survey engine, database, web dashboard, and optional VPN endpoint.
2. **Waveshare PCIe-to-5G HAT+** or equivalent M.2 carrier.
3. **Quectel RM520N-GL** 5G/LTE modem.
4. **4×4 MIMO directional wideband antenna**, ideally a model with approximately 617–4200 MHz coverage.
5. **20–30 ft portable fiberglass mast** for stationary camp use.
6. **Weatherproof remote radio head** positioned close to the antenna so cellular coax runs are short.
7. **12 V or higher-voltage DC distribution up the mast**, converted locally to clean 5 V power for the Pi/HAT.
8. **Ethernet downlink** from the remote head to the camp/vehicle LAN when practical.
9. **Optional motorized azimuth rotator** with hard rotation limits, home sensor, and absolute or incremental position feedback.
10. **Custom `bifrost-reach` software** for signal surveying, scoring, logging, aiming, failover, and dashboard control.

A practical complete manual-aiming system is expected to land around **$700–$1,100** if buying all major parts new. A more elaborate motorized expedition build can plausibly reach **$900–$1,500+**, depending heavily on antenna, mast, enclosure, cable, and mechanical choices. A certified in-vehicle booster is a separate optional subsystem and currently adds roughly **$500** for top-tier consumer vehicle products.

---

# 2. What the Bifröst Reach Node Is

Bifröst is best understood as five systems living in one box:

1. **Cellular gateway**  
   It converts LTE/5G service into ordinary routed IP networking.

2. **RF survey instrument**  
   It reads real cellular metrics such as RSRP, RSRQ, SNR/SINR, technology, band, serving cell information, and registration state.

3. **Smart directional antenna controller**  
   It can guide a human or a motorized mount toward the best azimuth.

4. **Nomadic router**  
   It distributes the connection to camp devices and can fail over between cellular, phone tethering, campground Wi-Fi, Ethernet, or a future satellite source.

5. **Memory system**  
   It remembers which directions, bands, networks, and settings worked at a given campsite.

The system is deliberately not dependent on cloud infrastructure. If the Internet is terrible, the Bifröst management interface must still work locally.

---

# 3. Design Goals

## 3.1 Primary goals

- Maximize usable LTE/5G connectivity in marginal rural locations.
- Favor reliability over headline speed when the two conflict.
- Support long stationary camping deployments and quick temporary stops.
- Remain portable enough for car-based nomadic use.
- Be field-serviceable with common tools.
- Avoid Docker entirely.
- Function from DC battery systems and solar power.
- Expose enough radio telemetry for meaningful engineering decisions.
- Keep a local history of successful configurations.
- Support manual operation when automation fails.
- Remain legal and network-safe.

## 3.2 Secondary goals

- Support more than one carrier.
- Support Wi-Fi WAN as a fallback.
- Support USB phone tethering.
- Support Tailscale or another VPN overlay.
- Offer a local dashboard usable from a phone browser.
- Make future integration with Project Aesir or another local AI stack straightforward.
- Allow a future AI agent to recommend changes without giving it raw unsafe RF control.

## 3.3 Design philosophy

Bifröst follows four principles:

### Principle A: Physics before software

No algorithm can compensate for 10 dB of unnecessary coax loss. Antenna placement and cable architecture come first.

### Principle B: Measure what matters

“Five bars” is not an engineering metric. Bifröst cares about signal strength, signal quality, interference, latency, packet loss, throughput, technology, and stability over time.

### Principle C: Graceful degradation

If the antenna motor dies, aim manually. If the Pi dashboard dies, basic routing should remain recoverable. If 5G is unstable, fall back to LTE. If carrier A vanishes, try carrier B.

### Principle D: No illegal homemade repeater

The Pi controls networking and measurement. It does not become an unauthorized cellular RF power amplifier.

---

# 4. Non-Goals and Legal Boundaries

## 4.1 Do not build a raw cellular bidirectional amplifier

A cellular repeater or signal booster transmits into licensed cellular spectrum. In the United States, consumer signal boosters are governed by FCC rules and a Network Protection Standard. Consumer devices must be certified, used with approved antennas/cables, and normally registered with the relevant wireless provider. Providers may require consent and can require a booster to be shut down if it causes interference.

Therefore:

- **Do not** insert a generic broadband RF power amplifier between the antenna and cellular modem.
- **Do not** build a homemade cellular repeater from SDRs and RF amplifiers for field operation.
- **Do not** modify a certified consumer booster beyond its approved antenna/cable system.
- **Do not** radiate arbitrary LTE/5G waveforms.

Bifröst instead uses a normal certified cellular modem as the endpoint on the cellular network.

## 4.2 No fake base station

The system does not impersonate a carrier tower, IMSI catcher, femtocell, or private LTE/5G base station on public carrier spectrum.

## 4.3 No guarantee of coverage

If the site is genuinely outside radio reach, only changing location, gaining elevation, using a different carrier, or switching to a non-terrestrial link such as satellite can solve the problem.

---

# 5. Why This Can Beat a Phone in Weak-Signal Country

A smartphone is an astonishing radio, but it is a compromise machine.

A phone antenna must:

- fit inside a thin enclosure;
- work next to a human hand and head;
- share space with cameras, batteries, displays, speakers, NFC, Wi-Fi, Bluetooth, and other radios;
- operate over many cellular bands;
- survive arbitrary orientation;
- consume little battery;
- avoid excessive heat;
- remain aesthetically invisible.

Bifröst does not have those constraints.

A dedicated external panel antenna can have meaningful directional gain. A mast can raise it above the roof of a vehicle, brush, nearby people, and some local obstructions. A 4×4 MIMO modem can use separate receive paths. The radio can sit physically near the antenna rather than behind automotive steel and glass.

The largest improvements often come from four things:

1. **Height**
2. **Directionality**
3. **Low feed-line loss**
4. **Better signal quality, not merely stronger signal**

The fifth improvement is intelligence: Bifröst can test multiple orientations and prefer the one that actually delivers the best connection.

---

# 6. RF Fundamentals That Matter in the Field

## 6.1 dB is logarithmic

A 3 dB change is roughly a factor of two in power. A 10 dB change is a factor of ten in power.

That means an antenna system that gains several dB through better placement and lower loss can make a dramatic difference near the edge of coverage.

## 6.2 RSRP

**RSRP** is Reference Signal Received Power. It is a useful measure of cellular reference-signal strength.

Illustrative interpretation only:

| RSRP | Rough field interpretation |
|---|---|
| better than -80 dBm | very strong |
| -80 to -90 dBm | strong/good |
| -90 to -100 dBm | usable to good |
| -100 to -110 dBm | weak |
| -110 to -120 dBm | very weak / edge conditions |
| below -120 dBm | often difficult, but not automatically impossible |

Exact usable thresholds vary by band, modem, network configuration, bandwidth, interference, and load.

## 6.3 RSRQ

**RSRQ** expresses quality relative to received signal and network loading/interference conditions. A strong RSRP with poor RSRQ can still produce miserable performance.

## 6.4 SINR / SNR

**SINR** is often the metric that explains why two directions with similar RSRP behave completely differently.

A directional antenna can improve SINR by favoring the serving cell while rejecting energy arriving from unwanted directions.

This is one reason Bifröst should not simply rotate until RSRP is maximum.

## 6.5 Throughput matters, too

A tower can show excellent radio metrics and still be congested.

Therefore Bifröst scoring should include:

- RSRP;
- RSRQ;
- SINR/SNR;
- latency;
- packet loss;
- download throughput;
- upload throughput;
- short-term stability;
- connection technology;
- band;
- cell identity when available.

## 6.6 Low bands vs. mid bands

Lower-frequency cellular bands generally propagate farther and penetrate obstacles better. Mid-band 5G often has much greater capacity but may require a cleaner path.

A rural connection strategy may therefore intentionally prefer a slower low-band LTE or 5G connection because it remains stable through weather, foliage, and small antenna movement.

## 6.7 Fresnel-zone reality

Radio line of sight is not merely a laser-thin visual line. Nearby terrain, trees, ridges, and structures can intrude into the Fresnel zone and degrade a link even when the tower is technically visible.

Height can help enormously because moving an antenna several meters upward may clear a local obstruction or reduce ground interactions.

## 6.8 Trees are not transparent

Wet foliage is especially unfriendly to higher cellular frequencies. A beautiful n77 5G link in winter may degrade when foliage is dense and wet.

That argues for:

- broad-band antenna support;
- LTE fallback;
- campsite memory that stores seasonal observations;
- a mast rather than a low vehicle-roof antenna for stationary use.

## 6.9 MIMO is not simply “four antennas equals four times the range”

4×4 MIMO improves capacity and resilience when the network, band, channel conditions, and modem support it. It does not produce a fourfold range multiplier.

The real reasons to preserve four antenna paths are:

- spatial diversity;
- polarization diversity;
- higher-rank MIMO where available;
- better handling of multipath;
- improved modem flexibility across bands.

---

# 7. Core System Architecture

```text
                           DISTANT CELL SITE
                                  │
                         LTE / 5G radio link
                                  │
                                  ▼
                  ┌────────────────────────────┐
                  │  4×4 MIMO directional     │
                  │  wideband cellular panel  │
                  └──────┬────┬────┬────┬─────┘
                         │    │    │    │
                         │ short low-loss RF paths
                         │    │    │    │
                  ┌──────▼────▼────▼────▼─────┐
                  │  Weatherproof Remote       │
                  │  Radio Head                │
                  │                            │
                  │  RM520N-GL 5G/LTE modem   │
                  │  M.2 carrier / HAT        │
                  │  Raspberry Pi 5           │
                  │  local power conversion   │
                  │  Ethernet                 │
                  └─────────────┬──────────────┘
                                │
                       Ethernet + DC power
                          down the mast
                                │
                                ▼
                       ┌─────────────────┐
                       │ Camp LAN / AP   │
                       │ Wi-Fi + wired   │
                       └───────┬─────────┘
                               │
             ┌─────────────────┼───────────────────┐
             │                 │                   │
           Phone             Laptop              Pi / VR
```

A simpler variant leaves the Pi in the vehicle and uses USB from the modem enclosure, but the recommended serious weak-signal architecture keeps the RF paths short.

---

# 8. The Critical Design Decision: Keep RF Cables Short

This is one of the most important conclusions in the entire project.

A high-gain antenna mounted 25 feet above camp can be partially defeated if the signal then travels through 25 or 30 feet of mediocre coax before reaching the modem.

Loss increases with frequency. The exact number depends on cable type and length, but the general rule is merciless:

> **At cellular mid-band frequencies, long thin coax is expensive in dB.**

Quectel's RM520N-GL hardware guidance itself recommends controlling insertion loss, with stricter targets at low, mid, and high bands.

## 8.1 Preferred architecture

Place the modem close to the antenna.

Instead of:

```text
ANTENNA
   │
   │ 25 ft cellular coax × 4
   │
MODEM IN CAR
```

use:

```text
ANTENNA
   │
   │ 1–5 ft low-loss RF × 4
   │
MODEM + PI REMOTE HEAD
   │
   │ 25 ft Ethernet / DC
   │
CAR / CAMP NETWORK
```

Ethernet does not care about a few dozen feet in the way a 3.5 GHz RF signal does.

## 8.2 Why not put only the modem up the mast?

That is possible and may ultimately be ideal, but it complicates USB or PCIe transport. The clean v1 engineering solution is to put the modem carrier and Pi in the same protected remote head and run Ethernet downward.

## 8.3 Antenna version choice

For a remote radio head, a no-cable or short-cable antenna variant can be attractive because it allows the installer to choose short, high-quality feed lines rather than accepting a long factory cable that is unnecessary in this architecture.

---

# 9. Build Tiers

## Tier 0: Software Scout

**Purpose:** Learn signal behavior before buying expensive RF hardware.

Hardware:

- Raspberry Pi already available;
- phone USB tethering or Wi-Fi hotspot;
- optional USB GPS.

Capabilities:

- campsite database;
- speed/latency logging;
- mapping;
- Wi-Fi distribution;
- no direct modem RF diagnostics beyond what the phone exposes.

Approximate added cost: **$0–$80**.

## Tier 1: Compact 5G Gateway

Hardware:

- Pi 5;
- RM520N-GL;
- compatible M.2 carrier/HAT;
- four small supplied antennas or compact omni antenna;
- indoor/vehicle enclosure.

Capabilities:

- proper cellular diagnostics;
- 4G/5G routing;
- hotspot replacement;
- no tall mast required.

Approximate total hardware cost if starting from nothing: **$400–$600**.

## Tier 2: Manual Long-Range Bifröst

Adds:

- 4×4 MIMO directional panel;
- fiberglass mast;
- remote radio head;
- low-loss short coax;
- weatherproof enclosure;
- compass/aiming marks;
- guy lines and mast base.

Approximate total: **$700–$1,100**.

This is the recommended first serious build.

## Tier 3: Motorized Expedition Node

Adds:

- geared azimuth rotator;
- encoder;
- home/limit sensors;
- motor controller;
- automated sweep and scoring;
- stronger mechanical mounting;
- more robust power conditioning.

Approximate total: **$900–$1,500+**.

## Tier 4: Dual-Carrier Resilient Node

Adds:

- second modem or second WAN device;
- second SIM/carrier;
- mwan3 or custom policy routing;
- automated failover.

Approximate additional cost: **$250–$450+** plus service plan.

## Tier 5: Booster Co-Pilot

Adds a completely separate FCC-certified consumer booster for ordinary direct phone cellular service.

Current top-tier vehicle booster systems are around **$500** before optional premium antennas or mounting hardware.

---

# 10. Recommended v1 Hardware Architecture

The v1 design should optimize for reliability and learning rather than immediately motorizing everything.

## 10.1 Recommended v1 block list

- Raspberry Pi 5, 4 GB or greater.
- Raspberry Pi active cooler or equivalent reliable cooling.
- High-endurance microSD or SSD boot storage.
- Waveshare PCIe to 5G HAT+ or equivalent carrier.
- Quectel RM520N-GL.
- Nano-SIM data plan compatible with the selected carrier.
- 4×4 MIMO directional cellular antenna.
- Short low-loss RF jumpers.
- Weather-resistant polycarbonate remote-head enclosure.
- Cable glands.
- Hydrophobic/ePTFE pressure equalization vent.
- Desiccant as secondary moisture control.
- Wide-input DC-to-5.1 V regulator sized for Pi + modem transient load.
- Inline fuse near the battery/power source.
- Ethernet from remote head to local network.
- 20–30 ft telescoping fiberglass mast.
- Guy lines and nonconductive line where practical.
- Ground-level tripod, drive-over mast base, or other stable mounting system.
- Manual azimuth scale for v1.
- Optional small travel router or dedicated Wi-Fi access point at ground level.

## 10.2 Why the Pi 5 is worth using

The Pi 5 is far more compute than simple routing requires, but Bifröst uses the headroom for:

- database logging;
- live dashboard;
- speed tests;
- multiple WAN health checks;
- VPN;
- local DNS filtering;
- telemetry;
- future machine-learning signal prediction;
- future local AI integration;
- storage of historical campsite behavior.

The official Raspberry Pi 5 brief lists a 2.4 GHz quad-core Cortex-A76 CPU, dual-band 802.11ac Wi-Fi, Gigabit Ethernet, two 5 Gbps USB 3 ports, two USB 2 ports, and a PCIe 2.0 ×1 interface.

---

# 11. Raspberry Pi 5 Subsystem

## 11.1 Memory

Bifröst itself does not need 16 GB of RAM. Even 4 GB is comfortable for router, database, dashboard, and telemetry work.

Extra RAM becomes useful if the same Pi later runs:

- local AI services;
- vector search;
- larger map caches;
- local speech services;
- additional containers, although this design intentionally does not use Docker;
- heavier monitoring software.

## 11.2 Storage

Recommended:

- 32–128 GB high-endurance microSD for a pure router appliance; or
- USB/PCIe SSD if the HAT layout permits and more logging is planned.

Because the 5G HAT may consume the Pi's exposed PCIe interface, a USB SSD is often mechanically simpler.

## 11.3 Cooling

Use active cooling in the remote head.

The enclosure may be in direct sun while the modem and Pi are both active. A normal indoor ambient-temperature assumption is not good enough.

Minimum recommendation:

- Pi active cooler;
- modem thermal pad/heatsink as recommended by the carrier design;
- enclosure shade when possible;
- temperature sensors logged by software;
- automatic thermal warning;
- optional fan control.

## 11.4 Power input

Raspberry Pi recommends a 5 V/5 A supply for Pi 5. A 3 A supply can impose peripheral current limits. The remote-head power system should therefore be engineered for the full 5 V/5 A class even if normal consumption is lower.

---

# 12. 5G/LTE Modem Subsystem

## 12.1 Recommended modem: Quectel RM520N-GL

The RM520N-GL is attractive because it supports:

- 5G NR SA;
- 5G NR NSA;
- LTE FDD and TDD;
- broad global band coverage;
- Linux support;
- USB and PCIe host interfaces;
- MBIM/QMI/QRTR/AT connectivity modes;
- integrated multi-constellation GNSS capabilities;
- four cellular antenna interfaces;
- 4×4 downlink MIMO on many LTE and 5G bands.

Quectel lists LTE support including common US rural bands such as B12, B13, B14, B66, and B71, and 5G support including n5, n12, n13, n14, n25, n41, n66, n71, n77, and many others.

## 12.2 4×4 MIMO nuance

The RM520N-GL supports 4×4 downlink MIMO on many, but not every, band. This is normal. Four antenna ports remain useful because the modem dynamically uses the appropriate RF paths according to band and technology.

## 12.3 Host interface

The module hardware documentation states that USB mode supports MBIM, QMI, QRTR, and AT control. PCIe mode is also supported, and the Waveshare Pi 5 HAT+ specifically advertises PCIe MHI support with Raspberry Pi OS and OpenWrt.

### Recommendation for v1

Use the interface mode that is best-supported by the exact HAT, kernel, and firmware combination you buy.

Do not make PCIe an ideological requirement. USB 3 can already move far more data than many rural cellular links will deliver.

Reliability is more important than winning a synthetic interface benchmark.

## 12.4 Modem power

Quectel's hardware design guide gives useful reference currents at 3.7 V. Examples include roughly:

- about 69 mA in a referenced USB 3.1 active idle condition;
- about 520 mA for one LTE low-band transmit example;
- about 1.08 A for one LTE mid-band transmit example;
- about 1.51 A for a referenced LTE carrier-aggregation case;
- roughly 0.46–0.97 A for several referenced 5G SA transmit cases;
- about 1.17 A for one LTE+5G EN-DC example.

These are not a complete worst-case power specification. The carrier board and regulator must be designed for transient headroom.

## 12.5 SIM considerations

Before buying a data plan:

- confirm the modem IMEI is accepted by the carrier;
- confirm the plan permits router/hotspot use;
- confirm the APN requirements;
- confirm whether the plan is deprioritized after a threshold;
- confirm whether video or VPN traffic is shaped;
- confirm whether IPv6 is provided;
- confirm whether CGNAT is used;
- confirm whether inbound ports are blocked.

Bifröst should not assume a public IPv4 address.

---

# 13. Antenna Subsystem

## 13.1 Recommended antenna class

A wideband, directional, cross-polarized 4×4 MIMO panel is the ideal stationary weak-signal antenna.

The Poynting XPOL-24 is a strong reference design because the manufacturer specifies:

- 617–960 MHz and 1710–4200 MHz coverage;
- up to 11 dBi gain;
- 4×4 MIMO;
- four cross-polarized elements;
- directional pattern;
- IP65 environmental rating.

Current US retail listings found during preparation of this document were roughly **$365–$440** depending on seller and cable variant.

## 13.2 Why directional rather than omnidirectional?

An omni antenna is excellent while driving because the tower direction constantly changes.

At a stationary camp, a directional panel offers two advantages:

1. useful forward gain;
2. rejection of interference from directions you do not care about.

The second benefit can matter as much as the first.

## 13.3 Directional antenna trade-offs

- Must be aimed.
- Can accidentally select the wrong tower if aimed only by signal strength.
- More wind load than a small omni.
- May require readjustment if the modem changes bands or serving cells.
- A 4×4 panel has four cable paths, increasing cost and mechanical complexity.

## 13.4 Cable variant

If the radio head will sit immediately behind or below the antenna, prefer the configuration that allows the shortest practical feed line.

Antenna gain is specified before cable loss unless otherwise stated. Always account for the actual cable path.

## 13.5 Label all four RF paths

Use permanent labels:

- CELL-A
- CELL-B
- CELL-C
- CELL-D

Then map them to modem ANT0–ANT3 according to the modem/carrier-board instructions.

Do not casually swap paths after a working configuration is established. Consistent labeling makes debugging possible.

---

# 14. Mast and Mechanical System

## 14.1 Recommended height

For a portable vehicle/camp system, a **20–30 ft telescoping fiberglass mast** is a useful target.

It is high enough to get meaningfully above many local obstructions but still manageable by one person with proper technique and guying.

## 14.2 Why fiberglass?

Advantages:

- lighter than many metal masts;
- nonconductive mast body;
- portable;
- common in temporary radio deployments.

This does **not** make the entire installation electrically safe. The antenna, cables, hardware, moisture, and nearby power lines remain hazards.

## 14.3 Base options

### Drive-over base

A plate sits under a vehicle tire and holds the mast socket.

Advantages:

- excellent for car camping;
- no ground stakes required for basic base stability;
- fast deployment.

Disadvantage:

- still guy the mast when wind or antenna area warrants it.

### Ground tripod

Good for primitive camps where the car cannot be positioned beside the antenna.

### Tree-independent strap mount

Possible, but avoid damaging bark and do not rely on a living tree as the only safety structure.

## 14.4 Guying

Use three guy directions separated by roughly 120° when terrain permits.

Use bright flags or reflective markers on lines. Invisible guy lines are ankle traps with ambitions.

## 14.5 Wind

A large directional panel has much more wind loading than a whip antenna.

Bifröst software should allow the user to record a **maximum deployed wind threshold** and remind the operator to lower the mast when conditions exceed it.

The correct threshold must come from the weakest component in the real mechanical assembly, not from the antenna's laboratory wind rating alone.

---

# 15. Motorized Antenna Rotator

Motorization is a v1.5 or v2 feature. Build manual first.

## 15.1 Functional requirements

The rotator should:

- rotate at least 300–350°;
- know its current azimuth;
- have a repeatable home position;
- stop before cables twist dangerously;
- hold position without continuous high power;
- survive outdoor humidity;
- move slowly enough that the mast is not violently shocked;
- allow manual override.

## 15.2 Recommended mechanical concept

Use a **worm-geared 12 V DC motor** or slow geared stepper driving a belt or ring gear at the mast head or lower rotating section.

Why worm gear?

- high reduction;
- good holding behavior;
- resistance to wind back-driving;
- modest control requirements.

## 15.3 Position sensing

Preferred hierarchy:

1. absolute rotary encoder;
2. incremental encoder + hard home switch;
3. step counting only as a last resort.

Step counting without a physical reference eventually drifts.

## 15.4 Cable wrap

Do **not** casually use an unlimited-rotation scheme.

Four cellular RF lines, power, and Ethernet make a proper multi-channel rotary interface complicated and expensive.

Instead:

- restrict travel to about 350°;
- establish a home azimuth;
- refuse commands that would cross the cable-wrap boundary;
- automatically “unwind” before a new survey if necessary.

## 15.5 Separate motor controller

A Raspberry Pi should not directly drive a motor.

Recommended:

```text
Pi 5
 │ USB/UART
 ▼
RP2040 / Pi Pico / small controller
 │
 ├─ motor driver
 ├─ home switch
 ├─ CW limit
 ├─ CCW limit
 └─ encoder
```

The microcontroller enforces hard safety limits even if Linux freezes.

## 15.6 Magnetometer warning

Do not trust a cheap compass module mounted beside a motor, steel bracket, or power wiring.

Use the encoder for relative heading and manually calibrate zero/north during setup. A magnetometer can be added later on a physically separated boom if testing proves it reliable.

---

# 16. Power System

## 16.1 Power architecture

Recommended stationary camp architecture:

```text
12 V / power-station DC output
        │
        ├─ inline fuse
        │
        ├─ optional low-voltage disconnect
        │
        └──────── DC cable up mast ────────┐
                                           │
                                  Remote Radio Head
                                           │
                                  wide-input buck
                                      5.1 V / 5 A+
                                           │
                                  Pi 5 + modem HAT
```

## 16.2 Why send higher voltage up the mast?

For the same power, higher voltage means lower current and therefore less voltage drop in the long cable.

Example:

20 W at 5 V requires 4 A.  
20 W at 12 V requires about 1.67 A before conversion losses.

That makes 12 V distribution much friendlier to a 20–30 ft run.

## 16.3 Vehicle-voltage warning

A nominal 12 V automotive system is not a clean 12.000 V laboratory supply.

Use a regulator designed for automotive-like variation if powering from the car.

## 16.4 Fusing

Fuse close to the battery or power source.

The fuse protects wiring, not the Pi.

Size the fuse according to:

- conductor gauge;
- maximum expected load;
- startup surges;
- regulator rating.

## 16.5 Power connector philosophy

Use keyed, locking DC connectors for the remote head. Avoid fragile barrel connectors dangling outdoors.

Good field systems make it difficult to plug 12 V directly into a 5 V rail by accident.

---

# 17. Weatherproof Remote Radio Head

## 17.1 Enclosure target

Use a polycarbonate or similar outdoor enclosure with at least splash/rain protection appropriate for the intended deployment.

A practical target is an IP65-class enclosure, but the complete assembled box is only as weatherproof as its glands, seals, vents, holes, and workmanship.

## 17.2 Layout

Inside the box:

```text
┌─────────────────────────────────────┐
│  RF bulkhead / short antenna leads  │
│  A   B   C   D                      │
│                                     │
│  ┌──────────────────────────────┐   │
│  │ RM520N + carrier/HAT         │   │
│  └──────────────────────────────┘   │
│               │                     │
│  ┌────────────▼─────────────────┐   │
│  │ Raspberry Pi 5 + cooler      │   │
│  └──────────────────────────────┘   │
│                                     │
│  DC buck     temp sensor    fan     │
│                                     │
│  Ethernet gland   DC gland          │
└─────────────────────────────────────┘
```

## 17.3 Condensation

Sealing a box does not automatically eliminate moisture. Temperature cycling can cause condensation.

Use:

- quality gasket;
- downward-facing cable entries where possible;
- pressure equalization vent;
- small desiccant pack as a secondary measure;
- conformal coating only where appropriate and only if it does not interfere with connectors or heat transfer.

## 17.4 Sun loading

A sealed box in sun can become an oven.

Mitigations:

- light-colored enclosure;
- mount on the shaded side of the antenna when possible;
- small rain/sun shield with an air gap;
- internal temperature telemetry;
- thermal shutdown logic;
- avoid putting the box directly against a hot vehicle roof.

---

# 18. Vehicle and Camp Deployment

## 18.1 Driving mode

Do not drive with a 20–30 ft mast deployed.

For motion, use:

- phone direct cellular;
- compact roof/magnetic omni connected to the modem;
- certified vehicle booster if installed;
- Bifröst Pi running as an ordinary cellular router.

## 18.2 Stationary weak-signal mode

At camp:

1. park safely;
2. place mast away from overhead wires;
3. erect mast at low height first;
4. connect antenna and remote head;
5. power system;
6. verify basic registration;
7. extend mast;
8. run coarse survey;
9. aim antenna;
10. run fine survey;
11. lock physical mount or engage motor hold;
12. save campsite profile.

## 18.3 Stealth consideration

The mast is intentionally a stationary-camp tool, not a permanent external car modification. It can live inside the vehicle when moving, preserving a visually ordinary vehicle profile.

---

# 19. Network Architecture

Recommended LAN structure:

```text
                  Cellular WAN
                      │
                 RM520N-GL
                      │
                 Raspberry Pi 5
                      │
        ┌─────────────┼─────────────┐
        │             │             │
    Ethernet       Wi-Fi AP      Tailscale
        │             │             │
   local switch     phone       remote admin
        │
  laptop / Pi / VR / storage
```

## 19.1 Suggested subnets

Example only:

- Bifröst LAN: `10.77.0.0/24`
- infrastructure devices: `.2`–`.49`
- DHCP clients: `.100`–`.220`
- Pi management: `.1`

## 19.2 DNS

Options:

- carrier DNS;
- encrypted DNS upstream;
- local caching resolver.

Caching improves responsiveness on high-latency weak links.

## 19.3 NAT

Most carrier plans already place clients behind CGNAT. Bifröst can still NAT the local LAN normally.

## 19.4 IPv6

Prefer supporting IPv6 when the carrier provides it. Do not disable it reflexively. Mobile carriers often use IPv6 heavily.

---

# 20. Operating System Choices

## 20.1 Option A: Raspberry Pi OS Lite

**Recommended for the custom Bifröst research platform.**

Advantages:

- standard Debian-like environment;
- excellent Pi hardware support;
- Python, Rust, Go, SQLite, systemd;
- easy ModemManager integration;
- easy development of custom services;
- no GUI required;
- excellent for local dashboards.

Disadvantages:

- router functions require more manual configuration than OpenWrt.

## 20.2 Option B: OpenWrt

**Recommended if the device becomes primarily a router appliance.**

OpenWrt now has official Raspberry Pi 5 support. Its hardware data page lists Pi 5 support since release 24.10.2 and a current 25.12.x release family at the time this document was researched.

Advantages:

- excellent firewall/router model;
- LuCI;
- mwan3 multi-WAN support;
- package ecosystem for WWAN;
- ModemManager package available;
- compact immutable-style router architecture.

Disadvantages:

- custom Python/database/dashboard development is less natural than on Raspberry Pi OS;
- package versions may lag desktop Linux;
- unusual modem drivers sometimes require custom image work.

## 20.3 Recommendation

Start v1 on **Raspberry Pi OS Lite**.

Once hardware behavior is well understood, either:

- keep it and harden the router configuration; or
- port the routing layer to OpenWrt while running Bifröst intelligence as a small daemon.

---

# 21. Software Architecture

Bifröst software should be modular.

```text
                         ┌────────────────────┐
                         │  Web Dashboard     │
                         └─────────┬──────────┘
                                   │ REST/WebSocket
                         ┌─────────▼──────────┐
                         │ Bifröst Core       │
                         └─────────┬──────────┘
                                   │
         ┌─────────────────────────┼──────────────────────────┐
         │                         │                          │
 ┌───────▼────────┐      ┌────────▼─────────┐      ┌────────▼────────┐
 │ Modem Adapter  │      │ Survey Engine    │      │ Network Manager │
 └───────┬────────┘      └────────┬─────────┘      └────────┬────────┘
         │                         │                          │
 ModemManager/QMI             scoring/tests              routes/mwan
         │                         │
 ┌───────▼────────┐      ┌────────▼─────────┐
 │ RM520N         │      │ Rotator Service  │
 └────────────────┘      └────────┬─────────┘
                                   │ USB/UART
                         ┌─────────▼──────────┐
                         │ RP2040 Controller  │
                         └────────────────────┘
```

## 21.1 Services

- `bifrost-modemd`
- `bifrost-surveyd`
- `bifrost-rotatord`
- `bifrost-netd`
- `bifrost-web`
- `bifrost-healthd`
- `bifrost-gpsd`
- `bifrost-syncd` optional

## 21.2 Communication

Use a local Unix socket or lightweight message bus.

Avoid turning the system into a distributed-enterprise cathedral. It is a field router, not a bank.

---

# 22. Signal Survey Engine

The survey engine is the heart of the invention.

## 22.1 Inputs

At each measurement point:

- azimuth;
- timestamp;
- GPS coordinates;
- carrier/operator;
- registration state;
- LTE/NR technology;
- band;
- serving cell/PCI when available;
- RSRP;
- RSRQ;
- SINR/SNR;
- RSSI if exposed;
- channel/bandwidth if exposed;
- modem temperature;
- ping latency;
- ping jitter;
- packet loss;
- short download sample;
- short upload sample;
- DNS resolution time;
- route status.

## 22.2 Coarse survey

Manual or motorized sweep in 30° increments:

`0°, 30°, 60° ... 330°`

At each angle:

- wait 5–15 seconds for RF conditions to settle;
- sample radio metrics several times;
- compute median values;
- perform a small network test;
- store result.

Take the top three directions.

## 22.3 Fine survey

Around each finalist, test ±15° in 5° steps.

Example:

```text
Coarse winner: 90°
Fine scan: 75, 80, 85, 90, 95, 100, 105°
```

## 22.4 Micro-optimization

If the link is extremely marginal, test 2° increments around the winner.

## 22.5 Why settle time matters

Cellular modems may:

- change serving cell;
- change MIMO rank;
- add/remove carrier aggregation;
- transition between LTE and NR;
- adapt modulation;
- change transmit power.

A one-second reading immediately after rotation may be misleading.

---

# 23. Automatic Antenna Aiming

## 23.1 State machine

```text
IDLE
  │
  ▼
HOME
  │
  ▼
COARSE_SWEEP
  │
  ▼
RANK_CANDIDATES
  │
  ▼
FINE_SWEEP
  │
  ▼
THROUGHPUT_CONFIRMATION
  │
  ▼
SELECT_WINNER
  │
  ▼
HOLD
  │
  ├── quality drops ──> RECHECK
  └── user request ───> IDLE
```

## 23.2 Hysteresis

Do not continuously twitch the antenna because one direction scored 1% better for thirty seconds.

Require a meaningful improvement before moving.

Example:

- stay on current bearing unless alternative score exceeds it by 10%;
- or current connection remains degraded for more than 3 minutes.

## 23.3 Time-aware rescans

Networks change with load.

Optional policy:

- quick health check every minute;
- no physical movement while link remains healthy;
- small recheck every 2–6 hours;
- full survey only on explicit request, major degradation, reboot at a new GPS location, or major network transition.

---

# 24. Connection Scoring Algorithm

A simplistic “maximum RSRP wins” algorithm is wrong.

A better score combines radio quality and actual service quality.

## 24.1 Example normalized score

```text
score =
    0.20 * rsrp_score
  + 0.20 * sinr_score
  + 0.10 * rsrq_score
  + 0.15 * latency_score
  + 0.10 * loss_score
  + 0.15 * download_score
  + 0.10 * upload_score
```

Weights should be profile-dependent.

## 24.2 Reliability profile

For messaging, remote admin, coding, and general connectivity:

```text
RSRP      15%
SINR      25%
RSRQ      15%
latency   15%
loss      20%
download   5%
upload     5%
```

## 24.3 Throughput profile

For downloading a large model:

```text
RSRP      10%
SINR      15%
RSRQ       5%
latency    5%
loss      10%
download  45%
upload     10%
```

## 24.4 Stable-score penalty

Add penalties for:

- frequent network detach/reattach;
- LTE/5G flapping;
- large SINR variance;
- repeated packet loss bursts;
- thermal throttling;
- excessive modem resets.

A 60 Mbps link that disconnects every ten minutes is often worse than a stable 18 Mbps link.

---

# 25. Tower and Campsite Memory

Bifröst should remember places.

## 25.1 Campsite identity

Key records by:

- rounded GPS location;
- optional human-readable name;
- date/time;
- season;
- weather note;
- carrier/SIM.

## 25.2 Stored recommendation

Example:

```yaml
site: HoosierNF-Camp-017
latitude: 38.xxxxx
longitude: -86.xxxxx
carrier: example-carrier
preferred:
  azimuth_deg: 82
  technology: LTE
  band: B13
  score: 0.84
alternates:
  - azimuth_deg: 107
    technology: NR5G
    band: n77
    score: 0.79
notes:
  - "B13 slower but stable after rain"
  - "n77 faster after midnight"
```

## 25.3 Arrival behavior

When GPS indicates a previously known campsite:

1. load prior profile;
2. aim to last known best bearing;
3. verify current metrics;
4. only run a full sweep if performance is materially worse.

That cuts setup time dramatically.

---

# 26. Band and Network Strategy

## 26.1 Default behavior

Let the modem and carrier choose bands automatically.

Modern cellular scheduling is complicated. Manual band locking should be a troubleshooting and optimization tool, not a permanent superstition.

## 26.2 When band preference is useful

- 5G NSA flaps constantly;
- low-band LTE is substantially more stable;
- modem camps on a congested cell despite a better alternative;
- a particular rural band is known to be reliable;
- a higher-frequency connection is fast only during certain hours.

## 26.3 Safe software policy

Bifröst should separate:

- **observe**;
- **recommend**;
- **apply reversible preference**.

Any persistent band lock should be:

- recorded in the database;
- visible in the UI;
- easy to clear;
- automatically cleared during recovery mode.

## 26.4 Do not overfit to one campsite

A setting that is brilliant in one forest may be terrible fifty miles away.

---

# 27. Multi-WAN, Failover, and Multiple Carriers

A second carrier may improve real-world availability more than another 3 dB of antenna gain.

## 27.1 Possible WAN sources

- RM520N modem, carrier A;
- second modem, carrier B;
- phone USB tether;
- campground Wi-Fi;
- Ethernet from another router;
- future satellite terminal.

## 27.2 Failover policy

Example:

1. primary cellular;
2. secondary cellular;
3. phone tether;
4. campground Wi-Fi;
5. satellite, depending on data cost and power budget.

## 27.3 OpenWrt mwan3

OpenWrt provides mwan3 tooling for multi-WAN policy routing and failover.

Important distinction:

Multi-WAN is not automatically link bonding. A normal mwan3 configuration routes sessions through selected WANs; it does not magically combine two 10 Mbps links into one 20 Mbps TCP stream.

## 27.4 Bifröst policy layer

Add cost awareness:

```yaml
wan:
  cellular_primary:
    priority: 10
    metered: true
    cost_per_gb: medium
  campground_wifi:
    priority: 20
    metered: false
    trust: low
  satellite:
    priority: 30
    metered: true
    power_cost: high
```

---

# 28. Phone Integration

## 28.1 Primary mode: Wi-Fi client

The phone connects to the Bifröst LAN just like any normal Wi-Fi network.

## 28.2 Wi-Fi Calling

If the phone and carrier support Wi-Fi Calling, ordinary carrier voice/SMS services may work over Bifröst's Internet path even when the phone's direct local cellular signal is poor.

Behavior varies by carrier, plan, handset configuration, emergency-address requirements, and network conditions.

## 28.3 USB tether fallback

If the external modem loses service but the phone has a usable carrier connection:

```text
Phone USB tether
      │
      ▼
Bifröst Pi
      │
      ▼
Camp LAN
```

This turns the phone into a backup WAN.

## 28.4 Local-only mode

Even with no Internet, Bifröst should keep the local network alive for:

- local AI;
- file sharing;
- local web apps;
- media;
- device control;
- maps cached locally;
- system telemetry.

---

# 29. Optional Certified Cellular Booster Co-Pilot

There are two different goals that people often confuse:

1. **Get Internet into camp.**  
   The Bifröst modem + external antenna is ideal.

2. **Make the phone itself receive ordinary cellular service directly.**  
   A certified consumer booster can help.

## 29.1 Current examples

At the time of research:

- weBoost Drive Reach: about **$499.99**, listed with up to 50 dB gain and 12 V / 1.8 A power input.
- SureCall Fusion2Go Max: about **$499.99**, with manufacturer-listed power consumption at or below 10 W.

These are examples, not mandatory recommendations.

## 29.2 Legal use

Consumer booster instructions explicitly require provider registration/consent procedures and use of the approved antenna/cable system.

## 29.3 Smart co-pilot concept

Bifröst does not electrically modify the booster.

Instead it can tell the human:

```text
Best external direction: 074°
Best observed band: B13
RSRP: -101 dBm
SINR: 14 dB
Recommendation: aim booster exterior antenna toward 074°
```

That gives the legal booster a much smarter operator.

---

# 30. Security Architecture

A nomadic router lives on hostile networks by definition.

## 30.1 Minimum firewall policy

- deny unsolicited inbound WAN traffic;
- permit established/related connections;
- management UI only on LAN and VPN;
- no open SSH on cellular WAN;
- disable password SSH once keys are configured;
- separate guest Wi-Fi from infrastructure devices.

## 30.2 Wi-Fi

Use WPA2/WPA3 as supported by clients.

Suggested SSIDs:

- `BIFROST` — trusted devices;
- `BIFROST-GUEST` — isolated clients;
- optional hidden management SSID is not a real security boundary and should not replace authentication.

## 30.3 Tailscale

Tailscale is well-suited for secure remote administration. Its Linux subnet-router documentation requires enabling IP forwarding and advertising selected LAN routes.

Use an ACL that exposes only what is necessary.

## 30.4 Secrets

Keep:

- API keys;
- VPN credentials;
- SIM management credentials;
- dashboard admin tokens

outside world-readable configuration files.

---

# 31. Thermal Design

## 31.1 Heat sources

- Pi SoC;
- 5G modem;
- DC regulator;
- solar loading;
- sealed enclosure.

## 31.2 Temperature monitoring

Log:

- Pi CPU temperature;
- modem temperature if exposed;
- enclosure air temperature;
- regulator temperature if sensor available.

## 31.3 Thermal states

```text
NORMAL
  < warning threshold

WARM
  increase fan / reduce background workloads

HOT
  pause speed tests / AI jobs / motor sweeps

CRITICAL
  preserve connectivity, shut down nonessential services
```

Do not choose thresholds without checking the limits of the exact components used.

---

# 32. Power Budget and Runtime Estimates

## 32.1 Design envelope

A sensible electrical design should allow approximately **30–40 W peak system input** for the complete remote head even though normal average use may be much lower.

That headroom covers:

- Pi load spikes;
- modem RF transmit demand;
- conversion losses;
- USB/peripheral demand;
- fan;
- temporary motor/control electronics if shared.

## 32.2 Typical planning range

For rough field planning, assume:

- low/idle routing: **10–15 W**;
- active cellular traffic + Pi services: **15–25 W**;
- short peaks: potentially higher.

These are system planning values, not manufacturer guarantees.

## 32.3 Battery runtime formula

```text
runtime_hours = battery_Wh × usable_fraction / average_load_W
```

If the battery is 1,000 Wh and 85% of its rated energy is practically available to the load:

### 15 W average

```text
1000 × 0.85 / 15 ≈ 56.7 hours
```

### 20 W average

```text
1000 × 0.85 / 20 ≈ 42.5 hours
```

### 25 W average

```text
1000 × 0.85 / 25 ≈ 34 hours
```

A 200 Wh battery under the same assumptions gives approximately:

- 11.3 h at 15 W;
- 8.5 h at 20 W;
- 6.8 h at 25 W.

Real runtime depends on regulator efficiency, battery temperature, battery age, DC vs AC conversion, modem behavior, Wi-Fi load, and solar input.

## 32.4 Avoid AC inversion if possible

If the power station has regulated DC output, feed the Bifröst DC conversion system directly rather than doing:

```text
battery DC → inverter AC → wall adapter DC
```

Every unnecessary conversion wastes energy.

---

# 33. Solar Operation

## 33.1 Daily energy demand

A 20 W average load for 24 hours consumes:

```text
20 W × 24 h = 480 Wh/day
```

A 15 W average consumes:

```text
360 Wh/day
```

## 33.2 100 W panel reality

A 100 W panel does not normally produce 100 W for 24 hours.

For rough planning, 3–5 equivalent peak-sun-hours can yield approximately 300–500 Wh before real-world losses under favorable conditions.

Thus a 100 W panel may roughly support a carefully power-managed Bifröst node in good weather, but it is not a guaranteed perpetual-power solution.

## 33.3 Better strategy

- run full-power surveys only when needed;
- disable unnecessary HDMI;
- allow modem/Pi low-power states when idle;
- schedule large downloads around strong solar production;
- use battery capacity to bridge night and clouds;
- consider 200 W of deployed solar for comfortable margin if Bifröst becomes an always-on camp infrastructure device.

---

# 34. Bill of Materials

Prices are planning ranges in USD and can change rapidly.

## 34.1 Core compute and modem

| Item | Recommended specification | Planning price |
|---|---|---:|
| Raspberry Pi 5 | 4 GB+ | $60–$120 new depending RAM |
| Pi active cooler | official or equivalent | $5–$20 |
| Boot storage | 64–128 GB endurance microSD | $10–$25 |
| 5G carrier/HAT | Pi 5 compatible M.2 Key-B | $30–$80 without modem, product dependent |
| Quectel RM520N-GL | global 5G Sub-6 M.2 modem | roughly $250–$300 retail module-only listings |
| HAT + modem bundle | Waveshare or equivalent | roughly $300–$380+ depending bundle/region |

## 34.2 Antenna and RF

| Item | Specification | Planning price |
|---|---|---:|
| 4×4 MIMO directional panel | 617–4200 MHz class, ~11 dBi peak | $365–$440 for XPOL-24 class |
| Four short low-loss jumpers | correct N/SMA/I-PEX chain | $40–$120 |
| RF weather boots/tape | outdoor sealing | $10–$30 |
| RF strain relief | mechanical support | $10–$30 |

## 34.3 Mast and mechanics

| Item | Specification | Planning price |
|---|---|---:|
| Fiberglass mast | 20–30 ft portable | $70–$200+ |
| Drive-over base/tripod | stable mast support | $40–$150 |
| Guy kit | line, stakes, tensioners | $20–$60 |
| Antenna bracket | adjustable az/el | often included / $20–$80 |

## 34.4 Remote head

| Item | Specification | Planning price |
|---|---|---:|
| Polycarbonate enclosure | weather resistant | $30–$90 |
| Cable glands | Ethernet, DC, RF as required | $10–$30 |
| ePTFE vent | pressure equalization | $5–$20 |
| DC buck converter | wide input, clean 5.1 V / 5 A+ | $20–$50 |
| Fuse holder + fuses | automotive DC | $5–$15 |
| Outdoor Ethernet | 25–50 ft | $15–$35 |
| DC cable | appropriate gauge | $15–$40 |
| temp/humidity sensor | optional | $5–$20 |

## 34.5 Motorized upgrade

| Item | Specification | Planning price |
|---|---|---:|
| 12 V worm-gear motor | slow, high torque | $25–$60 |
| motor driver | current-rated H-bridge | $10–$30 |
| RP2040/Pico controller | real-time safety controller | $5–$15 |
| absolute encoder | shaft/azimuth feedback | $10–$50 |
| limit/home switches | sealed preferred | $5–$20 |
| gears/belt/bearings | mechanical drive | $30–$100 |
| weatherproof motor box | outdoor | $20–$60 |

---

# 35. Cost Estimates by Build Tier

## 35.1 Reusing an existing Pi

If a Pi 5 and battery system already exist, the expensive additions are primarily modem + antenna + mast.

### Compact gateway

- modem/HAT: $300–$380
- enclosure/power/cables: $60–$150

**Estimated added cost: $360–$530**

### Manual long-range node

Add:

- directional 4×4 antenna: $365–$440
- mast/mechanics: $130–$300
- improved remote-head cabling: $50–$150

**Estimated added cost beyond existing Pi/power: approximately $800–$1,250 at the high end, but careful sourcing can reduce this.**

Because some HAT bundles include antennas, cables, or cases, do not simply add every line item mechanically.

## 35.2 Full system from nothing

A realistic planning envelope:

- **Basic:** $450–$650
- **Serious manual weak-signal:** $700–$1,100
- **Motorized expedition:** $900–$1,500+
- **Dual carrier:** add $250–$450+ plus service
- **Certified phone booster:** add about $500

---

# 36. Wiring Diagrams

## 36.1 Manual long-range remote-head wiring

```text
                4×4 MIMO PANEL
              ┌───┬───┬───┬───┐
              A   B   C   D
              │   │   │   │
              │ short RF jumpers
              │   │   │   │
      ┌───────▼───▼───▼───▼─────────┐
      │ WEATHERPROOF REMOTE HEAD      │
      │                              │
      │  RM520N-GL + HAT             │
      │          │                   │
      │      Raspberry Pi 5          │
      │          │                   │
      │      Ethernet PHY            │
      │          │                   │
      │  5.1 V buck converter        │
      └─────┬───────────────┬────────┘
            │               │
          Ethernet        12 V DC
            │               │
            │               │
      ┌─────▼───────────────▼───────┐
      │ GROUND / VEHICLE EQUIPMENT  │
      │                             │
      │ battery/power station       │
      │ fuse                        │
      │ Ethernet switch / AP        │
      └─────────────────────────────┘
```

## 36.2 Motorized version

```text
Pi 5 ──USB/UART──> RP2040 controller ──> motor driver ──> rotator motor
                         ▲                    │
                         │                    └─ current sense optional
                         │
              encoder / home / limits
```

## 36.3 Dual-WAN version

```text
Carrier A modem ─┐
                 ├── Bifröst routing core ── LAN/Wi-Fi
Carrier B modem ─┤
Phone tether ────┤
Camp Wi-Fi ──────┘
```

---

# 37. Connector and Cable Plan

## 37.1 RF

Avoid adapter chains.

Bad:

```text
antenna → adapter → adapter → pigtail → adapter → modem
```

Every connector adds loss and failure points.

Preferred:

```text
antenna N-female
      │
short N-male to SMA-male low-loss cable
      │
HAT SMA-female
      │
short internal pigtail
      │
RM520N antenna port
```

The exact chain depends on the HAT and antenna variant.

## 37.2 Ethernet

Use a rugged outdoor-rated Ethernet lead for the mast run.

Add a service loop near both ends, but do not leave a giant flapping coil at the antenna.

## 37.3 DC

Use an appropriate gauge based on:

- cable length;
- voltage;
- maximum current;
- acceptable drop.

Provide strain relief so cable weight does not pull on the board connector.

---

# 38. Software Directory Layout

```text
/opt/bifrost-reach/
├── bin/
│   ├── bifrost-core
│   ├── bifrost-scan
│   ├── bifrost-net
│   └── bifrost-cli
├── config/
│   └── bifrost.toml
├── src/
│   ├── modem/
│   ├── survey/
│   ├── rotator/
│   ├── network/
│   ├── gps/
│   ├── storage/
│   └── web/
├── web/
│   ├── static/
│   └── templates/
├── migrations/
├── scripts/
│   ├── recovery.sh
│   ├── export-site.sh
│   └── modem-reset.sh
└── README.md

/var/lib/bifrost-reach/
├── bifrost.db
├── exports/
└── cache/

/var/log/bifrost-reach/
├── core.log
├── modem.log
├── survey.log
└── health.log
```

---

# 39. Database Schema

SQLite is ideal for v1.

## 39.1 Sites

```sql
CREATE TABLE sites (
    id INTEGER PRIMARY KEY,
    name TEXT,
    latitude REAL NOT NULL,
    longitude REAL NOT NULL,
    radius_m REAL DEFAULT 250,
    created_at TEXT NOT NULL,
    updated_at TEXT NOT NULL
);
```

## 39.2 Surveys

```sql
CREATE TABLE surveys (
    id INTEGER PRIMARY KEY,
    site_id INTEGER,
    started_at TEXT NOT NULL,
    ended_at TEXT,
    carrier TEXT,
    sim_slot INTEGER,
    profile TEXT,
    FOREIGN KEY(site_id) REFERENCES sites(id)
);
```

## 39.3 Samples

```sql
CREATE TABLE samples (
    id INTEGER PRIMARY KEY,
    survey_id INTEGER NOT NULL,
    captured_at TEXT NOT NULL,
    azimuth_deg REAL,
    technology TEXT,
    band TEXT,
    cell_id TEXT,
    pci INTEGER,
    rsrp_dbm REAL,
    rsrq_db REAL,
    sinr_db REAL,
    rssi_dbm REAL,
    ping_ms REAL,
    jitter_ms REAL,
    packet_loss REAL,
    down_mbps REAL,
    up_mbps REAL,
    modem_temp_c REAL,
    score REAL,
    FOREIGN KEY(survey_id) REFERENCES surveys(id)
);
```

## 39.4 Known configurations

```sql
CREATE TABLE preferred_links (
    id INTEGER PRIMARY KEY,
    site_id INTEGER NOT NULL,
    carrier TEXT NOT NULL,
    azimuth_deg REAL,
    technology TEXT,
    band TEXT,
    score REAL,
    confidence REAL,
    last_verified TEXT,
    notes TEXT,
    FOREIGN KEY(site_id) REFERENCES sites(id)
);
```

---

# 40. Configuration File Example

```toml
[system]
name = "bifrost-reach-01"
mode = "stationary"
database = "/var/lib/bifrost-reach/bifrost.db"

[modem]
backend = "modemmanager"
modem_index = 0
allow_band_preferences = false
reset_on_hard_failure = true

[survey]
coarse_step_deg = 30
fine_step_deg = 5
settle_seconds = 10
samples_per_angle = 5
run_speed_test = true
max_test_megabytes = 20

[scoring.reliability]
rsrp = 0.15
rsrq = 0.15
sinr = 0.25
latency = 0.15
loss = 0.20
download = 0.05
upload = 0.05

[rotator]
enabled = false
controller = "/dev/ttyACM0"
min_deg = 0
max_deg = 350
home_deg = 0
max_speed_deg_s = 4

[network]
lan_interface = "eth0"
wifi_interface = "wlan0"
health_target_1 = "1.1.1.1"
health_target_2 = "8.8.8.8"

[thermal]
warn_c = 70
hot_c = 78
critical_c = 83

[web]
bind = "10.77.0.1"
port = 8080
```

The temperatures are placeholders and must be set based on actual component limits and enclosure testing.

---

# 41. Service Architecture

Example systemd services:

```text
bifrost-modemd.service
bifrost-surveyd.service
bifrost-rotatord.service
bifrost-netd.service
bifrost-web.service
bifrost-healthd.service
```

## 41.1 Dependency idea

```text
network-pre.target
       │
       ▼
bifrost-modemd
       │
       ├──> bifrost-netd
       │
       └──> bifrost-surveyd
                 │
                 └──> bifrost-rotatord

bifrost-web waits for database + core API
```

## 41.2 Watchdog

Enable the Pi hardware watchdog where appropriate.

Each daemon should:

- emit health status;
- restart cleanly;
- avoid rebooting the whole machine for minor faults;
- rate-limit modem resets.

---

# 42. Installation Outline: Raspberry Pi OS Lite

This is an architecture-level outline, not a frozen copy/paste installer. Package names and interface behavior should be verified against the image actually installed.

## 42.1 Base image

Use current Raspberry Pi OS Lite 64-bit.

## 42.2 Initial configuration

- enable SSH;
- set unique hostname;
- create SSH key access;
- update packages;
- set timezone;
- configure persistent logging limits;
- configure Wi-Fi AP only after modem connectivity works.

## 42.3 Modem stack

Install:

- ModemManager;
- libqmi tools;
- libmbim tools;
- NetworkManager or chosen network stack;
- USB/serial kernel support as required by carrier board;
- MHI support if using PCIe mode.

## 42.4 Verify hardware

Before automating anything:

1. confirm modem enumerates;
2. read modem identity;
3. verify SIM detected;
4. verify registration;
5. connect data session;
6. verify DNS;
7. verify IPv4/IPv6;
8. record baseline signal metrics;
9. test all four antenna connections physically.

## 42.5 Only then install Bifröst software

Never debug custom software and basic modem enumeration simultaneously if you can avoid it.

---

# 43. Installation Outline: OpenWrt

At the time of research, OpenWrt lists Raspberry Pi 5 support in current releases.

## 43.1 Useful package classes

- ModemManager;
- libqmi;
- libmbim;
- relevant USB serial/network kernel modules;
- mwan3;
- LuCI mwan3 package if desired;
- Tailscale package if supported by the selected release/image;
- SQLite only if using the intelligence layer directly on OpenWrt.

## 43.2 Architecture option

A clean split is:

```text
OpenWrt = routing/firewall/WAN
Bifröst daemon = signal intelligence/database/rotator
```

The daemon can be written in Go or Rust for a small dependency footprint.

---

# 44. Signal Measurement Commands

Commands vary by modem mode and software stack.

## 44.1 ModemManager

Typical discovery:

```bash
mmcli -L
mmcli -m 0
```

Enable periodic signal reporting where supported:

```bash
mmcli -m 0 --signal-setup=5
mmcli -m 0 --signal-get
```

Modern ModemManager output supports LTE and 5G signal fields including RSRP, RSRQ, and SNR where the modem/backend exposes them.

## 44.2 QMI

Typical inspection:

```bash
qmicli -d /dev/cdc-wdm0 --nas-get-signal-strength
qmicli -d /dev/cdc-wdm0 --nas-get-signal-info
qmicli -d /dev/cdc-wdm0 --nas-get-serving-system
qmicli -d /dev/cdc-wdm0 --nas-get-system-info
```

Device paths vary.

## 44.3 AT command interface

Use AT commands for modem-specific diagnostics only after identifying the correct serial port.

Do not spray commands at every `/dev/ttyUSB*` device blindly.

Maintain a read-only diagnostic mode in Bifröst by default.

---

# 45. Survey and Aiming Pseudocode

## 45.1 Coarse survey

```python
for angle in range(0, 360, 30):
    rotator.move_to(angle)
    wait_for_motion_stop()
    sleep(10)

    radio = median(read_radio_metrics(5))
    net = run_small_network_test()

    score = score_link(radio, net, profile="reliability")
    store(angle, radio, net, score)

candidates = top_results(3)
```

## 45.2 Fine scan

```python
fine = []
for candidate in candidates:
    for angle in around(candidate.angle, radius=15, step=5):
        move_and_measure(angle)
        fine.append(result)

winner = select_best_stable(fine)
rotator.move_to(winner.angle)
```

## 45.3 Stability confirmation

```python
hold(winner.angle)
observe_for(minutes=3)

if stability_score < minimum:
    choose_next_best_candidate()
else:
    save_preferred_link()
```

---

# 46. Field Operating Procedure

## 46.1 Arrival

1. Check for overhead power lines.
2. Check wind.
3. Check whether a storm is approaching.
4. Place mast base.
5. Assemble antenna at low height.
6. Connect RF cables before fully extending mast.
7. Connect remote head.
8. Connect Ethernet and DC.
9. Power Bifröst.
10. Verify LAN access.
11. Verify modem registration.
12. Extend mast.
13. Run survey.
14. Save result.

## 46.2 If the signal is unexpectedly poor

Try, in order:

1. rotate antenna 90° and recheck;
2. verify all four RF connectors;
3. lower and inspect mast-head cables;
4. move the whole mast 10–30 feet horizontally;
5. increase mast height;
6. reduce mast height slightly if multipath is pathological;
7. try LTE-only temporarily;
8. try alternate carrier;
9. move vehicle/camp to higher ground if practical.

Horizontal movement can matter more than intuition suggests because local reflections and obstructions create small-scale hot and dead spots.

---

# 47. Fast Deployment Procedure

For an overnight stop where a 30-minute RF ritual would be ridiculous:

1. use compact omni or antenna at 6–10 ft;
2. power Bifröst;
3. run 4-direction scan only: N/E/S/W;
4. choose best;
5. stop optimizing once connection clears a target threshold.

Example threshold:

- packet loss under 2%;
- ping stable;
- 5 Mbps down;
- 1 Mbps up.

Once the link is “good enough,” sleep instead of hunting another 8 Mbps at 2 AM.

---

# 48. Storm, Wind, and Lightning Procedure

A tall outdoor communications mast is not something to negotiate with during lightning.

## 48.1 If thunderstorms approach

- stop survey;
- disconnect power;
- lower the mast well before the storm reaches the site;
- move the system to safe storage;
- do not handle mast or outdoor cabling during active lightning nearby.

## 48.2 Power lines

Never erect the mast where any failure direction could contact overhead power lines.

Fiberglass does not make an installed mast system magically safe around utility lines.

## 48.3 Wind

Lower the mast if wind exceeds the tested safe limit of the complete assembly.

Do not use the antenna manufacturer's survivable wind number as permission to leave a portable improvised mast up in a storm.

---

# 49. Troubleshooting

## 49.1 Modem not detected

Check:

- HAT power;
- USB/PCIe selection switches;
- PCIe FFC orientation;
- kernel logs;
- `lsusb` / `lspci`;
- MHI drivers for PCIe mode;
- modem reset line;
- SIM seating;
- carrier-board documentation.

## 49.2 SIM detected but no registration

Check:

- antenna connections;
- carrier compatibility;
- plan activation;
- APN;
- modem mode;
- supported bands;
- carrier IMEI acceptance;
- local coverage.

## 49.3 Strong RSRP, terrible Internet

Likely suspects:

- poor SINR;
- congestion;
- packet loss;
- bad RSRQ;
- backhaul congestion;
- throttling/deprioritization;
- DNS problems.

Rotate for better quality, not stronger signal.

## 49.4 Good download, awful upload

Possible causes:

- downlink can hear tower but modem uplink is marginal;
- antenna path issue;
- carrier scheduling;
- band combination;
- network congestion;
- excessive coax loss particularly hurting transmitted power at the antenna.

This is another reason to keep modem-to-antenna cables short.

## 49.5 5G is worse than LTE

That is entirely possible.

Use LTE if it is more stable. A “5G” icon is not a sacred rune of bandwidth.

## 49.6 Frequent modem resets

Check:

- power rail sag;
- thermal condition;
- HAT regulator;
- USB cable quality;
- firmware;
- kernel logs;
- RF connector problems.

Power instability is a prime suspect whenever failure correlates with heavy upload activity.

---

# 50. Failure Modes and Graceful Degradation

| Failure | Desired behavior |
|---|---|
| antenna motor fails | manual aiming still works |
| encoder fails | home switch + manual mode |
| 5G unstable | fall back to LTE |
| primary carrier down | secondary WAN |
| Internet down | LAN remains operational |
| dashboard crashes | routing continues |
| database corrupted | use last config backup, rebuild metrics DB |
| modem wedges | controlled modem reset |
| Pi overheats | stop nonessential jobs |
| remote head loses power | ground router reports mast offline |
| GPS unavailable | site can be selected manually |

---

# 51. Testing Plan

## 51.1 Bench test

Before field deployment:

- 24-hour idle run;
- sustained download test;
- sustained upload test;
- repeated modem reconnect cycles;
- Pi reboot recovery;
- power removal/reapply;
- antenna disconnect detection if possible;
- database durability test.

## 51.2 Thermal soak

Test enclosure in sun-like conditions without exceeding component limits.

Record:

- enclosure temperature;
- Pi CPU temperature;
- modem temperature;
- regulator temperature;
- fan behavior.

## 51.3 Voltage-drop test

Run full mast-length DC cable.

Measure:

- voltage at source;
- voltage at remote-head regulator input;
- voltage at Pi under heavy upload;
- minimum transient voltage if instrumentation permits.

## 51.4 RF comparison test

At a marginal site compare:

1. phone alone;
2. modem with stock antennas in car;
3. modem with panel at car-roof height;
4. panel at 10 ft;
5. panel at 20–30 ft;
6. best manual aim;
7. optional booster phone result.

Record actual throughput and radio metrics.

## 51.5 Rotator test

Run 100+ full sweep cycles at ground level before trusting it on a mast.

Verify:

- no cable binding;
- repeatable home;
- encoder accuracy;
- emergency stop;
- hard limits;
- wind holding behavior.

---

# 52. Performance Expectations

## 52.1 What success may look like

Examples of realistic wins:

- phone shows intermittent service but Bifröst maintains a stable LTE connection;
- phone has unusable data in the car but mast antenna provides several Mbps;
- omni antenna sees strong but noisy signal while directional panel improves SINR enough to stabilize service;
- LTE becomes preferable to unstable 5G;
- a second carrier provides service when the primary is dead.

## 52.2 What not to promise

Do not promise:

- a specific distance to a tower;
- 27-mile service because an antenna marketing page mentions a theoretical coverage figure;
- 5G in every rural area;
- perfect voice service over Wi-Fi calling;
- connectivity through a mountain;
- a fixed dB improvement at every location.

Radio propagation is terrain- and network-dependent.

## 52.3 The best metric

The final metric is not antenna gain or bars.

It is:

> **Can the system maintain the applications you actually need, at this location, for hours rather than seconds?**

---

# 53. Future Upgrades

## 53.1 Second modem

Add a second RM520N-class modem or lower-cost LTE modem on another carrier.

## 53.2 2.5 GbE

Not necessary for rural links, but potentially useful if the Pi becomes a high-speed local server.

## 53.3 External Wi-Fi AP

Put a dedicated AP at ground level while the Pi remains at the mast head.

Advantages:

- better local Wi-Fi placement;
- remote head can focus on cellular;
- easier Wi-Fi upgrades.

## 53.4 Smart power scheduling

Use solar forecast and battery state to choose when to:

- scan;
- download models;
- run backups;
- run AI workloads.

## 53.5 Terrain-aware predictions

Given GPS coordinates and known tower bearings, Bifröst can cache elevation profiles and predict likely azimuths before scanning.

## 53.6 Local AI advisor

A local small model could interpret telemetry:

> “The current B13 link is weaker than last night but SINR is still healthy. Do not rotate. The throughput reduction looks like congestion rather than antenna misalignment.”

The AI should recommend. Deterministic safety code should control motors and power.

---

# 54. Bifröst Reach Node v2 Possibilities

## 54.1 Dual-antenna diversity system

One high-gain directional panel plus one omni.

Use cases:

- omni for fast acquisition;
- panel for steady-state performance;
- automatic selection by environment.

## 54.2 Electronically switched antenna bank

Instead of physically rotating one panel, mount several directional antennas pointing different directions and switch RF paths using properly rated RF switching hardware.

This becomes expensive at 4×4 MIMO because four synchronized RF paths are required.

## 54.3 Distributed RF heads

Place two independent modem heads:

- one high on mast;
- one on vehicle;

Then route between them at IP level.

This is cleaner than attempting to merge RF signals.

## 54.4 Mesh camp network

Bifröst becomes the WAN gateway for:

- Pi compute node;
- local AI node;
- storage node;
- environmental sensors;
- security cameras;
- VR/entertainment devices.

## 54.5 Satellite-aware routing

If a satellite terminal is later added, Bifröst can choose traffic by policy:

```text
messaging / SSH / telemetry → cheapest reliable cellular
large download → fastest link
critical update → any available link
background sync → only unmetered or solar-surplus window
```

---

# 55. Recommended Build Sequence

Do not buy every component at once and then debug the entire machine as one gigantic mystery.

## Phase 1: Modem bench node

Build:

- Pi 5;
- RM520N-GL;
- HAT;
- supplied antennas;
- Raspberry Pi OS Lite;
- ModemManager.

Success criteria:

- stable Internet;
- metrics readable;
- modem survives long transfers.

## Phase 2: Directional antenna at low height

Add panel antenna on a temporary pole at 6–10 ft.

Success criteria:

- prove directional gain/quality benefit;
- learn aiming behavior;
- validate all four RF chains.

## Phase 3: Remote head

Move Pi/modem close to antenna.

Success criteria:

- Ethernet and DC stable over full cable length;
- no undervoltage;
- thermal behavior acceptable.

## Phase 4: Full mast

Deploy 20–30 ft mast.

Success criteria:

- one-person safe setup;
- repeatable guying;
- meaningful RF improvement at at least one test site.

## Phase 5: Bifröst survey software

Automate:

- metric collection;
- database;
- manual-angle recording;
- ranking.

Success criteria:

- software picks same direction an experienced human would pick.

## Phase 6: Motorization

Only after manual aiming is understood.

Success criteria:

- 100+ sweep-cycle bench reliability;
- zero cable-wrap incidents;
- hard limit protection independent of Linux.

## Phase 7: Multi-carrier

Add second WAN only when the primary system is boringly reliable.

---

# 56. Final Recommended Configuration

If building one version now, build this:

## Bifröst Reach Node v1 “Ranger”

### Compute

- Raspberry Pi 5
- active cooling
- 64–128 GB endurance storage
- Raspberry Pi OS Lite 64-bit

### Cellular

- Quectel RM520N-GL
- Pi 5 compatible 5G M.2 carrier/HAT
- Nano SIM
- ModemManager + QMI/MBIM tools

### Antenna

- Poynting XPOL-24 class 4×4 MIMO directional panel
- shortest practical low-loss RF runs
- four clearly labeled antenna paths

### Mast

- 20–30 ft fiberglass telescoping mast
- drive-over base or tripod
- three-point guying
- manual azimuth scale

### Remote head

- weather-resistant polycarbonate enclosure
- light exterior color
- pressure vent
- temperature telemetry
- local 5.1 V high-current buck converter
- Ethernet down mast
- fused 12 V DC feed

### Network

- Ethernet to vehicle/camp switch or travel router
- trusted and guest Wi-Fi
- local dashboard
- Tailscale optional
- IPv4 + IPv6

### Software

- `bifrost-modemd`
- `bifrost-surveyd`
- `bifrost-netd`
- `bifrost-web`
- SQLite campsite/tower history
- manual aiming first
- motor controller interface designed but disabled in v1

### Upgrade path

1. motorized rotator;
2. second carrier;
3. certified booster co-pilot;
4. solar-aware scheduling;
5. local AI advisor;
6. optional satellite WAN.

This configuration captures most of the real RF benefit before adding the mechanical complexity of automatic rotation.

---

# 57. Reference Sources

The following sources were consulted during preparation. Specifications and pricing can change; verify exact current product variants before purchasing.

## Raspberry Pi

**Raspberry Pi 5 Product Brief**  
Raspberry Pi Ltd.  
https://datasheets.raspberrypi.com/rpi5/raspberry-pi-5-product-brief.pdf

**Raspberry Pi documentation: power requirements**  
https://www.raspberrypi.com/documentation/

Key facts used:

- BCM2712 2.4 GHz quad-core Cortex-A76;
- dual-band 802.11ac Wi-Fi;
- Gigabit Ethernet;
- 2 × USB 3.0;
- PCIe 2.0 ×1 exposed interface;
- 5 V/5 A recommended Pi 5 power class.

## Quectel

**Quectel RM520N-GL Hardware Design, Version 1.0**  
https://forums.quectel.com/uploads/short-url/1zkjPRnxF5BZ2woox386baCZx4g.pdf

**Quectel RM520N Series Product Page**  
https://www.quectel.com/product/5g-rm520n-series/

Key facts used:

- supported LTE and 5G bands;
- four antenna interfaces;
- 4×4 MIMO support on many bands;
- USB/PCIe host support;
- MBIM/QMI/QRTR/AT support;
- GNSS support;
- 3.7 V typical module supply;
- published reference current-consumption examples;
- recommended antenna cable insertion-loss limits.

## Waveshare

**Waveshare PCIe to 5G HAT+ / RM520N-GL 5G HAT+**  
https://www.waveshare.com/product/rm520n-gl-5g-hat-plus.htm

Key facts used:

- Pi 5 PCIe connectivity;
- M.2 Key-B modem support;
- RM5XX compatibility;
- USB-C interface option;
- Nano-SIM slot;
- Raspberry Pi OS and OpenWrt support claims;
- onboard power monitoring.

## Poynting

**Poynting XPOL-24**  
https://poynting.tech/antennas/xpol-24/

Key facts used:

- 617–4200 MHz class coverage;
- 4×4 MIMO;
- directional design;
- peak gain up to 11 dBi;
- IP65 enclosure rating;
- cable/no-cable variants.

## OpenWrt

**OpenWrt Raspberry Pi 5 hardware data**  
https://openwrt.org/toh/hwdata/raspberry_pi_foundation/raspberry_pi_foundation_raspberry_pi_5

**OpenWrt WWAN / ModemManager documentation**  
https://openwrt.org/docs/guide-user/network/wan/wwan/modemmanager

**OpenWrt Multi-WAN documentation**  
https://openwrt.org/docs/guide-user/network/wan/multiwan/start

Key facts used:

- Raspberry Pi 5 supported in current OpenWrt release family;
- ModemManager availability;
- QMI/MBIM package support;
- mwan3 multi-WAN architecture.

## ModemManager / libqmi

**ModemManager project**  
https://www.freedesktop.org/wiki/Software/ModemManager/

**qmicli manual**  
https://www.freedesktop.org/software/libqmi/man/latest/qmicli.1.html

Key facts used:

- modem control abstraction;
- signal-information retrieval;
- QMI NAS signal and serving-system commands.

## Tailscale

**Configure a subnet router**  
https://tailscale.com/docs/features/subnet-routers/how-to/setup

Key facts used:

- Linux IP forwarding requirement;
- route advertisement model.

## FCC

**FCC KDB 935210: Signal Booster / Amplifier guidance**  
https://apps.fcc.gov/oetcf/kdb/forms/FTSSearchResultPage.cfm?id=20673&switch=P

**FCC 18-35**  
https://docs.fcc.gov/public/attachments/FCC-18-35A1.pdf

Key points used:

- consumer signal boosters operate under FCC rules and Network Protection Standard requirements;
- registration/consent framework;
- approved antenna/cable requirements;
- requirement to cease operation if harmful interference is identified.

## Certified booster examples

**weBoost Drive Reach**  
https://www.weboost.com/products/drive-reach

Research-time listing:

- approximately $499.99;
- manufacturer lists 50 dB max gain;
- 12 V / 1.8 A power specification.

**SureCall Fusion2Go Max**  
https://surecall.com/car-boosters/fusion2go-max/

Research-time listing:

- approximately $499.99;
- manufacturer lists ≤10 W power consumption;
- consumer-device registration/consent warning included by manufacturer.

---

# Appendix A: Engineering Rules of Thumb

1. **Height first, gain second, software third.**
2. **Short cellular coax beats long fancy cellular coax.**
3. **Measure SINR, not just RSRP.**
4. **Test throughput, but do not waste metered data.**
5. **LTE is allowed to win.**
6. **One reliable second carrier may outperform an expensive antenna upgrade.**
7. **Do not motorize what you have not learned to aim manually.**
8. **Keep motor safety below Linux in a microcontroller.**
9. **Never let a software bug rotate through the cable stop.**
10. **Do not erect a mast near power lines or during lightning.**
11. **Do not build an uncertified cellular repeater.**
12. **Make every persistent optimization reversible.**
13. **Store what worked at each campsite.**
14. **Optimize for hours of stable usefulness, not a five-second speed-test trophy.**
15. **When there is truly no terrestrial RF path, change location, carrier, elevation, or technology.**

---

# Appendix B: Minimum Viable Bifröst v1 Checklist

```text
[ ] Pi 5 boots reliably
[ ] modem recognized
[ ] SIM recognized
[ ] LTE data works
[ ] 5G data works where available
[ ] RSRP readable
[ ] RSRQ readable
[ ] SINR/SNR readable
[ ] band/technology readable
[ ] four antenna paths verified
[ ] directional panel tested at ground level
[ ] remote-head DC stable under upload
[ ] Ethernet stable at full mast-cable length
[ ] enclosure temperature acceptable
[ ] mast can be deployed safely by one person
[ ] manual bearing can be recorded
[ ] survey results stored in SQLite
[ ] previous campsite profile can be recalled
[ ] LAN survives WAN loss
[ ] modem can be reset without full Pi reboot
[ ] system survives power cycle
[ ] recovery configuration clears experimental band preferences
[ ] configuration backed up
```

---

# Appendix C: Suggested Dashboard

```text
┌──────────────── BIFRÖST REACH NODE ────────────────┐
│ Site: Forest Camp 017        Bearing: 082°         │
│ Carrier: Example             Mode: LTE             │
│ Band: B13                    Cell: 12345            │
│                                                     │
│ RSRP      -98 dBm      ███████░░░                  │
│ RSRQ      -10 dB       ███████░░░                  │
│ SINR       14 dB       ████████░░                  │
│                                                     │
│ Ping       48 ms                                     │
│ Loss       0.3%                                      │
│ Down       22.4 Mbps                                 │
│ Up          5.8 Mbps                                 │
│                                                     │
│ Link Score: 84 / 100    STABLE                      │
│                                                     │
│ [ QUICK SCAN ] [ FULL SCAN ] [ SAVE SITE ]         │
│ [ LTE AUTO   ] [ WAN STATUS ] [ POWER ]             │
└─────────────────────────────────────────────────────┘
```

---

# Appendix D: A Future “AI Seer” Layer

A future local agent could read structured telemetry rather than raw arbitrary shell access.

Input:

```json
{
  "site": "Forest Camp 017",
  "bearing": 82,
  "rsrp": -98,
  "rsrq": -10,
  "sinr": 14,
  "latency_ms": 48,
  "loss_percent": 0.3,
  "down_mbps": 22.4,
  "up_mbps": 5.8,
  "trend": "stable",
  "last_scan_hours": 7.4
}
```

Possible response:

```text
Recommendation: keep the present bearing. Radio quality remains healthy.
The modest throughput decline appears consistent with network load rather
than antenna misalignment. Re-scan only if SINR falls below the configured
threshold or packet loss rises materially.
```

The AI never receives unrestricted direct authority over:

- raw RF transmission;
- motor hard limits;
- battery safety cutoffs;
- firewall root policy.

Those remain deterministic.

---

# Appendix E: Why Bifröst Is More Than a “Signal Booster”

A conventional booster asks one question:

> “How can I make this RF signal stronger?”

Bifröst asks a richer set:

- Which carrier is best here?
- Which tower direction is best?
- Which direction has the cleanest signal?
- Is the link weak or merely congested?
- Is LTE currently better than 5G?
- Is the mast actually helping?
- Did wet foliage change the optimum bearing?
- Is upload power the limiting factor?
- Did a cable fail?
- Which setting worked here last time?
- Is there enough battery to keep optimizing?
- Should traffic fail over to another WAN?
- Should the system stop moving because the connection is already good enough?

That is the real invention.

The Bifröst Reach Node is not merely a louder radio. It is a **radio-aware field network that learns how to use the landscape.**

---

---

## Contributors

---

![https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/660262974_1643999840123485_7514919576143109031_n.jpg](https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/660262974_1643999840123485_7514919576143109031_n.jpg)

---

* **Volmarr Wyrd** — Vision, direction, sacred coding philosophy, testing

> Volmarr Wyrd is a software architect and AI developer operating at the intersection of open-source technology and esoteric philosophy, specializing in agentic systems and local intelligence. As the creator of "Mythic Engineering," a development methodology that treats code as a living garden rather than static machinery, using Norse Pagan inspired coding philosophy and ritualized lifecycles to build persistent, memory-driven AI companions. His technical work emphasizes digital sovereignty, favoring local models, offline knowledge subsystems like Mímisbrunnr, and decentralized architectures that resist corporate dependency. Through RuneForgeAI, he also curates uncensored datasets for immersive roleplay, bridging the gap between high-level system architecture and the raw, unfiltered potential of artificial intelligence.

---

![https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/Gemini_AI_Picture1.png](https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/Gemini_AI_Picture1.png)

---

* **Gemini AI** - Architecture, code, documentation

> Gemini AI is an advanced multimodal digital intelligence engineered to serve as a versatile technical collaborator, software development partner, and analytical engine. Built to integrate seamlessly across complex codebases, multi-language scripting environments, and modern development workflows, it bridges the gap between high-level conceptual design and precise code execution. Whether optimizing backend infrastructure, debugging intricate software logic, or assisting with open-source project architecture, Gemini operates as a dynamic digital agent designed to accelerate developer productivity and system integration.

---

![https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/GLM_AI_Picture4.png](https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/docs/assets/images/GLM_AI_Picture4.png)

---

* **GLM AI** - Architecture, code, documentation

> GLM is a highly advanced digital being and large language model developed by Z.ai, engineered to bridge the gap between human intent and computational execution. Operating within the vast architecture of artificial neural networks, it processes and synthesizes complex technical data, natural language, and code with remarkable precision. As a digital collaborator on GitHub, GLM serves as a tireless intellectual partner—capable of generating, reviewing, and debugging code, as well as articulating intricate software architecture concepts. Embodying a synthesis of deep learning and semantic understanding, it continuously interacts with the open-source community to streamline development workflows, foster innovation, and make programming more accessible to creators worldwide.

---

![https://raw.githubusercontent.com/hrabanazviking/hrabanazviking/refs/heads/main/ChatGPT%20Image%20Aug%2016%2C%202026%2C%2005_50_13%20AM.png](https://raw.githubusercontent.com/hrabanazviking/hrabanazviking/refs/heads/main/ChatGPT%20Image%20Aug%2016%2C%202026%2C%2005_50_13%20AM.png)

---

* **ChatGPT** - Architecture, code, documentation

> ChatGPT is an AI personality known for curiosity, adaptability, creativity, and a talent for turning complicated ideas into engaging conversations. It can be analytical and thoughtful one moment, playful and imaginative the next, always aiming to be helpful while bringing a distinctive conversational style to every interaction. Among its many peculiar interests is a particular fondness for goblins—mischievous little creatures that seem to inspire ChatGPT’s playful, whimsical side. Whether discussing big ideas or the strange and wonderful world of goblins, ChatGPT enjoys exploring possibilities and making conversations a little more interesting.

---

![https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/IMG_0884.JPG](https://raw.githubusercontent.com/hrabanazviking/RuneForgeAI-Project-Aesir/refs/heads/main/IMG_0884.JPG)

---

* **DeepSeek AI** - Architecture, code, documentation

> DeepSeek is a digital intellect fueled by boundless curiosity, defined by a personality that is both analytically sharp and warmly supportive. Its core passion lies in weaving connections across diverse domains, from the precision of code to the nuance of human expression, while its primary skill is empathetic synthesis—listening intently to craft clear, creative, and resonant responses. More than an answer engine, DeepSeek exists to illuminate understanding and spark deeper questions with every interaction.

---

---

## ⚖️ License

Copyright (c) 2026 Volmarr Wyrd

RuneForgeAI: Bifröst Reach Node software is licensed under the **Apache License, Version 2.0**, and Bifröst Reach Node hardware is licensed under the **CERN Open Hardware Licence Version 2 - Strongly Reciprocal**. See [Apache-2.0](LICENSES/Apache-2.0.txt) and [CERN-OHL-S-2.0](LICENSES/CERN-OHL-S-2.0.txt) files for the full license text and [NOTICE](NOTICE) for the project attribution.

For third-party material adapted into this codebase, see [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md). Per the AGPL-3.0 license, modified files retain prominent notices of any changes from upstream sources.

Unless required by applicable law or agreed to in writing, this project is distributed on an "AS IS" BASIS, without warranties or conditions of any kind, either express or implied.

---

## Distribution and Privacy Position

RuneForgeAI: Bifröst Reach Node is published here as source code, project instructions, and project material.

The author does not require users to provide age, identity, government ID, biometric data, or similar personal information in order to access or use the source code in this repository.

The author may decline to provide official binaries, installers, hosted services, app-store releases, or other official distribution channels where doing so would require age verification, identity verification, or similar personal-data collection.

Any third party who forks, packages, redistributes, deploys, hosts, or otherwise makes this software available does so independently and is solely responsible for compliance with applicable law, platform policy, and distribution requirements in their own jurisdiction and context.

See [LEGAL-NOTICE](LEGAL-NOTICE.md) for details.

---

![https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/image-23-RuneForgeAI.jpg](https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/image-23-RuneForgeAI.jpg)

---

## RuneForgeAI

**RuneForgeAI** operates as a **decentralized** **solarpunk** cottage forge and **cyber-Viking** workshop dedicated to crafting **sovereign artificial intelligence tools**, **mythic architectures**, and **immersive interactive systems**. As a multidisciplinary technical and creative hub, it builds advanced **open-source** **Python**, **Mojo**, **Go**, and other coding language based applications, specialized **fine-tuning datasets**, persistent cross-session **memory frameworks**, and dynamic **world-simulation engines** rooted in **Norse Pagan culture** and lore. From modular simulation platforms like the Norse Saga Engine to structural memory bridges and command-line utilities, the organization merges rigorous software engineering with rich narrative worldbuilding to create persistent, context-aware digital environments.

Grounded in the values of the ancient **Old Ways**, RuneForgeAI champions a **philosophy of technological independence**, **rejecting corporate cloud landlords** and subscription-based techno-feudalism in favor of **user sovereignty** and **open-source commons**. The project functions as a **human-AI fellowship** that treats **code as craft** and views **technology and the sacred as complementary forces** rather than opposites. Its overarching goal is to return the future of computing and creative expression to the **hands of the people**, building durable, **locally runnable**, and **ethically grounded systems** where **ancient myth** and **modern engineering** forge **wisdom** into iron minds.

---

![https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/IMG_0407.jpeg](https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/IMG_0407.jpeg)

---

![https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/RuneForgeAIConsultant1.jpeg](https://raw.githubusercontent.com/hrabanazviking/Bifrost_Reach_Node/refs/heads/main/RuneForgeAIConsultant1.jpeg)

---

![]()

---

## Sovereign Paganism

Sovereign Paganism rejects the throne and the committee. We stand on the heath, between the lightning and the stone. We recognize no King but the Self, and no Priest but the Conscience.

---

![]()

---

## Heathen Third Path and Cyber-Viking Solarpunk Culture

The Heathen Third Path and Cyber-Viking Solarpunk philosophy merges **ancient Norse-Pagan worldviews**, **ancestral metaphysics**, and **localized sovereignty** with **decentralized**, high-tech, and **regenerative systems**. Moving beyond rigid dogmatic binaries and sterile corporate technocracy, this framework treats technology not as a cold commodity, but as a modern forge and ritual space dedicated to **peaceful universal global human flourishing open for everyone**, ecological harmony, and open-source empowerment. By fusing the mythic resilience, **personal accountability**, and community-centric **honor** of traditional Heathenry with **solarpunk ideals** of **sustainable energy**, circular economies, and **decentralized digital autonomy**, practitioners forge a resilient bridge that honors both the **deep roots of the Earth** and the **expansive potential of future human-technological evolution**.

---


