# Network Plan

## Overview

RF Network Kit creates an isolated Ethernet control network for AV/RF devices.

Its purpose is to provide predictable DHCP-based addressing for wireless audio systems and other temporary control devices.

---

## Default Topology

```text
MacBook Pro
┌────────────────────────────┐
│ RF Network Kit            │
│ dnsmasq DHCP server       │
│ Host IP: 10.99.0.1        │
└─────────────┬──────────────┘
              │
              │ Ethernet
              │
        ┌─────▼─────┐
        │ L2 Switch │
        └─────┬─────┘
              │
   ┌──────────┼──────────┐
   │          │          │
┌──▼──┐   ┌───▼───┐  ┌───▼────┐
│QLXD │   │SLX-D+ │  │Other   │
│DHCP │   │DHCP   │  │Clients  │
└─────┘   └───────┘  └────────┘
```

---

## IP Addressing

| Device | Addressing Mode | Range |
|--------|-----------------|-------|
| MacBook host | Static | `10.99.0.1` |
| RF clients | DHCP | `10.99.0.100-200` |

---

## DHCP Options

Current defaults:

- subnet mask: `255.255.255.0`
- gateway: none
- DNS: none
- lease time: `12h`

This network is intentionally isolated.

No internet routing is provided.

---

## Why Not Auto-IP?

Many RF devices fall back to:

```text
169.254.0.0/16
```

This works, but has drawbacks:

- unpredictable addresses
- slower discovery
- harder troubleshooting
- inconsistent startup behavior

DHCP provides deterministic network behavior.

---

## Intended Use Cases

Supported scenarios:

- Shure Wireless Workbench
- temporary RF coordination
- live events
- touring
- broadcast
- isolated AV control networks

Not intended for:

- enterprise LAN deployment
- routed production networks
- internet-connected infrastructure