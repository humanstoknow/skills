---
name: gadget-philips-hue-bridges
description: >-
  Control lights, brightness, and existing room, zone, or scene actions through a Philips Hue
  CLIP v2 bridge using Muse Home Link. Use a local application key or normal physical-button
  pairing; excludes legacy BSB001.
---

# Philips Hue Bridges: Local Light and Scene Control

Use this skill when fresh discovery identifies a Hue bridge exposing CLIP v2 and the user requests lights, brightness or an existing scene.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Match a Hue bridge, not an individual Zigbee bulb. `_hue._tcp` and bridge metadata can identify the current endpoint.
- Read bridge model/API information, such as `/api/0/config`, after host authorization. Confirm CLIP v2 support on the square BSB002 bridge.
- Round BSB001 uses a different legacy API and is not covered by this recipe.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- Use an existing local application key or obtain one with the user's physical bridge-button approval. An account/cloud discovery service is not required.
- For pairing, POST a documented `devicetype` to `/api` while the link-button window is open and securely store the returned username/application key. Do not log it.
- Validate the bridge's TLS identity using a configured/pinned bridge trust path; do not disable certificate validation globally.

## Workflow

1. Send the stored key in the `hue-application-key` header through a credential-aware client, never as a credential-bearing shell command.
2. GET `/clip/v2/resource/light`, `/room`, `/zone` and `/scene` as needed. Resolve resource IDs from current responses and disambiguate names before writes.
3. Read the target light's on/dimming/color capability fields and connectivity. Only send fields the device actually supports.
4. PUT `/clip/v2/resource/light/<id>` with a minimal body such as `{"on":{"on":true}}` or `{"dimming":{"brightness":40}}`. Brightness is percent; honor the reported minimum. Color temperature, when requested and supported, uses the reported `mirek_schema` range.
5. For an existing room/zone, resolve its `grouped_light` service before a group write. For an existing scene, PUT `/clip/v2/resource/scene/<id>` with `{"recall":{"action":"active"}}`. Confirm the affected group is intended.
6. Inspect both response `data` and `errors`, then poll the affected lights after transitions. Group broadcasts can miss an individual bulb.

## Verify the Result

- Confirm resulting per-light state, not only a group response or scene acknowledgement.
- For a group where every lamp must change, identify any straggler and re-read before an individually scoped correction.
- A scene may contain multiple intended states; compare to its actions rather than assuming every member should be on.

## Limits

- No UDP Entertainment streaming, cloud discovery, automatic firmware updates, group creation or automation editing in this skill.
- Check capabilities separately for Bridge Pro and each bulb. Unsupported effects remain unsupported.

## Sources

- [Hue developer documentation](https://developers.meethue.com/develop/hue-api-v2/)
- [Hue getting started](https://developers.meethue.com/develop/get-started-2/)
