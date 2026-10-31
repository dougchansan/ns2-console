# ns2-console

the parts of ns2-savekit that were about controlling the switch itself rather
than its saves: esp32 pad emulation, the pad-driven boot/power cycle, screen
classification + ocr, and the rpc probe tools.

split out so the save kit stops carrying it. history is preserved: `git log`
here goes back to the original commits.

## what's here

- `tools/esp32_pro/`, `tools/esp32_hid/`, `tools/esp32_gc/` — firmware sketches
  (pro controller, keyboard, gamecube pad); build artifacts stay gitignored.
- `tools/harness.cjs`, `loop.cjs`, `cycle.py`, `unwedge.cjs`,
  `pad-player1.cjs` — the pad-driven boot and power-cycle harness.
- `tools/console-state.cjs`, `tools/vision/ocr(.swift)` — screen classification
  and home-menu navigation.
- `tools/probe-commands.cjs`, `probe-info-reader.cjs`, `probe-sd.cjs` — rpc
  capability probes.
- `tools/ftp-mirror.py` — mirror a romfs tree from the OunceCake FTP server
  (content goes to [ns2-assets](https://github.com/dougchansan/ns2-assets)).
- `docs/RESULTS.md` — the full hardware log.
- `docs/HANDOFF-PRO-CONTROLLER.md`, `docs/MODLOADER.md`.

the rpc host itself is separate (`ns2jb-rpc`). the engine patches these tools
need are in [ns2-savekit/patches](https://github.com/dougchansan/ns2-savekit/tree/main/patches)
— apply them to the `ns2jb` checkout before a run.

## tests

offline, no console:

```sh
node tests/test_console_state.cjs     # screen classification (9)
node tests/test_harness_guard.cjs     # session/power-cycle rules (5)
```

these two need the sketches built into `tools/esp32_*/build/` first (that
directory is gitignored), because the harness flashes a real image:

```sh
node tests/test_harness_powercycle.cjs  # mock console, power-cycle (2)
node tests/test_harness_sequence.cjs    # mock console, whole cycle (5)
```
