# Project 58–59
Retro Infrastructure Lab (Levels 1–3 Complete)

## Overview

This lab simulates a small office infrastructure moving from a 2006-style flat network toward a 2026-style controlled and segmented environment.

Completed levels:
- Level 1 – Network + SSH + Logging
- Level 2 – WireGuard VPN
- Level 3 – iptables Firewall

All systems run Ubuntu Server 22.04 inside an isolated Internal Network.

---

# Architecture

## Network

Internal network: LAB5859  
Subnet: 10.56.0.0/24  

WireGuard overlay network: 10.56.0.0/24  

## Machines

| Host      | IP          | Role |
|-----------|------------|------|
| GW-LAB    | 10.56.0.1  | Gateway, VPN server, firewall, logging |
| MGMT      | 10.56.0.10 | Central SSH management node |
| CLIENT-1  | 10.56.0.21 | WireGuard peer |
| CLIENT-2  | 10.56.0.22 | Internal client |

---

# Level 1 – Network, SSH, Logging

## What existed before
- DHCP-based networking
- No segmentation
- Local console administration
- Volatile logs

## What was implemented

### Static Networking (Netplan)
- Declarative network configuration
- Persistent IP assignment
- Predictable addressing model

Replaced:
Ad-hoc DHCP configuration

---

### Centralized SSH Access
- OpenSSH installed on all nodes
- Key-based authentication from MGMT
- Remote administrative control

Replaced:
Local console / password-based access

---

### Persistent System Logging (GW-LAB)
- Enabled persistent journald storage
- Verified SSH logging
- Real-time monitoring via `journalctl -k -f`

Replaced:
Volatile log storage and blind administration

---

# Level 2 – WireGuard VPN

## What existed before
- Flat internal network
- No encrypted overlay
- No separation between logical zones

## What was implemented

- WireGuard installed on GW-LAB and CLIENT-1
- Key pair generation (public/private)
- Secure peer configuration
- Virtual interface `wg0`
- Encrypted tunnel (10.59.0.0/24)

Verification:
- `ip a` shows wg0
- `wg` shows handshake and transfer
- VPN ping successful

Replaced:
Legacy/self-built VPN concepts from early 2000s

---

# Level 3 – Firewall (iptables)

## What existed before
- Open listening services
- Implicit trust model

## What was implemented

Default policy:
- INPUT: DROP
- FORWARD: DROP
- OUTPUT: ACCEPT

Explicitly allowed:
- Loopback
- Established / related connections
- SSH only from MGMT (10.58.0.10)
- WireGuard UDP 51820
- HTTP 8000 (lab service)

Added:
- Rate-limited logging for dropped packets

Verification:
- SSH allowed from MGMT
- SSH blocked from CLIENT nodes
- Drop logs visible via `journalctl -k -f`

Replaced:
Flat trust model with deny-by-default principle

---

# Current Security State

The lab now includes:

- Isolated internal network
- Centralized remote management
- Persistent logging
- Encrypted peer-to-peer VPN
- Principle of least privilege firewall

This infrastructure reflects a shift from:
2006 open internal trust
to
2026 controlled and observable network design.

---

# Snapshot Points

Recommended VM snapshots:
- LEVEL1-CLEAN
- LEVEL2-WG-OK
- LEVEL3-FW-ENABLED
