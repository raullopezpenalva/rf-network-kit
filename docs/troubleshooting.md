# Troubleshooting

## Devices do not receive IP addresses

Check dnsmasq:

```bash
rf-status
```

Check DHCP traffic:

```bash
sudo tcpdump -i en8 -nn port 67 or port 68
```

Expected:

```text
DHCPDISCOVER
DHCPOFFER
DHCPREQUEST
DHCPACK
```

---

## Wrong Ethernet interface

List interfaces:

```bash
networksetup -listallhardwareports
```

Use correct adapter:

```bash
RF_INTERFACE=en5 rf-up
```

---

## dnsmasq not installed

Error:

```text
dnsmasq: command not found
```

Install:

```bash
brew install dnsmasq
```

---

## Port already in use

Check:

```bash
sudo lsof -i :67
```

Another DHCP server may already be running.

Stop conflicting service.

---

## Interface stuck in static mode

Return to DHCP:

```bash
rf-down
```

Or manually:

```bash
sudo networksetup -setdhcp "AX88179B"
```

---

## Devices still using Auto-IP

Symptoms:

```text
169.254.x.x
```

Possible causes:

- dnsmasq not running
- DHCP blocked
- wrong interface selected
- device booted before DHCP server started

Fix:

1. run `rf-up`
2. reboot RF device
3. verify DHCP traffic

---

## Verify connectivity

Ping host:

```bash
ping 10.99.0.1
```

Check ARP:

```bash
arp -a
```

Scan subnet:

```bash
nmap -sn 10.99.0.0/24
```