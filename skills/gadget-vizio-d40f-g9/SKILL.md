---
name: gadget-vizio-d40f-g9
description: >-
  Control supported VIZIO D40f-G9 TV functions through paired local SmartCast HTTPS using Muse
  Home Link. Use the shared Google Cast skill for media only when a separate Cast receiver is
  actually advertised.
---

# VIZIO D40f-G9

Use this skill when the confirmed VIZIO D40f-G9 exposes a compatible SmartCast local management service.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm exact model and firmware from existing device information and the current service endpoint.
- VIZIO SmartCast management and Google Cast are different interfaces. A SmartCast product name alone does not prove a currently available Cast receiver.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- An already-configured TV and a compatible pyvizio client over HomeLink HTTPS.
- Normal on-screen PIN pairing when required, secure storage of the returned authentication token, and appropriate device-specific TLS trust.
- Permission for the requested TV operation; media playback also requires a suitable receiver-reachable source.

## Workflow

1. Use the documented SmartCast pairing challenge/PIN flow with the user, or reuse valid stored credentials. Do not bypass the pairing prompt.
2. Read available power, input, app and remote-control state using the model-compatible client. Resolve IDs from device responses.
3. Perform the requested supported input/app/remote action and retain API result/error information. Do not assume a generic endpoint works on every firmware.
4. Read state after the action. If the TV is in a standby mode that disables its network API, report that limitation.
5. For an independently advertised Cast receiver, follow the [Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md) for media/status/volume; keep its sessions and verification distinct from SmartCast management.

## Verify the Result

- Confirm token-authorized access and the resulting TV state separately.
- A remote-key acknowledgment may need visual confirmation; it does not prove successful navigation.
- A Cast load is successful only under the shared skill's playback checks.

## Limits

- No UDP wake, universal standby access, cloud control, firmware changes or general app installation.
- No duplicate Cast procedure or assumed media hosting.

## Sources

- [pyvizio local SmartCast client](https://github.com/raman325/pyvizio)
- [Shared Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md)
