# RuneForgeAI: Bifröst Reach Node — Software

> **Software stack for the RuneForgeAI: Bifröst Reach Node intelligent off-grid cellular communications platform.**

The `software/` directory contains the user-space software responsible for controlling, monitoring, routing, optimizing, and automating a Bifröst Reach Node.

RuneForgeAI: Bifröst combines a Raspberry Pi or similar Linux edge computer with a 4G/5G modem, external MIMO antennas, optional antenna positioning hardware, and intelligent network-management software to create a portable cellular communications outpost for rural, remote, nomadic, emergency, and experimental use.

The software does **not** function as an unauthorized cellular repeater or homemade RF amplifier. It controls commercially available modem and networking hardware and operates on the data/network side of the cellular connection.

---

# Table of Contents

* [Overview](#overview)
* [Design Philosophy](#design-philosophy)
* [What the Software Does](#what-the-software-does)
* [Architecture](#architecture)
* [Major Components](#major-components)
* [Suggested Directory Structure](#suggested-directory-structure)
* [Operating Modes](#operating-modes)
* [Supported Hardware](#supported-hardware)
* [Software Requirements](#software-requirements)
* [Installation](#installation)
* [Configuration](#configuration)
* [Modem Management](#modem-management)
* [Signal Metrics](#signal-metrics)
* [Signal Scanning](#signal-scanning)
* [Antenna Positioning](#antenna-positioning)
* [Network Routing](#network-routing)
* [Multi-WAN](#multi-wan)
* [Wi-Fi Access Point](#wi-fi-access-point)
* [GPS and Location Awareness](#gps-and-location-awareness)
* [Site Memory](#site-memory)
* [Web Dashboard](#web-dashboard)
* [API](#api)
* [Logging](#logging)
* [Security](#security)
* [Power-Aware Operation](#power-aware-operation)
* [Development](#development)
* [Testing](#testing)
* [Deployment](#deployment)
* [Troubleshooting](#troubleshooting)
* [Roadmap](#roadmap)
* [Licensing](#licensing)
* [Safety and Regulatory Notice](#safety-and-regulatory-notice)

---

# Overview

The Bifröst Reach Node software turns a small Linux computer into the control system for an intelligent cellular communications station.

Instead of relying entirely on the tiny internal antennas and limited placement options of a smartphone, Bifröst can use:

* external cellular modems
* 4×4 MIMO antennas
* directional antennas
* elevated antenna masts
* GPS
* antenna rotators
* multiple cellular carriers
* Ethernet
* Wi-Fi
* tethered phones
* campground or public Wi-Fi
* VPNs
* future satellite links

The Raspberry Pi acts as the **brain of the system**.

The cellular modem remains the actual certified radio interface.

---

# Design Philosophy

Bifröst follows several core engineering principles.

## 1. Intelligence Instead of Brute Force

The system does not attempt to create an uncontrolled RF amplifier.

Instead, Bifröst improves communications through:

* better antennas
* better antenna placement
* higher elevation
* directional gain
* tower discovery
* modem telemetry
* automatic network selection
* antenna aiming
* carrier diversity
* intelligent routing

---

## 2. Keep RF Paths Short

Long cellular coaxial cable runs can introduce significant signal loss.

The preferred architecture places the cellular modem physically close to the external antenna.

```text
CELL TOWER
    │
    │ RF
    ▼
[ MIMO ANTENNA ]
    │
    │ SHORT RF CABLES
    ▼
[ CELLULAR MODEM ]
    │
    │ USB / PCIe / Ethernet
    ▼
[ RASPBERRY PI ]
    │
    ├── Wi-Fi
    ├── Ethernet
    ├── VPN
    └── Local services
```

Where practical, ordinary digital networking and DC power should travel the longer distance rather than cellular RF coax.

---

## 3. Deterministic Core

Critical communications functions should not depend on AI.

The following functions should remain deterministic:

* modem control
* routing
* failover
* watchdogs
* antenna limits
* motor safety
* network health checks
* power management
* firewall rules

AI may eventually assist with interpretation and recommendations, but should not override safety-critical controls.

---

## 4. Offline First

A Bifröst Reach Node may operate where Internet access is unreliable by definition.

The software should therefore remain useful without cloud services.

Core functionality should operate locally.

---

## 5. Modular Architecture

Bifröst should not depend on one specific:

* Raspberry Pi
* carrier
* modem
* antenna
* GPS receiver
* motor controller
* Linux distribution
* Wi-Fi adapter

Hardware-specific behavior should be abstracted wherever practical.

---

# What the Software Does

The Bifröst software stack may provide:

* cellular modem detection
* modem initialization
* SIM status monitoring
* carrier detection
* cellular band detection
* LTE/5G mode detection
* RSRP monitoring
* RSRQ monitoring
* RSSI monitoring
* SINR monitoring
* cell-ID tracking
* tower observations
* signal-history logging
* GPS location logging
* directional antenna scanning
* motorized antenna aiming
* best-direction discovery
* connection-quality scoring
* latency testing
* packet-loss testing
* throughput testing
* multi-carrier failover
* multi-WAN management
* local Wi-Fi access point
* Ethernet routing
* DNS management
* VPN integration
* firewall configuration
* site-memory storage
* historical network comparison
* local dashboard
* REST API
* command-line tools
* system health monitoring
* temperature monitoring
* power monitoring
* watchdog behavior
* graceful degraded operation

---

# Architecture

A full Bifröst system can be represented as several logical layers.

```text
┌─────────────────────────────────────────────┐
│               USER INTERFACES               │
│                                             │
│  Web Dashboard   CLI   REST API   Mobile UI │
└──────────────────────┬──────────────────────┘
                       │
┌──────────────────────▼──────────────────────┐
│                BIFRÖST CORE                 │
│                                             │
│ Signal Scoring                              │
│ Site Memory                                 │
│ Scan Coordination                           │
│ Network Decisions                           │
│ Health Monitoring                           │
└─────────────┬───────────────┬───────────────┘
              │               │
      ┌───────▼──────┐ ┌──────▼─────────┐
      │ MODEM LAYER  │ │ ANTENNA LAYER  │
      │              │ │                │
      │ MBIM / QMI   │ │ Motor Control  │
      │ AT Commands  │ │ Encoder        │
      │ ModemManager │ │ Compass        │
      └───────┬──────┘ └──────┬─────────┘
              │               │
      ┌───────▼───────────────▼─────────┐
      │          HARDWARE LAYER          │
      │                                  │
      │ 4G/5G Modem   GPS   MCU   Sensors│
      └──────────────────────────────────┘
```

Networking runs alongside these layers:

```text
CELLULAR
   │
   ▼
MODEM
   │
   ▼
BIFRÖST ROUTER
   │
   ├── Wi-Fi AP
   ├── Ethernet LAN
   ├── VPN
   ├── Pi nodes
   ├── laptops
   ├── phones
   └── other devices
```

---

# Major Components

## Bifröst Core

The main orchestration service.

Responsibilities may include:

* reading modem telemetry
* calculating connection scores
* initiating scans
* storing observations
* selecting preferred connections
* coordinating antenna control
* exposing system state
* responding to API commands

Suggested service name:

```text
bifrost-core
```

---

## Modem Service

Hardware abstraction layer for cellular modems.

Possible interfaces:

* ModemManager
* MBIM
* QMI
* AT commands
* vendor-specific modem commands

Suggested module:

```text
bifrost-modem
```

---

## Signal Scanner

Performs repeatable measurements of cellular conditions.

Possible measurements:

* RSRP
* RSRQ
* SINR
* RSSI
* latency
* packet loss
* download throughput
* upload throughput
* jitter
* connection stability

Suggested module:

```text
bifrost-scan
```

---

## Antenna Controller

Controls optional motorized directional antenna systems.

Potential hardware:

* stepper motors
* geared DC motors
* servo systems
* rotary encoders
* limit switches
* electronic compass
* RP2040 or other microcontroller

Suggested module:

```text
bifrost-rotator
```

---

## Network Manager

Handles routing and WAN policy.

Responsibilities may include:

* default-route selection
* interface health
* failover
* load balancing
* captive-network handling
* DNS
* NAT
* VPN routing

Suggested module:

```text
bifrost-network
```

---

## Site Memory

Stores previous observations.

Suggested database:

```text
SQLite
```

The database can remember:

* GPS coordinates
* carrier
* tower/cell ID
* band
* radio technology
* antenna bearing
* signal quality
* throughput
* latency
* timestamp
* modem
* antenna
* notes

Suggested module:

```text
bifrost-memory
```

---

## Dashboard

Local web interface for monitoring and controlling the system.

Suggested service:

```text
bifrost-dashboard
```

Possible dashboard information:

* connection status
* carrier
* LTE/5G mode
* current band
* RSRP
* RSRQ
* SINR
* IP address
* antenna direction
* GPS coordinates
* active WAN
* recent scans
* system temperature
* power state
* connected clients

---

# Suggested Directory Structure

The exact repository structure may evolve.

A recommended layout is:

```text
software/
├── README.md
├── pyproject.toml
├── requirements.txt
│
├── bifrost/
│   ├── __init__.py
│   ├── core/
│   │   ├── controller.py
│   │   ├── state.py
│   │   └── events.py
│   │
│   ├── modem/
│   │   ├── manager.py
│   │   ├── modemmanager.py
│   │   ├── mbim.py
│   │   ├── qmi.py
│   │   └── at.py
│   │
│   ├── signal/
│   │   ├── metrics.py
│   │   ├── scoring.py
│   │   ├── scanner.py
│   │   └── throughput.py
│   │
│   ├── antenna/
│   │   ├── controller.py
│   │   ├── rotator.py
│   │   ├── compass.py
│   │   └── limits.py
│   │
│   ├── network/
│   │   ├── routing.py
│   │   ├── failover.py
│   │   ├── wan.py
│   │   ├── wifi.py
│   │   └── vpn.py
│   │
│   ├── location/
│   │   ├── gps.py
│   │   └── sites.py
│   │
│   ├── memory/
│   │   ├── database.py
│   │   ├── schema.py
│   │   └── history.py
│   │
│   ├── api/
│   │   ├── server.py
│   │   └── models.py
│   │
│   ├── dashboard/
│   │   ├── server.py
│   │   └── static/
│   │
│   ├── power/
│   │   ├── monitor.py
│   │   └── policy.py
│   │
│   └── config/
│       ├── loader.py
│       └── defaults.py
│
├── config/
│   └── bifrost.example.toml
│
├── systemd/
│   ├── bifrost-core.service
│   └── bifrost-dashboard.service
│
├── scripts/
│   ├── install.sh
│   ├── uninstall.sh
│   └── diagnostics.sh
│
├── tests/
│
└── docs/
```

This structure is a design recommendation and may differ from the current implementation.

---

# Operating Modes

Bifröst may eventually support several operating modes.

## Station Mode

Normal campsite or fixed deployment.

```text
External antenna
      │
      ▼
 Cellular modem
      │
      ▼
 Raspberry Pi
      │
      ▼
 Local Wi-Fi / Ethernet
```

---

## Survey Mode

Collect radio measurements while manually or automatically changing antenna direction.

Useful for determining:

* strongest tower direction
* best carrier
* best band
* best mast position
* best antenna orientation

---

## Auto-Aim Mode

Automatically rotate a directional antenna and evaluate signal conditions.

Typical workflow:

```text
Start scan
   │
   ▼
Rotate antenna
   │
   ▼
Measure signal
   │
   ▼
Store results
   │
   ▼
Repeat
   │
   ▼
Compare candidates
   │
   ▼
Return to best bearing
```

---

## Travel Mode

Designed for operation while mobile.

Directional antenna rotation may be disabled.

The system prioritizes:

* omnidirectional antennas
* automatic carrier recovery
* WAN failover
* minimal intervention

---

## Low-Power Mode

For solar or battery-constrained operation.

May disable or reduce:

* throughput testing
* frequent scans
* dashboard refresh rate
* antenna movement
* nonessential services

---

## Emergency Mode

Prioritizes reliability rather than maximum throughput.

Possible behavior:

* prefer stable LTE over unstable 5G
* minimize WAN switching
* disable unnecessary services
* preserve battery power
* expose system-health diagnostics

---

# Supported Hardware

The software is intended to remain modular.

Initial development targets may include:

## Compute

* Raspberry Pi 5
* Raspberry Pi 4
* Raspberry Pi Zero 2 W for lightweight controller roles
* other ARM64 Linux SBCs
* x86-64 Linux systems

---

## Cellular Modems

Primary reference platform:

* Quectel RM520N-GL

Future modem support may include additional:

* Quectel modules
* Sierra Wireless modules
* Fibocom modules
* Telit modules
* USB cellular modems
* M.2 cellular modems

Actual support depends on available drivers and modem-control interfaces.

---

## Antennas

The software is antenna-agnostic.

Potential configurations include:

* 2×2 MIMO omnidirectional
* 4×4 MIMO omnidirectional
* 2×2 MIMO directional
* 4×4 MIMO directional
* panel antennas
* LPDA arrays

---

## GPS

Supported or planned interfaces may include:

* modem-integrated GNSS
* USB GPS
* UART GPS
* gpsd-compatible receivers

---

## Rotator Controller

Recommended design:

```text
Raspberry Pi
    │
    │ USB / UART
    ▼
RP2040-class MCU
    │
    ├── motor driver
    ├── limit switches
    ├── encoder
    └── sensors
```

The microcontroller should enforce physical safety limits independently of the Pi.

---

# Software Requirements

Expected platform:

```text
Linux
```

Recommended environments:

* Raspberry Pi OS Lite
* Debian
* Ubuntu Server
* OpenWrt where supported

Potential dependencies include:

```text
ModemManager
NetworkManager
libqmi
libmbim
gpsd
SQLite
Python 3
systemd
iproute2
nftables
hostapd
dnsmasq
```

The final dependency list will evolve with implementation.

---

# Installation

> **Status:** Development installation instructions may change while Bifröst is under active development.

Clone the project:

```bash
git clone https://github.com/<OWNER>/<REPOSITORY>.git
cd <REPOSITORY>/software
```

Create a Python virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install Python dependencies:

```bash
pip install -r requirements.txt
```

If the project uses `pyproject.toml`:

```bash
pip install -e .
```

System packages may also be required.

Example Debian/Raspberry Pi OS packages:

```bash
sudo apt install \
    modemmanager \
    network-manager \
    libqmi-utils \
    libmbim-utils \
    gpsd \
    gpsd-clients \
    sqlite3 \
    nftables
```

Exact packages depend on the selected networking backend.

---

# Configuration

Bifröst should use a human-readable configuration format.

Recommended:

```text
TOML
```

Example:

```toml
[node]
name = "bifrost-01"

[modem]
backend = "modemmanager"
device = "auto"

[signal]
scan_interval = 30
enable_throughput_tests = false

[antenna]
mode = "manual"
bearing = 0

[gps]
enabled = true

[memory]
database = "/var/lib/bifrost/bifrost.db"

[network]
enable_wifi_ap = true
enable_failover = true

[dashboard]
enabled = true
bind = "0.0.0.0"
port = 8080
```

A complete example configuration should eventually be maintained at:

```text
software/config/bifrost.example.toml
```

---

# Modem Management

Bifröst should support multiple modem-management backends.

Preferred initial abstraction:

```text
Bifröst Modem API
       │
       ├── ModemManager
       ├── QMI
       ├── MBIM
       └── AT commands
```

ModemManager provides a useful high-level interface while QMI, MBIM, and AT commands allow access to capabilities not exposed at higher layers.

The modem layer should expose normalized values regardless of the underlying modem.

Example internal representation:

```text
carrier
technology
band
cell_id
rsrp
rsrq
rssi
sinr
connection_state
temperature
ip_address
```

---

# Signal Metrics

Bifröst should distinguish between different RF measurements rather than reducing everything to vague "signal bars."

Important metrics include:

## RSRP

Reference Signal Received Power.

Primarily represents cellular signal strength.

Generally:

```text
Higher / less negative = stronger
```

---

## RSRQ

Reference Signal Received Quality.

Useful for understanding signal quality and interference.

---

## SINR

Signal-to-Interference-plus-Noise Ratio.

Extremely important when evaluating whether a strong signal is actually usable.

A weak but clean signal may outperform a stronger signal buried in interference.

---

## RSSI

Received Signal Strength Indicator.

Useful, but less cellular-specific than RSRP.

---

# Signal Scanning

Signal scanning is one of Bifröst's core capabilities.

A basic scan might collect:

```text
Timestamp
GPS
Carrier
Technology
Band
Cell ID
RSRP
RSRQ
SINR
RSSI
Latency
Packet loss
Bearing
```

A deeper test may also include:

```text
Download throughput
Upload throughput
Jitter
Connection stability
```

Throughput testing should be performed sparingly because it can:

* consume cellular data
* consume significant power
* create unnecessary network traffic
* distort normal usage

---

# Connection Scoring

Selecting the "best" connection should not simply mean selecting the strongest signal.

A possible scoring model could consider:

```text
Score =
    signal quality
  + SINR quality
  + stability
  + latency
  + packet-loss performance
  + throughput
  - switching penalty
  - power penalty
```

Weighting should be configurable.

For example, emergency mode may prioritize:

```text
stability > throughput
```

while high-performance mode may prioritize:

```text
throughput + latency
```

---

# Antenna Positioning

Directional antenna systems may support manual or automatic positioning.

Example sweep:

```text
Bearing    RSRP     SINR     Latency
000°       -116      1 dB     110 ms
030°       -108      7 dB      78 ms
060°       -101     12 dB      53 ms
090°        -97     16 dB      42 ms
120°       -104      9 dB      65 ms
150°       -114      2 dB     100 ms
```

Bifröst may select:

```text
090°
```

and return the antenna to that bearing.

---

# Rotator Safety

Mechanical antenna systems require independent safety controls.

The Linux host should **not** be the sole protection against over-rotation.

Recommended protections include:

* physical hard stops
* limit switches
* encoder limits
* motor-current monitoring
* MCU watchdog
* cable-wrap limits
* maximum continuous runtime
* emergency stop behavior

The microcontroller should reject unsafe movement commands even if the Pi requests them.

---

# Network Routing

Bifröst can function as the Internet gateway for an entire campsite or mobile installation.

Example:

```text
              CELLULAR
                  │
                  ▼
             Bifröst Node
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
     Phone      Laptop      Pi Node
       │
       ▼
      VR
```

Typical functions may include:

* NAT
* DHCP
* DNS
* firewall
* IPv4 forwarding
* IPv6 forwarding
* interface isolation
* VPN routing

---

# Multi-WAN

Future Bifröst configurations may use multiple Internet sources.

Example:

```text
                   ┌── Cellular Modem A
                   │
                   ├── Cellular Modem B
                   │
BIFRÖST ROUTER ────┼── Phone USB Tether
                   │
                   ├── External Wi-Fi
                   │
                   └── Satellite
```

Possible policies include:

* failover
* weighted routing
* lowest-latency routing
* lowest-cost routing
* highest-reliability routing
* manual selection

---

# Wi-Fi Access Point

Bifröst may provide a local Wi-Fi network for nearby devices.

Possible implementation:

```text
hostapd
dnsmasq
nftables
```

or an equivalent networking stack.

Example SSID:

```text
BIFROST-NODE
```

Production systems should require strong authentication.

---

# Wi-Fi Calling

A smartphone connected to the Bifröst Wi-Fi network may be able to use carrier-supported Wi-Fi Calling.

This can allow ordinary phone calls and messaging services to operate over the Bifröst Internet connection even when direct cellular reception at the phone itself is poor.

Availability depends on:

* phone
* carrier
* account configuration
* carrier policy
* network conditions

---

# GPS and Location Awareness

Location information allows Bifröst to associate radio observations with physical places.

Possible GPS data:

```text
latitude
longitude
altitude
heading
speed
accuracy
timestamp
```

This enables location-specific network memory.

---

# Site Memory

A major long-term Bifröst feature is the ability to remember previous network conditions.

Example:

```text
Location: Forest Camp Alpha

Carrier: Example Cellular
Technology: LTE
Band: B13
Cell ID: 182736
Bearing: 074°
RSRP: -98 dBm
SINR: 14 dB
Latency: 47 ms

Status:
Reliable
```

When Bifröst later returns to the same site, it can begin with previously successful settings.

---

# Suggested Database Schema

Conceptual example:

```sql
CREATE TABLE observations (
    id INTEGER PRIMARY KEY,
    timestamp TEXT NOT NULL,
    latitude REAL,
    longitude REAL,
    carrier TEXT,
    technology TEXT,
    band TEXT,
    cell_id TEXT,
    bearing REAL,
    rsrp REAL,
    rsrq REAL,
    rssi REAL,
    sinr REAL,
    latency_ms REAL,
    packet_loss REAL,
    download_mbps REAL,
    upload_mbps REAL
);
```

The actual production schema may differ.

---

# Web Dashboard

The dashboard should expose useful information without requiring SSH access.

Suggested main display:

```text
BIFRÖST REACH NODE

Status: ONLINE

Carrier: Example Cellular
Network: 5G
Band: n77

RSRP: -101 dBm
RSRQ: -10 dB
SINR: 17 dB

Antenna: 084°
WAN: Modem 1

Latency: 43 ms

GPS: FIXED

Battery: 78%
CPU Temperature: 52°C
```

Potential controls:

* scan
* reconnect modem
* select WAN
* rotate antenna
* return home
* enable automatic aiming
* run diagnostics
* view historical measurements
* export data

---

# API

Bifröst should expose a local API so other software can interact with the node.

Potential endpoints:

```text
GET  /api/status
GET  /api/modem
GET  /api/signal
GET  /api/network
GET  /api/location
GET  /api/history

POST /api/scan
POST /api/modem/reconnect
POST /api/antenna/rotate
POST /api/antenna/scan
POST /api/network/select
```

The final API is not yet stable.

---

# CLI

A command-line interface may provide:

```bash
bifrost status
bifrost modem
bifrost signal
bifrost scan
bifrost network
bifrost gps
bifrost history
bifrost antenna status
bifrost antenna rotate 90
```

Example:

```text
$ bifrost status

Bifröst Reach Node
------------------
State:       ONLINE
Carrier:     Example Cellular
Network:     LTE
Band:        B13
RSRP:        -97 dBm
SINR:        15 dB
Latency:     51 ms
Bearing:     072°
WAN:         modem0
```

---

# Logging

Logs should be available both for troubleshooting and long-term analysis.

Suggested categories:

```text
core
modem
network
signal
antenna
gps
power
api
dashboard
```

Example location:

```text
/var/log/bifrost/
```

When using systemd, services should also integrate with:

```bash
journalctl
```

Example:

```bash
journalctl -u bifrost-core
```

---

# Security

A Bifröst node is a network gateway and should be treated accordingly.

Recommended principles:

* no default public dashboard exposure
* no default WAN-side management interface
* strong Wi-Fi authentication
* firewall enabled
* least-privilege services
* no hard-coded passwords
* no API credentials committed to Git
* secure configuration permissions
* signed or verified releases where practical
* regular security updates

Remote access should preferably use a secure overlay VPN such as:

```text
Tailscale
```

or another trusted VPN solution.

---

# Power-Aware Operation

Bifröst is intended for off-grid use.

Software should eventually consider:

* battery state
* charging state
* solar input
* CPU temperature
* modem temperature
* modem power consumption
* motor power consumption
* scan frequency

Example policy:

```text
Battery > 50%
    Full performance

Battery 25–50%
    Reduce scanning frequency

Battery 10–25%
    Disable automatic antenna sweeps

Battery < 10%
    Emergency connectivity only
```

The exact policy should remain user-configurable.

---

# Health Monitoring

Bifröst should detect common failures automatically.

Examples:

* modem disappeared
* SIM inaccessible
* modem registered but no Internet
* DNS failure
* interface lost
* antenna controller offline
* GPS lost
* database failure
* excessive CPU temperature
* modem overheating
* low-voltage condition

Possible recovery sequence:

```text
Detect failure
      │
      ▼
Confirm failure
      │
      ▼
Restart service
      │
      ▼
Reconnect modem
      │
      ▼
Reset modem
      │
      ▼
Switch WAN
      │
      ▼
Alert user
```

---

# Development

The software should be developed so that hardware interfaces can be mocked.

This allows development on an ordinary computer without requiring a 5G modem or antenna rotator.

Example:

```text
RealModemBackend
MockModemBackend

RealGPSBackend
MockGPSBackend

RealRotatorBackend
MockRotatorBackend
```

This is particularly useful for:

* CI
* unit testing
* development
* debugging
* simulation

---

# Event Model

Internal components should communicate through clear state and events rather than tightly coupling every subsystem.

Example events:

```text
ModemConnected
ModemDisconnected
SignalChanged
TowerChanged
LocationChanged
WanChanged
ScanStarted
ScanCompleted
AntennaMoved
LowPower
HardwareFault
```

This makes future expansion much easier.

---

# Testing

Testing should eventually include:

## Unit Tests

Test individual functions and modules.

```bash
pytest
```

---

## Integration Tests

Test interaction between:

* modem service
* database
* scanner
* routing
* API
* rotator controller

---

## Simulated RF Tests

Recorded modem telemetry can be replayed.

Example:

```text
Bearing 000° -> RSRP -116
Bearing 030° -> RSRP -110
Bearing 060° -> RSRP -103
Bearing 090° -> RSRP -96
```

The scanner should correctly select:

```text
090°
```

---

## Hardware-in-the-Loop Testing

Eventually test with:

* actual modem
* SIM
* antenna
* GPS
* rotator
* Raspberry Pi
* battery system

---

# Deployment

Production Bifröst systems should preferably run core components as services.

Example:

```text
bifrost-core.service
bifrost-dashboard.service
```

Example commands:

```bash
sudo systemctl enable bifrost-core
sudo systemctl start bifrost-core
```

Check status:

```bash
systemctl status bifrost-core
```

View logs:

```bash
journalctl -u bifrost-core -f
```

---

# Troubleshooting

## Modem Not Detected

Check USB devices:

```bash
lsusb
```

Check ModemManager:

```bash
mmcli -L
```

Inspect kernel logs:

```bash
dmesg
```

---

## Modem Detected but No Network

Inspect:

```bash
mmcli -m 0
```

Possible causes include:

* SIM not recognized
* incorrect APN
* no carrier coverage
* modem not registered
* antenna issue
* unsupported cellular band
* carrier restrictions

---

## Weak Signal

Check:

* antenna connections
* MIMO port mapping
* antenna orientation
* antenna height
* RF cable length
* RF connector condition
* nearby obstructions
* modem bands
* carrier availability

Do not assume the strongest RSRP automatically provides the best network performance.

---

## Good Signal but Poor Performance

Possible causes:

* poor SINR
* tower congestion
* packet loss
* carrier throttling
* network routing
* overloaded band
* unstable 5G connection

Try comparing:

```text
LTE
5G
different bands
different bearings
different carriers
```

where supported and lawful.

---

# Roadmap

Potential software milestones:

## Phase 1 — Basic Node

* modem detection
* Internet connection
* modem telemetry
* CLI status
* local configuration

## Phase 2 — Signal Intelligence

* signal logging
* RSRP/RSRQ/SINR parsing
* SQLite database
* signal scoring
* scan history

## Phase 3 — Local Networking

* Wi-Fi access point
* Ethernet routing
* firewall
* local DNS
* VPN integration

## Phase 4 — Smart Antenna

* antenna-controller protocol
* motorized azimuth control
* automated directional sweeps
* best-bearing selection
* physical safety limits

## Phase 5 — Site Memory

* GPS
* campsite profiles
* historical tower data
* known-good bearing recall
* map visualization

## Phase 6 — Multi-WAN

* multiple cellular modems
* tethered-phone WAN
* external Wi-Fi WAN
* automated failover
* routing policies

## Phase 7 — Energy Intelligence

* battery monitoring
* solar-aware policies
* power modes
* thermal policies

## Phase 8 — Advanced Intelligence

Potential future capabilities:

* connection forecasting
* tower-behavior analysis
* route connectivity mapping
* automated deployment recommendations
* AI-assisted interpretation of radio conditions

AI components must remain subordinate to deterministic networking and hardware safety systems.

---

# Contributing

Contributions are welcome.

Useful contribution areas include:

* Linux networking
* Raspberry Pi development
* cellular modem support
* ModemManager
* QMI
* MBIM
* RF telemetry
* GPS
* OpenWrt
* Python
* Rust
* frontend development
* embedded systems
* antenna rotator control
* power management
* documentation
* field testing

Please keep portability in mind when adding hardware-specific features.

New hardware implementations should preferably be added through interfaces rather than directly embedded throughout the core software.

---

# Licensing

Software contained in this directory is licensed under the:

**Apache License, Version 2.0**

SPDX:

```text
Apache-2.0
```

Unless otherwise stated, this includes:

* source code
* scripts
* daemons
* APIs
* web interfaces
* networking services
* utilities
* software examples
* configuration software

See:

```text
../LICENSE
../NOTICE
../LICENSES/Apache-2.0.txt
```

Hardware design files are separately licensed under:

```text
CERN-OHL-S-2.0
```

See the repository-level licensing documentation for details.

---

# Safety and Regulatory Notice

RuneForgeAI: Bifröst Reach Node software is intended to control and manage lawful networking and communications hardware.

It does **not** authorize:

* illegal cellular transmission
* unauthorized cellular repeaters
* homemade bidirectional cellular amplifiers
* interference with carrier networks
* operation outside legal RF requirements

Cellular transmission is performed by commercially manufactured modem hardware operating according to its certifications, supported bands, network permissions, and applicable regulations.

Users are responsible for ensuring that their equipment and deployment comply with applicable laws, carrier requirements, radio-frequency regulations, electrical requirements, and safety standards.

---

# The Bifröst Principle

RuneForgeAI: Bifröst does not attempt to overpower the radio horizon.

It listens, measures, remembers, adapts, climbs higher, points more precisely, chooses more intelligently, and makes the most of every surviving thread of connectivity.

**Find the signal. Forge the path. Reach beyond.** 📡ᚱ⚡

---
