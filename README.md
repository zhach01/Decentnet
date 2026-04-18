# decentnet

A decentralized, serverless, peer-first networking framework in Python.

**Status: Release 0.2.0 candidate — the codebase now combines peer identity,
distributed discovery, direct signaling, NAT/ICE/relay work, peer deployment
configuration, onboarding/share flows, local onboarding web UX, and operator
validation tooling in one package.**

Every device is a peer. Peers can run on PCs, Raspberry Pis, phones, and other
mixed devices. A peer can discover others, advertise signed endpoints, connect
directly when possible, fall back when needed, and help new peers join the mesh
without relying on a permanent central messaging server.

## What decentnet is

The distributed network's job is to share **signed connection metadata about
peers** — not user messages, not voice, not files. After peers resolve each
other through the fabric, they connect **directly** via a signed signaling
handshake.

Each peer can act as both client and server. Some peers may be longer-lived,
relay-capable, or especially useful for onboarding and discovery, but the
architecture remains peer-to-peer and serverless.

## Current headline capabilities

- Cryptographic peer identity with key rotation and keystore support.
- Signed locator records with manual, automatic, and hybrid endpoint publication.
- Local cache + distributed directory / DHT participation.
- Direct signed signaling (`hello`, `ring`, `bye`) with trust promotion.
- NAT discovery, STUN, ICE candidate selection, and relay fallback.
- Peer-first deployment config with profile presets.
- Peer lifecycle UX: `setup`, `up`, `status`, `down`.
- Onboarding/share flows:
  - bundle export/import
  - join starter meshes
  - mesh packs
  - compact invites
  - invite URLs
  - QR SVG/PNG export
  - local onboarding web flow.
- Operator validation:
  - doctor
  - dashboard
  - local/two-host/matrix/soak harnesses.

## New peer-first deployment and onboarding docs

- `docs/PEER_DEPLOYMENT.md` — peer profiles, bind vs advertise, config strategy, startup UX.
- `docs/ONBOARDING.md` — bundles, invites, mesh packs, onboarding web flow, LAN peer discovery.
- `docs/REAL_NETWORK.md` — two-host, matrix, soak, and pack workflows.
- `docs/PRODUCTION_RUNBOOK.md` — operator checklist and deployment/run procedures.
- `docs/THREAT_MODEL.md` — security model.
- `CHANGELOG.md` — release history.
- `docs/RELEASE_NOTES_0.2.0.md` — release highlights and upgrade notes.

## Install

```bash
python -m pip install -e .
```

For tests:

```bash
python -m pytest tests/ -q
```

## Fast start

### 1) Create a peer config from a profile

```bash
decentnet profile list
decentnet config init --profile laptop --force
```

Or use the setup wizard:

```bash
decentnet setup --profile laptop --yes
```

### 2) Validate the peer config

```bash
decentnet doctor --config ~/.decentnet/config.toml
```

### 3) Start the peer

```bash
decentnet up --config ~/.decentnet/config.toml
```

### 4) Inspect status

```bash
decentnet status --config ~/.decentnet/config.toml
decentnet dashboard --help
```

## Peer profiles

Built-in profiles are peer-oriented, not server presets:

- `laptop`
- `rpi`
- `phone`
- `helper`

Use:

```bash
decentnet profile show rpi
decentnet config init --profile rpi --out ./config.toml --force
```

## Onboarding and sharing

### Peer bundle

```bash
decentnet bundle export --out peer-a-bundle.json
decentnet bundle inspect peer-a-bundle.json --json
decentnet bundle import peer-a-bundle.json --json
```

### Join several peers at once

```bash
decentnet join --dir ./starter_bundles --json
```

### Starter mesh packs

```bash
decentnet meshpack export --include-self --dir ./starter_bundles --out ./starter-mesh.json
decentnet meshpack inspect ./starter-mesh.json --json
decentnet meshpack import ./starter-mesh.json --json
```

### Compact invites + QR

```bash
decentnet invite export --print-url --html-out ./peer-invite.html \
  --qr-svg-out ./peer-invite-qr.svg --qr-png-out ./peer-invite-qr.png
```

### Local onboarding web flow

```bash
decentnet invite serve --host 0.0.0.0 --port 8088
```

Then on another device on the same LAN:

```text
http://<peer-ip>:8088/
```

The onboarding page supports:
- token / URL import
- nearby known-peer listing
- live refresh
- trust/session-aware peer status
- one-click Hello/Ring actions.

## Bind vs advertise

A key deployment rule:

- **bind** = where the peer listens locally
- **advertise** = what addresses it tells other peers to use

For real multi-device networking, use a real bind host such as `0.0.0.0` or a
specific interface IP, not loopback-only transport when you intend to advertise
LAN/VPN/public addresses.

Example peer config:

```toml
[transport]
bind_host = "0.0.0.0"
bind_port = 45001

[advertise]
mode = "hybrid"
auto_publish = true
auto_lan = true
auto_vpn = true

[interfaces]
deny = ["lo", "docker0", "virbr0"]
```

See `packaging/config.example.toml` and `docs/PEER_DEPLOYMENT.md` for more.

## Validation surfaces

The codebase includes:
- unit tests
- signaling / NAT / ICE / relay suites
- local validation bundle flow
- two-host orchestration
- matrix / soak tooling
- onboarding and invite flow tests
- dashboard snapshot tests
- real-network tests that remain opt-in.

## Known limits

- Real-network STUN/NAT behaviour still depends on the environment.
- Relay support is cooperative and intentionally lighter than a full TURN service.
- The onboarding web flow is local-first and lightweight, not a hosted control plane.

See `docs/` for per-layer design and deployment guidance.


## Local onboarding mesh health

The onboarding web flow now exposes `GET /api/mesh_health` and shows a live local mesh summary with peer counts, reachable-guess counts, trusted sessions, established sessions, relay usage, and own-record presence.
