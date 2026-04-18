# decentnet 0.2.0 release notes

## Summary

Release `0.2.0` is the first unified candidate that packages together:
- networking reliability work
- peer deployment configuration
- peer lifecycle UX
- onboarding/share flows
- local onboarding web UX
- operator tooling and validation.

This release moves the project much closer to an easy-to-deploy **serverless, decentralized, peer-first network runtime**.

## Highlights

### Core networking
- Automatic signaling bootstrap using attached signed `LocatorRecord`s.
- ICE candidate probing starts automatically after session establishment.
- Selected path persistence plus keepalive-driven path health and restart.
- Relay fallback with explicit lifecycle tracking and safer reuse/invalidation.
- Runtime candidate refresh when endpoint sets change.

### Peer deployment
- Peer-first TOML config for transport bind, advertise policy, interface filters, and capabilities.
- Config generation and profile presets for `laptop`, `rpi`, `phone`, and `helper` peers.
- Peer lifecycle commands: `setup`, `up`, `status`, `down`.
- Profile-aware doctor and status reporting.
- Auto LAN endpoint detection with allow/deny filters.

### Onboarding and sharing
- Peer bundles.
- Multi-peer `join` workflow.
- Starter `meshpack` export/import.
- Compact invite tokens.
- Invite URLs.
- QR SVG/PNG export.
- Local onboarding web flow with invite import, nearby known peers, live refresh, richer trust/session-aware peer status, and one-click Hello/Ring actions.

### Operator tooling
- Two-host harness with local, SSH, matrix, soak, local-validate, and pack modes.
- Dashboard trend views and import/exportable snapshots.
- Stronger deployment-oriented `doctor` output.

## Upgrade notes

- Operators should prefer profile-driven peer config and lifecycle commands instead of long ad-hoc CLI command sequences.
- For real multi-device networking, ensure the peer bind host is not loopback-only when advertising LAN/VPN/public addresses.
- Use the onboarding/share flows instead of manual raw record juggling when possible.

## Tested surfaces

The release candidate was validated through focused signaling/ICE/relay suites, mesh integration with rebind/partition/restart churn, operator harness flows, dashboard export/import tests, onboarding/share flow tests, and local real-UDP smoke checks.

## Known limits

- NAT traversal is improved but still environment-dependent; hard NAT cases may still require relay use.
- Cooperative relay support is intentionally simpler than full TURN service behavior.
- Public-internet long-haul campaigns still need operator-managed hosts or VMs.
- The onboarding web flow is intentionally lightweight and local-first.
