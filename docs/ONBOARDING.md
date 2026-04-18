# Peer onboarding and sharing

This guide covers the peer-native onboarding flows added to DecentNet.

## Goals

Make it easy for a new device to join a decentralized mesh without relying on a
central server.

## Bundle flow

Export a peer bundle:

```bash
decentnet bundle export --out peer-a-bundle.json
```

Inspect it:

```bash
decentnet bundle inspect peer-a-bundle.json --json
```

Import it:

```bash
decentnet bundle import peer-a-bundle.json --json
```

## Connect flow

Import a bundle and try first contact:

```bash
decentnet connect peer-a-bundle.json --hello
```

Optional:
- `--ring`
- `--body`

## Join flow

Import several peers from files or a directory:

```bash
decentnet join --dir ./starter_bundles --hello --hello-limit 2 --json
```

## Mesh packs

Export a small starter mesh:

```bash
decentnet meshpack export --include-self --dir ./starter_bundles --out starter-mesh.json
```

Inspect:

```bash
decentnet meshpack inspect starter-mesh.json --json
```

Import:

```bash
decentnet meshpack import starter-mesh.json --hello --hello-limit 2 --json
```

## Compact invites

Export a compact invite token:

```bash
decentnet invite export --print-token
```

Export a compact invite URL:

```bash
decentnet invite export --print-url
```

Inspect:

```bash
decentnet invite inspect --token '<TOKEN>' --json
decentnet invite inspect --url 'decentnet://invite?token=...' --json
```

Import:

```bash
decentnet invite import --token '<TOKEN>' --json
decentnet invite import --url 'decentnet://invite?token=...' --json
```

## QR + HTML invite export

```bash
decentnet invite export \
  --print-url \
  --html-out ./peer-invite.html \
  --qr-svg-out ./peer-invite-qr.svg \
  --qr-png-out ./peer-invite-qr.png
```

## Local onboarding web flow

Serve a local onboarding page:

```bash
decentnet invite serve --host 0.0.0.0 --port 8088
```

Then on another device on the same LAN:

```text
http://<peer-ip>:8088/
```

The page currently supports:
- view local invite info
- scan a QR for the invite URL
- paste/import token or URL
- nearby known-peer listing
- live refresh
- richer trust/session-aware peer status
- Import / Hello / Ring actions on nearby peer cards

## Nearby known peers

The onboarding page can surface peers already known by the serving peer. This
makes it easier for a new peer to join a small local mesh quickly.

## Recommended onboarding progression

1. `setup` a peer profile.
2. `up` the peer.
3. Export an invite or serve the onboarding page.
4. Import via invite, bundle, or mesh pack.
5. Trigger `hello`, then `ring` if desired.


## Local mesh health

The onboarding server now exposes `GET /api/mesh_health` and the page shows a live local mesh summary. It includes peer record count, reachable-guess peers, trusted sessions, established sessions, relay session count, and whether the local peer has an own published record.
