---
name: gadget-wyze-rtsp-cameras
description: >-
  View or capture frames and bounded recordings from compatible Wyze Cam v2, Cam v3, or Pan v1
  through Muse Home Link. Use only when legacy RTSP firmware is already installed and enabled
  with TCP media transport; not firmware installation or stock-camera access.
---

# Wyze Cameras with Existing RTSP Firmware

Use this skill when an exact compatible Wyze Cam v2, Cam v3 or Cam Pan v1 already runs an enabled RTSP build.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm the camera model and installed firmware with the user or existing device configuration. A vendor prefix or open port alone cannot establish the firmware.
- Use the existing configured RTSP endpoint/path and its current address. Do not infer support for stock cameras or other generations.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- An already-installed, enabled and working RTSP build with supplied stream credentials.
- An RTSP/media client configured for interleaved TCP for both control and media, with all sockets routed through HomeLink.
- Explicit permission for the requested view, frame or bounded recording, and approved private storage.

## Workflow

1. Tell the user that these legacy RTSP builds are discontinued and may lack later security patches or app features. This skill does not recommend installing them.
2. Connect to the configured stream with credentials supplied through a credential-aware client, not a command-line URL or logged argument.
3. Explicitly request RTP/RTCP interleaved over the RTSP TCP connection. For an FFmpeg-based client the relevant transport option is `rtsp_transport=tcp`; do not allow fallback to UDP.
4. Inspect the media description, authenticate and decode the requested live view, single frame or bounded recording. Stop the session once the request is satisfied.

## Verify the Result

- Confirm a fresh decodable frame from the intended camera; an RTSP login or stream description is not image delivery.
- Check timestamps, codec errors and requested duration. Do not present a cached frame as current.
- If the stream requires UDP or authentication cannot be satisfied securely, stop and report the limitation.

## Limits

- Read-only camera access: no PTZ, siren, settings or firmware changes.
- No stock-camera support, firmware sourcing/flashing, cloud bridge deployment or universal Wyze coverage.
- Treat every frame and audio track as sensitive; no background recording, third-party upload or broader sharing.

## Sources

- [FFmpeg RTSP protocol and transport options](https://ffmpeg.org/ffmpeg-protocols.html#rtsp)
