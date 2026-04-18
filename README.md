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
