---
name: gadget-google-tv-streamer
description: >-
  Control Google TV Streamer media through Google Cast or remote keys and app links through
  Android TV Remote v2 using Muse Home Link. Use the currently available service and normal
  Remote v2 pairing; not a generic Google TV device skill.
---

# Google TV Streamer

## Humans to Know Editorial Context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

This is a community-sourced Muse device skill, adapted by Humans to Know; it is not an official integration. Before choosing the runtime, read [repository context and task handoff](references/repository-context.md). That supplement identifies the pinned SDK, Linux command boundary, setup dependencies and the evidence to retain for this device.

Use this skill when the confirmed Google TV Streamer advertises a usable Cast receiver and/or Android TV Remote v2 service.

## Identify the Device

- Match Google TV Streamer model identity and current firmware/service metadata rather than assuming every Google TV device is this model.
- Keep Cast and Remote v2 endpoints distinct. Availability of one does not prove the other has paired or can control the device.

## Prerequisites

Read the [Humans to Know repository context](references/repository-context.md) before network access. It is an editorial supplement for the upstream `home_link.md` reference, whose target is absent from this public snapshot.

- An already-provisioned device with the relevant interface available.
- For Remote v2, a compatible `androidtvremote2` client, the normal on-screen pairing code and securely retained client certificate/private key.
- For media playback, a user-approved source the receiver can already reach.

## Workflow

1. For Cast media, volume and status, use the [Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md), including receiver capabilities and result checks.
2. For Remote v2, use the confirmed service and the client's documented pairing/control connection setup over HomeLink TCP. Do not treat an open control port as a paired session.
3. If necessary, start normal user-approved pairing, complete the displayed code challenge and retain the client certificate/private key. Pairing and control are separate connections.
4. After authenticated connection, send the requested documented remote key or app link using the compatible client. Use supported power/volume behavior only when exposed by this device.
5. Observe available state and ask for visual confirmation where navigation has no reliable programmatic read-back.

## Verify the Result

- Keep discovery, pairing success and remote-control success as separate outcomes.
- For Cast, follow the shared skill and do not count an app launch or buffering state as playback.

## Limits

- No screenshots, package queries/installation, battery/screen inspection, developer/debugging setup or fallback control.
- No universal foreground-app query, app entitlement or off-state wake promise.

## Sources

- [Shared Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md)
- [Android TV Remote v2 client](https://github.com/tronikos/androidtvremote2)
