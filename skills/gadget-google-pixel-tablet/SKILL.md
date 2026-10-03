---
name: gadget-google-pixel-tablet
description: >-
  Play supported Cast media and control playback or volume on Google Pixel Tablet through Muse
  Home Link. Use the shared Google Cast skill only while the tablet exposes its receiver in
  supported docked, locked Hub Mode.
---

# Google Pixel Tablet

## Humans to Know Editorial Context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

This is a community-sourced Muse device skill, adapted by Humans to Know; it is not an official integration. Before choosing the runtime, read [repository context and task handoff](references/repository-context.md). That supplement identifies the pinned SDK, Linux command boundary, setup dependencies and the evidence to retain for this device.

Use this skill when current discovery identifies the Google Pixel Tablet with its Cast receiver currently available.

## Identify the Device

- Confirm Pixel Tablet model metadata and a currently advertised receiver. The tablet being on Wi-Fi is not sufficient.
- Match the current receiver to the user's intended device; use its current service endpoint, not a remembered address.
- The tablet must be in its supported docked, locked Hub Mode with Cast reception enabled. Check current availability; do not assume undocked or unlocked operation.

## Prerequisites

Read the [Humans to Know repository context](references/repository-context.md) before network access. It is an editorial supplement for the upstream `home_link.md` reference, whose target is absent from this public snapshot.

- The device is already provisioned and available as a receiver in its supported operating state.
- For new playback, an existing user-authorized media source must be reachable by the receiver. Device access alone does not supply media hosting.

## Workflow

1. Use the [Google Cast skill](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md) for discovery interpretation, receiver/media status, playback controls, volume, media loading and verification. That skill is the single protocol procedure.
2. Apply this profile's model and operating-state restrictions before selecting an operation. Missing media status while idle is not proof that the device is unreachable.
3. For an existing group, follow the shared skill's separate group-target rules and confirm that the user intended all group members.

## Verify the Result

- Follow the shared skill's operation-specific checks and report whether the result was actually observed on this device.

## Limits

- No arbitrary undocked casting, battery/screen inspection, screenshots, general tablet automation, private setup routes or device configuration.
- Do not infer device administration or general remote-control capabilities from Cast support.

## Sources

- [Shared Google Cast skill and protocol sources](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-google-cast/SKILL.md)
