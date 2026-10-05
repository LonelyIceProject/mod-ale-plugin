# ALE Lua engine for LonelyIce

ALE engine packaged as a LonelyIce plugin. Lua 5.2 is statically linked. Enabled packages may provide scripts under lua/. ALE differs from original Eluna; script compatibility must be checked individually.

Upstream: https://github.com/azerothcore/mod-ale.git

Pinned source: `bd74eae623ca63154d3eb49e1d187e872ef13370`.

This repository contains packaging, build configuration and patches, not a fork of upstream. Add its path to `LONELYICE_PLUGIN_DIRS` when building LonelyIce. Install the resulting package through the launcher or `--pkg install`.
