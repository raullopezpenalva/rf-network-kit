# macOS Notes

## Why dnsmasq instead of Docker?

Docker Desktop on macOS runs inside a Linux VM.

This means containers do not have direct Layer 2 access to the host Ethernet interface in the same way as Linux host networking.

DHCP relies on broadcast traffic:

```text
DHCP DISCOVER → 255.255.255.255
```

Because of Docker Desktop networking abstraction, running a DHCP server inside a container is not ideal for this use case.

dnsmasq running natively on macOS is simpler and more reliable.

---

## Network Interface Selection

Default interface:

```text
en8
```

This should match your Ethernet adapter.

Check available interfaces:

```bash
networksetup -listallhardwareports
```

Example:

```text
Hardware Port: AX88179B
Device: en8
```

Override interface:

```bash
RF_INTERFACE=en5 rf-up
```

---

## Permissions

The scripts require elevated privileges because:

- assigning static IP addresses
- starting dnsmasq
- stopping dnsmasq
- changing network configuration

macOS will request sudo credentials.

---

## Homebrew Dependency

Install:

```bash
brew install dnsmasq
```

Verify:

```bash
dnsmasq --version
```