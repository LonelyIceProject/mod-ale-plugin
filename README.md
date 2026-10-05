# ALE Lua engine for LonelyIce

ALE engine packaged as a LonelyIce plugin. Lua 5.2 is statically linked. Enabled packages may provide scripts under lua/. ALE differs from original Eluna; script compatibility must be checked individually.

Upstream: https://github.com/azerothcore/mod-ale.git

Pinned source: `bd74eae623ca63154d3eb49e1d187e872ef13370`.

This repository contains packaging, build configuration and patches, not a fork of upstream. Add its path to `LONELYICE_PLUGIN_DIRS` when building LonelyIce. Install the resulting package through the launcher or `--pkg install`.

Playerbots-specific Lua bindings are excluded from this base engine package; ordinary player, group and world scripting remains available.

## Building

Requires a LonelyIce build using core ABI `lonelyice-ac-2`, CMake, Git and the core's C++ dependencies. Add this repository's absolute path to the semicolon-separated `LONELYICE_PLUGIN_DIRS` CMake option; CMake fetches the pinned upstream revision and applies the patches in `patches/`. It never modifies your upstream checkout. Windows x64 and Linux x64 are supported.

```sh
cmake -S /path/to/lonelyice -B /path/to/build -DLONELYICE_CORE_DIR=/path/to/core -DLONELYICE_PLUGIN_DIRS=/path/to/mod-ale-plugin
cmake --build /path/to/build --config RelWithDebInfo --target mod-ale
```

The plugin folder is written under the build's `bin/plugins` directory (`bin/RelWithDebInfo/plugins` with Visual Studio). Package it with `LonelyIce --pkg pack <plugin-folder> <output-folder>`. The release ZIP contains both platform libraries where applicable. Source revisions are recorded in `plugin.json`; the port's source and patches remain available in this repository under the upstream license.

## Installing and configuring

Install `mod-ale` from the catalog, then restart the server. Configure the engine through the GUI or TUI; settings are saved in the active YAML profile. Lua 5.2.4 is included statically, so a separate Lua runtime is unnecessary. `LUA-LICENSE.txt` includes its MIT license.

`ALE.ScriptPath` defaults to `lua_scripts`. In addition, the engine discovers `lua/` inside enabled, successfully loaded plugin packages, including AIO. Disabled packages do not run Lua. Extensions load before ordinary scripts; ordinary scripts follow ALE's filename ordering, so scripts should use `require` for module dependencies. Auto reload watches the local scripts directory only; restart after updating packages.

This is ALE, not a replacement promising compatibility with every original Eluna script. Playerbots-specific bindings are excluded. Lua files execute with server process privileges; install scripts from trusted sources. Compilation was verified on Windows and Linux; game API behavior still needs in-game verification.
