# Changelog

All notable changes to `decentnet` are documented in this file.

## [0.2.0] - 2026-04-18

### Added
- Automatic ICE startup after `hello` / `hello_ack`.
- Selected-path persistence, keepalive-driven health checks, and ICE restart.
- Relay fallback, relay lifecycle hardening, and per-peer/operator safety budgets.
- Automatic peer-record bootstrap inside the signaling path.
- Runtime candidate refresh / trickle-style locator updates.
- Multi-node mesh, churn, rebind, partition, restart, and soak-style integration coverage.
- Live HTML dashboard with snapshot export/import and soak/operator report visibility.
- Two-host harness with orchestrate, matrix, soak, local-validate, and pack modes.
- Peer-first TOML configuration for transport bind, advertise policy, interface filters, and capabilities.
- Profile presets and config generation for `laptop`, `rpi`, `phone`, and `helper` peers.
- Peer lifecycle commands: `up`, `status`, `down`, and `setup`.
- Profile-aware doctor/status validation with bind-vs-advertise mismatch reporting.
- Auto LAN endpoint detection with allow/deny interface filtering.
- Shareable onboarding artifacts:
  - peer bundles
  - join import flow
  - mesh packs
  - compact invite tokens
  - invite URLs
  - QR SVG/PNG export
- Local onboarding web flow with:
  - invite import
  - nearby known-peer listing
  - live refresh
  - richer trust/session-aware peer status
  - one-click Hello/Ring actions.

### Changed
- Moved daemon peer transport toward real deployment defaults via configurable UDP bind host.
- Improved `publish_network` by supporting config-driven own-record publication.
- Expanded `doctor` from a basic environment check into a peer deployment readiness tool.
- Expanded `client status` with advertised endpoints, validation warnings, and endpoint strategy hints.
- Updated packaging templates and startup UX for easier PC / Raspberry Pi deployment.
- Updated README and operator docs to reflect peer-first deployment and onboarding workflows.

### Fixed
- Corrected the loopback-only daemon bind mismatch that broke true cross-namespace / multi-host signaling when peers advertised non-loopback addresses.
- Fixed stale bundle reuse in repeated two-host orchestration campaigns.
- Fixed ICE restart deferral so forced restarts requested during active ICE runs are not lost.
- Hardened startup, local validation, and onboarding flows around clearer errors and more deployable defaults.

### Known limits
- Real-network STUN/NAT tests are still opt-in and depend on external DNS and UDP reachability.
- Relay support is cooperative and production-hardening work remains policy-oriented rather than service-grade TURN compatibility.
- Long-duration public-internet soak automation still depends on operator environment and provisioning.
- The onboarding web flow is intentionally lightweight and local-first; it is not a remote hosted control plane.

## [0.1.0]

Initial alpha package metadata and early phased framework documentation.

- Local onboarding mesh health summary added via `/api/mesh_health`, plus a live mesh-health panel on the onboarding page.
