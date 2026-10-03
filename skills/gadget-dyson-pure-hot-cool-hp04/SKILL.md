---
name: gadget-dyson-pure-hot-cool-hp04
description: >-
  Read Dyson Pure Hot+Cool HP04 status and operate supported fan or heating controls through
  Muse Home Link. Use only with a compatible local MQTT interface and existing device
  credentials.
---

# Dyson Pure Hot+Cool HP04

## Humans to Know Editorial Context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

This is a community-sourced Muse device skill, adapted by Humans to Know; it is not an official integration. Before choosing the runtime, read [repository context and task handoff](references/repository-context.md). That supplement identifies the pinned SDK, Linux command boundary, setup dependencies and the evidence to retain for this device.

Use this skill when the confirmed device is a Dyson Pure Hot+Cool HP04 and compatible local MQTT credentials are already available.

## Identify the Device

- Confirm HP04 identity and the product type required by the compatible libdyson device class.
- Use the device's current MQTT endpoint. An open MQTT port or Dyson MAC prefix is not proof that authentication or this model's commands work.

## Prerequisites

Read the [Humans to Know repository context](references/repository-context.md) before network access. It is an editorial supplement for the upstream `home_link.md` reference, whose target is absent from this public snapshot.

- Existing device serial, product type and local MQTT password/credential in approved secret storage.
- A model-compatible libdyson client using HomeLink's TCP connection. Initialize it with the supplied local values rather than invoking account/cloud discovery.
- User permission for changes, especially heating mode or temperature.

## Workflow

1. Connect the HP04-compatible local client, authenticate and subscribe to the model's status topics before requesting its current state.
2. Read available fan, airflow, oscillation, heating and environmental state through the client's model-specific properties. Keep units and missing/stale sensor values explicit.
3. For the requested change, call the matching documented HP04 operation, such as fan power/speed, supported oscillation or heating target. Respect device ranges and modes; do not send guessed generic MQTT payloads.
4. Wait for and inspect the subsequent state update. Preserve device thermal protections and any mode restrictions.

## Verify the Result

- Authentication success is separate from receiving fresh state.
- Confirm the changed property in a new state message; a publish acknowledgment is not an appliance acknowledgment.
- Treat sensor readings as device measurements, not a guarantee of room-wide air quality or temperature.

## Limits

- No cloud credential retrieval, account login, firmware changes or universal Dyson-family support in this runtime path.
- An unavailable key or incompatible product type is a setup blocker, not a reason to brute-force credentials or use unauthenticated fallback.

## Sources

- [libdyson local clients and device support](https://github.com/shenxn/libdyson)
