# Generic Depth Ex

Extended depth-buffer detection add-on for [ReShade](https://reshade.me) (Windows x64).
Standalone fork of `examples/09-depth/generic_depth_addon.cpp` from
[crosire/reshade](https://github.com/crosire/reshade) — no ReShade recompile needed,
just drop `generic_depth_ex.addon64` next to `ReShade.dll`.

Built for **PCSX2 (DX12)** + **ENTHUSIA Professional Racing**, where the depth buffer
changes resolution between menu and race and stock auto-detection keeps losing it.

## Extra features vs built-in Generic Depth

- **Draw stats heuristic**: `Lowest vertices`, `Lowest draw calls` (empty stats always lose)
- **Filter by reversed depth**: `Any / Prefer reversed / Exclude reversed` (`[DEPTH] FilterReversed`)
- **Aspect ratio heuristic → `Match custom width and height range`**: min–max filter for
  width and height (`[DEPTH] FilterResolutionMinWidth/MaxWidth/MinHeight/MaxHeight`,
  `0` = no limit on that side)
- **Auto refresh every N frames** (`[DEPTH] AutoRefreshFrames`, default `600`, `0` = off):
  restarts detection from scratch so a selection stuck after a scene change gets
  re-picked automatically (skipped while a manual override checkbox is active)

## Install

1. Download `generic_depth_ex.addon64` from the latest
   [release](https://github.com/berlint-hub/Depth-buffer_extended/releases)
   (or build it via the `build / GenericDepthEx` GitHub Actions workflow).
2. Put it next to `ReShade.dll` (or into your `AddonPath`).
3. Disable the built-in detector in `ReShade.ini`, otherwise two detectors fight
   over the same data and the game crashes on startup:
   ```ini
   [ADDON]
   DisabledAddons=Generic Depth
   ```
4. In the overlay it shows up as **Generic Depth Ex**.

## Build

CI (`windows-2022`, x64 Release): checks out `crosire/reshade`, generates
`GenericDepthEx/generic_depth_ex.cpp` from clean upstream with
`GenericDepthEx/make_ex.py`, builds with CMake and uploads the `.addon64`
artifact. Local build needs `../reshade-main` (headers + `deps/imgui`):

```
cmake -S GenericDepthEx -B build -A x64 -DRESHADE_ROOT=<path-to-reshade-main>
cmake --build build --config Release
```

See `GenericDepthEx/README.md` for details.
