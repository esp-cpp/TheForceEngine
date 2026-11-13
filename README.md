# The Force Engine (Star Wars: Dark Forces) for ESP32 emulators

Dark Forces for esp-box-emu: a subset of [The Force Engine](https://github.com/luciusDXL/TheForceEngine) taken from BSzili's Amiga branch, with the ESP32 platform layer in `port/platform/`. `VENDOR.md` lists the source and every change made for the ESP32. `port/alloc_redirect.syms` renames the core's allocation calls so they land in the board's ROM block (see the firmware's component wrapper).

## Layout

- `tfe/`: the emulator core, as esp-box-emu has carried it (see below for its origin and changes).
- `port/`: the glue to the host firmware: `port/include/darkforces.hpp` is the API the
  firmware calls (init / run a frame / save / load / deinit, the video buffer for
  screenshots), `port/` implements it.

The port talks to the board through the `BoxEmu` singleton of
[esp-box-emu](https://github.com/esp-cpp/esp-box-emu) (`box-emu.hpp`: frame
buffers, `push_frame()`, `palette()`, `play_audio()`, `gamepad_state()`, ...).
[tab5-emu](https://github.com/esp-cpp/tab5-emu) provides the same API
(`using BoxEmu = Tab5Emu`), so the same sources build for both.

## Use

Add this repository as a submodule inside an ESP-IDF component and register its
sources from that component's `CMakeLists.txt` (each firmware keeps its own
component wrapper: the requirements, compile flags and linker fragment for
its target). See esp-box-emu's `components/darkforces/CMakeLists.txt`.

The history up to the split is esp-box-emu's history of `components/darkforces`.

## License

The Force Engine is under the GNU General Public License, version 2. The port is under the same license. The game data (`DARK.GOB` etc.) is not included: it comes from a copy of the game.
