---
name: gadget-meross-smart-plugs
description: >-
  Read status and switch compatible Meross smart plugs through Muse Home Link. Use the signed
  local HTTP interface with an existing device UUID and key; not cloud key acquisition or
  generic HomeKit onboarding.
---

# Meross Smart Plugs

Use this skill when the exact Meross plug and firmware support the signed local HTTP interface and its existing device key is available.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm the device UUID, model and firmware; a Meross vendor label alone is insufficient.
- Use the discovered local HTTP service and documented `/config` endpoint. HomeKit-branded variants must still be checked for this interface.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- The device UUID and existing local key, supplied through approved secret storage.
- A direct protocol client or an agent-authored implementation of the documented request envelope. Use the community implementation as a protocol reference; installing its host platform is not required.
- Permission to switch the actual load attached to the plug.

## Workflow

1. Send a signed read for `Appliance.System.All` and, where supported, `Appliance.System.Ability`. Use the protocol's message ID, timestamp, namespace, method, device context and signature exactly as documented.
2. Inspect reported abilities and current channel states. A device may expose `Appliance.Control.ToggleX`, an older toggle namespace or another model-specific interface; choose what it actually supports.
3. For a requested relay change, send the documented `SET` for that namespace and channel with the desired on/off value. Do not guess a channel or reuse an unrelated model's payload.
4. Validate the matching response, method/error information and message identity, then read state again using the supported namespace.

## Verify the Result

- A transport success or signed acknowledgment does not prove relay state. Compare a fresh state response with the requested channel/value.
- Do not infer power consumption from relay state, or promise metering unless the device advertises and documents that feature.

## Limits

- Cloud account/key acquisition is not part of this runtime skill. Stop for missing credentials; do not reset or re-pair the plug.
- No universal Meross model support, arbitrary namespace writes, firmware updates or separate HomeKit onboarding procedure.

## Sources

- [Meross local protocol community implementation](https://github.com/krahabb/meross_lan)
