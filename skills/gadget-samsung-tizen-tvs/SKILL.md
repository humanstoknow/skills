---
name: gadget-samsung-tizen-tvs
description: >-
  Control compatible Samsung Tizen TVs through their paired local WebSocket interface using
  Muse Home Link. Use for requested remote or app actions, and Frame Art operations only when
  the model and Art API support them.
---

# Samsung Tizen TVs

Use this skill when a confirmed Samsung Tizen TV exposes a compatible local remote WebSocket service.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm model, firmware and the actual remote endpoint. A Frame model's Art API is separate from the general remote interface.
- Use the returned device information and installed client's compatibility notes; do not infer Art support from the Samsung brand alone.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- A compatible local WebSocket client routed through HomeLink, with device-specific TLS trust preserved.
- User approval of the normal TV pairing prompt when required, and secure storage/reuse of its returned token.
- Permission for the requested screen/audio/app action. Frame art changes require permission for the particular content and operation.

## Workflow

1. Establish the documented local remote connection with a recognizable client name and valid saved token, or complete normal user-approved pairing.
2. Read available device, power, app or Art status using interfaces documented for this model. Do not substitute a cloud API when a local read is unavailable.
3. For remote control, send only the requested supported key or application operation; resolve real app identifiers rather than guessing them.
4. On an Art-capable Frame with a supported Art API version, use its documented status/mode, available-art selection or requested upload operation. Transfer only user-approved media through the documented local client protocol, and authorize any additional destination it requires.
5. Check the relevant response and available state after each action. Do not use a key sequence as an unverified substitute for an unavailable direct operation.

## Verify the Result

- A WebSocket acknowledgment or accepted key does not prove the desired on-screen outcome; use state read-back or user confirmation.
- Check Art API version/errors separately from remote pairing. Remote success does not establish Art support.
- Inspect state before retrying power or other toggle-like commands.

## Limits

- No cloud alternatives, UDP wake or device-to-agent media fetch without an existing reachable source.
- No universal Frame Art coverage, app installation, unrequested content replacement/deletion or firmware changes.
- A sleeping TV may not expose its API; do not promise wake from network presence alone.

## Sources

- [Samsung TV local WebSocket and Art API client](https://github.com/xchwarze/samsung-tv-ws-api)
