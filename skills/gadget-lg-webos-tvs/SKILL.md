---
name: gadget-lg-webos-tvs
description: >-
  Control supported LG webOS TV inputs, apps, audio, and remote functions through Muse Home
  Link. Use normal local SSAP WebSocket pairing; route Cast media to the shared Google Cast
  skill only if a separate receiver is advertised.
---

# LG webOS TVs

## Humans to Know Editorial Context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

This is a community-sourced Muse device skill, adapted by Humans to Know; it is not an official integration. Before choosing the runtime, read [repository context and task handoff](references/repository-context.md). That supplement identifies the pinned SDK, Linux command boundary, setup dependencies and the evidence to retain for this device.

Use this skill when a confirmed LG webOS display exposes its local SSAP WebSocket service.

## Identify the Device

- Confirm exact model/webOS version and the discovered WebSocket endpoint; capabilities vary by model and firmware.
- A DIAL advertisement is not proof of Cast. Only use the Cast path when current firmware independently advertises a Cast receiver.

## Prerequisites

Read the [Humans to Know repository context](references/repository-context.md) before network access. It is an editorial supplement for the upstream `home_link.md` reference, whose target is absent from this public snapshot.

- An already-configured display and a compatible SSAP client such as bscpylgtv using HomeLink TCP/WebSocket access.
- Normal on-screen approval for the client registration when needed; store the returned client key securely and reuse it.
- User permission for the requested input, app, audio, notification or remote operation.

## Workflow

1. Connect and register using the normal SSAP pairing flow, preserving the configured TLS trust and stored client key. A rejected/canceled prompt is not authorization.
2. Read available system, power, audio, input and application information. Select actual returned input/app IDs rather than guessing.
3. Issue the requested supported SSAP control: audio/mute, input selection, installed-app launch, notification or remote action. If a pointer/button socket URL is returned, validate its host and obtain authorization for a changed destination before opening it.
4. Monitor SSAP responses/subscriptions or read state again. For keys/notifications lacking full feedback, report the limitation.
5. If an independent Cast receiver is advertised and the user wants its media controls, follow the [Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md); apply its receiver capability checks and do not duplicate its procedure.

## Verify the Result

- Check SSAP success/error responses and the specific state affected, not merely WebSocket connectivity.
- Do not equate Cast app status with the TV's input or all foreground-app state.

## Limits

- No UDP wake, silent pairing bypass, cloud alternative or assumed inbound media server.
- Standby availability and installed apps vary by model. An unreachable sleeping TV cannot be promised a local wake through this skill.
- No arbitrary device configuration, software installation or screenshot path.

## Sources

- [bscpylgtv SSAP client](https://github.com/chros73/bscpylgtv)
- [Shared Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md)
