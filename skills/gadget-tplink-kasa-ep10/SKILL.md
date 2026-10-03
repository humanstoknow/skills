---
name: gadget-tplink-kasa-ep10
description: >-
  Read status and control relay or LED state on compatible TP-Link Kasa EP10 plugs through
  Muse Home Link. Use the confirmed legacy IOT local TCP protocol; EP10 has no energy metering
  and does not use the EP25 authentication path.
---

# TP-Link Kasa EP10: Relay and LED Control

Use this skill when fresh discovery and a scoped device read confirm a compatible Kasa EP10 and the user requests plug status or switching.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm exact model, hardware and firmware from legacy `system.get_sysinfo`, not a vendor prefix or open port.
- Use its discovered local TCP endpoint; the legacy service commonly uses 9999. Do not run client UDP discovery from the agent.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- Know the connected load and confirm that switching it is intended.
- The legacy interface needs no account credentials. If current firmware identifies a different protocol, do not force legacy XOR or silently switch to another family's skill.

## Workflow

1. Use python-kasa's explicitly configured legacy IOT plug client through the shared network access; supply the known endpoint without broadcast discovery.
2. For an agent-authored transport, legacy TCP frames use a four-byte big-endian payload length and TP-Link's documented XOR-autokey JSON encoding; prefer the maintained implementation over inventing encryption.
3. Read `{"system":{"get_sysinfo":{}}}` and retain `relay_state` and `led_off`.
4. Set the relay using `system.set_relay_state` with `state` 1 or 0, or the client's `turn_on`/`turn_off`. Set `system.set_led_off` only when an LED change is requested; `off:1` disables the LED.
5. Refresh the device state after each action. Check device error codes, not merely receipt of a TCP frame.

## Verify the Result

- Confirm the desired relay state; LED state is separate and inverted by `led_off`.
- A relay acknowledgement does not prove the connected appliance performed its own task.
- If a response is uncertain, read state before retrying; do not retry with a toggle.

## Limits

- EP10 has no energy meter. Do not promise watts, energy history or EP25 authentication/features.
- No reset, network changes, credential updates, schedules or firmware actions.

## Sources

- [python-kasa](https://github.com/python-kasa/python-kasa)
- [Documented legacy command examples](https://github.com/softScheck/tplink-smartplug)
