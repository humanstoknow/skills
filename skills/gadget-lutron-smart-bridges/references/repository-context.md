# Humans to Know editorial repository context

This reference is an editorial supplement to a community-sourced skill, not an official Meta, Muse or device-vendor integration. The upstream skills README explicitly describes these integrations as community sourced. Its referenced `home_link.md` is not present in the pinned public tree, so this file provides repository execution context rather than claiming to reproduce that missing document.

## Exact source and execution choice

- Repository: [https://github.com/facebookincubator/muse-gadget-sdk](https://github.com/facebookincubator/muse-gadget-sdk)
- Pinned commit: `468e5ba629f988ad107ba472555a8b632faac0d3`
- Original skill: [`skills/gadget-lutron-smart-bridges/SKILL.md`](https://raw.githubusercontent.com/facebookincubator/muse-gadget-sdk/468e5ba629f988ad107ba472555a8b632faac0d3/skills/gadget-lutron-smart-bridges/SKILL.md)
- SDK documentation: [Linux README](https://github.com/facebookincubator/muse-gadget-sdk/blob/468e5ba629f988ad107ba472555a8b632faac0d3/linux/README.md), [Linux AGENTS](https://github.com/facebookincubator/muse-gadget-sdk/blob/468e5ba629f988ad107ba472555a8b632faac0d3/linux/AGENTS.md), [ESP32 AGENTS](https://github.com/facebookincubator/muse-gadget-sdk/blob/468e5ba629f988ad107ba472555a8b632faac0d3/esp32/AGENTS.md), [community skills README](https://github.com/facebookincubator/muse-gadget-sdk/blob/468e5ba629f988ad107ba472555a8b632faac0d3/skills/README.md).

For an already configured LAN device, use the repository’s **Linux Home Link** on a Linux machine that can reach that device. Run the task’s compatible local protocol client on that machine as its configured run-as account through `system.run`. This skill file does not itself supply a network proxy, device credentials or a discovery tool. References in the original workflow to HomeLink transport/network access must be resolved to the available runtime; do not invent a Linux `network.*` command. Start from authorized current service metadata or a user-confirmed endpoint and preserve the upstream protocol/device limits.

The SDK is a local gadget client. It requires the Muse phone app, an SDK token for pairing and a connection to the user’s Muse VM. Review the live [Gadget SDK Terms](https://gadgets.muse.ai/sdk-terms) before using the service; the repository’s Apache 2.0 code license does not itself grant service access or establish permitted service use. The public repository does not supply a self-hosted Muse model or VM replacement. Linux pairing needs Bluetooth LE, supported Linux/Python and the documented BlueZ setup. Existing paired deployments can be reused. The Muse service can exercise the run-as account’s permissions, including its sudo access if present.

ESP32 is a separate choice for building or changing custom Muse gadget firmware, not a required way to control this existing LAN device. If custom firmware becomes the requested task, use the pinned ESP32 AGENTS: ESP-IDF **v6.0.1**, a supported identified board/profile, SDK token and app/VM pairing; verify build size, host tests and boot logs. Flashing another vendor’s device is outside this skill’s existing-device workflow.

## Real Linux command boundary

The implementation is [`linux/src/musegadget/executor.py`](https://github.com/facebookincubator/muse-gadget-sdk/blob/468e5ba629f988ad107ba472555a8b632faac0d3/linux/src/musegadget/executor.py): `COMMAND_SPECS` advertises `system.run`, `file.read`, `file.write` and `device.health`. `Executor.run` dispatches them, and `_child_options()` applies the chosen account to machine-touching child processes. Use an explicit interpreter/venv path when needed; that child environment does not inherit an arbitrary interactive shell environment.

`system.run` returns stdout, stderr, exit code, timeout and truncation information. The pinned implementation defaults to **120 seconds**, caps at **600 seconds**, and truncates each output stream at **96 KiB**. Keep long media/inventory results in approved local files and return bounded evidence. File operations transfer **64 KiB** chunks. Protocol credentials belong in approved private storage, not command arguments, stdout or pasted handoff logs.

Only extend the SDK command surface when the task actually needs it: add a spec and dispatch in `executor.py`, run machine access with `_child_options()`, and add behavior coverage in `linux/tests/test_executor.py`. `linux/src/musegadget/link_client.py` connects the encrypted Noise session and registers commands; restart/re-registration makes new commands visible. Preserve registration as `platform: "linux"`, `device_family: "homehub"`; do not advertise `device.ota` or use family `link` for Linux.

## Task-specific preparation and handoff

- Record bridge model, discovered endpoint, selected HAP or LEAP path and target accessory/service IDs. Keep only a credential-storage reference in the handoff; HAP keys and LEAP certificates are different credentials.
- For HAP with ff=1, retain the authentication-stage error if pairing fails. For writes, report each target’s fresh state and shade progress/final position; a partial batch result needs per-target inspection before retry.

## Verify the SDK separately from the device action

For SDK code changes, from `linux/` run `uv run --with pytest --with . pytest`. These host tests require no Bluetooth, device or network. Pairing/Noise changes also require the Python 3.9 + cryptography 3.3.2 compatibility run documented in AGENTS; installer changes require ShellCheck.

For a deployed Home Link, `musegadget info` checks identity/pairing; `sudo systemctl status musegadget` and `sudo journalctl -u musegadget -f` expose service state. A healthy start logs `commands run as <user>`, `Noise session established`, `sent link.register` and `registered with the Muse`. Reinstalling/restarting interrupts running commands, so inspect recent invoke activity first. Successful Home Link registration proves the control session, not the target device’s requested result: apply this skill’s protocol-specific verification and report unsupported or unconfirmed outcomes explicitly.

Modified-file notice: this reference was added by Humans to Know as an editorial supplement; it is not an upstream file. Distributed with the package’s Apache 2.0 LICENSE and NOTICE.
