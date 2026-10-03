---
name: gadget-zigbee2mqtt-gateways
description: >-
  Read or control supported child-device capabilities through an existing Zigbee2MQTT gateway
  using Muse Home Link. Use its authorized MQTT broker, configured base topic, and actual
  exposed properties; not gateway setup or device pairing.
---

# Zigbee2MQTT Gateways

Use this skill when the home already has a Zigbee2MQTT installation and an authorized, reachable MQTT broker.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Identify the existing broker, its configured Zigbee2MQTT base topic and the intended bridge. The MQTT broker's vendor or port alone does not identify a Zigbee gateway.
- Read `<base>/bridge/info` and `<base>/bridge/devices`. Resolve the requested child device by its stable identity and current friendly name.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- Existing broker address, discovered/confirmed TCP listener, TLS trust and broker credentials where required.
- The base topic and a compatible MQTT client such as Paho using HomeLink's TCP transport.
- Permission for the specific child device and action; broker access is not permission to change every device.

## Workflow

1. Subscribe to bridge/device state before requesting new data. Inspect each target's `definition.exposes`, including nested features, property names, endpoint names, types, ranges and allowed values.
2. Interpret an expose's `access` bitmask: 1 means published state, 2 permits set, and 4 permits get. Do not assume every state property is writable or queryable.
3. For a property with get support, publish the documented payload to `<base>/<friendly_name>/get`. Read the matching device state topic; a retained message may predate this interaction.
4. For an authorized change, publish only the supported property/value to `<base>/<friendly_name>/set`. Follow device-specific documentation for composite values or endpoints.
5. Observe the corresponding state update and bridge error responses. Battery-powered devices may sleep; report pending/unconfirmed delivery instead of repeating physical actions.

## Verify the Result

- Check reported state and timestamps when available. MQTT publish acknowledgment alone does not establish that a Zigbee device acted.
- Use the existing availability reporting when enabled; missing availability is not automatically proof of failure.
- For covers, locks or other physical actuators, distinguish requested target from achieved state.

## Limits

- No new broker/gateway installation, join-mode changes, device removal, arbitrary topic writes or fixed assumptions about household devices.
- This skill covers devices actually supported by the installed bridge and their exposed capabilities, not universal Zigbee support.

## Sources

- [Zigbee2MQTT MQTT topics and messages](https://www.zigbee2mqtt.io/guide/usage/mqtt_topics_and_messages.html)
- [Zigbee2MQTT exposed capabilities](https://www.zigbee2mqtt.io/guide/usage/exposes.html)
- [Paho MQTT Python client](https://github.com/eclipse-paho/paho.mqtt.python)
