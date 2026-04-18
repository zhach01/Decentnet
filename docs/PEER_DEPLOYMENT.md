# Peer deployment guide

This guide explains how to deploy DecentNet as a **peer-first** runtime on PCs,
Raspberry Pis, and similar devices.

## Design principle

Every node is a peer. A peer can:
- keep its own identity
- publish signed reachable endpoints
- discover and cache other peers
- connect directly when possible
- help onboard other peers.

Some peers may be longer-lived or relay-capable, but the architecture remains
serverless and decentralized.

## Bind vs advertise

This distinction is critical:

- **bind**: local socket listen address (`[transport]`)
- **advertise**: endpoints other peers should try (`[advertise]`)

Example:

```toml
[transport]
bind_host = "0.0.0.0"
bind_port = 45001

[advertise]
mode = "hybrid"
auto_publish = true
auto_lan = true
auto_vpn = true
```

If you advertise `10.x`, `192.168.x`, VPN, or public addresses while binding only
to `127.0.0.1`, other peers will not be able to reach you. `doctor` and `status`
now warn about this.

## Profiles

Built-in profiles:
- `laptop`
- `rpi`
- `phone`
- `helper`

Use them with:

```bash
decentnet profile list
decentnet profile show rpi
decentnet config init --profile rpi --force
```

Or the setup wizard:

```bash
decentnet setup --profile rpi --yes
```

## Recommended profile strategy

### Laptop
- good for interactive peer use
- usually `hybrid` advertise mode
- auto LAN/VPN enabled

### Raspberry Pi
- good for always-on home or lab peers
- typically `hybrid` or partly manual endpoint policy
- often a good relay-capable or onboarding helper peer

### Phone
- mobile-oriented, battery-sensitive defaults
- prefers automatic policy and lightweight onboarding flows

### Helper
- still a peer, not a central server
- suited to longer-lived nodes that help onboarding or relay fallback

## Interfaces

Use allow/deny filters to avoid bad interface choices:

```toml
[interfaces]
allow = ["eth0", "wlan0", "tailscale0", "wg0"]
deny = ["lo", "docker0", "virbr0"]
```

## Startup flow

```bash
decentnet doctor --config ~/.decentnet/config.toml
decentnet up --config ~/.decentnet/config.toml
decentnet status --config ~/.decentnet/config.toml
decentnet down --config ~/.decentnet/config.toml
```

`up` now previews:
- profile hint
- endpoint strategy
- bind host/port
- advertised endpoints
- validation warnings / blockers

## Config sections

Important sections:
- `[transport]`
- `[advertise]`
- `[interfaces]`
- `[capabilities]`

See `packaging/config.example.toml` for a template.

## Validation checklist

Before real deployment, confirm:
- bind host is not loopback-only if you need cross-device networking
- LAN/VPN/public endpoints match the actual interfaces you expect
- bad interfaces are denied
- `doctor` shows no bind-vs-advertise mismatch blockers
