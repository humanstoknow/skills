---
name: gadget-tplink-kasa-ep25
description: >-
  Control relay, LED, and supported auto-off settings or read energy measurements on TP-Link
  Kasa EP25 through Muse Home Link. Use the confirmed SMART local protocol and approved
  credentials, not EP10 legacy XOR.
---

# TP-Link Kasa EP25: Authenticated Switching and Energy Readings

Use this skill when fresh discovery identifies a compatible Kasa EP25 and the user requests switching, energy readings or supported auto-off.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm EP25 model, hardware/firmware, protocol family, encryption/login version and local service endpoint from available discovery metadata and authenticated device information.
- This path uses SMART local authentication, not EP10 legacy XOR. A port alone does not identify KLAP/AES or login version.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- Provide the device's approved TP-Link local-authentication credentials or supported stored credential hash through secure storage. These authenticate locally; no cloud-control login is part of the runtime flow.
- Know what load is attached. If credentials or the protocol configuration are missing, request setup instead of guessing or resetting.

## Workflow

1. Configure python-kasa for the known SMART device/transport and exact discovered endpoint; use HomeLink network routing instead of the library's broadcast discovery.
2. Authenticate and update device state. Read supported features before invoking a capability; keep raw feature dumps private because they can include setup codes.
3. Use `turn_on`/`turn_off` for requested relay changes. LED and auto-off features are separate operations; change them only when explicitly requested and exposed by this device.
4. Read the energy module/features for actual power and totals, preserving their reported units. Availability differs across firmware.
5. For auto-off, use the actual `auto_off_enabled` and `auto_off_minutes` features if present; read back both. Do not treat unsupported schedule APIs as equivalent.
6. Refresh after each action and inspect authentication/device errors separately from connection failures.

## Verify the Result

- Confirm resulting relay/LED state and requested auto-off settings.
- Report measured consumption with units; a working relay is not proof that the attached appliance is drawing power.

## Limits

- The SMART path does not support the legacy schedule module. Do not add schedules or use cloud/Matter/Home automation fallbacks.
- No reset, re-keying, Wi-Fi changes or firmware updates. Other Kasa/Tapo products require their own capability checks.

## Sources

- [python-kasa](https://github.com/python-kasa/python-kasa)
