---
name: gadget-yi-hack-cameras
description: >-
  Retrieve snapshots or bounded streams from cameras already running compatible yi-hack
  firmware through Muse Home Link. Use the installed fork's enabled HTTP or TCP-interleaved
  RTSP service; not stock-camera access, flashing, or camera control.
---

# Yi Cameras with Existing yi-hack Firmware

Use this skill when the camera already runs a confirmed compatible yi-hack fork with its local snapshot or RTSP service enabled.

## Humans to Know editorial context

Use the pinned [Muse Gadget SDK repository](https://github.com/facebookincubator/muse-gadget-sdk/tree/468e5ba629f988ad107ba472555a8b632faac0d3) for runtime setup. Existing LAN-device clients run on its Linux Home Link through `system.run`; the Muse app, SDK token and VM connection remain required.

Community-sourced instructions, not an official integration. Read [repository-context.md](references/repository-context.md) before using this standalone package; it identifies the exact upstream repository, runtime dependencies and evidence to hand back. The original workflow and device limits below are retained.

Modified-file notice: Humans to Know added the editorial context and standalone reference links to this upstream skill.

## Identify the Device

- Confirm the exact camera hardware, installed yi-hack fork/version and enabled interfaces from existing configuration.
- Use that fork's documented endpoint/path; do not assume all Yi cameras or yi-hack forks share a snapshot URL or stream name.

## Prerequisites

Read the [Humans to Know editorial repository context](references/repository-context.md) for the pinned SDK, Linux execution boundary and task-specific verification. This supplement resolves the missing public `home_link.md` reference.

- Already-installed compatible firmware and its existing HTTP/RTSP credentials.
- A credential-aware HTTP or RTSP client using HomeLink. RTSP media must support interleaved TCP.
- User permission for the particular camera and requested image/view/recording.

## Workflow

1. Select the installed fork's documented HTTP snapshot endpoint or enabled RTSP stream. Use only the current authorized host and service.
2. For a still snapshot, make the documented authenticated read and verify the returned image type/content rather than saving an HTML error page.
3. For RTSP, explicitly use interleaved TCP for control and media; do not fall back to UDP. Decode only the requested frame or bounded stream.
4. Store the result in the user's approved private location and close the session after completing the request.

## Verify the Result

- Check that the image is fresh, decodable and from the requested camera, not cached content or a login response.
- An available web UI or open RTSP port is not proof of successful image capture.

## Limits

- Read-only snapshot/view/capture scope; no camera settings, firmware changes or arbitrary CGI operations.
- No stock-camera support, automatic flashing or claim that firmware changes are harmless or reversible.
- No UDP streams, unsupported hardware/fork combinations, implicit recording or media sharing.

## Sources

- [yi-hack Allwinner v2 documentation and supported models](https://github.com/roleoroleo/yi-hack-Allwinner-v2)
