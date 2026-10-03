---
name: gadget-irobot-roomba-braava
description: >-
  Read status and send supported cleaning, pause, resume, or dock commands to compatible Wi-Fi
  Roomba/Braava robots through Muse Home Link. Use documented local MQTT/TLS with existing
  BLID and password, not cloud credential retrieval.
---

# iRobot Roomba and Braava

## Humans to Know Editorial Context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

This is a community-sourced Muse device skill, adapted by Humans to Know; it is not an official integration. Before choosing the runtime, read [repository context and task handoff](references/repository-context.md). That supplement identifies the pinned SDK, Linux command boundary, setup dependencies and the evidence to retain for this device.

Use this skill when the exact Wi-Fi Roomba or Braava model and firmware support the documented local protocol.

## Identify the Device

- Confirm the robot model, firmware and protocol compatibility, then use its current local endpoint.
- The presence of TCP 8883 is not sufficient to identify an iRobot or prove local control.

## Prerequisites

Read the [Humans to Know repository context](references/repository-context.md) before network access. It is an editorial supplement for the upstream `home_link.md` reference, whose target is absent from this public snapshot.

- The robot's existing BLID and local password in approved secret storage.
- A compatible dorita980 local client or equivalent MQTT/TLS implementation routed through HomeLink; retain device-specific TLS handling.
- A commissioned robot and explicit permission for a cleaning, pause or dock action, with the area safe for movement.

## Workflow

1. Instantiate the client's local interface with the current endpoint, BLID, password and appropriate protocol version. Do not invoke cloud login or credential-discovery helpers.
2. Authenticate and subscribe to the supported state feed. Read mission/cleaning state, battery, docking and reported errors before any command.
3. Send only the model's documented requested cleaning, pause, resume or return-to-dock command. Check whether the current mission state requires a particular sequence rather than issuing generic commands blindly.
4. Monitor fresh mission state and errors, and release the local session when finished. Avoid competing sessions if the robot reports a connection conflict.

## Verify the Result

- Confirm the robot's reported mission changed, not merely that MQTT accepted a message.
- Distinguish returning to base from docked/charging and paused from completed cleaning.
- Resolve uncertain delivery by reading state before issuing another movement command.

## Limits

- No cloud credential retrieval, UDP discovery from the agent, new provisioning or universal support for all Roomba/Braava models.
- Room maps, targeted jobs, mop/water controls and station functions are outside scope unless documented for the exact model and already configured.
- Do not bypass physical, battery, bin or safety conditions.

## Sources

- [dorita980 local iRobot protocol and supported commands](https://github.com/koalazak/dorita980)
