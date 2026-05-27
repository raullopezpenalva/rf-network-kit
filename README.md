# RF Network Kit

Portable macOS utility for quickly deploying an isolated DHCP-based RF control network for Shure Wireless Workbench and similar AV/RF devices.

Designed for live events, touring, broadcast, and temporary AV deployments where you need a fast, predictable Ethernet control network without relying on Auto-IP (169.254.x.x).

---

## Why?

Many wireless audio devices (such as Shure QLX-D / SLX-D+) default to:

- DHCP (if available)
- Auto-IP / Link-Local (`169.254.0.0/16`) if no DHCP exists

While Auto-IP works, it has drawbacks:

- unpredictable addressing
- slower device discovery
- harder troubleshooting
- inconsistent behavior across devices
- poor scalability for larger RF control networks

This tool provides a lightweight DHCP server directly from your macOS laptop, turning it into a portable RF control node.

Perfect for:

- Shure Wireless Workbench
- temporary RF coordination networks
- venue troubleshooting
- isolated AV control networks
- quick field engineering deployments

---

## Architecture

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
        │ GS305E    │
        │ L2 Switch │
        └─────┬─────┘
              │
   ┌──────────┼──────────┐
   │          │          │
┌──▼──┐   ┌───▼───┐  ┌───▼───┐
│QLXD │   │SLXD+  │  │Other  │
│DHCP │   │DHCP   │  │Devices│
└─────┘   └───────┘  └───────┘
```

---

## Features

- Quick DHCP server deployment
- Fixed host IP assignment
- Start / stop utility commands
- Interface status inspection
- Lightweight (`dnsmasq`)
- No cloud dependencies
- Works fully offline
- Ideal for field AV / RF workflows

---

## Requirements

- macOS
- Homebrew
- `dnsmasq`
- Ethernet adapter

Install dnsmasq:

```bash
brew install dnsmasq
```

---

## Project Structure

```text
rf-network-kit/
├── config/
│   └── dnsmasq-rf.conf
├── docs/
│   └── network-plan.md
├── logs/
├── scripts/
│   ├── rf-up
│   ├── rf-down
│   └── rf-status
├── .gitignore
├── LICENSE
└── README.md
```

---

## Default Network Configuration

| Parameter | Value |
|---------|------|
| Host IP | `10.99.0.1` |
| Netmask | `255.255.255.0` |
| DHCP Range | `10.99.0.100 - 10.99.0.200` |
| Gateway | none |
| DNS | none |

This is an isolated control network by design.

---

## Installation

Clone repository:

```bash
git clone https://github.com/YOUR_USERNAME/rf-network-kit.git
cd rf-network-kit
```

Install dependencies:

```bash
brew install dnsmasq
```

Make scripts executable:

```bash
chmod +x scripts/*
```

Create global command symlinks:

```bash
mkdir -p ~/bin

ln -s "$(pwd)/scripts/rf-up" ~/bin/rf-up
ln -s "$(pwd)/scripts/rf-down" ~/bin/rf-down
ln -s "$(pwd)/scripts/rf-status" ~/bin/rf-status
```

Ensure `~/bin` is in PATH:

```bash
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

## Usage

Start RF control network:

```bash
rf-up
```

Check status:

```bash
rf-status
```

Stop RF network:

```bash
rf-down
```

---

## Typical Workflow

At venue:

1. Connect laptop Ethernet adapter
2. Connect switch
3. Connect wireless receivers
4. Run:

```bash
rf-up
```

5. Launch Wireless Workbench
6. Devices receive DHCP leases automatically
7. Coordinate RF

After event:

```bash
rf-down
```

---

## Custom Configuration

Override defaults:

```bash
RF_INTERFACE=en8 rf-up
```

or:

```bash
RF_HOST_IP=192.168.50.1 rf-up
```

---

## Roadmap

Planned improvements:

- static DHCP reservations by MAC
- automatic Ethernet adapter detection
- lease inspection
- RF device discovery helpers
- Wireless Workbench launcher integration
- profile presets (`rf`, `dante`, `ndi`)
- logging improvements

---

## Disclaimer

This tool is intended for isolated AV control networks.

Do not deploy this on production enterprise networks without understanding DHCP implications.

---

## License

MIT

---

## Author

Built for practical field engineering workflows in live AV / RF environments.