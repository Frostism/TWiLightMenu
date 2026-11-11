# 3DS HOME Menu overhaul

Target: the 3DS theme in `romsel_dsimenutheme`, running on a Nintendo DSi.

## Layout

The two physical screens are both 256x192. Keep the upper and lower display roles separate:

- Upper screen: selected software presentation, software title, and system/status information.
- Lower screen: touch-first software grid, page/scroll affordances, and the HOME Menu toolbar.
- Preserve the existing ROM browser, directory handling, launch validation, per-game settings, and boot paths.

## Implementation order

1. Replace the 3DS theme's horizontal carousel presentation with a multi-row software grid. Keep the existing file-browser entry indices as the source of truth; visual positions must not become file indices.
2. Update D-pad and touch hit-testing together. Selection, scrolling, folder entry/exit, drag/reorder, and page transitions must agree on the same grid geometry.
3. Build the upper-screen selected-title presentation and lower-screen toolbar from existing banner metadata and device state.
4. Add selection, scrolling, folder, and launch transitions after the static layout and input behavior are stable.
5. Rework the 3DS theme's texture set and settings presentation to match the HOME Menu visual language.
6. Build and test on DSi hardware or an accurate emulator, including large directories, empty directories, folders, both storage devices, and launch edge cases.

## Source map

- `romsel_dsimenutheme/arm9/source/main.cpp`: theme selection and texture initialization.
- `romsel_dsimenutheme/arm9/source/graphics/graphics.cpp`: frame updates and sprite rendering.
- `romsel_dsimenutheme/arm9/source/graphics/ThemeTextures.cpp` and `ThemeTextures.h`: theme texture loading and sprite access.
- `romsel_dsimenutheme/arm9/source/graphics/ThemeConfig.cpp`: theme-specific layout and palette configuration.
- `romsel_dsimenutheme/arm9/source/fileBrowse.cpp`: cursor movement, touch input, directory navigation, and launch behavior.

## Constraints

- Do not change behavior for the DSi, Saturn, HBL, or other themes.
- Keep the 256x192 per-screen coordinate space and account for DSi hardware limits.
- Do not assume 3DS-only services or hardware are available on DSi.
- Prefer reusing existing banner/icon data and texture infrastructure over duplicating ROM discovery or launch logic.
- Do not treat a source edit as complete until it has compiled and the affected interactions have been tested.
